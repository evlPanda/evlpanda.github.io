---
layout: post
title:  "Get All AppClasses in an AppPackage"
date:   2019-09-15 12:34:56 +1000
categories: PeopleCode
blurb: This example code creates an array of all AppClasses in an AppPackage. Useful for some patterns that use multiple classes of the same type.
---

This example code creates an array of all AppClasses in an AppPackage. Useful for some patterns that use multiple classes of the same type.

The crux of this is some SQL:  
{% highlight sql %}
select appclassid
from psappclassdefn
where packageroot = :1
and qualifypath = :2
and appclassid <> :3
{% endhighlight %}

This all assumes an interface, and that all AppClasses in the AppPackage are of this type.  
The Array is an array of the same interface.  
The SQL filters out the interface itself: ```and appclassid <> :3```

{% highlight java %}
class ArrayOfAppClasses
   method get(&packageRoot as string, &path as string, &interface as string) returns Array of APIObject;
end-class;   

method get
   local Array of APIObject &AppClasses= CreateArrayRept(CreateObject(&packageRoot | ":" | &path | ":" | &interface), 0);
   Local SQL &AppClassesSQL = CreateSQL("select appclassid from psappclassdefn where packageroot = :1 and qualifypath = :2 and appclassid <> :3", &packageRoot, &path, &interface);
   Local string &appClass;
   While &AppClassesSQL.Fetch(&appClass)
      &AppClasses.Push(CreateObject(&packageRoot | ":" | &path | ":" | &appClass));
   End-While;
   return &AppClasses;
end-method;
{% endhighlight %}

If your classes take parameters when creating you'll need to mod or extend or whatever the above to use [CreateObjectArray(*Class_Name, Array_of_Args*)](https://docs.oracle.com/cd/E92519_02/pt856pbr3/eng/pt/tpcl/langref_PeopleCodeBuilt-inFunctionsAndLanguageConstructs_C.html#ub00fb66b-2299-4bb0-a977-e187ee7d0577) instead.

I've just slapped the above together, mainly so that I can recall the SQL involved in future. I haven't even tested it. Should be close though, and the main bit is really the SQL.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyMDA1NzUzNzksLTU5Nzc5NTA4NywzND
gwNTM5NjgsNjE4MjMxNjIwLDE2MzgxMTY4OTIsNjcxNjk0ODgx
XX0=
-->
