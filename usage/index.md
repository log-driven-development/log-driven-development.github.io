---
layout: page
title: "Usage"
date: 
modified:
excerpt:
image:
  feature:
---

One of the best things about LDD is how easy it is to integrate into your workflow. First, we'll go over some basic setup recommendations and then we'll introduce some core concepts. 

> This section needs help! If you have any additional setup steps for unique environments/stacks, or would like to propose some changes to the core concepts as we work towards standardization, please consider submitting a pull request.

##Setting Up

In order to get started with Log Driven Development, you'll need access to the output of your environment's logging facilities. Some examples

###PHP

In PHP, you'd want to find your error_log file. Then you can...

{% highlight bash %}
tail -f /the/location/of/your/error_log
{% endhighlight %}

###JavaScript (Front-end)

For JavaScript, you need only open the javascript console in your browser. 

##Core Concepts

First and foremost, the best test is *no test*. Tests are a sign of weak code. If you need to run a test suite each time a commit lands, you're doing something wrong. In LDD, you write your tests within your code, test directly, and when the test passes, you simply remove or comment out the test. Keeps code clean, and adheres to the <abbr title="Write Everything Twice">WET</abbr> principle - for if you need a test again, you should have to write the test again. Repeating this exercise will lead to less-buggy code in the future, as you will not want to rewrite tests over and over. This is a thing that makes sense.

####Removing vs. Commenting

Ideally, you wouldn't have any tests in your code. Once a test passes and you are ready to <abbr title="Filezilla">FTP</abbr> your changes to prod, you can remove the test code. If you are iterating on a feature or a bugfix, and need to see how the rest of the code behaves after a fix without test noise, it is permissable to simply comment out the test code.

####WET Principle

The <abbr title="Write Everything Twice">WET</abbr> - or, "Write Everything Twice" Principle is part of the core philosophy of LDD. Rather than creating all sorts of additional work up front by planning and creating "re-usable" components, LDD lends itself to the procedural style of development. If you need to re-use something, that is a perfect use-case for the Copy and Paste functions available in any environment. You can simply adapt the pasted version of your code to fit with the new context, and even extend or refactor it using new knowledge or optimizations. WET principle does allow for growth of the programmer, and a mark of that is a growing codebase.

{% highlight bash %}
The more code written === the more work being done solving problems.
{% endhighlight %}

While ideal LDD strives towards zero LOC for tests, it still follows the WET principle while writing tests. 

##Variables

Variables should be written to logs in human readable format, all caps, followed by a space, the word "is", a colon, and another space. The variable value should be concatenated to the end of this string.

######Example - PHP

{% highlight php %}
<?php
$hello = "world";
error_log("HELLO is: " . $hello);
?>
{% endhighlight %}

######Example - JavaScript

{% highlight javascript %}
var hello = "world";
console.log("HELLO is: " + hello);
{% endhighlight %}

###Functions

Functions will typically have an announcement message at the top of the function, announcements at the top of any if/else or switch statements, and announcement of return values. Depending on what needs to be tested, the return value, or the amount of variables passed to the function, you may need more. See the examples below for ways one might use LDD to test a function.

######Example - PHP

{% highlight php %}
<?php
function hello($world) {
  error_log("INSIDE HELLO");
	error_log("WORLD is: " . $world);
	if(!empty($world) && $world !== "world") {
	  error_log("INSIDE IF");
          $world = 'world';
  }
  error_log("WORLD is: " . $world);
  return $world;
}

$world = "hello";
error_log("BEFORE HELLO");
error_log("WORLD is: " . $world);
$world = hello($world);
error_log("AFTER HELLO");
error_log("WORLD is: " . $world);
?>
{% endhighlight %}

######Example - JavaScript

{% highlight javascript %}

var hello = function(world) {
  console.log("INSIDE HELLO");
  console.log("WORLD is: " + world);
  if(world.length > 0 && world !== "world") {
    console.log("INSIDE IF");
    world = "world";
  }
  console.log("WORLD is: " + world);
  return world;
};

var world = "hello";
console.log("BEFORE HELLO");
console.log("WORLD is: " + world);
world = hello(world);
console.log("AFTER HELLO");
console.log("WORLD is: " + world);

{% endhighlight %}


###If/Else
If/else statements can start with simple announcements as you enter each condition. Below is an example.

######Example - PHP

{% highlight php %}
<?php
$hello = "world";

if($hello === "world") {
	error_log("INSIDE IF");
	return $hello;
} else {
	error_log("INSIDE ELSE");
	// SHOULD NEVER HAPPEN
}
?>
{% endhighlight %}

######Example - JavaScript

{% highlight javascript %}

var hello = "world";

if(hello === "world") {
	console.log("INSIDE IF");
	return hello;
} else {
	console.log("INSIDE ELSE");
	// SHOULD NEVER HAPPEN
}

{% endhighlight%}