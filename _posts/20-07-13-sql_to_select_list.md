---
layout: post
title:  "Aggregating rows into a list. Oracle SQL."
date:   2020-07-12 12:34:56 +1000
categories: SQL
blurb: How to return rows as a list.

---

```
select listagg(field_name, ', ') within group (order by field_name)
from ps_some_table
```
