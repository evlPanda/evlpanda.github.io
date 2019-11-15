---
layout: post
title:  "How To Download a File From The Server Using PeopleCode"
date:   2019-06-26 12:34:56 +1000
categories: [PeopleCode, Files]
blurb: There's no obvious way to *download* a file object that is in your code. A PeopleSoft Utility class has a very handy function for doing exactly this,  viewStringAsAttachment.
---

There's no obvious way to *download* a file object that is in your code. The ```PTFP_FEED:UTILITY:Utility``` class has a very handy function for doing exactly this,  [viewStringAsAttachment.](https://docs.oracle.com/cd/F13640_01/pt857pbr2/eng/pt/tpcr/langref_UtilityClassMethods-1b6639.html#u717f010e-484a-44f6-aaca-988e080ca02f)

{% highlight ruby %}
import PTFP_FEED:UTILITY:Utility;
Local &Util = create PTFP_FEED:UTILITY:Utility();
&Util.viewStringAsAttachment(&YourFile.GetString(), _"downloadName.txt"_, false)
[% endhighlight %}
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTY5MDU3MjcyNSwtMTI2NjcxMjM5LC0xMj
Q4OTA1Nzg3XX0=
-->