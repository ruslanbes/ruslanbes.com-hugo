---
title: Binary search tree Node class in Java
author: Ruslan Bes
type: post
date: 2017-10-27T21:07:08+00:00
url: /2017/10/27/binary-search-tree-node-class-in-java/
dsq_thread_id:
  - 6246204380
categories:
  - Java
tags:
  - Binary search tree

---
As a supportive thing for some [careercup][1] challenges I wrote a class for a [BST][2] Node.

<pre class="brush: java; title: ; notranslate" title="">public class Node {
        Node left;
        Node right;
        Node parent;
        int data;

        Node(int data, Node parent) {
            this.data = data;
            this.parent = parent;
        }

        /**
         * Put an int into left node and return it
         * @return Node
         */
        protected Node putLeft(int data) {
            if (this.left == null) {
                this.left = new Node(data, this);
                return this.left;
            } else {
                return left.put(data);
            }

        }

        /**
         * Put an int into right node and return it
         * @return Node
         */
        protected Node putRight(int data) {
            if (this.right == null) {
                this.right = new Node(data, this);
                return this.right;
            } else {
                return right.put(data);
            }


        }

        /**
         * Ask Node to put an int into its children and return it
         * @return Node
         */
        public Node put(int data) {
            if (data == this.data) {
                return this;
            } else {
                return data &lt; this.data ? this.putLeft(data) : this.putRight(data);
            }
        }

        @Override
        public String toString() {
            // Example: 20 {10, 30}
            String l = this.left != null ? String.valueOf(left.data) : "";
            String r = this.right != null ? String.valueOf(right.data) : "";

            String c = "";
            if (this.left != null || this.right != null) {
                c = " {"+ l +", " + r +"}";
            }

            return String.valueOf(this.data) + c;
        }
    }
</pre>

It also stores the reference to the parent Node so you can traverse the tree once you have the Node object

Usage:

<pre class="brush: java; title: ; notranslate" title="">Node root = new Node(20, null);
root.put(10);
root.put(30);
root.put(5);
System.out.println(root); // 20 {10, 30}
System.out.println(root.left); // 10 {5, }
System.out.println(root.right.parent); // 20 {10, 30}
</pre>

I didn&#8217;t add getters here to make the code shorter ([KISS][3])

 [1]: https://careercup.com
 [2]: https://en.wikipedia.org/wiki/Binary_search_tree
 [3]: https://en.wikipedia.org/wiki/KISS_principle