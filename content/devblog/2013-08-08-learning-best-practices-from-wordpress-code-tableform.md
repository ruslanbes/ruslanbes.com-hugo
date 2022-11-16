---
title: Learning best practices from WordPress code – Tableform
author: Ruslan Bes
type: post
date: 2013-08-08T16:56:40+00:00
url: /2013/08/08/learning-best-practices-from-wordpress-code-tableform/
dsq_thread_id:
  - 4864335431
categories:
  - PHP
  - WordPress
tags:
  - Best practice
  - Table

---
Learning new frameworks is always a good idea even if you&#8217;re not going to use them in your job. For instance I don&#8217;t like WordPress, but there are things inside worth mentioning. One of them is how they output tables.

So, imagine you traveled to year 2100, where refrigerators finally managed to detect food an got their [sites in the internet][1]. You are assigned a task to write a front-end for refrigerator website. Luckily you have all the data about the products stored in the MySQL v21.1 database table.

The table is called **products** and has following fields

  * id, INT
  * name, VARCHAR
  * category, VARCHAR
  * created, DATETIME
  * last_accessed, DATETIME
  * expiry_date, DATETIME
  * screenshot, VARCHAR # URL to the image file on server
  * temperature, DECIMAL
  * etc. (add more fields in comments)

For simplicity let&#8217;s output the data only from this table, not from other referenced tables (categories, tags, refrigerator_sections etc.).

Okay, so that&#8217;s the task. You have `$records` array and `$columns` list and you need to fill a function:

<pre class="brush: php; title: ; notranslate" title="">function display_rows() {
  // Get the records
  $records = $this-&gt;items;

  // Get the registered columns
  $columns = $this-&gt;get_columns();

  // ???
}</pre>

How would you do it? Probably the first thing that you figure out is

<pre class="brush: php; title: ; notranslate" title="">function display_rows() {
  // Get the records
  $records = $this-&gt;items;

  // Get the registered columns
  $columns = $this-&gt;get_columns();

  foreach ($records as $record) {
    extract($record);
    echo "&lt;tr&gt;";
    echo "&lt;td&gt;" . $id . "&lt;/td&gt;";
    echo "&lt;td&gt;" . htmlspecialchars($name) . "&lt;/td&gt;";
    echo "&lt;td&gt;" . htmlspecialchars($category) . "&lt;/td&gt;";
    echo "&lt;td&gt;" . $created . "&lt;/td&gt;";
    echo "&lt;td&gt;" . $last_accessed . "&lt;/td&gt;";
    echo "&lt;td&gt;" . $expiry_date . "&lt;/td&gt;";
    echo "&lt;td&gt;" . '&lt;img src="' . $screenshot . '"/&gt;' . "&lt;/td&gt;";
    echo "&lt;td&gt;" . $temperature . " °C &lt;/td&gt;";
    echo "&lt;/tr&gt;";
  }
}</pre>

Fine. Now a customer from LG is calling you. &#8220;Hey, our temperature sensors are a bit buggy&#8221;, he says, &#8220;can you please remove that field for our fridges?&#8221;. Then a customer from Samsung shows up: &#8220;Hey, we did tested your site on our very old phone, Samsung Galaxy S4, and the site is too wide for it. Can you please show for the mobile version only two fields, expiry date and a name, in that order?&#8221;. Finally your boss comes to you and says that we need to give user an ability to hide any column and rearrange them in any order, otherwise we&#8217;ll get sued for public discrimination of varchars and datetimes.

The solution for this problem implemented in WordPress is very simple and elegant. You just have to ensure that `$columns` contains only visible columns in the correct order and then use **switch** statement. Here you go.

<pre class="brush: php; title: ; notranslate" title="">function display_rows() {
  // Get the records
  $records = $this-&gt;items;

  // Get the registered columns
  $columns = $this-&gt;get_visible_columns();


  foreach($records as $record) {
    echo "&lt;tr&gt;";
    foreach ($columns as $column) {
      switch ($column) {
        case 'id':
        case 'created':
        case 'last_accessed':
        case 'expiry_date':
          echo "&lt;td&gt;" . $record[$column] . "&lt;/td&gt;";
          break;
        case 'temperature':
          echo "&lt;td&gt;" . $record[$column] . " °C &lt;/td&gt;";
          break;
        case 'name':
        case 'category':
          echo "&lt;td&gt;" . htmlspecialchars($record[$column]) . "&lt;/td&gt;";        
          break;
        case 'screenshot':
          echo "&lt;td&gt;" . '&lt;img src="' . $record[$column] . '"/&gt;' . "&lt;/td&gt;";
          break;
      }
    }
    echo "&lt;/tr&gt;";
  }
}
</pre>

 [1]: http://en.wikipedia.org/wiki/Internet_refrigerator