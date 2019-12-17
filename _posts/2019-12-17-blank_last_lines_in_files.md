---
layout: post
title:  "Blank Lines at end of Files"
date:   2019-12-17 13:50:56 +1000
categories: [PeopleCode, Files]
blurb: By default, it seems, files created in PeopleCode have an extra blank line at the end.
---

By default, it seems, files created in PeopleCode have an extra blank line at the end.
This can be corrected by setting .SetRecTerminator() to carriage return (char 13).

{% highlight java %}
   Local File &File;
   &File = GetFile("newfile.txt", "W", "A", %FilePath_Relative);
   &File.SetRecTerminator(Char(13));
   &File.WriteLine("hello");
   &File.Close();
{% endhighlight %}

By the way there is an easy way to [download files you are working with](https://evlpanda.github.io/peoplecode/files/2019/06/26/download_file_from_server.html).
