---
title: Kubectl Basic Tips
author: Ruslan Bes
type: post
date: 2019-09-27T20:54:44+00:00
url: /2019/09/27/kubectl-basic-tips/
featured_image: /rbes-content/uploads/2019/09/kubectl.jpg
classic-editor-remember:
  - block-editor
categories:
  - Kubernetes
  - 'Tips &amp; tricks'
tags:
  - kubectl

---
<figure class="wp-block-image"><img loading="lazy" width="705" height="705" src="https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2019/09/kubectl.jpg?resize=705%2C705&#038;ssl=1" alt="" class="wp-image-1503" srcset="https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2019/09/kubectl.jpg?w=960&ssl=1 960w, https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2019/09/kubectl.jpg?resize=150%2C150&ssl=1 150w, https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2019/09/kubectl.jpg?resize=300%2C300&ssl=1 300w, https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2019/09/kubectl.jpg?resize=768%2C768&ssl=1 768w" sizes="(max-width: 705px) 100vw, 705px" data-recalc-dims="1" /></figure> 

If you just started using&nbsp;`kubectl`&nbsp; tool there are some must-have things to ease your work. They are tested on Linux and Mac, and may work on Windows in [MinGW][1] or [WSL][2] but I can&#8217;t test it.

## Alias to k {#KubectlBasicTips-Aliastok}

Because writing `kubectl` is too much effort and you have a chance for a typo. The trick is to change it to k.

Add to your `~/.bashrc` (or if you prefer zsh then to `~/.zshrc` ):

<pre class="wp-block-code"><code>alias k="kubectl"</code></pre>

Restart the shell. After that you can write all the basic commands:

<pre class="wp-block-code"><code>$ k get pods
$ k get nodes
$ k config get-contexts</code></pre>

## Completion {#KubectlBasicTips-Completion}

Completion in kubectl helps you to find the right pods, nodes and works pretty much like completion for filenames in shell.

For the latest guide refer to the [official docs][3]. The guide depends on which OS you use.

You will need again to edit the `.bashrc`&nbsp; or `.zshrc`&nbsp; config file and restart the shell.

<pre class="wp-block-code"><code># do OS-specific stuff to include bash-completion 
source &lt;(kubectl completion bash)</code></pre>

For some reason bash-completion didn&#8217;t work for me on MacOS Mojave. The solution was a bit radical: switch from bash to zsh. 

In this example I list all my contexts (contexts are essentially different clusters). There are 3 of them. One is from Google Cloud (gke- prefix), and I want to switch to it

<pre class="wp-block-code"><code>$ k config get-contexts
CURRENT   NAME                                      CLUSTER                                   AUTHINFO                                        NAMESPACE
          gke_jonsnow-245010_europe-west1-b_kubia   gke_jonsnow-245010_europe-west1-b_kubia   gke_jonsnow-245010_europe-west1-b_kubia
*         kube-aws-ruslanbes-5678109-d1-kub         kube-aws-ruslanbes-5678109-d1-kub         kube-aws-ruslanbes-5678109-d1-kub-admin
          kube-aws-ruslanbes-5678109-s1-kub         kube-aws-ruslanbes-5678109-s1-kub         kube-aws-ruslanbes-5678109-s1-kub-admin

$ k config use-context nc&lt;tab> ⤵
$ k config use-context ncpltws-c-env6-k8s
Switched to context "ncpltws-c-env6-k8s".</code></pre>

## Watch {#KubectlBasicTips-Watch}

Sometimes you&#8217;d like to monitor what&#8217;s going on with your cluster, most often &#8211; the status of your pods.

<pre class="wp-block-code"><code>$ watch kubectl get pods</code></pre>

To make&nbsp;`watch`&nbsp; also work with&nbsp;`k`&nbsp; you need to add one line to your `.bashrc`&nbsp; or `.zshrc`&nbsp; config file and restart the shell.

<pre class="wp-block-code"><code>alias watch='watch -c '</code></pre>

Notice the space between&nbsp;`-c`&nbsp; and the quote. This is specifically to handle aliased commands. You can also omit `-c`&nbsp; &#8211; this parameter tells to colorize output if possible.

<pre class="wp-block-code"><code>$ watch k get pods
 
Every 2.0s: kubectl get pods
 
NAME          READY   STATUS    RESTARTS   AGE
dnsutils      1/1     Running   0          6d4h
kubia-5g6x8   1/1     Running   0          6d3h
kubia-drkm8   1/1     Running   0          18d
kubia-sd4g7   1/1     Running   0          6d3</code></pre>

 [1]: https://en.wikipedia.org/wiki/MinGW
 [2]: https://en.wikipedia.org/wiki/Windows_Subsystem_for_Linux
 [3]: https://kubernetes.io/docs/tasks/tools/install-kubectl/#enabling-shell-autocompletion