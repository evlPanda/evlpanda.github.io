---
layout: post
title:  "How to Create Sibling Records in a Stand-alone Rowset"
date:   2018-06-01 12:34:56 +1000
categories: PeopleCode, Rowset
blurb: This is not often used, and rather difficult to find any information on.
---

This is not often used, and rather difficult to find any information on.

Of course it *is* right there in PeopleBooks, of you read carefully.

But here is a copy-paste example:

```&MyRowset = CreateRowset(Record.PARENT, Field.CONTROL_FIELD, Record.PARENT_SIBLING, CreateRowset(Record.CHILD, Field.CONTROL_FIELD, Record.CHILD_SIBLING));```

Note that the 2nd record listed there is supposed to be a Related Display Record, and ```Field.CONTROL_FIELD``` its control field. I've found you can create sibling Records like the above, and just use any field for the ``CONTROL_FIELD```.
