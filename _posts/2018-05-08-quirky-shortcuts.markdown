---
layout: post
title: Quirky Shortcuts
date: 2018-05-08 13:31:19.000000000 +10:00
type: post
---

**Update**: I guess it's obvious now I was primed for [Clean Code](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882?SubscriptionId=AKIAILSHYYTFIVPWUY6Q&tag=duckduckgo-osx-20&linkCode=xm2&camp=2025&creative=165953&creativeASIN=0132350882)

I've been using the methods outlined in this book for a couple of weeks now. Everything is so much better now. I feel like I only have to use half my brain all day.

---


Some quirky little ways of writing things I've been doing lately. I don't use them all the time because they can be harder to parse, but interesting nonetheless.

#If-Then-Else in 1 line.

If setting a boolean value via an If-Then-Else statement you can often do it in one line.

Standard:

{% highlight ruby %}
Local boolean &display;
&display = &HelperObject.IsSomething(&someVal);
If &display = true Then
  SOME_FIELD.Visible = true;
Else
  SOME_FIELD.Visible = false;
End-If;
{% endhighlight %}

Quirky Shortcut:

{% highlight ruby %}
    SOME_FIELD.Visible = &HelperObject.IsSomething(&someVal);
{% endhighlight %}

Yeah. Nah. I reckon that is easier to parse than the standard way.

#Using Properties of a Returned Object

What?

Let's say you have a method that returns an array:

{% highlight ruby %}
    Method GetStuff(&param as string) returns array of string;
{% endhighlight %}

You can use GetStuff.Len, for example. The property of the array itself. Direct-like.

Standard:

{% highlight ruby %}
    Local array of string &someArray;
    &someArray = &HelperObject.GetStuff("x");
    FIELD_COUNT_STUFF.Value = &someArray.Len;
{% endhighlight %}

Quirky Shortcut:

{% highlight ruby %}
    FIELD_COUNT_STUFF.Value = &HelperObject.GetStuff("x").Len;
{% endhighlight %}

Again, I reckon that's actually easier for the brain to parse.
¯\_(ツ)_/¯
