---
layout: post
title:  "PeopleCode to Default Effective Sequence"
date:   2020-03-11 01:23:45 +1000
categories: PeopleCode
blurb: Copy Pasta.

---
Copy Pasta.

EFFSEQ.FieldDefault()

```
GetField().Value = 1;
Local integer &i;
Local Rowset &Rowset = GetRowset();
For &i = 1 To &Rowset.ActiveRowCount
   If &i <> CurrentRowNumber() And
         &Rowset(&i).GetRecord(1).EFFDT.Value = GetRecord().EFFDT.Value And
         &Rowset(&i).GetRecord(1).EFFSEQ.Value >= GetField().Value Then
      GetField().Value = &Rowset(&i).GetRecord(1).EFFSEQ.Value + 1;
   End-If;
End-For;
```

EFFDT.FieldChange()

```
GetRecord().EFFSEQ.SetDefault();
```
