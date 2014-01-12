---
title: More readable ternary statements in PHP
description: A quick tip for producing more readable code.
tags:
  - PHP
  - Ternary Operator
  - Tips
layout: post
---

We all know the virtues of the ternary operator in PHP, allowing you to set / return values based on another value without having to build a full if statement:

{% highlight php startinline %}
$bar = 'string';
$foo = $bar == 'string' ? TRUE : FALSE; // Will produce $foo = TRUE;
{% endhighlight %}

But did you also know that (like most of PHP) the ternary operator is whitespace independant, meaning that if you have a really long value you want to check, something like this in Drupal:

{% highlight php startinline %}
// Get the value of a field.
$field = $node->field_my_awesome_long_field_name[LANGUAGE_NONE][0]['value'];
{% endhighlight %}

Then you will end up with a rather shitty, wrapping statement in your editor like this:

{% highlight php startinline %}
$field = isset($node->field_my_awesome_long_field_name[LANGUAGE_NONE][0]['value']) ? $node->field_my_awesome_long_field_name[LANGUAGE_NONE][0]['value']; : NULL;
{% endhighlight %}

But instead, why don't you split each value onto it's own line, like this:

{% highlight php startinline %}

$field = isset($node->field_my_awesome_long_field_name[LANGUAGE_NONE][0]['value']) // Condition

	? $node->field_my_awesome_long_field_name[LANGUAGE_NONE][0]['value']; // If TRUE

	: NULL; // if FALSE
{% endhighlight %}

Look at that! So much more readable, and without any extra control structures, since you're doing a straight assignment.
