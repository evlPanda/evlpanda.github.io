---
layout: post
title:  "CleanCodeExample01"
date:   2018-06-30 12:49:57 +1000
categories: clean code
---

There may be nothing harder to follow than a deeply nested series of loops and ifs directing a collection of obfuscated variable and method names.

**Before:**

{% highlight ruby linenos %}
While &user_keys = False and
  &wallet = False and
  &phone <> "Y"
  If &user_keys = True Then
    If &wallet = True Then
      If &phone = "Y" Then
        LeaveForWork();
      Else
        &obj_ph = PhoneFinder(&user);
        &phone = "Y";
      End-If;
    Else
      DoFind("wallet", &user);
    End-If;
  Else
    &keys = LoadKeys(&user, "house", true, null);
  End-If;
End-While;
{% endhighlight %}

I'm dizzy.

**After:**

{% highlight ruby linenos %}
  If not &hasHouseKeys Then
    GetHouseKeys();
  End-If;
  If not &hasWallet Then
    GetWallet();
  End-If;
  If not &hasPhone Then
    GetPhone();
  End-If;
  LeaveForWork();
{% endhighlight %}

40% less code and *so much* easier to read.

You can remove *nested ifs* by checking for exceptions first. This leaves a clear [happy path](https://en.wikipedia.org/wiki/Happy_path).
