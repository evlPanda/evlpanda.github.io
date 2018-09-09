---
layout: post
title:  "FullCalendar"
date:   2019-09-09 12:34:56 +1000
blurb:  "Implementing the FullCalendar jQuery plugin"
categories: peoplecode
---

[FullCalendar](https://fullcalendar.io) is an open source, jQuery plugin that I
really wanted to use in an application I was building. I had already spent hours
, or was it days, building my own calendar from scratch using HTML and CSS.
I guess my Google-force must have been week when I started because I didn't come
across FullCalendar. Nonetheless I learnt a lot making my own, and had it looking
really good in a weekly agenda view.

FullCalendar does all this and more, of course, so I have written a wrapper that
allows you to easily create and use calendars.

If you find this useful drop me a note : )

[I've put it on github](https://github.com/evlPanda/PeopleSoftFullCalendar) and
will try to keep it up-to-date. Nonetheless what's on github now should see you
making a calendar very quickly.

{% highlight ruby %}
import YOUR_APP_PACKAGE:CALENDAR:FullCalendar;

local any &Calendar;
local any &Event;

&Calendar = create YOUR_APP_PACKAGE:CALENDAR:FullCalendar();

&Event = &Calendar.NewEvent();
&Event.Title = "Christmas";
&Event.Start = "2018-12-25T09:00";
&Event.End = "2018-12-25T17:00";
&Event.ClassName = "red";
&Calendar.Events.Push(&Event);

&Event = &Calendar.NewEvent();
&Event.Title = "Boxing Day";
&Event.Start = "2018-12-25T09:00";
&Event.End = "2018-12-25T17:00";
&Event.ClassName = "orange";
&Calendar.Events.Push(&Event);

PS_SOME_RECORD.HTMLAREA.Value = &Calendar.Draw();
{% endhighlight %}
