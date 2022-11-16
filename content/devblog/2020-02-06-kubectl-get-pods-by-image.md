---
title: Kubectl – get pods by image
author: Ruslan Bes
type: post
date: 2020-02-06T19:16:06+00:00
url: /2020/02/06/kubectl-get-pods-by-image/
classic-editor-remember:
  - block-editor
categories:
  - Kubernetes
tags:
  - kubectl

---
A small tip mainly useful for troubleshooting the cluster — getting all pods that have containers with certain image.

This is not a trivial task because the pod does not connect to an image directly. Instead a pod has one or more containers which in turn are created from their own images. 

Thankfully all this information is in the pod definition, so we can get it and format it.

Note that in most cases you don&#8217;t need to do it because for single-container-pods the convention is to include the name of the image in the pod. But for special cases here is the trick how to get it.

You need to have the **kubectl** installed. Run:

<pre class="wp-block-code"><code>$ kubectl get pods --all-namespaces -o jsonpath="{range .items[*]}{.metadata.name}{' '}{.status.phase}{' '}{.spec.containers[*].image}{'\n'}{end}" | grep "&lt;IMAGE_NAME>" | column -t -s " "</code></pre>

## Example

Create 2 pods from the same **nginx** image and retrieve them with a command:

<pre class="wp-block-code"><code>$ kubectl run docs --image=nginx:1.17.8 --restart=Never
pod/docs created
 
$ kubectl run blog --image=nginx:1.16.1 --restart=Never
pod/blog created
 
$ kubectl get pods --all-namespaces -o jsonpath="{range .items[*]}{.metadata.name}{' '}{.status.phase}{' '}{.spec.containers[*].image}{'\n'}{end}" | grep "nginx" | column -t -s " "
blog  Running  nginx:1.16.1
docs  Running  nginx:1.17.8</code></pre>

## Explanation

The whole magic happens in the command: 

<pre class="wp-block-code"><code>kubectl get pods --all-namespaces -o jsonpath="{range .items[*]}{.metadata.name}{' '}{.status.phase}{' '}{.spec.containers[*].image}{'\n'}{end}"</code></pre>

It gets the json-s of all pods and allows you to filter interesting data from the pod definition using a [jsonpath][1] .

  * The&nbsp;`{range .items[*]` } lets you to iterate all the objects and glue the values from the same pod together.
  * The&nbsp;`.metadata.name`&nbsp; and&nbsp;`.status.phase`&nbsp; are the pieces of information we need.
  * `.spec.containers[*].image`&nbsp; loops through all the containers, so if you have pods with multiple containers this command will glue them with a space.

The second part is just filtering and formatting the data, converting &#8221; &#8221; to the separator for the columns.

<pre class="wp-block-code"><code>grep "nginx" | column -t -s " </code></pre>

This is not an ideal way to do it but it&#8217;s pretty fast, easy to understand and works most of the times unless your image name is used in some other unrelated pods. In that case you can adjust the grep filter to exclude them.

 [1]: https://kubernetes.io/docs/reference/kubectl/jsonpath/