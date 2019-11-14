---
layout: post
title:  "Get All AppClasses in an AppPackage"
date:   2019-09-15 12:34:56 +1000
categories: PeopleCode
blurb: This example code creates an array of all AppClasses in an AppPackage. Useful for any pattern that uses multiple sub-classes of the same type.

---

This example code creates an array of all AppClasses in an AppPackage. Useful for any pattern that uses multiple sub-classes of the same type.

In this example there is an interface and all AppClasses in the AppPackage are of this type; PRIOR_RECOGNITION:AutoAssess:Assessments:AssessmentInt. They all have a common method or two.

The Array is an array of the same interface. The SQL filters out the interface itself.

{% highlight java %}
import YOUR_PACKAGE:Example:Interface;

method getArrayOfAppClasses() returns Array of YOUR_PACKAGE:Example:Interface;

method getArrayOfAppClasses
   local Array of YOUR_PACKAGE:Example:Interface &AppClasses;
   &AppClasses= CreateArrayRept(create YOUR_PACKAGE:Example:Interface(), 0);
   Local string &packageRoot = "YOUR_PACKAGE";
   Local string &path = "Example";
   Local SQL &AssessmentsObjectsSQL = CreateSQL("select appclassid from psappclassdefn where packageroot = :1 and qualifypath = :2 and appclassid  <> 'Interface'", &packageRoot, &path);
   Local string &appClass;
   While &AssessmentsObjectsSQL.Fetch(&appClass)
      &Assessments.Push(CreateObject(&packageRoot | ":" | &path | ":" | &appClass));
   End-While;
end-method;
{% endhighlight %}

I've just slapped the above together, mainly so that I can recall the SQL involved in the future, but it could be refactored into a nice little helper class that takes a path to an interface and returns the array, or similar.
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTYzODExNjg5Miw2NzE2OTQ4ODFdfQ==
-->