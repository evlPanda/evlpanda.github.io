---
layout: post
title:  "Extending the PeopleCode API"
date:   2018-07-08 12:34:5 +1000
categories: PeopleCode, Clean Code
---

The premise of a class being [open for extension, but closed for modification](https://en.wikipedia.org/wiki/Open–closed_principle) extends (ha) to PeopleCode's API too. Record, Field, Rowset and the rest of the gang are all open for extension.

If you are already familiar with extending and overriding classes skip to *Where
Was I?* below. This post got a lot longer than I intended, and goes into the
principles of [SOLID](https://en.wikipedia.org/wiki/SOLID).

# Open For Extension, Closed For Modification

If your eyes just glazed over reading the Wikipedia description fear not, it is written in technical jargon.

What the open closed principle simply means is that good code is written so that if another developer wants to modify the code's behaviour they can do so by *extending and overriding* it, not modifying the code itself. The code is open for extension and closed for modification.

The original developer needs to keep this premise in mind when designing their class. It is one of the 5 principles of [SOLID](https://en.wikipedia.org/wiki/SOLID).

The beauty of this principle is that by overriding, or extending, existing code we don't affect it and any other code already using it, so we don't need to retest it and all the code that already uses it. The change we make is decoupled from the original code we are using and doesn't affect it. Sounds good, right?

Note that code has to be written that *it can be* overridden. It is a professional skill.

# An Example

Here's a simple example. Let's say an employee's bonus is derived as 10% of all
sales for the year, and is encapsulated in some SQL.

{% highlight java %}
class Employee
   property string id;
   property number salary get;
   property number bonus get;
   method Employee(&id_ as string);
end-class;

get salary
   local number &s;
   SQLExec(SQL.EMPLOYEE_SALARY, %This.id, &s);
   return &s;
end-get;

get bonus
   local number &b;
   SQLExec(SQL.EMPLOYEE_BONUS, %This.id, &b);
   return &b;
end-get;

method Employee
   %This.id = &id_;
end-method;
{% endhighlight %}

Notice we have no indentation at all. All the code is left-aligned. Neat as a
pin.

Notice that each method does *one thing*.

Now, let's say we have a new requirement: employees with over 10 years
history at the company get their bonus at the end of the year, plus 10% of their
salary (for sticking the year out).

We *could* do something like this:

```
get bonus
   local number &b;
   SQLExec(SQL.EMPLOYEE_BONUS, %This.id, &b);
   /* begin: new requirement */
   local integer &yearsAtCompany;
   SQLExec(SQL.EMPLOYEE_YEARS_AT_COMPANY, %This.id, &yearsAtCompany);
   if &yearsAtCompany >= 10 then
      &b = &b + (%This.salary * 0.1);
   end-if;
   /* end: new requirement */
   return &b;
end-get;
```

Messy, *because* we've just broken 2 of the rules of SOLID; the Single Responsibility Principle,
and the Open-Closed Principle. Tsk, tsk, tsk.

The Single Responsibility Principle is broken because this method now does two
things, it now also checks for and handles long-term employee bonuses, and the Open-Closed
principle is broken because we just modified some already implemented code.

We don't really know what unintended side-effects our change might have. To *thoroughly*
test our change we also have to test all the code in our system that is
using ```Employee.bonus```.

The better approach is to create a class specifically for long-term employees,
that extends and overrides the original ```Employee``` class, instead of modifying it.

```
import Employee

class LongTermEmployee extends Employee
   property number bonus get;
end-class;

get bonus
   return %Super.bonus + (%Super.salary * 0.1);
end-get;
```

**We haven't
touched the ```Employee``` class at all.** We've modified code without modifying code. If you follow.

The new ```LongTermEmployee``` class can now be unit tested, etc. etc.

# Wiring it up

Of course we still have to figure out if the employee is a long-term employee or not, but
instead of shoving the logic for this in our ```Employee``` object we can "wire it up"
at a higher level in our code base.

I like to think that there are two distinct types of layers of code; "the object"
and "the wiring" that uses and/or sticks objects together. These are often one above the other,
and often repeat one after the other up the layers, as we build objects and wire
them together into bigger objects.

*"An engine is a part made up of parts made up of parts."* - me, just now.

I'd wire it up something like this. A naive factory.

```
import Employee, LongTermEmployee, Contractor;

class EmployeeFactory
   method getEmployee(&id as string) returns Employee;
private
   method isLongTermEmployee(&id as string) returns boolean
   method isContractor(&id as string) returns boolean
end-class;

method getEmployee
   Evaluate true
   When isLongTermEmployee(&id)
      return create LongTermEmployee(&id);
   When isContractor(&id)
      return create Contractor(&id);
   End-Evaluate;
   return create Employee(&id);
end-method;


/* private */

method isLongTermEmployee
   local integer &yearsAtCompany;
   SQLExec(SQL.EMPLOYEE_YEARS_AT_COMPANY, &id, &yearsAtCompany);
   return (&yearsAtCompany >= 10);
end-method;

method isContractor
   local integer &isContractor;
   SQLExec(SQL.EMPLOYEE_IS_CONTRACTOR, &id, &isContractor);
   return (&isContractor = 1);
end-method;
```

By the way the ```getEmployee()``` method there, where we finally implement our ```switch```
statement, is an example of a [Happy Path](https://en.wikipedia.org/wiki/Happy_path). Basically
we run through all the exceptions first, "returning early", leaving the final
result as the default, or happy path. Very common. Good practise.

The beauty of this factory is that we can continue adding as many types of
employees as we can dream up, *without affecting the existing code*. We plug
them in here. I've added ```Contractor```.

This is an example of how to build big, huge, or enormous code bases without it turning into spaghetti!

We also create classes that are specific; Employee, Long Term Employee, Contractor, Part Time, etc. etc.

Finally, how to get an employee's bonus:

```
import EmployeeFactory;

Local EmployeeFactory &EmployeeFactory = create EmployeeFactory();
Local Employee &Employee = EmployeeFactory.getEmployee("12345678");

Local number &bonus = &Employee.bonus;
```

Note that we can add more employee types to the factory and this code need not change.
It's "guts" are hidden. Objects are decoupled. We just use it to get the bonus.

# Where was I?

Anyway, the crux of this post is that all of the PeopleCode API can be extended
and overridden.

This works!

{% highlight ruby %}
class FieldX extends Field
  method FieldX(&fieldName as string);
end-class;

method FieldX
  /*+ &fieldName as string */
  %super = CreateField(@("Field." | &fieldName));
end-method;
{% endhighlight %}

What this means is you can extend and override the methods and properties of *any* of the delivered PeopleCode objects, such as Field, Record, RowSet, Array and so on.

For example let's override [Field.SetDefault()](https://docs.oracle.com/cd/F13640_01/pt857pbr2/eng/pt/tpcr/langref_FieldClassMethods-07149e.html#u1c899b67-55be-4a8b-a115-978968a260c4), and we'll even *pass in* the Field object instead of instantiating it.

{% highlight ruby %}
class FieldX extends Field
  method FieldX(&Field_ as Field);
  method SetDefault();
end-class;

method FieldX
  /* &FieldIn as Field */
  %Super = &Field_;
end-method;

method SetDefault
  /* overrides Field.SetDefault() */
  %Super.Value = "X";
end-method;
{% endhighlight %}

This class could then be used to "override" the standard behaviour:

{% highlight ruby %}
import YOUR_LIBRARY:FieldX;

Local Field &ThisField = GetField();

Local YOUR_LIBRARY:FieldX &DefaultsToX;
&DefaultsToX = create YOUR_LIBRARY:FieldX(&ThisField);

&DefaultsToX.SetDefault();
{% endhighlight %}


I'll post some useful classes that I'm playing with at the moment some time in the near future.
