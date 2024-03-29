---
layout: post
title: "Getting Run Control and Operator ID in an Application Engine"
date: 2024-02-01 12:34:56 +1000
categories: AppEngine
blurb: I always forget how to do this.
---

1. Add a State Record not unlike TL_RUNCNTL_AET to the AppEngine.
2. Add a SQL Step:

```
%SelectInit(OPRID, RUN_CNTL_ID)
SELECT OPRID, RUN_CNTL_ID
FROM PS_AERUNCONTROL
WHERE PROCESS_INSTANCE = %Bind(PROCESS_INSTANCE)
```
From here you can select more than just Operator ID and Run control from the PS_AERUNCONTROL Record.

Alternately, a much simpler version will work even when running on the client, for basic information:

```
%SelectInit(OPRID, RUN_CNTL_ID)
SELECT %OperatorID, %RunControl
FROM DUAL
```
