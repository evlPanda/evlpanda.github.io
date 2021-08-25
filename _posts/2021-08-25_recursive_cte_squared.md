---
layout: post
title:  "Recursive CTE Squared!"
date:   2021-08-24 01:23:46 +1000
categories: SQL
blurb: Nested recursive CTEs.

---
# The Issue

This is a hard one to describe! 

PeopleSoft has PS_SCH_DEFN_DTL (and PS_SCH_DEFN_TBL as its header). This holds the SHIFT_ID and SCHED_HRS for each of the DAYNUM (1-7 typically) in its data.

The problem with this is that the 'From Date' (EFFDT) can be any day of the week. So where DAYNUM = 1 will be whatever the 'From Date' is.

When Time & Labour is calculating everything based on the DUR it can't tell which DAYNUM the DUR is for a given employee.

I created a sort of 'Mapping Table' that essentially takes a Work Schedule's 'From Date' (EFFDT) and a DUR (date) (See how confusing this is?) and it returns the correct DAYNUM from the employee's Work Schedule.

One could write a giant SQL case statement with every possibliity, but that is just plain sloppy, and tedious and huge and not reusable.

But, essentially this 'Mapping Table' is saying something not unlike:

If employeeWorkScheduleStartDate = 1 (Sunday) and DUR = 1 (Sunday) then return 1 (Sunday)
If employeeWorkScheduleStartDate = 1 (Saturday) and DUR = 2 (Saturday) then return 2 (Saturday)
...
If employeeWorkScheduleStartDate = 2 (Saturday) and DUR = 1 (Sunday) then return 7 (Saturday)
If employeeWorkScheduleStartDate = 2 (Saturday) and DUR = 2 (Monday) then return 1 (Sunday)
...
If employeeWorkScheduleStartDate = 3 (Tuesday) and DUR = 2 (Monday) then return 7 (Saturday)
If employeeWorkScheduleStartDate = 3 (Tuesday) and DUR = 3 (Tuesday) then return 1 (Sunday)

etc. etc.

The entire mapping table is generated below, in the SQL Results.

# The Solution

This one was worth recording as it is a recursive CTE from *another* recursive CTE. Tricky. Loopy. Bit of a headache, really.



## SQL

```
WITH SHIFT_DAY_OF_WEEK (SCHED_DAY1, DUR_DAY, SCHED_DAYOFWEEK) AS (  
	SELECT 1, 1, 1  
	UNION ALL  
	SELECT 
	  SCHED_DAY1  
	, DUR_DAY+1  
	, SCHED_DAYOFWEEK+1  
	FROM SHIFT_DAY_OF_WEEK  
	WHERE SCHED_DAYOFWEEK < 7
)
, SHIFT_SCHED_DAY (SCHED_DAY1, DUR_DAY, SCHED_DAYOFWEEK) AS (  
	SELECT *  
	FROM SHIFT_DAY_OF_WEEK  
	UNION ALL  
	SELECT 
	  SCHED_DAY1 + 1  
	, DUR_DAY  
	, CASE WHEN (SCHED_DAYOFWEEK - 1) <= 0 THEN (SCHED_DAYOFWEEK - 1) + 7 ELSE (SCHED_DAYOFWEEK - 1) END  
	FROM SHIFT_SCHED_DAY  
	WHERE SCHED_DAY1 < 7
)  

SELECT * FROM SHIFT_SCHED_DAY ORDER BY 1, 2, 3
;
```

## Example Use

To get the correct row from PS_SCH_DEFN_DTL C you add arguments, almost passing in two parameters, for the Schedules EFFDT and the DUR.

```
...
FROM PS_SCH_DEFN_DTL C
...
AND C.DAYNUM = 
	(SELECT SSD.SCHED_DAYOFWEEK
	FROM SHIFT_SCHED_DAY SSD
	WHERE SSD.SCHED_DAY1 = DATEPART(DW, C.EFFDT)
	AND SSD.DUR_DAY = DATEPART(DW, A.DUR) )
...
```

## SQL Results

```
SCHED_DAY1	DUR_DAY	SCHED_DAYOFWEEK
1				1			1
1				2			2
1				3			3
1				4			4
1				5			5
1				6			6
1				7			7
2				1			7
2				2			1
2				3			2
2				4			3
2				5			4
2				6			5
2				7			6
3				1			6
3				2			7
3				3			1
3				4			2
3				5			3	
3				6			4
3				7			5
4				1			5
4				2			6
4				3			7
4				4			1
4				5			2
4				6			3
4				7			4
5				1			4
5				2			5
5				3			6
5				4			7
5				5			1
5				6			2
5				7			3
6				1			3
6				2			4
6				3			5
6				4			6
6				5			7
6				6			1
6				7			2
7				1			2
7				2			3
7				3			4
7				4			5
7				5			6
7				6			7
7				7			1
```
