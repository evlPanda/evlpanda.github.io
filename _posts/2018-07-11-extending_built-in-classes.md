---
layout: post
title:  "ExtendBuiltInClasses"
date:   2018-07-08 12:34:5 +1000
blurb:  "Extend the built-in library"
categories: peoplecode
---

This works:

{% highlight ruby %}
class FieldX extends Field
  method FieldX(&fieldName as string);
end-class;

method FieldX
  /*+ &fieldName as string */
  %super = CreateField(@("Field." | &fieldName));
end-method;
{% endhighlight %}

What this means is you can extend and override the methods and properties of *any* of the delivered PeopleCode classes, such as Field, Record, RowSet, Array and so on.

For a useless, but illustrative example; perhaps some food-for-thought:

{% highlight ruby %}
class FieldX extends Field
  method FieldX(&fieldIn as Field);
  method MakeRequired();
  property string Colour;
end-class;

method FieldX
  /* &fieldIn as Field */
  %super = &fieldIn;
end-method;

method MakeRequired
  If none(%This.Value) Then
    WinMessage("It works!");
  End-If;
end-method;
{% endhighlight %}

I'll post some useful classes that I'm playing with at the moment some time in the near future.
