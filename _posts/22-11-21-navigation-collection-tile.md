---
layout: post
title:  "Creating a New Navigation Collection as a New Homepage Tile"
date:   2022-11-21 12:34:56 +1000
categories: Navigation
blurb: How to create a new navigation collection, and add it to a Homepage as a Tile.

---

# Create a Navigation Collection

   PeopleTools > Portal > Structure & Content
   Portal Objects/Navigation Collections

Add a Folder, then add Content References inside the Folder. 
These will appear as the left-hand-menu items.

# Create a Tile for the Navigation Collection

   PeopleTools > Portal > Structure & Content
   Fluid Structure Content/Fluid Pages

Create a Content Reference
URL Type = 	PeopleSoft Generic URL
Portal URL = 	c/NUI_FRAMEWORK.PT_AGSTARTPAGE_NUI.GBL
		?CONTEXTIDPARAMS=TEMPLATE_ID:PTPPNAVCOL
		&scname=YOUR_NAVIGATION_COLLECTIONS_NAME
		&PanelCollapsible=Y
		&PTPPB_GROUPLET_ID=GP_PROCESSING 	(not sure?)
		&CRefName=THIS_CONTENT_REFERENCE_NAME 	(to return to?)

# Add to HomePage

   PeopleTools > Portal > Structure & Content
   Fluid Structure Content/Fluid Homepages

Add your content ref/tile on the 'Tile Content' tab.

# Run Portal Security Sync

   PeopleTools > Portal > Portal Security Sync

# Add to Application Designer Project

   All the above are Portal Objects; you can add them by name into your Project and migrate.
   Be sure to run Portal Security Sync once again post migration.

	
