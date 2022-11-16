---
title: Spring In Action for hybris developers
author: Ruslan Bes
type: post
date: 2019-03-12T20:03:34+00:00
url: /2019/03/12/spring-in-action-for-hybris-developers/
classic-editor-remember:
  - block-editor
categories:
  - Java
  - SAP Commerce (hybris)
tags:
  - spring

---
 

[Spring In Action][1]&nbsp;is considered a classic and nearly must-read for anyone who whats to start with Spring development. I would say it is enough to read the book briefly and get the main idea, which is:<figure class="wp-block-image">

<img loading="lazy" width="400" height="400" src="https://i0.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2019/03/configurations-configurations-everywhere.jpg?resize=400%2C400&#038;ssl=1" alt="" class="wp-image-1317" srcset="https://i0.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2019/03/configurations-configurations-everywhere.jpg?w=400&ssl=1 400w, https://i0.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2019/03/configurations-configurations-everywhere.jpg?resize=150%2C150&ssl=1 150w, https://i0.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2019/03/configurations-configurations-everywhere.jpg?resize=300%2C300&ssl=1 300w" sizes="(max-width: 400px) 100vw, 400px" data-recalc-dims="1" /> </figure> 

The book consists of 4 parts:

  1. Core Spring: configuration, wiring, aspects, Spring Expression Language (SpEL).
  2. Spring on the web: MVC, Controllers, Controller advices, views, Spring web flow.
  3. Spring in the back end: Configuring JDBC, ORM, Working with NoSQL Databases, Caching, Securing methods.
  4. Integrating Spring: Calling remote services, Creating REST API, Messaging, Sending emails, Managing beans with JMX

I would say for hybris developer Part 1 should be too easy as it covers the very basics.

Part 2 (and also Part 1) is most useful for frontend devs who need to do some backend magic from time to time.

Part 3 has too many things that we don&#8217;t use because of hybris&#8217;s custom implementation.

Part 4 has some useful things for backend devs, I especially recommend to check the JMX part, as it allows monitoring hybris properties and changing values on-the-fly.

For the rest of the book I made some notes during my reading and want to share with you the things I was not aware of.

## RequestMapping annotation tricks {#SpringInAction-RequestMappingannotationtricks}

Standard hybris code has example of parametrizing RequestMapping&nbsp;  
  
`@RequestMapping(value = "/paymentinfo/{id}", method = RequestMethod.PUT)`  
  
and you also may be aware that you can use wildcards \* and \** inside the path like this:  
  
`@RequestMapping("**/newsletter") // any path of any depth ending with "newsletter"`  
  
What is interesting is that the &#8220;value&#8221; can also be an array and you can write things like  
  
`@RequestMapping(value={"newsletter", "service/newsletter"})&nbsp;// match only these 2 urls`

## Unit-testing Controllers {#SpringInAction-Unit-testingControllers}

You can test controllers with MockMvc

The example is like this

<div class="wp-block-syntaxhighlighter-code ">
  <pre class="brush: java; title: ; notranslate" title="">
public class HomeControllerTest {
 
    @Test
    public void testHomePage() throws Exception {
        HomeController controller = new HomeController();
        MockMvc mockMvc = standaloneSetup(controller).build();
        mockMvc.perform(get("/"))
            .andExpect(view().name("home")); // test expected view name
    }
}
</pre>
</div>

##  
Spring taglib has some interesting features {#SpringInAction-Springtaglibhassomeinterestingfeatures}

`<%@ taglib uri="<a href="http://www.springframework.org/tags">http://www.springframework.org/tags</a>" prefix="s" %>`  
  
`<s:htmlEscape>` Sets the default HTML escape value for the current page  
`<s:escapeBody>` Escapes the content in the body of the tag  
  
Spring `url` tag can accept parameters (unlike the core `c:url`)

<div class="wp-block-syntaxhighlighter-code ">
  <pre class="brush: xml; title: ; notranslate" title="">
&lt;s:url href="/spitter/{username}" var="spitterUrl"&gt;
&lt;s:param name="username" value="jbauer" /&gt;
&lt;/s:url&gt;
</pre>
</div>

## Controller methods do not necessarily need to return strings. {#SpringInAction-Controllermethodsdonotnecessarilyneedtoreturnstrings.}

They can return arbitrary objects that will be injected into views

<div class="wp-block-syntaxhighlighter-code ">
  <pre class="brush: java; title: ; notranslate" title="">
@RequestMapping(method=RequestMethod.GET)
public List&lt;Spittle&gt; spittles() {
    return spittleRepository.findSpittles(Long.MAX_VALUE, 20));
}
</pre>
</div>

that works! We return object/list instead of String  
  
Spring will find a view that corresponds the method name&nbsp;`splittles`&nbsp;and add to the model the var&nbsp;`splittleList`&nbsp;(derived from the returned object).  
  
Not really convenient, but useful in some kind of API where all methods are like this. Using model is more explicit and convenient

## ControllerAdvice {#SpringInAction-ControllerAdvice}

Using `@ControllerAdvice` and `@ExceptionHandler` to handle error pages. 

## Authorization {#SpringInAction-Authorization}

There is a possibility to control the Authorization process using&nbsp;`@PreAuthorize`&nbsp;and&nbsp;`@PostAuthorize`&nbsp;annotations and use&nbsp;`@PreFilter`/`@PostFilter`&nbsp;to filter elements out of collections returned from or passed into a method.  
Summary:

  * `@PreAuthorize`&nbsp;Restricts access to a method before invocation based on the result of evaluating an expression
  * `@PostAuthorize`&nbsp;Allows a method to be invoked, but throws a security exception if the expression evaluates to false
  * `@PostFilter`&nbsp;Allows a method to be invoked, but filters the results of that method based on an expression
  * `@PreFilter`&nbsp;Allows a method to be invoked, but filters input prior to entering the method

 [1]: https://www.amazon.com/Spring-Action-Covers-4/dp/161729120X