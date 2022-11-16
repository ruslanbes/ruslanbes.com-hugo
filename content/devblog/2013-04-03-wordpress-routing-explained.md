---
title: WordPress routing explained
author: Ruslan Bes
type: post
date: 2013-04-03T18:07:46+00:00
url: /2013/04/03/wordpress-routing-explained/
dsq_thread_id:
  - 4832620831
categories:
  - WordPress
tags:
  - routing
  - WordPress insides
  - WTF

---
In this article I&#8217;ll explain how WordPress parses request URL and decides what to show and what to do. Unlike other frameworks like <a title="Drupal hook_menu" href="http://api.drupal.org/api/drupal/modules!system!system.api.php/function/hook_menu" target="_blank">Drupal</a> or <a title="Kohana routing" href="http://kohanaframework.org/3.0/guide/kohana/routing" target="_blank">Kohana</a> where you basically should define the mapping &#8220;URL-pattern → function call&#8221;, in WordPress the whole process is much more complicated and at the same time less flexible. Of course, there is a nice <a title="WP Router" href="http://WordPress.org/extend/plugins/wp-router/" target="_blank">WP Router</a> plugin, but let&#8217;s try to look what WordPress can do on its own.

# Module &#8220;Except&#8221;

Let&#8217;s make a toy module called &#8220;Except&#8221;. Recall that WordPress by default allows you to see all posts of a selected category by requesting `http://wp.example.com/category/ABCDE/` URL. What we want from this module is that when someone asks for a URL `http://wp.example.com/except/category/ABCDE/` then we show all posts except from category ABCDE. 

You can download the final module now from [here][1] and learn-by-example or you can stay with me and learn how to build it step by step.

First let&#8217;s try and enter those URLs on some WP-supported site. I took Groupon Blog as example:  
<a href="https://blog.groupon.com/category/cities/" target="_blank">https://blog.groupon.com/category/cities/</a> works.  
<a href="https://blog.groupon.com/except/category/cities/" target="_blank">https://blog.groupon.com/except/category/cities/</a> — Page Not Found. Unsurprisingly WordPress tells us that it doesn&#8217;t know what you&#8217;re asking for.

## Step 0: Skeleton

I start with the default template for a Module.

<pre class="brush: php; auto-links: false; title: ; notranslate" title="">/* Plugin Name: Except
Plugin URI: http://ruslanbes.com/projects/except
Description: Make urls like http://wp.example.com/except/category/ABCDE/ display all posts except from category ABCDE
Version: 2013.03.26
Author: Ruslan Bes &lt; me@ruslanbes.com &gt;
Author URI: http://ruslanbes.com/devblog
*/

define('EXCEPT_PATH', dirname(__FILE__) .'/');

class Except {

}
$Except = new Except();</pre>

It only defines three things:

  1. `EXCEPT_PATH` constant to reference any files inside the module (not used here but very handy in a plugin-writer life)
  2. A class `Except`,
  3. A singleton object of that class `$Except`

## Step 1: Hooking into rewrite rules

First thing to do is to tell WordPress that when the module is first **activated**, WordPress should **add new rewrite rules** to the default list, and refresh it. And to be clean and nice on **deactivate** we should remove those rules.

<pre class="brush: php; auto-links: false; collapse: true; highlight: [0,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29]; light: false; title: ; toolbar: true; notranslate" title="">/*
Plugin Name: Except
Plugin URI: http://ruslanbes.com/projects/except
Description: Make urls like http://wp.example.com/except/category/ABCDE/ display all posts except from category ABCDE
Version: 2013.03.26
Author: Ruslan Bes &lt; me@ruslanbes.com &gt;
Author URI: http://ruslanbes.com/devblog
*/

define('EXCEPT_PATH', dirname(__FILE__) .'/');

class Except {
  function __construct() {
      register_activation_hook( __FILE__, array($this, 'activate') );
      register_deactivation_hook( __FILE__, array($this, 'deactivate') );
  }

  function activate() {
    add_action( 'generate_rewrite_rules', array($this, 'add_rewrite_rules') );
    global $wp_rewrite; $wp_rewrite-&gt;flush_rules(); // force call to generate_rewrite_rules()
  }

  function deactivate() {
    global $wp_rewrite; $wp_rewrite-&gt;flush_rules();
  }

  function add_rewrite_rules(){
    // ???
  }

}
$Except = new Except();</pre>

We left the key function `add_rewrite_rules()` empty for now. I&#8217;m gonna do some explanations and then we fill in the code.

### How WordPress rewrite rules works

So what are rewrite rules? And where do they come from? And why do we need to hook into them?

First of all, &#8220;what&#8221;. Rewrite rules is an array like this:

<pre>array = 
  category/(.+?)/feed/(feed|rdf|rss|rss2|atom)/?$: string = index.php?category_name=$matches[1]&feed=$matches[2]
  category/(.+?)/(feed|rdf|rss|rss2|atom)/?$: string = index.php?category_name=$matches[1]&feed=$matches[2]
  category/(.+?)/page/?([0-9]{1,})/?$: string = index.php?category_name=$matches[1]&paged=$matches[2]
  category/(.+?)/?$: string = index.php?category_name=$matches[1]
  tag/([^/]+)/feed/(feed|rdf|rss|rss2|atom)/?$: string = index.php?tag=$matches[1]&feed=$matches[2]
  tag/([^/]+)/(feed|rdf|rss|rss2|atom)/?$: string = index.php?tag=$matches[1]&feed=$matches[2]
  tag/([^/]+)/page/?([0-9]{1,})/?$: string = index.php?tag=$matches[1]&paged=$matches[2]
  tag/([^/]+)/?$: string = index.php?tag=$matches[1]
  ...</pre>

This array is stored in the database under the option `rewrite_rules` and it is loaded into WordPress inside `WP_Rewrite::wp_rewrite_rules()`. As you may already figured out from the above example, `rewrite_rules` contains instructions on how to transform URL from address bar into the usual GET string. It is needed to support Human Readable URLs, that is, to send both Alice who typed `http://wp.example.com/category/carol` and Bob who typed `http://wp.example.com/index.php?category_name=carol` to the same page. That&#8217;s all. This also explains why do we care about it — because we want to have our nice short URLs. And this also answers the question why it is stored into the option and not generated on every page load — simply because normally it shouldn&#8217;t change between page calls.

### Step 2½: Setting our rewrite rules

So now we know what we need to add to the rewrite rules in our `add_rewrite_rules()` code. We probably need something like:

<pre>array = 
  except/category/(.+?)/feed/(feed|rdf|rss|rss2|atom)/?$: string = index.php?except_category_name=$matches[1]&feed=$matches[2]
  except/category/(.+?)/(feed|rdf|rss|rss2|atom)/?$: string = index.php?except_category_name=$matches[1]&feed=$matches[2]
  except/category/(.+?)/page/?([0-9]{1,})/?$: string = index.php?except_category_name=$matches[1]&paged=$matches[2]
  except/category/(.+?)/?$: string = index.php?except_category_name=$matches[1]</pre>

Let&#8217;s try it!

<pre class="brush: php; auto-links: false; collapse: true; highlight: [0,28,29,30,31,32,33,34]; light: false; title: ; toolbar: true; notranslate" title="">/*
Plugin Name: Except
Plugin URI: http://ruslanbes.com/projects/except
Description: Make urls like http://wp.example.com/except/category/ABCDE/ display all posts except from category ABCDE
Version: 2013.03.26
Author: Ruslan Bes &lt; me@ruslanbes.com &gt;
Author URI: http://ruslanbes.com/devblog
*/

define('EXCEPT_PATH', dirname(__FILE__) .'/');

class Except {
  function __construct() {
      register_activation_hook( __FILE__, array($this, 'activate') );
      register_deactivation_hook( __FILE__, array($this, 'deactivate') );
  }

  function activate() {
    add_action( 'generate_rewrite_rules', array($this, 'add_rewrite_rules') );
    global $wp_rewrite; $wp_rewrite-&gt;flush_rules(); // force call to generate_rewrite_rules()
  }

  function deactivate() {
    global $wp_rewrite; $wp_rewrite-&gt;flush_rules();
  }

  function add_rewrite_rules($wp_rewrite){
    $rules = array(
      'except/category/(.+?)/feed/(feed|rdf|rss|rss2|atom)/?$'  =&gt; 'index.php?except_category_name=$matches[1]&feed=$matches[2]',
      'except/category/(.+?)/(feed|rdf|rss|rss2|atom)/?$'       =&gt; 'index.php?except_category_name=$matches[1]&feed=$matches[2]',
      'except/category/(.+?)/page/?([0-9]{1,})/?$'              =&gt; 'index.php?except_category_name=$matches[1]&paged=$matches[2]',
      'except/category/(.+?)/?$'                                =&gt; 'index.php?except_category_name=$matches[1]',
      );
    $wp_rewrite-&gt;rules = $rules + (array)$wp_rewrite-&gt;rules;
  }

}
$Except = new Except();</pre>

Great. Now **activate the module** and try to go to `http://wp.example.com/except/category/enter_some_gibberish_here/`. Instead of Page Not Found error you will now see the main page of your blog. That&#8217;s much better. WordPress knows that the URL is valid but doesn&#8217;t yet know what to show you, so it shows you all posts.

## Step 3: Filtering the posts

Now we need to hook into WordPress query and filter the posts. Luckily there is a beautiful place for it — the <a href="http://codex.WordPress.org/Plugin_API/Action_Reference/pre_get_posts" target="_blank">pre_get_posts</a> hook. Let&#8217;s try it.

<pre class="brush: php; auto-links: false; collapse: true; highlight: [16,38,39,40,41,42,43,44]; light: false; title: ; toolbar: true; notranslate" title="">/*
Plugin Name: Except
Plugin URI: http://ruslanbes.com/projects/except
Description: Make urls like http://wp.example.com/except/category/ABCDE/ display all posts except from category ABCDE
Version: 2013.03.26
Author: Ruslan Bes &lt; me@ruslanbes.com &gt;
Author URI: http://ruslanbes.com/devblog
*/

define('EXCEPT_PATH', dirname(__FILE__) .'/');

class Except {
  function __construct() {
      register_activation_hook( __FILE__, array($this, 'activate') );
      register_deactivation_hook( __FILE__, array($this, 'deactivate') );
      add_action( 'pre_get_posts', array($this, 'pre_get_posts') );
  }

  function activate() {
    add_action( 'generate_rewrite_rules', array($this, 'add_rewrite_rules') );
    global $wp_rewrite; $wp_rewrite-&gt;flush_rules(); // force call to generate_rewrite_rules()
  }

  function deactivate() {
    global $wp_rewrite; $wp_rewrite-&gt;flush_rules();
  }

  function add_rewrite_rules($wp_rewrite){
    $rules = array(
      'except/category/(.+?)/feed/(feed|rdf|rss|rss2|atom)/?$'  =&gt; 'index.php?except_category_name=$matches[1]&feed=$matches[2]',
      'except/category/(.+?)/(feed|rdf|rss|rss2|atom)/?$'       =&gt; 'index.php?except_category_name=$matches[1]&feed=$matches[2]',
      'except/category/(.+?)/page/?([0-9]{1,})/?$'              =&gt; 'index.php?except_category_name=$matches[1]&paged=$matches[2]',
      'except/category/(.+?)/?$'                                =&gt; 'index.php?except_category_name=$matches[1]',
      );
    $wp_rewrite-&gt;rules = $rules + (array)$wp_rewrite-&gt;rules;
  }

  function pre_get_posts( $query ) {
      if ( ! is_admin() && $query-&gt;is_main_query() && $query-&gt;get( 'except_category_name' ) ) { // check if user asked for a non-admin page and that query contains except_category_name var
          $category = get_category_by_slug( $query-&gt;get( 'except_category_name' ) ); // get the category object
          $query-&gt;set( 'cat', '-' . $category-&gt;term_id ); // set the condition - everything except that category
          $query-&gt;is_home = FALSE; // Tell WordPress we are not at home page
      }
  }
}
$Except = new Except();</pre>

Try now some testing with some real category name `http://wp.example.com/except/category/news/`. 

Oops! It Looks we were ignored

Why?

The trouble is in the `$query->get( 'except_category_name' )` call. The truth is that WordPress obediently transforms your call to `index.php?except_category_name=news` but it does not automatically extract that value from the query string. It extracts only those vars which you are explicitly asked him to extract (I&#8217;ll explain how to do it in a second). And that&#8217;s the most stupid thing in this whole process. It is quite obvious that if I defined a rewrite rule with `some_var=some_value` part then I **want** to read that some_value! Otherwise I wouldn&#8217;t bother defining it would I? And I have no idea why WordPress developers decided it the other way and why they added additional step that just makes things more complicated and a tiny bit slower.

## Step 4: Missing Chain. Query Var

Okay we can further complain about unfairness of the world, but job is job — we need to finish it. And the thing we need to update is the array called [$public\_query\_vars][2]. It&#8217;s time to do it.

<pre class="brush: php; auto-links: false; collapse: true; highlight: [17,48,49,50,51]; light: false; title: ; toolbar: true; notranslate" title="">/*
Plugin Name: Except
Plugin URI: http://ruslanbes.com/projects/except
Description: Make urls like http://wp.example.com/except/category/ABCDE/ display all posts except from category ABCDE
Version: 2013.03.26
Author: Ruslan Bes &lt; me@ruslanbes.com &gt;
Author URI: http://ruslanbes.com/devblog
*/

define('EXCEPT_PATH', dirname(__FILE__) .'/');

class Except {
  function __construct() {
      register_activation_hook( __FILE__, array($this, 'activate') );
      register_deactivation_hook( __FILE__, array($this, 'deactivate') );
      add_action( 'pre_get_posts', array($this, 'pre_get_posts') );
      add_filter( 'query_vars', array($this, 'query_vars') );
  }

  function activate() {
    add_action( 'generate_rewrite_rules', array($this, 'add_rewrite_rules') );
    global $wp_rewrite; $wp_rewrite-&gt;flush_rules(); // force call to generate_rewrite_rules()
  }

  function deactivate() {
    global $wp_rewrite; $wp_rewrite-&gt;flush_rules();
  }

  function add_rewrite_rules($wp_rewrite){
    $rules = array(
      'except/category/(.+?)/feed/(feed|rdf|rss|rss2|atom)/?$'  =&gt; 'index.php?except_category_name=$matches[1]&feed=$matches[2]',
      'except/category/(.+?)/(feed|rdf|rss|rss2|atom)/?$'       =&gt; 'index.php?except_category_name=$matches[1]&feed=$matches[2]',
      'except/category/(.+?)/page/?([0-9]{1,})/?$'              =&gt; 'index.php?except_category_name=$matches[1]&paged=$matches[2]',
      'except/category/(.+?)/?$'                                =&gt; 'index.php?except_category_name=$matches[1]',
      );
    $wp_rewrite-&gt;rules = $rules + (array)$wp_rewrite-&gt;rules;
  }

  function pre_get_posts( $query ) {
      if ( ! is_admin() && $query-&gt;is_main_query() && $query-&gt;get( 'except_category_name' ) ) { // check if user asked for a non-admin page and that query contains except_category_name var
          $category = get_category_by_slug( $query-&gt;get( 'except_category_name' ) ); // get the category object
          $query-&gt;set( 'cat', '-' . $category-&gt;term_id ); // set the condition - everything except that category
          $query-&gt;is_home = FALSE; // Tell WordPress we are not at home page
      }
  }

  function query_vars($public_query_vars){
    array_push($public_query_vars, 'except_category_name');
    return $public_query_vars;
  }

}
$Except = new Except();</pre>

If you test your code now you&#8217;ll see it filters out the category. But that&#8217;s not all.

## Step 5: Not all? You&#8217;ve got to be kidding me

To be honest we have a tiny problem with the very first step when we defined rewrite rules. To illustrate it, think of a following case. You have one module Except\_Category and you have a second module Except\_Tag which does the same thing but for tags. Now imagine you&#8217;re activating them one after another. Here is what will happen to the rewrite rules.  
<img loading="lazy" src="https://i0.wp.com/ruslanbes.com/devblog/wp-content/uploads/2013/03/rewrite-rules.png?resize=463%2C250" alt="rewrite rules" width="463" height="250" class="aligncenter size-full wp-image-417" srcset="https://i2.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2013/03/rewrite-rules.png?w=463&ssl=1 463w, https://i2.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2013/03/rewrite-rules.png?resize=300%2C161&ssl=1 300w" sizes="(max-width: 463px) 100vw, 463px" data-recalc-dims="1" />  
So, rewrite rules from the module which was activated first are not preserved. Why? 

The thing is that when you go to WordPress Dashboard and activate a module Except_Tag, WordPress will call `Except_Tag::activate()` function. But it will do that only for this new module — since all other modules are already activated, there is no reason to call activate hook for them.

So, WordPress will execute only these two lines

<pre class="brush: php; auto-links: false; collapse: false; highlight: [4,5]; title: ; notranslate" title="">class Except_Tag {
  …
  function activate() {
    add_action( ‘generate_rewrite_rules’, array($this, ‘add_rewrite_rules’) ); // Except_Tag rules
    global $wp_rewrite; $wp_rewrite-&gt;flush_rules(); // force call to generate_rewrite_rules()
  }
  …
}</pre>

That means that at the moment of `$wp_rewrite->flush_rules()` the rules from `Except_Category::activate()` will be missed.

So putting new rewrite rules into activation hook is a bad idea.

Let’s think. We need to put them into a function that satisfies these two conditions:  
1) It is called ONLY when the module is active and not just during activation event.  
2) It is called as early as possible. That is, we need to put them them before anyone might call `$wp_rewrite->flush_rules()`

We know that during init stage, WordPress looks which modules are active and calls include_once(&#8216;…/module/module.php&#8217;) for each of them. And we have a nice last line in our main module file:

`$Except = new Except();`

So it seems, that `Except::__construct()` is the perfect place for that. Let&#8217;s try it.

<pre class="brush: php; auto-links: false; collapse: true; highlight: [16,21,26]; light: false; title: ; toolbar: true; notranslate" title="">/*
Plugin Name: Except
Plugin URI: http://ruslanbes.com/projects/except
Description: Make urls like http://wp.example.com/except/category/ABCDE/ display all posts except from category ABCDE
Version: 2013.03.26
Author: Ruslan Bes &lt; me@ruslanbes.com &gt;
Author URI: http://ruslanbes.com/devblog
*/

define('EXCEPT_PATH', dirname(__FILE__) .'/');

class Except {
  function __construct() {
      register_activation_hook( __FILE__, array($this, 'activate') );
      register_deactivation_hook( __FILE__, array($this, 'deactivate') );
      add_action( 'generate_rewrite_rules', array($this, 'add_rewrite_rules') );
      add_action( 'pre_get_posts', array($this, 'pre_get_posts') );
      add_filter( 'query_vars', array($this, 'query_vars') );
  }

  function activate() {
    global $wp_rewrite; $wp_rewrite-&gt;flush_rules(); // force call to generate_rewrite_rules()
  }

  function deactivate() {
    remove_action( 'generate_rewrite_rules', array($this, 'add_rewrite_rules') );
    global $wp_rewrite; $wp_rewrite-&gt;flush_rules();
  }

  function add_rewrite_rules($wp_rewrite){
    $rules = array(
      'except/category/(.+?)/feed/(feed|rdf|rss|rss2|atom)/?$'  =&gt; 'index.php?except_category_name=$matches[1]&feed=$matches[2]',
      'except/category/(.+?)/(feed|rdf|rss|rss2|atom)/?$'       =&gt; 'index.php?except_category_name=$matches[1]&feed=$matches[2]',
      'except/category/(.+?)/page/?([0-9]{1,})/?$'              =&gt; 'index.php?except_category_name=$matches[1]&paged=$matches[2]',
      'except/category/(.+?)/?$'                                =&gt; 'index.php?except_category_name=$matches[1]',
      );
    $wp_rewrite-&gt;rules = $rules + (array)$wp_rewrite-&gt;rules;
  }

  function pre_get_posts( $query ) {
      if ( ! is_admin() && $query-&gt;is_main_query() && $query-&gt;get( 'except_category_name' ) ) { // check if user asked for a non-admin page and that query contains except_category_name var
          $category = get_category_by_slug( $query-&gt;get( 'except_category_name' ) ); // get the category object
          $query-&gt;set( 'cat', '-' . $category-&gt;term_id ); // set the condition - everything except that category
          $query-&gt;is_home = FALSE; // Tell WordPress we are not at home page
      }
  }

  function query_vars($public_query_vars){
    array_push($public_query_vars, 'except_category_name');
    return $public_query_vars;
  }

}
$Except = new Except();</pre>

Now everything is fine. As long as the module is enabled additional rewrite rules will be there. But the moment it is being deactivated the rules are instantly removed

That&#8217;s it. If you need the module you can get it from [here][1] .

 [1]: http://ruslanbes.com/projects/except/except.zip
 [2]: http://codex.WordPress.org/WordPress_Query_Vars