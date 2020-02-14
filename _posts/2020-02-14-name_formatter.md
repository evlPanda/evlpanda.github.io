---
layout: post
title:  "Formatting Names using SQL"
date:   2020-02-13 01:23:45 +1000
categories: PeopleCode SQL
blurb: Formatting names using SQL.

---
You've got a list of names and they are all in upper or lower case. How do you format "MCDONALD" to "McDonald"? What about all those Middle Eastern, Asian names?

This is kinda sneaky. I like it. It simply uses data in ```ps_names``` to return the most often used formatting of a given name. 

```
with formatted_name as(
	select distinct last_name, count(*) as count
	from ps_names
	where upper(last_name) = 'MCDONALD'
	group by last_name)
select min(last_name)
from formatted_name
where count =
	(select max(count) from formatted_name)
```
