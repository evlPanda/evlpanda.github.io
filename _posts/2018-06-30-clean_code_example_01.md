---
layout: post
title:  "CleanCodeExample01"
date:   2018-06-30 12:49:57 +1000
categories: clean code
---

There may be nothing harder to follow than a deeply nested series of loops and ifs with a collection of obfuscated variable and/or function names.

**Before:**

{% highlight ruby %}
/* Make sure we have keys and phone and wallet
   before leaving for work. */
While &user_keys = False and
  &wallet = False and
  &phone <> "Y"

  /* If we have all of them. */
  If &user_keys = True Then
    If &wallet = True Then
      If &phone = "Y" Then
        P_Leave_For_Work();
      Else

        /* Get phone. */
        &obj_ph = PhoneFinder(&user);
        local integer &x;

        /* Look for phone. */
        for &x = 1 to &all_places.len
          if &all_places[&x] = "there is my phone!"
            /* found the phone! */
            &pocket.push(&phone);
            &phone = "Y";
          end-if;
        end-for;

      End-If; /* end phone if */

    Else    
      /* Find wallet. */
      DoFind("wallet", &user);
    End-If;

  Else
    /* Find the keys. */
    &keys = LoadKeys(&user, "house", true, null);
  End-If;

End-While;
{% endhighlight %}

**After:**

{% highlight ruby %}

function GetPhone
  local integer &i;
  for &i = 1 to &places.len
    if &places[&i] = "there is my phone!"
      &pocket.push(&phone);
    end-if;
  end-for;
end-function;

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

You can remove *nested ifs* by checking for exceptions first. This leaves a clear [happy path](https://en.wikipedia.org/wiki/Happy_path).

Group logical chunks into their own functions.

Comments aren't required if you name your variables and functions self-descriptively.

Coding Horror goes into it some more [in this article](https://blog.codinghorror.com/flattening-arrow-code/).
