---
layout: post
title:  "The Correct Way to Delete PeopleSoft Fluid Forms"
date:   2021-01-11 12:34:56 +1000
categories: Fluid Forms
blurb: There is no easy way to delete Fluid Forms.

---
I've had immense difficulty tidying up and migrating Fluid Forms. This post is a living document on how to delete unwanted Forms, migrate Forms, and tidy up orphaned rows.

Below is the SQL you can use to delete/migrate Fluid Form configuration. Note that there is some PeopleCode that needs to be temporarily disabled post running the SQL/DMS too!

## 1. The SQL/DMS

```
DELETE FROM PS_FORM_TYPE WHERE FORM_TYPE in ('FORMNAME');
DELETE FROM PS_FORM_TYPE_LG WHERE FORM_TYPE in ('FORMNAME');
DELETE FROM PS_FORM_TYPE_ATTCH WHERE FORM_TYPE in ('FORMNAME');
DELETE FROM PS_FORM_TYPE_AT_LG WHERE FORM_TYPE in ('FORMNAME');

DELETE PS_EOFM_FORMROLE WHERE FORM_TYPE in ('FORMNAME');
DELETE PS_EOFM_TILE_IMG WHERE FORM_TYPE in ('FORMNAME');

DELETE FROM PS_FS_SD_REC WHERE SD_RECNAME in ('FORMNAME');
DELETE FROM PS_FS_SD_REC_LG WHERE SD_RECNAME in ('FORMNAME');
DELETE FROM PS_FS_SD_RECFLD WHERE SD_RECNAME in ('FORMNAME');
DELETE FROM PS_FS_SD_RECFLD_LG WHERE SD_RECNAME in ('FORMNAME');
DELETE FROM PS_FS_SD_FLDCD WHERE SD_RECNAME in ('FORMNAME');
DELETE FROM PS_FS_SD_FLDCD_LG WHERE SD_RECNAME in ('FORMNAME');
DELETE FROM PS_FS_SD_FLDPC WHERE SD_RECNAME in ('FORMNAME');

DELETE FROM PS_EOCF_DE_NATIVE WHERE EOCF_NATIVE_CODE LIKE '%FORMNAME%' ;
DELETE PS_EOCF_PHRASE_DFN WHERE EOCF_PHRASE_ID IN (SELECT B.EOCF_PHRASE_ID FROM PS_FS_SD_PGGRP B WHERE B.PNLGRPNAME = 'FORM_ADD' AND B.SD_GRP_NAME in ('FORMNAME') );

DELETE FROM PS_FS_SD_GRP WHERE SD_GRP_NAME in ('FORMNAME');		
DELETE FROM PS_FS_SD_GRP_LG WHERE SD_GRP_NAME in ('FORMNAME');
DELETE FROM PS_FS_SD_GRPFLD WHERE SD_GRP_NAME in ('FORMNAME');
DELETE FROM PS_FS_SD_GRPFLD_LG WHERE SD_GRP_NAME in ('FORMNAME');
DELETE FROM PS_FS_SD_PGGRP WHERE PNLGRPNAME = 'FORM_ADD' AND SD_GRP_NAME in ('FORMNAME');

-- AWE: 
DELETE FROM PS_EOAW_PATH where EOAWPRCS_ID = 'EOFM' and EOAWDEFN_ID in ('FORMNAME');
DELETE FROM PS_EOAW_PATH_LNG where EOAWPRCS_ID = 'EOFM' and EOAWDEFN_ID in ('FORMNAME');
DELETE FROM PS_EOAW_PRCS where EOAWPRCS_ID = 'EOFM' and EOAWDEFN_ID in ('FORMNAME');
DELETE FROM PS_EOAW_PRCS_LNG where EOAWPRCS_ID = 'EOFM' and EOAWDEFN_ID in ('FORMNAME');
DELETE FROM PS_EOAW_STAGE where EOAWPRCS_ID = 'EOFM' and EOAWDEFN_ID in ('FORMNAME');
DELETE FROM PS_EOAW_STEP where EOAWPRCS_ID = 'EOFM' and EOAWDEFN_ID in ('FORMNAME');
DELETE FROM PS_EOAW_STEP_LNG where EOAWPRCS_ID = 'EOFM' and EOAWDEFN_ID in ('FORMNAME');
DELETE FROM PS_EOAW_STG_LNG where EOAWPRCS_ID = 'EOFM' and EOAWDEFN_ID in ('FORMNAME');
DELETE FROM PS_EOAW_TIMEOUT where EOAWPRCS_ID = 'EOFM' and EOAWDEFN_ID in ('FORMNAME');
DELETE FROM PS_EOAW_TIMEOUTDEF where EOAWPRCS_ID = 'EOFM' and EOAWDEFN_ID in ('FORMNAME');

DELETE FROM PS_EOAWCRTA where EOAWCRTA_ID like '%EOFM%FORMNAME%';  
DELETE FROM PS_EOAWCRTA_LNG where EOAWCRTA_ID like '%EOFM%FORMNAME%';
DELETE FROM PS_EOAWCRTA_REC where EOAWCRTA_ID like '%EOFM%FORMNAME%';
DELETE FROM PS_EOAWCRTA_RECLNG where EOAWCRTA_ID like '%EOFM%FORMNAME%';
DELETE FROM PS_EOAWCRTA_VAL where EOAWCRTA_ID like '%EOFM%FORMNAME%';

-- If you save/copy this as a DMS, this is where your import would be.
-- IMPORT *;

-- Clean up any and all orphaned phrases.
DELETE A 
FROM PS_EOCF_PHRASE_DFN A 
WHERE EOCF_LASTUPDOPRID NOT LIKE 'PS%'
AND EXISTS	
	(SELECT 1
	FROM PS_FS_SD_PGGRP B
	WHERE B.PNLGRPNAME = 'FORM_ADD' AND B.MARKET = 'GBL' AND B.PNLNAME ='FORM'
	AND B.EOCF_PHRASE_ID = A.EOCF_PHRASE_ID)
AND NOT EXISTS 
	(SELECT 1 
	FROM PS_EOCF_DE_NATIVE C
	WHERE C.EOCF_CODE_PTR = A.EOCF_CODE_PTR)
;

DELETE A 
FROM PS_FS_SD_PGGRP A
WHERE A.PNLGRPNAME = 'FORM_ADD' 
AND A.MARKET = 'GBL' 
AND A.PNLNAME ='FORM'
AND NOT EXISTS
	(SELECT 1
	FROM PS_EOCF_PHRASE_DFN B
	WHERE B.EOCF_PHRASE_ID = A.EOCF_PHRASE_ID)
;

```

## 2. The PeopleCode

You'll also need to clear some Rowset cache. In the following AppClass method temporarily comment out the If statement so that the cache gets created again.

You can trigger it by filling out any new form in the 'My Forms' tile/component.

**FS_SD:Runtime:Utilities:SDCachedMetaDataReader._getComponentRowset()**
```
method _getComponentRowset
   /+ Returns Rowset +/
   ...
   
   &cache = GetRowsetCache(&strCacheName);
   &rowset = &cache.Get();
   /*   If &rowset = Null Then */
   
   ...recreates cache
   
   /*  End-If; */
   
   Return &rowset;
end-method;   
```

After you've filled in a new form you can revert the PeopleCode back to delivered (optional in development, really).

