---
layout: post
title:  "Using IN() in SQL"
date:  2018-08-20 12:34:56 +1000
categories: [PeopleCode, SQL]
blurb: We can't combine bind variables and the "IN" clause, but there is another way.
---

We can't combine bind variables and the "IN" clause, but there is another way.

You can’t do this:

{% highlight sql %}
select 1
from psroleuser
where roleuser = :1
and rolename in (:2)
[% endhighlight %}

```rolename in (:2)``` just doesn't work : (

However, you can do this ...rather more complex method:

{% highlight sql %}
select 1
from psroleuser
where roleuser = :1
and rolename in (
   select regexp_substr(:2, '[^,]+', 1, level) token
   from dual
   connect by level <= length(:2) - length(replace(:2, ',', '')) + 1
)
[% endhighlight %}
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTg5MDE5MTQwMiwxNzI4ODk4MzY3XX0=
-->