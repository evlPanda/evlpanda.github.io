---
layout: post
title:  "Blank Last Lines in Files"
date:   2019-06-26 12:34:56 +1000
categories: [PeopleCode, Files]
blurb: Situation: Files have an extra blank line at the end.
---

Situation: Files have an extra blank line at the end.
This can be corrected by setting .SetRecTerminator() to line feed (char 13).
```
   Local File &File;
   &File = GetFile("newfile.txt", "W", "A", %FilePath_Relative);
   &File.SetRecTerminator(Char(13));
   &File.WriteLine("hello");
   &File.Close();
   ```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTYwNTM2Nzc2Ml19
-->