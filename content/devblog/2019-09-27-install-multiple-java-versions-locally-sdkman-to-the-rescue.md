---
title: Install multiple Java versions locally. SDKman to the rescue.
author: Ruslan Bes
type: post
date: 2019-09-27T21:08:02+00:00
url: /2019/09/27/install-multiple-java-versions-locally-sdkman-to-the-rescue/
classic-editor-remember:
  - block-editor
categories:
  - Java
tags:
  - SDKman

---
<figure class="wp-block-image">![][1]</figure> 

With the new Java Release Cycle you may need to have Java 8 (previous LTS) and Java 11 (current LTS) installed simultaneously and switch back and forth between them.

If you&#8217;re using Linux or MacOs then you can use <https://sdkman.io/> . It is a tool to maintain multiple SDK-s locally without conflicts. Besides Java you can use it for gradle, maven, groovy etc.

# Step-by-step guide {#InstallmultipleJavaversionslocally.SDKmantotherescue.-Step-by-stepguide}

## Install sdkman {#InstallmultipleJavaversionslocally.SDKmantotherescue.-Installsdkman}

<pre class="wp-block-code"><code>$ curl -s "https://get.sdkman.io" | bash</code></pre>

## List versions of Java {#InstallmultipleJavaversionslocally.SDKmantotherescue.-ListversionsofJava}

<pre class="wp-block-code"><code>$ sdk list java
================================================================================
Available Java Versions
================================================================================
 Vendor        | Use | Version      | Dist    | Status     | Identifier
--------------------------------------------------------------------------------
 AdoptOpenJDK  |     | 12.0.1.j9    | adpt    |            | 12.0.1.j9-adpt
               |     | 12.0.1.hs    | adpt    |            | 12.0.1.hs-adpt
               |     | 11.0.4.j9    | adpt    |            | 11.0.4.j9-adpt
               |     | 11.0.4.hs    | adpt    |            | 11.0.4.hs-adpt
               |     | 8.0.222.j9   | adpt    |            | 8.0.222.j9-adpt
               |     | 8.0.222.hs   | adpt    |            | 8.0.222.hs-adpt
 Amazon        |     | 11.0.4       | amzn    |            | 11.0.4-amzn
               |     | 8.0.222      | amzn    |            | 8.0.222-amzn
               |     | 8.0.202      | amzn    |            | 8.0.202-amzn
 Azul Zulu     |     | 13.0.0       | zulu    |            | 13.0.0-zulu
               |     | 12.0.2       | zulu    |            | 12.0.2-zulu
               |     | 11.0.4       | zulu    |            | 11.0.4-zulu
               |     | 10.0.2       | zulu    |            | 10.0.2-zulu
               |     | 9.0.7        | zulu    |            | 9.0.7-zulu
               |     | 8.0.222      | zulu    |            | 8.0.222-zulu
               |     | 8.0.202      | zulu    |            | 8.0.202-zulu
               |     | 7.0.232      | zulu    |            | 7.0.232-zulu
               |     | 7.0.181      | zulu    |            | 7.0.181-zulu
 Azul ZuluFX   |     | 11.0.2       | zulufx  |            | 11.0.2-zulufx
               |     | 8.0.202      | zulufx  |            | 8.0.202-zulufx
 BellSoft      |     | 13.0.0       | librca  |            | 13.0.0-librca
               |     | 12.0.2       | librca  |            | 12.0.2-librca
               |     | 11.0.4       | librca  |            | 11.0.4-librca
               |     | 8.0.222      | librca  |            | 8.0.222-librca
 GraalVM       |     | 19.2.0       | grl     |            | 19.2.0-grl
               |     | 19.2.0.1     | grl     |            | 19.2.0.1-grl
               |     | 19.1.1       | grl     |            | 19.1.1-grl
               |     | 19.0.2       | grl     |            | 19.0.2-grl
               |     | 1.0.0        | grl     |            | 1.0.0-rc-16-grl
 Java.net      |     | 14.ea.15     | open    |            | 14.ea.15-open
               |     | 13.0.0       | open    |            | 13.0.0-open
               |     | 12.0.2       | open    |            | 12.0.2-open
               |     | 11.0.2       | open    |            | 11.0.2-open
               |     | 10.0.2       | open    |            | 10.0.2-open
               |     | 9.0.4        | open    |            | 9.0.4-open
 SAP           |     | 12.0.2       | sapmchn |            | 12.0.2-sapmchn
               |     | 11.0.4       | sapmchn |            | 11.0.4-sapmchn
================================================================================
Use the Identifier for installation:
 
    $ sdk install java 11.0.3.hs-adpt
================================================================================</code></pre>

## Install Java (JDK) versions {#InstallmultipleJavaversionslocally.SDKmantotherescue.-InstallJavaversions}

For JDK there are now multiple vendors. So besides the version you need to specify a vendor as well. The final version looks like `<java.version>-<vndr>` 

The general rule now is to use `AdoptOpenJDK`  since it has as for now the best community support. There is also incoming `GraalVM`  with promises to make Java extremely fast. It has its own versioning system not related to normal Java-s

<blockquote class="wp-block-quote">
  <p>
    AdoptOpenJDK has builds with two virtual machines: HotSpot (classic) and j9 (modern faster). You might want to have both in case you have problems with one of them.
  </p>
</blockquote>

<pre class="wp-block-code"><code>$ sdk install java 8.0.222.hs-adpt
$ sdk install java 8.0.222.j9-adpt
$ sdk install java 11.0.4-sapmchn</code></pre>

You will be asked if you want to set the installed versions as default Java. You can choose to do it or not to do it. I will show you how to change it. 

## Switch between versions {#InstallmultipleJavaversionslocally.SDKmantotherescue.-Switchbetweenversions}

Switch java version **temporarily** only for this shell:

<pre class="wp-block-code"><code>$ sdk use java 11.0.4-sapmchn
Using java version 11.0.4-sapmchn in this shell.
 
$ sdk current java
Using java version 11.0.4-sapmchn
 
$ java -version
openjdk version "11.0.4" 2019-07-17 LTS
OpenJDK Runtime Environment (build 11.0.4+11-LTS-sapmachine)
OpenJDK 64-Bit Server VM (build 11.0.4+11-LTS-sapmachine, mixed mode</code></pre>

Switch java version **globally** for this and all future shells:

<pre class="wp-block-code"><code>$ sdk default java 11.0.4.hs-adpt
Default java version set to 11.0.4.hs-adpt
 
$ sdk current java
Using java version 11.0.4.hs-adpt
 
$ java -version
openjdk version "11.0.4" 2019-07-16
OpenJDK Runtime Environment AdoptOpenJDK (build 11.0.4+11)
OpenJDK 64-Bit Server VM AdoptOpenJDK (build 11.0.4+11, mixed mode)</code></pre>

# Where are the installed files? {#InstallmultipleJavaversionslocally.SDKmantotherescue.-Wherearetheinstalledfiles?}

Check the directory `~/.sdkman/candidates` 

You can use it in the IntelliJ SDKs configuration<figure class="wp-block-image">

[<img loading="lazy" width="705" height="362" src="https://i0.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2019/09/IntelliJ_SDKMan.png?resize=705%2C362&#038;ssl=1" alt="" class="wp-image-1514" srcset="https://i0.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2019/09/IntelliJ_SDKMan.png?w=1049&ssl=1 1049w, https://i0.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2019/09/IntelliJ_SDKMan.png?resize=300%2C154&ssl=1 300w, https://i0.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2019/09/IntelliJ_SDKMan.png?resize=768%2C394&ssl=1 768w" sizes="(max-width: 705px) 100vw, 705px" data-recalc-dims="1" />][2]</figure> 

# Is there an auto-update? {#InstallmultipleJavaversionslocally.SDKmantotherescue.-Isthereanauto-update?}

No, but you can do it manually.

<pre class="wp-block-code"><code>$ sdk upgrade java 11.0.4-sapmchn
 
java is up-to-date</code></pre>

# What about other SDKs like groovy, scala, kotlin etc? {#InstallmultipleJavaversionslocally.SDKmantotherescue.-WhataboutotherSDKslikegroovy,scala,kotlinetc?}

You can list all SDKs supported by sdkman:

<pre class="wp-block-code"><code>$ sdk list</code></pre>

For other SDKs the process is the same except you normally have one vendor and only need to tell which version you want.

 [1]: https://sdkman.io/assets//img/sdk-man-small-pattern.svg
 [2]: https://i0.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2019/09/IntelliJ_SDKMan.png?ssl=1