---
layout: post
title:  "PeopleTools Colour Scheme"
date:   3019-07-09 12:34:56 +1000
blurb:  "Stylise your IDE"
categories: editor

---

Instructions and background on the Solarised colour scheme.

[Solarized](https://ethanschoonover.com/solarized/) is one of the most loved IDE colour themes ever. *Wired* goes into it better than I can [in this article](https://www.wired.com/story/very-mathematical-history-perfect-color-combination/).

There is now support for PeopleTool's Application Designer, of course, but you can use the configuration I manually setup myself by updating your Application Designer configuration. Note that Application Designer doesn't really have a very "rich" option of colours, and you wont' really get more than 3 or so at a time. But still, I've been using this for years now and can't go back.

Export your PeopleTools configuration file. Open it and edit the ``PSIDE`` values below:

```
[PSIDE]
...
FontFace=REG_SZ=Consolas
FontPoints=REG_DWORD=9
...
CustomColors8=REG_SZ=3549952,7695960,9868419,14018798,35253,3093212,12874092,10002730
CustomColors16=REG_SZ=4339207,8616805,10592659,14939901,1461195,8533715,13798182,39301
PeoplecodeText=REG_SZ=Text,8616805,14939901,0,0
PeoplecodeTextSelection=REG_SZ=Text Selection,14939901,10592659,0,0
PeoplecodeNumber=REG_SZ=Number,3093212,14939901,0,1
PeoplecodeOperator=REG_SZ=Operator,8616805,14939901,1,1
PeoplecodeComment=REG_SZ=Comment,9868419,14939901,0,1
PeoplecodeQuotedString=REG_SZ=QuotedString,10002730,14939901,0,1
PeoplecodeKeyword=REG_SZ=Keyword,13798182,14939901,0,1
PeoplecodeClassDefn=REG_SZ=ClassDefn,8533715,14939901,0,1
PeoplecodeBuiltin=REG_SZ=Builtin,8533715,14939901,0,1
```

Now import the above, solarised, configuration file.
