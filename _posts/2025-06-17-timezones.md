---
layout: post
title: "SQL for Time Zones"
date: 2025-06-17 00:00:00 +1000
categories: SQL
blurb: Avoid timezone hell with this SQL
---

```
SELECT 
   CAST('2025-07-01 17:00' as datetime
   AT TIME ZONE 'Eastern Standard Time'
   AT TIME ZONE 'AUS Eastern Standard Time'
```

This will return 1st of July 2025, 5pm in New York in Sydney time; 2025-07-02 07:00.

It considers daylight savings time and all.
