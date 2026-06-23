---
layout: post
title: "Generic Field Errors"
date: 2026-06-22 00:00:00 +1000
categories: SQL
blurb: Finally?
---

```
   &Field.Style = "PSEDITBOX";
   If &someCondition = 1 Then
      &Field.Style = "PSERROR";
      &Field.SetCursorPos(%Page);
      Error MsgGetText(230, 1, "", "Your Error Message.");
   End-If;
```
Copy pasta and save 10 seconds.

Alternative i'll check in future...

...

...i *really* should know this by now. super embarrassing. I mean, it's not like I haven't been doing this for 20 years or anything.


```
   &Field.ErrorDisplay = false;
   If &someCondition = 1 Then
      &Field.ErrorDisplay = true;
      Error MsgGetText(230, 1, "", "Your Error Message.");
   End-If;
```