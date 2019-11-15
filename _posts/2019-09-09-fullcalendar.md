---
layout: post
title:  "Implementing Full Calendar in PeopleSoft"
date:   2019-09-09 12:34:56 +1000
blurb:  "Implementing FullCalendar"
categories: PeopleCode
blurb: FullCalendar is an open source jQuery plugin that I really wanted to use in an application I was building. I ended up creating a wrapper for it to be used in PeopleSoft.
---

[FullCalendar](https://fullcalendar.io) is an open source jQuery plugin that I
really wanted to use in an application I was building. I ended up creating a wrapper
for it to be used in PeopleSoft.

I had already spent hours, or was it days, building my own calendar from scratch using HTML and CSS.
I guess my Google-force must have been weak when I started because I didn't come
across FullCalendar. Nonetheless I learnt a lot making my own, and had it looking
really good in a weekly agenda view.

FullCalendar does all this and more, of course, so I have written a wrapper that
allows you to easily create and use calendars in PeopleSoft.

If you find this useful drop me a note : )

[Clone it from github](https://github.com/evlPanda/PeopleSoftFullCalendar).

Example use:

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

There are some more examples in the [:Subclasses](https://github.com/evlPanda/PeopleSoftFullCalendar/tree/master/Subclasses) Package.
The ```InstructorSchedule``` class will work out-of-the-box with the given instructor.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2NDMxNDQxNzldfQ==
-->