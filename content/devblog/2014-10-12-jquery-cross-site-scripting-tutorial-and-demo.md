---
title: Cross-Domain AJAX tutorial and demo
author: Ruslan Bes
type: post
draft: true
date: 2014-10-12T11:28:20+00:00
url: /2014/10/12/jquery-cross-site-scripting-tutorial-and-demo/
dsq_thread_id:
  - 4832620854
classic-editor-remember:
  - block-editor
categories:
  - PHP
tags:
  - CORS
  - cross-domain ajax
  - cross-site scripting
  - iframe
  - JSONP
  - postMessage
  - same-origin policy

---
After I learned about [4 types of implementing cross-domain scripting][1] I thought that it would be nice to create a working demo. After all, I believe in &#8220;Learn By Example&#8221;. Besides since that article was published some new methods arrived, so why not write yet another tutorial.  
<!--more-->

Quick recap: you have two HTML pages located on the two different sites: `http://www.a.com/page_a.html` and `http://www.b.com/page_b.html` and you want `page_a.html` to get some information from `page_b.html`. For instance `page_b` knows the first name of the person, who is browsing both `page_a` and `page_b`, and `page_a` wants to use this information to say &#8220;Hi %firstname%&#8221;.

If they were located on the same domain, `http://<strong>www.common.com</strong>/page_a.html` and `http://<strong>www.common.com</strong>/page_b.html` it would be no problem. However, since they are on different domains, they cannot talk to each other, as that would violate [same-origin policy][2], an important principle, that prevents some Russian script-kiddie from stealing your PayPal password in order to sell it on some South-african black market to some Chinese student who would use it to buy some marijuana from some Brazilian dealer.

The `www.a.com` and `www.b.com` can agree on some rules to make cross-site scripting between them possible and there are several main ways to do that:

  1. [CORS (Cross-Origin Resourse Sharing)][3]
  2. [JSONP][4]
  3. [window.postMessage][5]
  4. [Local proxy][6]
  5. [MessageChannel][7]
  6. [Fragment Identifier Messaging][8]

_Before you continue I advise you to look at the [easyXDM library][9] which is absolutely recommended for using in real projects instead of building your own solution. This article is for those who want to understand the inner working of such libraries._

## 1. CORS (Cross-Origin Resource Sharing) {#cors}

Let&#8217;s begin with three pair of people trying to talk to each other. [Alice][10] and [Bob][11], [Cindy][12] and [Douglas][13], [Eve][14] and [Fred][15]. Bob is not at home right now and is visiting Alice, so they are on the same domain. All other folks are on separate domains. First, the girls try to use ajax requests to get the messages from boys:





All of them used the same [$.get][16] method. Alice received her message successfully because Bob was on the same domain. Cindy failed to receive a message from Douglas who is on different domain. Eve had the same problem with Fred, but luckily Fred sends the HTTP header that allows Eve to use the data from Fred on her domain.

<pre class="brush: php; title: ; notranslate" title="">&lt;?php
  header('Access-Control-Allow-Origin: https://eve.ruslanbes.com');
?&gt;</pre>

If you have Firebug or similar developer console, open it, and you will see something like that:  
<img loading="lazy" class="aligncenter size-full wp-image-573" src="https://i2.wp.com/ruslanbes.com/devblog/rbes-content/uploads/2014/10/firebug.jpg?resize=705%2C116" alt="firebug" width="705" height="116" srcset="https://i2.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2014/10/firebug.jpg?w=773&ssl=1 773w, https://i2.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2014/10/firebug.jpg?resize=300%2C49&ssl=1 300w" sizes="(max-width: 705px) 100vw, 705px" data-recalc-dims="1" />  
which tells you exactly what is wrong.

Notice that Fred only allows to use his message on Eve&#8217;s domain. If Cindy tries to steal Fred&#8217;s message, she would fail:  


In extreme case Fred can set the header to:

<pre class="brush: php; title: ; notranslate" title="">&lt;?php
  header('Access-Control-Allow-Origin: *');
?&gt;</pre>

meaning, that everyone can get the message.

**Side-note**: Dimitar Ivanov mentioned that ajax request to Fred does not use cookies by default. If Eve needs to allow Fred to use cookies when responding to her request, she has to use XHR with `withCredentials` attribute and Fred must set `"Access-Control-Allow-Credentials: true"` header. The cookies we are talking about are the ones on fred.ruslanbes.com and Eve cannot access them, only Fred can. [Code sample][17], [Demo from Mozilla documentation][18]. 

At the end of the first chapter we left Cindy unhappy, let&#8217;s look if she can find another way to communicate with Douglas.

## 2. JSONP (JSON Padding) {#jsonp}

JSON Padding is an interesting technique that implements cross-site AJAX using a browser hack. It tries to exploit the fact that the browser can load JavaScript using the <script> tag from any domain. Sadly, unlike the usual AJAX you can load only JavaScript files in this way. Besides, when you include external script you don&#8217;t have any way to tell at which moment it was fully loaded, that is, you don&#8217;t have [.done()][19] function. So if the Eve has a tag like this:

<pre class="brush: jscript; title: ; notranslate" title="">&lt;!-- www.eve.com/getmessage.html --&gt;
&lt;script type="text/javascript" src="www.fred.com/json.js"&gt;&lt;/script&gt;</pre>

and the `www.fred.com/json.js` contains a JSON:

<pre class="brush: jscript; title: ; notranslate" title="">// www.fred.com/json.js
{
"name":"Fred",
"message":"Hi"
}</pre>

that would be the same as if Eve had this right from the start:

<pre class="brush: jscript; title: ; notranslate" title="">&lt;!-- www.eve.com/getmessage.html --&gt;
&lt;script type="text/javascript"&gt;
{
"name":"Fred",
"message":"Hi"
}
&lt;/script&gt;</pre>

an anonymous object that she can&#8217;t access.  
The technique is that instead of JSON Fred sends an instruction to call a JavaScript function and passes the JSON as a parameter:

<pre class="brush: jscript; title: ; notranslate" title="">&lt;!-- www.eve.com/getmessage.html --&gt;
&lt;script type="text/javascript"&gt;
function logMessage(fred) {
  console.log(fred.message)
}
&lt;/script&gt;
&lt;script type="text/javascript" src="www.fred.com/jsonp.js"&gt;&lt;/script&gt;</pre>

<pre class="brush: jscript; title: ; notranslate" title="">// www.fred.com/jsonp.js
logMessage({
"name":"Fred",
"message":"Hi"
})</pre>

Again we need Fred and Eve to play together. Let&#8217;s check if other folks can make it. This time girls will use [$.getJSON][20] function.



In order to use JSONP Fred accepts the &#8220;callback&#8221; parameter from incoming URL. Eve knows that and adds `"callback=?"` to the URL string. This magical `XXXXX=?` part tells jQuery to use JSONP mode and it will substitute `?` with some not-used function name like `jQuery211049818158980495376_1412848463345`.

If you look in the Firebug again you&#8217;ll notice something strange:  
<img loading="lazy" src="https://i0.wp.com/ruslanbes.com/devblog/rbes-content/uploads/2014/10/firebug2.jpg?resize=705%2C130" alt="firebug2" width="705" height="130" class="aligncenter size-full wp-image-583" srcset="https://i2.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2014/10/firebug2.jpg?w=778&ssl=1 778w, https://i2.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2014/10/firebug2.jpg?resize=300%2C55&ssl=1 300w" sizes="(max-width: 705px) 100vw, 705px" data-recalc-dims="1" /> 

Where is the Eve&#8217;s request?

Here it is, on the NET tab.  
<img loading="lazy" src="https://i1.wp.com/ruslanbes.com/devblog/rbes-content/uploads/2014/10/firebug3-net.jpg?resize=705%2C141" alt="firebug3-net" width="705" height="141" class="aligncenter size-full wp-image-585" srcset="https://i2.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2014/10/firebug3-net.jpg?w=759&ssl=1 759w, https://i2.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2014/10/firebug3-net.jpg?resize=300%2C60&ssl=1 300w" sizes="(max-width: 705px) 100vw, 705px" data-recalc-dims="1" /> 

You can check the name of generated callback there too.

Again, no luck for Cindy. This time Cindy can learn the technique from Eve and get the Eve&#8217;s message from Fred. Nothing can prevent that. In the next chapter we will use iframes for communication and check if Cindy can hack into them.

## 3. window.postMessage {#postMessage}

This time parties will communicate through an iframe. Girls will host the initial pages and include boys using `<iframe src="www.boy.com/page.html"/>` tags.

Again, for Alice and Bob there are no problems. But how to establish a connection between frames from different domains? Unfortunately `"Access-Control-Allow-Origin"` has no effect here (although I don&#8217;t see the reason why not) and the frame contents are not JSON, so both CORS and JSONP are useless here:  


<div class='easySpoilerWrapper' style=''>
  <table class='easySpoilerTable' border='0' style='text-align:center;' align='center' bgcolor='FFFFFF' >
    <tr style='white-space:normal;'>
      <th class='easySpoilerTitleA' style='white-space:normal;font-weight:normal;text-align:left;vertical-align:middle;font-size:120%;color:#000000;'>
        Spoiler Inside: Useless
      </th>
      
      <th class='easySpoilerTitleB'style='text-align:right;vertical-align:middle;font-size:100%; white-space:nowrap;'>
        <a href='' onclick='wpSpoilerSelect("spoilerDiv65fb8001"); return false;' class='easySpoilerButtonOther' style='font-size:100%;color:#000000;background-color:#fcfcfc;background-image:none;border: 1px inset;border-style:solid;border-color:#cccccc; margin: 3px 0px 3px; padding: 4px; ' align='right'>Select</a><a href='' onclick='wpSpoilerToggle("spoilerDiv65fb8001",true,"Show","Hide","fast",false); return false;' id='spoilerDiv65fb8001_action' class='easySpoilerButton' value="Show" align='right' style='font-size:100%;color:#000000;background-color:#fcfcfc;background-image:none;border: 1px inset;border-style:solid;border-color:#cccccc; margin: 3px 0px 3px 5px; padding: 4px;'>Show</></th> </tr> 
        
        <tr>
          <td class='easySpoilerRow' colspan='2' style=''>
            <div id='spoilerDiv65fb8001' class='easySpoilerSpoils'  style='display:none; white-space:wrap; overflow:auto; vertical-align:middle;'>
              <img class="aligncenter" src="https://i1.wp.com/1.bp.blogspot.com/-Tg3znfdgfDc/UY28UxqktDI/AAAAAAAAAMY/2SjyKBS-2zk/s1600/Vogon.jpg?w=705&#038;ssl=1" alt="Useless" data-recalc-dims="1" />
            </div>
          </td>
        </tr></table> 
        
        <div class='easySpoilerConclude' style=''>
          <table class='easySpoilerTable' border='0' style='text-align:center;' frame='box' align='center' bgcolor='FFFFFF'>
            <tr>
              <th class='easySpoilerEnd' style='width:100%;'>
              </th>
              
              <td class='easySpoilerEnd' style='white-space:nowrap;' colspan='2'>
              </td>
            </tr>
            
            <tr>
              <td class='easySpoilerGroupWrapperLastRow' colspan='2' style=''>
              </td>
            </tr>
          </table>
        </div></div> </p> 
        
        <p>
          Luckily we are living in year 2014 and <a href="https://developer.mozilla.org/en-US/docs/Web/API/window.postMessage" title="window.postMessage">window.postMessage</a> is already invented. And this is exactly the right tool for the job. The idea is that sender frame calls the postMessage method of the receiver:
        </p>
        
        <pre class="brush: jscript; title: ; notranslate" title="">
receiver_iframe.postMessage({"name":"Sender", "message":"Hi"}, 'http://www.receiver.com');
// not the correct code
</pre>
        
        <p>
          and the receiver should register a handler for messages:
        </p>
        
        <pre class="brush: jscript; title: ; notranslate" title="">
$( window ).on("message", function( event ) {
  if (event.origin !== "http://www.sender.com") return; // not the correct code
  console.log(event.data.name + " says: " + event.data.message)
})
</pre>
        
        <p>
          This is in theory. In practice we will need to add some more code, but let&#8217;s look at the demo:
        </p>
        
        <p>
        </p>
        
        <p>
          Okay. Alice can simply query <code>$("#bobs_iframe").contents()</code> and get any information she needs. Cindy does that too, but in cross-domain mode the <code>contents()</code> will be empty. The most interesting part is with Eve.
        </p>
        
        <p>
          Eve uses following line:
        </p>
        
        <pre class="brush: jscript; title: ; notranslate" title="">
$("#freds_iframe").get(0).contentWindow.postMessage("How about a lunch?", 'https://fred.ruslanbes.com')
</pre>
        
        <p>
          The <code>get(0)</code> is needed to get the iframe object. Using the <code>.contentWindow</code> property we get limited access to the iframe&#8217;s <code>window</code> object and execute <code>postMessage()</code> call.
        </p>
        
        <p>
          Fred is ready to receive it:
        </p>
        
        <pre class="brush: jscript; title: ; notranslate" title="">
$( window ).on("message", function( event ){
  event = event.originalEvent // ???
  if (event.origin !== "https://eve.ruslanbes.com") return;
  parent.postMessage( $("#message_to_eve").text(), "https://eve.ruslanbes.com" );
})
</pre>
        
        <p>
          okay, what is line 2 doing here?
        </p>
        
        <p>
          The thing is that jQuery has internal <a href="https://api.jquery.com/category/events/event-object/" title="jQury Event">event handler</a> that is executed before a JavaScript event is passed to a target. It does some browser-fixing stuff and passes event mostly with its original properties, but at this moment (jQuery 2.1.1) it does not properly handle the <code>"message"</code> event. We use <code>event.originalEvent</code> to extract it.
        </p>
        
        <p>
          The rest is as expected, Fred sends the message back to Eve using Eve&#8217;s window (<code>parent</code>). Eve receives it.
        </p>
        
        <pre class="brush: jscript; title: ; notranslate" title=""> // Eve
$( window ).on("message", function( event ){
  event = event.originalEvent
  if (event.origin !== "https://fred.ruslanbes.com") return;
  $("#freds_message").val(event.data)
})
</pre>
        
        <p>
          Some notes about this mechanism. First the message itself is better to be a string or number, but not an Object. The problem with Object is that it is <a href="http://caniuse.com/#search=postMessage" title="Can I use postMessage">not supported up to IE 9</a>. If you still need it, use a workaround:
        </p>
        
        <pre class="brush: jscript; title: ; notranslate" title="">
// sender.com 
// ...
postMessage( JSON.stringify( {"name":"Sender", "message":"Hi"} ), "http://receiver.com" )
// ...

// receiver.com
// ...
var data = jQuery.parseJSON( event.data )
// ...
</pre>
        
        <p>
          Second is that you have to include the target URL with every request and check it manually, which can matter if you have hundreds of messages going simultaneously.
        </p>
        
        <p>
          If you open Firebug and click the Eve&#8217;s button you can see additional debug messages:<br /> <img loading="lazy" src="https://i1.wp.com/ruslanbes.com/devblog/rbes-content/uploads/2014/10/evemessage.jpg?resize=705%2C176" alt="evemessage" width="705" height="176" class="aligncenter size-full wp-image-601" srcset="https://i2.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2014/10/evemessage.jpg?w=779&ssl=1 779w, https://i2.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2014/10/evemessage.jpg?resize=300%2C74&ssl=1 300w" sizes="(max-width: 705px) 100vw, 705px" data-recalc-dims="1" />
        </p>
        
        <p>
          One thing is worth mentioning. When Eve <em>sends</em> a message she uses <em>Fred&#8217;s window</em>: <code>$("#freds_iframe").get(0).contentWindow.postMessage( ... )</code>, but when she wants to <em>receive</em> the message, she registers handler on <em>her</em> window: <code>$( window ).on("message", ... )</code>.
        </p>
        
        <p>
          Same thing with Fred. Fred sends the message to Eve&#8217;s window: <code>parent.postMessage</code>, but handles the message on his frame.
        </p>
        
        <p>
          But what if we changed the composition of the frames and Eve is now not <code>parent</code> but <code>parent.parent</code> or even <code>parent.parent.parent.$("#eves_iframe")</code>. Obvious question: can we put all the handling/receiving logic into one frame? For example the <code>top</code> since &#8220;top&#8221; is always available. Let&#8217;s try it.
        </p>
        
        <p>
          Eve&#8217;s code is changed to:
        </p>
        
        <pre class="brush: jscript; title: ; notranslate" title="">// Eve
top.postMessage("How about a lunch?", 'https://fred.ruslanbes.com')
// and
$( top ).on("message", function( event ){
  event = event.originalEvent
  if (event.origin !== "https://fred.ruslanbes.com") return;
  $("#freds_message").val(event.data)
})
</pre>
        
        <p>
          Fred&#8217;s:
        </p>
        
        <pre class="brush: jscript; title: ; notranslate" title="">// Fred
$( top ).on("message", function( event ){
  event = event.originalEvent
  if (event.origin !== "https://eve.ruslanbes.com") return;
  $( top ).postMessage( $("#message_to_eve").text(), "https://eve.ruslanbes.com" );
})
</pre>
        
        <p>
          Let&#8217;s check it:<br />
        </p>
        
        <p>
          Nope, it doesn&#8217;t work and if you open Firebug you&#8217;ll see a strange error:<br /> <img loading="lazy" src="https://i2.wp.com/ruslanbes.com/devblog/rbes-content/uploads/2014/10/jqueryerror.jpg?resize=705%2C101" alt="jqueryerror" width="705" height="101" class="aligncenter size-full wp-image-603" srcset="https://i2.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2014/10/jqueryerror.jpg?w=783&ssl=1 783w, https://i2.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2014/10/jqueryerror.jpg?resize=300%2C42&ssl=1 300w" sizes="(max-width: 705px) 100vw, 705px" data-recalc-dims="1" />
        </p>
        
        <p>
          This message is raised by Eve&#8217;s and Fred&#8217;s attempts to register onMessage event on the &#8220;top&#8221; frame which belongs to neither&#8217;s domain. This is a nasty limitation, but we have to live with it for now.
        </p>
        
        <p>
          Nice, we now know 3 methods and have angry Cindy, who thinks nobody wants to talk to her. Time to fix it.
        </p>
        
        <h2 id="proxy">
          4. Local proxy
        </h2>
        
        <p>
          This by far the easiest, most powerful and, if implemented improperly, most insecure way to circumvent cross-domain policy. First of all it does not require to change anything in the second party&#8217;s code. The first party sets a simple proxy like this:
        </p>
        
        <pre class="brush: php; title: ; notranslate" title="">
&lt;?php
$url = $_GET['url'];
$parsed = parse_url($url);
if ($parsed['host'] !== 'douglas.ruslanbes.com') {
  header('HTTP/1.0 403 Forbidden');
  die("Forbidden");
}

$ch = curl_init();

curl_setopt($ch, CURLOPT_URL, $url);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
$output = curl_exec($ch);
curl_close($ch);
echo $output;
</pre>
        
        <p>
          This proxy looks at the parameter &#8220;url&#8221;, checks if it belongs to Douglas and sends the request by itself. So Cindy can now send requests like: <code>"https://cindy.ruslanbes.com/proxy/proxytodouglas.php?url="+ encodeURI("https://douglas.ruslanbes.com/cors/douglas.php")</code>.
        </p>
        
        <p>
          Note of caution: <strong>Always</strong> check the url your proxy is working with. Second note: if you check it with RegExps, always do a proof-test with requests like this <code>"https://cindy.ruslanbes.com/proxy/proxytodouglas.php?url="+ encodeURI("http://www.attackthissite.com/?param=https://douglas.ruslanbes.com")</code>
        </p>
        
        <p>
          Okay, testing.
        </p>
        
        <p>
        </p>
        
        <p>
          Works fine. Cindy finally managed to get the message.
        </p>
        
        <h2 id="MessageChannel">
          5. MessageChannel
        </h2>
        
        <p>
          The <a href="http://msdn.microsoft.com/en-us/library/windows/apps/hh441303.aspx">Channel Messaging</a> is a new technique that is meant to fix some problems with postMessage. Unfortunately it is completely unsupported by Internet Explorer up to IE9 and Firefox up to FF35.
        </p>
        
        <p>
          Let&#8217;s rewrite the Cindy&#8217;s and Douglas&#8217;s iframe code to use message channels:
        </p>
        
        <pre class="brush: jscript; title: ; notranslate" title=""> // Cindy
$("#get_douglass_message").click(function() {
  if (window.MessageChannel) { // check the support of MessageChannel
    if (typeof window.myMessageChannel == 'undefined') { // when you click the button first time
      console.log("Cindy: Establishing channel with Douglas") 
      window.myMessageChannel = new MessageChannel(); // Create MessageChannel object
      window.myMessageChannel.port1.onmessage = function( event ) { // attach listener for messages
        console.log("Cindy: I got message from Douglas throught the port")
        $("#douglass_message").val(event.data) 
      }
      var w = $("#douglass_iframe").get(0).contentWindow
      w.postMessage("Hi Douglas", 'https://douglas.ruslanbes.com', [window.myMessageChannel.port2]) // send Douglas the second port of the channel
      $( '#get_douglass_message' ).text( "Say 'Hi again' to Douglas" )
    } else {
      console.log("Cindy: Sending message through the port")
      window.myMessageChannel.port1.postMessage("Hi again")
    }
    
  } else {
    $("#douglass_message").val("&lt;Failed: MessageChannel unsupported&gt;")
  }
})
</pre>
        
        <p>
          Lots of code here, let&#8217;s check what&#8217;s going on. When you click the button first time, lines 4-13 are used. We create MessageChannel object and save it as <code>window.MessageChannel</code>. At line 12 we use the same postMessage function but with third parameter <em>transfer</em>. The <em>transfer</em> parameter is an array of <a href="https://developer.mozilla.org/en-US/docs/Web/API/Transferable">Transferable</a> objects. When we send them this way, the sender (Cindy) loses control over them, and the receiver (Douglas) gains it. Cindy sends <code>window.myMessageChannel.port2</code> &#8211; that means Cindy cannot do something like <code>window.myMessageChannel.port2.onmessage = function() {alert("hi")}</code> anymore.
        </p>
        
        <p>
          After the connection is established and button is clicked again, Cindy uses lines 15-16. Note that she uses the same <code>postMessage</code> but this time she sends only the message and nothing else.
        </p>
        
        <p>
          Let&#8217;s look at the Douglas&#8217;s code:
        </p>
        
        <pre class="brush: jscript; title: ; notranslate" title=""> // Douglas
$( window ).on("message", function( event ){
  console.log("Douglas: I got message from someone")
  event = event.originalEvent
  if (event.origin !== "https://cindy.ruslanbes.com") return;
  console.log("Douglas: It's from Cindy, she says: " + event.data)
  console.log("Douglas: Attaching message handler to port from Cindy")
  var portToCindy = event.ports[0];
  portToCindy.onmessage = function( event ) {
        console.log("Douglas: I got message from Cindy through the port")
        this.postMessage("Hi again, Cindy")        
  }
  portToCindy.postMessage("Hi, Cindy. Channel established")
})
</pre>
        
        <p>
          So, Douglas get the <code>port2</code> from the initial postMessage: <code>var portToCindy = event.ports[0]</code>, then attaches the onmessage handler and sends the message using <code>portToCindy.postMessage</code>. Let&#8217;s check. Remember, you should <strong>not</strong> use Firefox < 35 or IE < 10:
        </p>
        
        <p>
        </p>
        
        <p>
          Works. If you look at the console, you&#8217;ll see a following dialog:<br /> <img loading="lazy" src="https://i0.wp.com/ruslanbes.com/devblog/rbes-content/uploads/2014/10/messagelog.png?resize=700%2C193" alt="messagelog" width="700" height="193" class="aligncenter size-full wp-image-610" srcset="https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2014/10/messagelog.png?w=700&ssl=1 700w, https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2014/10/messagelog.png?resize=300%2C82&ssl=1 300w" sizes="(max-width: 700px) 100vw, 700px" data-recalc-dims="1" />
        </p>
        
        <h2 id="fragment">
          6. Fragment Identifier Messaging
        </h2>
        
        <p>
          This method was probably the first one used to fight the SOP for the iframes case. Recall that &#8220;sender&#8221; window have a limited access to the &#8220;receiver&#8221; window and can even change some things inside. One of the things it <strong>can</strong> actually change is the <code>window.location</code> attribute, that is, URL of a window. So the trick is to paste the message into this attribute and let the window/iframe read it.
        </p>
        
        <p>
          Now if you change the location to a completely different page the window will reload. So how can you change it so that the window is not reloaded?
        </p>
        
        <p>
          Easy. Simply change what goes after &#8220;#&#8221;(hash) sign.
        </p>
        
        <p>
          Unfortunately since we are using the thing that was not supposed for messaging, we should be ready for some limitations. The most important is that all browsers have some limit for what they think you can put as a URL. Most of the browsers has this limit at about 100 000, but IE has a limit at <a href="http://www.boutell.com/newfaq/misc/urllength.html">2083 chars</a> which is kinda frustrating. Let&#8217;s try to build the demo:
        </p>
        
        <p>
        </p>
        
        <p>
          Well, it probably does not work. In Firefox it raises <a href="https://bugzilla.mozilla.org/show_bug.cgi?id=442738">NS_ERROR_DOM_PROP_ACCESS_DENIED</a> error because Firefox thinks that having iframe inside an iframe and playing with their location is way too much for a normal script. Other browsers have problems too. It should work however if you open the Cindy&#8217;s home directly: <a href="https://cindy.ruslanbes.com/fragment/cindy.php">https://cindy.ruslanbes.com/fragment/cindy.php</a>.
        </p>
        
        <p>
          Let&#8217;s look at the code of Douglas (the code of Cindy is very similar):
        </p>
        
        <pre class="brush: jscript; title: ; notranslate" title=""> // Douglas
window.addEventListener("hashchange", getMessageFromCindy, false) // track the hash change event
function getMessageFromCindy() {
  var hash = window.location.hash.replace(/^#/,'') // get what goes after the has sign
  console.log("Cindy says:" + hash) 
  parent.location = "https://cindy.ruslanbes.com/fragment/cindy.php" + "#" + $("#message_to_cindy").text() // send a message back using hash
}
</pre>
        
        <p>
          Some notes about this solution.
        </p>
        
        <p>
          Note 1: We cannot check if the parent message comes from Cindy. The <code>parent.location</code> is write-only property, so we can only hope it is Cindy and not Boris or someone like him :)
        </p>
        
        <p>
          Note 2: We have to know the initial parent window address. If it&#8217;s not <code>https://cindy.ruslanbes.com/fragment/cindy.php</code> then the whole communication will not work. One way to workaround this is to pass the parent address into the hash along with the message json-encoded, like <code>{"callback":"https://cindy.ruslanbes.com/fragment/cindy.php", "message":"Hi, Douglas"}</code>.
        </p>
        
        <h2 id="summary">
          Summary
        </h2>
        
        <p>
          Small cheat-sheet listing all methods:
        </p>
        
        <table id="cheatsheet">
          <tr>
            <th>
              Method
            </th>
            
            <th>
              Advantages
            </th>
            
            <th>
              Disadvantages
            </th>
            
            <th>
              Support
            </th>
          </tr>
          
          <tr>
            <td>
              CORS
            </td>
            
            <td class="adv">
              <ul>
                <li>
                  W3C recommendation
                </li>
              </ul>
            </td>
            
            <td class="disadv">
              <ul>
                <li>
                  Does not work in iframes
                </li>
              </ul>
            </td>
            
            <td class="browser-support">
              All modern browsers, except Opera Mini.
            </td>
          </tr>
          
          <tr>
            <td>
              JSONP
            </td>
            
            <td class="adv">
              <ul>
                <li>
                  Works universally across browsers
                </li>
                <li>
                  Supported in major JavaScript libraries
                </li>
              </ul>
            </td>
            
            <td class="disadv">
              <ul>
                <li>
                  Works only with JSON data
                </li>
              </ul>
            </td>
            
            <td class="browser-support">
              All browsers which support JavaScript.
            </td>
          </tr>
          
          <tr>
            <td>
              window.postMessage
            </td>
            
            <td class="adv">
              <ul>
                <li>
                  Pure JavaScript. You don&#8217;t have to be administrator on the server
                </li>
              </ul>
            </td>
            
            <td class="disadv">
              <ul>
                <li>
                  IE 8 and 9 allows only scalar data, but not objects
                </li>
                <li>
                  Overhead with constant checking of origins
                </li>
              </ul>
            </td>
            
            <td class="browser-support">
              All modern browsers. IE 8 and 9 — partially
            </td>
          </tr>
          
          <tr>
            <td>
              Local proxy
            </td>
            
            <td class="adv">
              <ul>
                <li>
                  You need to have access to the first party only
                </li>
                <li>
                  Browser-independent
                </li>
              </ul>
            </td>
            
            <td class="disadv">
              <ul>
                <li>
                  Usually less performant
                </li>
                <li>
                  Danger of misusing
                </li>
              </ul>
            </td>
            
            <td class="browser-support">
              All browsers.
            </td>
          </tr>
          
          <tr>
            <td>
              MessageChannel
            </td>
            
            <td class="adv">
              <ul>
                <li>
                  Pure JavaScript. You don&#8217;t have to be administrator on the server
                </li>
                <li>
                  Fast. You need to establish the channel only once
                </li>
              </ul>
            </td>
            
            <td class="disadv">
              <ul>
                <li>
                  Not supported by older versions of Firefox
                </li>
              </ul>
            </td>
            
            <td class="browser-support">
              Most browsers except: IE <= 9, Firefox <= 35, Opera mini, Android browser <= 4.3
            </td>
          </tr>
          
          <tr>
            <td>
              Fragment Identifier Messaging
            </td>
            
            <td class="adv">
              <ul>
                <li>
                  Almost universal support
                </li>
              </ul>
            </td>
            
            <td class="disadv">
              <ul>
                <li>
                  Only string messages
                </li>
                <li>
                  The maximum message length is limited by what browser considers a valid URL. In IE it can be no longer than about 2000 chars
                </li>
                <li>
                  Target iframe may not use hashes for page navigation
                </li>
              </ul>
            </td>
            
            <td class="browser-support">
              All modern browsers except Opera Mini. There are more limitation if both parties are iframes.
            </td>
          </tr>
        </table>
        
        <p>
          The source code of the demo is on Github: <a href="https://github.com/ruslanbes/crossdomain">ruslanbes/crossdomain</a>
        </p>

 [1]: http://jquery-howto.blogspot.co.uk/2013/09/jquery-cross-domain-ajax-request.html "4 types of cross-domain AJAX"
 [2]: https://en.wikipedia.org/wiki/Same-origin_policy "Same-origin policy"
 [3]: #cors
 [4]: #jsonp
 [5]: #postMessage
 [6]: #proxy
 [7]: #MessageChannel
 [8]: #fragment
 [9]: https://github.com/oyvindkinsey/easyXDM/#readme
 [10]: https://alice.ruslanbes.com
 [11]: https://bob.ruslanbes.com
 [12]: https://cindy.ruslanbes.com
 [13]: https://douglas.ruslanbes.com
 [14]: https://eve.ruslanbes.com
 [15]: https://fred.ruslanbes.com
 [16]: https://api.jquery.com/jquery.get/ "jQuery get"
 [17]: http://zinoui.com/blog/cross-domain-ajax-request
 [18]: http://arunranga.com/examples/access-control/credentialedRequest.html
 [19]: https://api.jquery.com/deferred.done/ "jQuery done"
 [20]: https://api.jquery.com/jquery.getjson/ "jQuery getJSON"