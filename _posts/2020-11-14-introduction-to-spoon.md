---
layout: post
title:  "Introduction to Code Transformation in Java using Spoon"
author: "Máximo Oliveira"
tags: Spoon
---

This post is an attempt to guide people that want to use the Spoon library for the first time to transform Java code.

In this tutorial, I´ll cover what Spoon is and how to set it up along with a small example of its usage.

# What is Spoon?
From the [spoon documentation][spoon-documentation] we have that:
{% highlight markdown %}
"Spoon is an open-source library to analyze, rewrite, transform, transpile Java
source code. It parses source files to build a well-designed AST with powerful
analysis and transformation API..."
{% endhighlight %}

This tells us that Spoon converts our Java program into an AST, which then allows us to filter the parts of the program we want to analyze/transform more easily.

# But what is an AST?
An AST (Abstract Syntax Tree) is a tree representation of source code. I won´t dive deep into this concept as there are multiple other sources that explain it very well, like [this one][ast-1], or [this one][ast-2].

Let's see the AST representation of the following block of code that compares two variables and stores one of its values on a pre-existing variable named `info`, based on the result of the comparison.

{% highlight java %}
  if(x > y)
    info = x;
  else
    info = y;
{% endhighlight %}

We have that the AST representation is:
<br>
![ast-representation](..\assets\img\ast1.png)
_AST representation of the previous block of code_

We can see that the child node `>` represents the binary expression inside the if statement. If this expression evaluates to true, the second child node will be executed, else it will be the third.

We can clearly understand how the previous block of code works, but how can we be sure that it is working correctly and that it covers all possible cases? One could argue that the developer in charge of writing that code also covered it with test cases and ensured it is working fully as expected. However, a few months later a new input causes the program to fail and after carefull inspection, the debugger finds that the code is missing an equality sign after `>`, so the correct code would be:

{% highlight java %}
  if(x >= y)
    info = x;
  else
    info = y;
{% endhighlight %}



[spoon-documentation]: http://spoon.gforge.inria.fr/
[ast-1]: https://www.twilio.com/blog/abstract-syntax-trees
[ast-2]: https://ruslanspivak.com/lsbasi-part7/