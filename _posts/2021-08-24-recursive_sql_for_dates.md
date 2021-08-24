---
layout: post
title:  "Recursive SQL to Generate a List of Dates"
date:   2021-08-24 01:23:45 +1000
categories: SQL
blurb: Loopy list of dates.

---
This returns all dates for the past year and is an example of a simple SQL recursive CTE or sub-query refactoring, or as I call it a “with statement”.

```
WITH daterange(dt) AS (
   SELECT CURRENT_DATE - 1 YEAR  
   FROM sysibm.sysdummy1  
   UNION ALL
   SELECT dt + 1 DAY  
   FROM daterange
   WHERE dt < CURRENT_DATE
)

select * from daterange;
```
