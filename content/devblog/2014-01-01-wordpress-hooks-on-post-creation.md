---
title: WordPress hooks on post creation
author: Ruslan Bes
type: post
date: 2014-01-01T01:17:07+00:00
url: /2014/01/01/wordpress-hooks-on-post-creation/
dsq_thread_id:
  - 4837278733
categories:
  - PHP
  - WordPress
tags:
  - WordPress insides
  - wp_insert_post

---
When you create a post in WordPress a bunch of actions happen and a lot of them allow hooking into. In this post I&#8217;ll examine the process of post creation in detail.

The function responsible for doing main stuff is called [wp\_insert\_post()][1].

And immediately there is something interesting about it. Namely when you just go to Dashboard and click [Posts]→[Add New] to just to get a form for a new record, WordPress already calls that function! So let&#8217;s look closely what it does.

# wp\_insert\_post explained

So wp\_insert\_post is called presumably whenever any change to a post occurs. What if we want to catch some specific event? Like when post was just published. Or just saved as draft. How can we determine if the post was published first time or just updated?

Let&#8217;s look at the wp\_insert\_post function closely.

<pre class="brush: php; title: ; notranslate" title="">function wp_insert_post( $postarr, $wp_error = false ) { ... }</pre>

When it&#8217;s called first time for presenting new form it&#8217;s being given following array of parameters:

`post_title: string = "Auto Draft"<br />
post_type: string = post<br />
post_status: string = auto-draft`

It gets then a bunch of default params, the most interesting one is $previous_status = &#8216;new&#8217; which we could hopefully use somehow.

<pre class="brush: php; highlight: [0,17]; title: ; notranslate" title="">function wp_insert_post( $postarr, $wp_error = false ) {
	global $wpdb;

	$user_id = get_current_user_id();

	$defaults = array('post_status' =&gt; 'draft', 'post_type' =&gt; 'post', 'post_author' =&gt; $user_id,
		'ping_status' =&gt; get_option('default_ping_status'), 'post_parent' =&gt; 0,
		'menu_order' =&gt; 0, 'to_ping' =&gt;  '', 'pinged' =&gt; '', 'post_password' =&gt; '',
		'guid' =&gt; '', 'post_content_filtered' =&gt; '', 'post_excerpt' =&gt; '', 'import_id' =&gt; 0,
		'post_content' =&gt; '', 'post_title' =&gt; '');

	$postarr = wp_parse_args($postarr, $defaults);
... 
	if ( ! empty( $ID ) ) {
		...
	} else {
		$previous_status = 'new';
	}
}</pre>

Now the first hook to be called is the filter `wp_insert_post_parent` but it&#8217;s used for pages so we don&#8217;t care so much about it.

<pre class="brush: php; title: ; notranslate" title="">function wp_insert_post( $postarr, $wp_error = false ) { 
...
	// Check the post_parent to see if it will cause a hierarchy loop
	$post_parent = apply_filters( 'wp_insert_post_parent', $post_parent, $post_ID, compact( array_keys( $postarr ) ), $postarr );
...
 }</pre>

After that [wp\_unique\_post_slug()][2] function is called which might trigger `wp_unique_post_slug` filter and bunch of other related filters. Probably it&#8217;s related to generation of the permalink

<pre class="brush: php; title: ; notranslate" title="">function wp_insert_post( $postarr, $wp_error = false ) { 
...
	$post_name = wp_unique_post_slug($post_name, $post_ID, $post_status, $post_type, $post_parent);
...
 }</pre>

The next filter called is `wp_insert_post_data`. Despite its name it is called on post update too. It gets all the `$data` about the post plus the original `$postarr`. This two arrays contain almost the same information, I won&#8217;t go into details because I don&#8217;t understand why there are two arrays in the first place. At this moment both these arrays has the same `$data['post_title'] == $postarr['post_title'] == 'Auto Draft'`. Also `$postarr['ID'] == 0` indicating that we are in the process of creating post. 

Now look closely. Up to this moment nothing has been written into db. This is your first chance to insert some <del>XSRF</del> valuable data into the post. Simply edit the `$data` param.

<pre class="brush: php; title: ; notranslate" title="">function wp_insert_post( $postarr, $wp_error = false ) { 
...
	// expected_slashed (everything!)
	$data = compact( array( 'post_author', 'post_date', 'post_date_gmt', 'post_content', 'post_content_filtered', 'post_title', 'post_excerpt', 'post_status', 'post_type', 'comment_status', 'ping_status', 'post_password', 'post_name', 'to_ping', 'pinged', 'post_modified', 'post_modified_gmt', 'post_parent', 'menu_order', 'guid' ) );
	$data = apply_filters('wp_insert_post_data', $data, $postarr);
	$data = wp_unslash( $data );
...
 }</pre>

_By the way I still fail to understand this coding convention about when to put a space between a bracket and a param.  
_ 

Depending on the calculated `$update` variable WordPress decides whether to call UPDATE or INSERT sql. Normally INSERT should be called only once — the first time. So it happens only when you open the &#8220;Add New&#8221; form. Imagine we are now in this workflow.

<pre class="brush: php; title: ; notranslate" title="">function wp_insert_post( $postarr, $wp_error = false ) { 
...
	if ( $update ) {
		do_action( 'pre_post_update', $post_ID, $data );
		if ( false === $wpdb-&gt;update( $wpdb-&gt;posts, $data, $where ) ) {
...
		}
	} else {
...
		if ( false === $wpdb-&gt;insert( $wpdb-&gt;posts, $data ) ) {
...
		}
	}
...
 }</pre>

BTW, you may notice that right before updating the `pre_post_update` action is called. This can be handy to distinguish the further calls.

Now post is inserted into db and $post_ID is generated. Even though you didn&#8217;t actually written anything!

Let&#8217;s go further. [wp\_set\_post_categories()][3], [wp\_set\_post_tags()][4], [wp\_set\_post_terms()][5] functions may be called which might trigger `add_term_relationship` and `added_term_relationship` actions. Also later `set_object_terms` action.

`$wpdb->update()` might be called for auto-drafts only to update the `post_name` that is, the slug for permalink.

[clean\_post\_cache()][6] function is called which executes `clean_post_cache` action and `clean_page_cache` action.

another `$wpdb->update()` might be called but only to set guid (also for permalink)

wp\_transition\_post_status() is called which does nothing but three action calls: `transition_post_status`, `{$old_status}_to_{$new_status}` and `{$new_status}_{$post->post_type}`. More information is [here][7]:

<pre class="brush: php; title: ; notranslate" title="">function wp_insert_post( $postarr, $wp_error = false ) { 
...
	wp_transition_post_status($data['post_status'], $previous_status, $post);
...
 }</pre>

If the post gets updated then `edit_post` action is called and then `post_updated` action. The difference is that `post_updated` receives two params: `$post_after` and `$post_before`. `$post_before` is the post after the very first `$wpdb->update` call whereas `$post_after` is the post after `edit_post` action.

Finally `save_post` and `wp_insert_post` actions are called with the same parameters. These two actions **are equivalent**!

<pre class="brush: php; title: ; notranslate" title="">function wp_insert_post( $postarr, $wp_error = false ) { 
...
	if ( $update ) {
		do_action('edit_post', $post_ID, $post);
		$post_after = get_post($post_ID);
		do_action( 'post_updated', $post_ID, $post_after, $post_before);
	}

	do_action( "save_post_{$post-&gt;post_type}", $post_ID, $post, $update );
	do_action( 'save_post', $post_ID, $post, $update );
	do_action( 'wp_insert_post', $post_ID, $post, $update );
...
 }</pre>

 [1]: http://codex.wordpress.org/Function_Reference/wp_insert_post
 [2]: http://codex.wordpress.org/Function_Reference/wp_unique_post_slug
 [3]: http://codex.wordpress.org/Function_Reference/wp_set_post_categories
 [4]: http://codex.wordpress.org/Function_Reference/wp_set_post_tags
 [5]: http://codex.wordpress.org/Function_Reference/wp_set_post_terms
 [6]: http://codex.wordpress.org/Function_Reference/clean_post_cache
 [7]: http://codex.wordpress.org/Post_Status_Transitions "Post Status Transitions"