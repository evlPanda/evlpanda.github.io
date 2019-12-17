---
layout: post
title:  "Blank Last Lines in Files"
date:   2019-06-26 12:34:56 +1000
categories: [PeopleCode, Files]
blurb: Situation: Files have an extra blank line at the end.
---

Situation: Files have an extra blank line at the end.
This can be corrected by setting .SetRecTerminator() to line feed (char 13).

{% highlight java %}
   Local File &File;
   &File = GetFile("newfile.txt", "W", "A", %FilePath_Relative);
   &File.SetRecTerminator(Char(13));
   &File.WriteLine("hello");
   &File.Close();
{% endhighlight %}

You can download the example file like this:

{% highlight java %}
   Local PTFP_FEED:UTILITY:Utility &Util = create PTFP_FEED:UTILITY:Utility();
   &Util.viewStringAsAttachment(GetFile("newfile.txt", "R", %FilePath_Relative).GetString(), "newfile.txt", False);
{% endhighlight %}
