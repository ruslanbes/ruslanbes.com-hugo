---
title: Passing iSAQB CPSA Foundation Level Exam in 2018
author: Ruslan Bes
type: post
date: 2018-12-23T12:12:14+00:00
url: /2018/12/23/passing-isaqb-cpsa-foundation-level-exam-in-2018/
featured_image: /rbes-content/uploads/2018/12/93785.jpg
dsq_thread_id:
  - 7124868397
classic-editor-remember:
  - block-editor
categories:
  - Learning
tags:
  - architecture
  - iSAQB
  - quality
  - software-architect

---
Recently I have taken a 4-day training culminated in the  
[iSAQB CPSA Foundation Level Exam][1]. The CPSA, by the way, stands for _Certified Professional for Software Architecture.&nbsp;_

In this post I want to share my thoughts about it. 

## Why this exam?  


The iSAQB organization is trying to position themselves as an industry standard, more or like _Project Management Institute_ with its PMBoK standard. But in the end I didn&#8217;t really specifically choose it, it was a suggestion by my boss from the company I&#8217;m currently working&nbsp; for — Netconomy, to take it.  


The exam is not focused on any specific architecture technology &#8211; Kubernetes, AWS, Oracle, whatever, but is rather about the theoretical standards. iSAQB claims that the certification has been gaining popularity over time:<figure class="wp-block-image">

<img src="https://i0.wp.com/www.isaqb.org/wp-content/uploads/2018/03/Folie2.jpg?w=705&#038;ssl=1" alt="" data-recalc-dims="1" /> <figcaption>https://www.isaqb.org/wp-content/uploads/2018/03/Folie2.jpg</figcaption></figure> 

## What have I learned?

I think, the best way to illustrate the benefit is this image:<figure class="wp-block-image">

<img loading="lazy" width="600" height="400" src="https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2018/12/before.png?resize=600%2C400&#038;ssl=1" alt="" class="wp-image-1053" srcset="https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2018/12/before.png?w=600&ssl=1 600w, https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2018/12/before.png?resize=300%2C200&ssl=1 300w" sizes="(max-width: 600px) 100vw, 600px" data-recalc-dims="1" /> </figure> <figure class="wp-block-image"><img loading="lazy" width="705" height="470" src="https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2018/12/After.png?resize=705%2C470&#038;ssl=1" alt="" class="wp-image-1054" srcset="https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2018/12/After.png?w=1061&ssl=1 1061w, https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2018/12/After.png?resize=300%2C200&ssl=1 300w, https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2018/12/After.png?resize=768%2C512&ssl=1 768w" sizes="(max-width: 705px) 100vw, 705px" data-recalc-dims="1" /></figure> 

So, my knowledge has become more organized.

## Have I learned anything else?  


The second benefit is that I have finally learned what the hell should software architect do and what he shouldn&#8217;t do. In essence: he should constantly battle. 

Battle with a client against requirements that do not make sense. 

Battle with Project Manager who: &#8220;_we need this feature yesterday and I don&#8217;t have a budget for your, howd&#8217;ya call it, reee-fucktoring_&#8220;. 

Battle with developers, who have got used to having no documentation at all and are frustrated that implementing small changes takes weeks.

However it&#8217;s not that bad. At least you learn the standard ways to communicate the architecture (hint: that&#8217;s your _numero uno_ responsibility) and you also learn the standards you can refer to. You can strongly argue your views to get budget for things.

## Course topics  


### Basic Concepts

The definitions of what exactly the Software Architecture is. Software Architect&#8217;s responsibilities. The difference of Software Architecture compared to Business Architecture. Conway&#8217;s Law.

### Influencing Factors

Functional vs Quality Requirements. Constraints (Organizational, Technical). Architecture goals (hint: the main goal is that the product does not degrade after many years of changes).

### Design and Development of Software Architectures

From _System Context view_ to _Black Box view_ to _White Box view_. The ways how to visualize your ideas and decisions step-by-step from a general perspective to the technical details (down to&nbsp; classes/files). Explanation why is Business Architecture orthogonal to the Technical Architecture. _Top-down_ vs. _Bottom-up_ approach in describing architecture. Specific approaches: DDD, Model-Driven Architecture, Reference Architectures. Basically, what standard languages or metaphors you can use to describe what you are going to create, so that you don&#8217;t invent your own ones.

### Architectural and Design Patterns

  * Architectural Patterns: MVC/MVVM, Master-Slave, Client/Server, RPC.
  * Design Patterns: Facade, Adapter, Factory etc. _you know them_  
    

### Interfaces and Dependencies

So, you have already described the blocks, now how would you describe interactions between those blocks? Types of dependencies (e.g. _temporal_ when internal service waits until a remote service is finished working and hangs). [SOLID][2] principles.

### Cross-Cutting Concepts

Basically, you should understand what it is and how it influences the architecture. Logging, Exception Handling, Security, UI Layer. This is something you never find in specification from a client (&#8217;cause they don&#8217;t care) and this is something **you** should actively fight for.

### Risk Management

Surprise! That&#8217;s your job now as well. Learn [FMEA][3]. Next!  


### Specification and Communication

Attributes of a good technical documentation. Reminder to adapt documentation to each class of readers. Context View → Building Block View → Runtime View (Sequence, States) → Deployment View (4 nerdZ). Basically, we learn UML diagrams again &#8217;cause there&#8217;s nothing better.

### Software Architecture and Quality

Quality Models (Basically just [ISO 25010][4]). Quality Metrics: Lines of Code (_haha, lol_), Cyclomatic Complexity, blablabla. Qualitative Methods: [ATAM][5] (Know in Russia as &#8220;АТАМУНИХВООБЩЕКАПЕЦ&#8221;), Quality Trees etc. A big topic is how to assess the amount of shit that the development team has produced over the years.

### Tools

Final topic. SonarQube, Sonargraph (has nothing to do with the former, it&#8217;s just a fu-, I mean, _wonderful_ naming coincidence). Tools for code generation (there are no good ones).  


### All topics in one place (clickable)  
<figure class="wp-block-image">

[<img loading="lazy" width="705" height="523" src="https://i0.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2018/12/topics.jpg?resize=705%2C523&#038;ssl=1" alt="" class="wp-image-1074" srcset="https://i0.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2018/12/topics.jpg?w=2000&ssl=1 2000w, https://i0.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2018/12/topics.jpg?resize=300%2C222&ssl=1 300w, https://i0.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2018/12/topics.jpg?resize=768%2C569&ssl=1 768w, https://i0.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2018/12/topics.jpg?resize=1200%2C890&ssl=1 1200w, https://i0.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2018/12/topics.jpg?w=1410&ssl=1 1410w" sizes="(max-width: 705px) 100vw, 705px" data-recalc-dims="1" />][6]</figure> 

## Exercises

The best part of the training were exercises. We were split to 3 teams and given the description of a software — in our case it was a place where people could lend the tools they don&#8217;t use — and description of use-cases: lending, borrowing, login and others. 

Each team first would create and present context view, then white box view, then perform risk analysis and defend each of the presentations against the other teams. That was kinda fun.  


## Exam

You get 43 random question from a tight pool (feels like 45-50 total). The questions are of 2 types. 

_Exempli gratia:_  


<blockquote class="wp-block-quote">
  <p>
    Type 1: &#8220;What is beneficial to do if you are working as architect in the scenario when <description_of_a_scenario> ?&#8221;
  </p>
</blockquote>

<table class="wp-block-table">
  <tr>
    <td>
      [Beneficial] [Not beneficial]
    </td>
    
    <td>
    </td>
  </tr>
  
  <tr>
    <td>
      [] []<br />
    </td>
    
    <td>
      Do A
    </td>
  </tr>
  
  <tr>
    <td>
      [] []<br />
    </td>
    
    <td>
      Do B<br />
    </td>
  </tr>
  
  <tr>
    <td>
      [] []<br />
    </td>
    
    <td>
      Do C
    </td>
  </tr>
  
  <tr>
    <td>
      [] [ ]<br />
    </td>
    
    <td>
      Do D
    </td>
  </tr>
</table>

Here you have a bunch of statements and you have to decide which ones are true and which ones are not that true. 

<blockquote class="wp-block-quote">
  <p>
    Type 2: &#8220;Which ones of these are <some_definition> (2 correct answers)?&#8221;
  </p>
</blockquote>

  * [] A
  * [] B
  * [] C
  * [] D
  * [] E

Here you are allowed to select 0,1 or 2 answers.

### Calculation of points

The calculation of points is very funny. For each question there is a **maximum**: either 1 or 2 points. If you answer completely correctly — guess all true/false statements, you get the maximum points. Otherwise you can get any fraction of points between 0 and **maximum**. Be aware that **a wrong answer to a statement counts as a negative fraction**.&nbsp; 

For the second example here is what you get for your answers if the maximum has been 1: 

  * Two marked, both are correct answers = 1 point
  * One marked and is a correct answer = 0.5 (1 of 2 guessed right)
  * One correct, one incorrect = 0 (+0.5 &#8211; 0.5)
  * Two marked and both are incorrect = 0 (you can&#8217;t get less than 0)

Now the main problem with answers is that many of them are **very** **controversial**, not covered in the training or contain some difficult phrases. And all this ambiguity is especially frustrating when you get the result saying: 

<blockquote class="wp-block-quote">
  <p>
    &#8220;You got 59.85% correct. Required score is 60%. Sorry, you did not pass.&#8221;
  </p>
</blockquote>

<div class="wp-block-image">
  <figure class="aligncenter"><img src="https://i1.wp.com/media.giphy.com/media/kHU8W94VS329y/giphy.gif?w=705&#038;ssl=1" alt="" data-recalc-dims="1" /></figure>
</div>

So the best strategy I have coined is to mark only those answers where you are pretty sure they are correct and skip the rest. Don&#8217;t get greedy, otherwise  
<figure class="wp-block-image">

<img src="https://i2.wp.com/media.giphy.com/media/njYrp176NQsHS/giphy.gif?w=705&#038;ssl=1" alt="" data-recalc-dims="1" /> </figure> 

Oh yes, you got it — required score to pass the exam is **60%**.  


## Post Mortem

Would I recommend passing the training? Yes, I guess so. It gives you certainty in things you already do and maybe some hints as to how to do it better. Remember, a Software Architect is _not about writing working code_.

Would I take the exam results seriously? Well, not really. We had a group of 9 people _from the same company_ and if you plot our results you get this:  
<figure class="wp-block-image">

<img loading="lazy" width="571" height="335" src="https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2018/12/plot-1.png?resize=571%2C335&#038;ssl=1" alt="" class="wp-image-1063" srcset="https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2018/12/plot-1.png?w=571&ssl=1 571w, https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2018/12/plot-1.png?resize=300%2C176&ssl=1 300w" sizes="(max-width: 571px) 100vw, 571px" data-recalc-dims="1" /> </figure> 

Top cluster contains scores 74-85% for three guys who have already worked as architects for like three years so it kinda confirms their experience. 

The close group in the middle: 65-68%

Two unlucky ones got the score slightly under 60%  


Which means, there is not much sense dividing people by whether they passed or not, but it makes sense to say to a colleague: &#8220;Suck it I&#8217;ve got much better score than you, haha!&#8221;. No, don&#8217;t do it please.  


Another point is that the training itself and the exam are felt very beta. Things and terms are changing, complete topics are thrown away and replaced. Maybe, in 6 months the questions pool will contain 83% new questions.

<div class="wp-block-image">
  <figure class="aligncenter is-resized"><img loading="lazy" src="https://vignette.wikia.nocookie.net/mlp/images/3/3f/True-story-neil-patrick-harris.png/revision/latest?cb=20141205200447" alt="" width="200" height="187" /></figure>
</div>

And yes, I&#8217;ve passed.

 [1]: https://www.isaqb.org/certifications/?lang=en&fbclid=IwAR3EgjStp70bJmtXPfQlGR6Oba8pn5HOy4Ct_IMiRxu8Y1Gi-rl4yVQF5U4
 [2]: https://en.wikipedia.org/wiki/SOLID
 [3]: https://en.wikipedia.org/wiki/Failure_mode_and_effects_analysis
 [4]: https://en.wikipedia.org/wiki/ISO/IEC_9126#Developments
 [5]: https://en.wikipedia.org/wiki/Architecture_tradeoff_analysis_method
 [6]: https://i0.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2018/12/topics.jpg?ssl=1