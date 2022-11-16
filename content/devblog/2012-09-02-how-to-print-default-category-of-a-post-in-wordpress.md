---
title: How to print default category of a post in WordPress
author: Ruslan Bes
type: post
date: 2012-09-02T16:35:06+00:00
url: /2012/09/02/how-to-print-default-category-of-a-post-in-wordpress/
dsq_thread_id:
  - 5123271805
categories:
  - WordPress
tags:
  - Hikari Category Permalink

---
Sometimes you need to print one category that characterizes the post. Imagine you have a &#8220;Most popular&#8221; widget and you wish to print the category link near each post title. How would you do that? Here is a short recipe. It applies to Wordress 3.4.1 but I assume it could be applied to a wide variety of versions.

  1. Install the [Hikari Category Permalink][1] plugin. It adds the &#8220;Permalink&#8221; button near each category when you edit a post. Now you can define manually which category to treat as &#8220;main&#8221;
  2. Write a small function anywhere you wish. I usually create a plugin named after a site name that  (example.com) and put all my helpers there. The function is like that: <pre class="brush: php; title: ; notranslate" title="">function example_get_main_category($post_id) {
    // extract the category from hikari permalink plugin
    $category_id = get_post_meta($post_id, '_category_permalink', true);
    if ( $category_id ) {
      return get_term($category_id, 'category');
    }

    // no main category defined, get the default one
    $terms = wp_get_object_terms($post_id, 'category');
    if ( $terms ) {
      return $terms[0];
    }

}</pre>
    
    **Note:** By default WordPress would return the category with the lowest [term\_taxonomy\_id][2].</li> 
    
      * Now you can output taxonomy link like this: <pre class="brush: php; title: ; notranslate" title="">$category = example_get_main_category($r-&gt;ID);
            $render_category = sprintf('&lt;a href="%s" class="category-url"&gt;%s&lt;/a&gt;', get_term_link($category), $category-&gt;name);
</pre></ol> 
    
    That&#8217;s all.

 [1]: http://wordpress.org/extend/plugins/hikari-category-permalink/
 [2]: http://codex.wordpress.org/WordPress_Taxonomy "Taxonomy schema"