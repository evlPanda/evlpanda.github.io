---
layout: post
title:  "Get All AppClasses in an AppPackage"
date:   2019-09-15 12:34:56 +1000
categories: PeopleCode
blurb: This example code creates an array of all AppClasses in an AppPackage. Useful for any pattern that uses multiple sub-classes of the same type.

---

This example code creates an array of all AppClasses in an AppPackage. Useful for any pattern that uses multiple sub-classes of the same type.

In this example there is an interface and all AppClasses in the AppPackage are of this type; PRIOR_RECOGNITION:AutoAssess:Assessments:AssessmentInt. They all have a common method or two.

The Array is an array of the same type. The SQL filters out the interface itself.

This is well handy to enforce the single responsibility principle; each class in the package does its own thing.

This method allows you to add more AppClasses to the AppPackage and they’ll all available to the client without changing any code.

{% highlight java %}
method createAssessments
   &Assessments = CreateArrayRept(create PRIOR_RECOGNITION:AutoAssess:Assessments:AssessmentInt(), 0);
   Local string &packageRoot = "PRIOR_RECOGNITION";
   Local string &path = "AutoAssess:Assessments";
   Local SQL &AssessmentsObjectsSQL = CreateSQL("select appclassid from psappclassdefn where packageroot = :1 and qualifypath = :2 and appclassid  'AssessmentInt'", &packageRoot, &path);
   Local string &appClass;
   While &AssessmentsObjectsSQL.Fetch(&appClass)
      &Assessments.Push(CreateObject(&packageRoot | ":" | &path | ":" | &appClass));
   End-While;
end-method;
{% endhighlight %}
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzNTg0MDQ3NTJdfQ==
-->