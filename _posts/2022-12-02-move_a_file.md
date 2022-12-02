---
layout: post
title:  "PeopleCode method to move a file"
date:   2022-12-02 12:34:56 +1000
categories: PeopleCode, Files
blurb: PeopleCode method to move a file using Java
---

PeopleCode method to move a file using Java

```
class Utilities
   method move(&fromFilePath As string, &fromPathType As integer, &toFilePath As string, &toPathType As integer)
end-class;

/* Use %FilePath_Relative or %FilePath_Absolute for &fromPathType and &toPathType */
method move
   /+ &fromFilePath as String, +/
   /+ &fromPathType as Integer, +/
   /+ &toFilePath as String, +/
   /+ &toPathType as Integer +/
   If &fromFilePath = "" Or
         &toFilePath = "" Then
      throw CreateException(0, 0, "&fromFilePath and &toFilePath are required.")
   End-If;
   If Not FileExists(&fromFilePath, %fromPathType) Then
      throw CreateException(0, 0, "&fromFilePath does not exist: " | &fromFilePath)
   End-If;
   Local JavaObject &Source, &Target, &Files, &Options, &StandardCopyOptions;
   &Files = GetJavaClass("java.nio.file.Files");
   &Source = CreateJavaObject("java.io.File", &fromFilePath).toPath();
   &Target = CreateJavaObject("java.io.File", &toFilePath).toPath();
   &Options = CreateJavaObject("java.nio.file.CopyOption[]");
   If FileExists(&toFilePath, %toPathType) Then
      &StandardCopyOptions = GetJavaClass("java.nio.file.StandardCopyOption");
      &Options = CreateJavaObject("java.nio.file.CopyOption[]", &StandardCopyOptions.REPLACE_EXISTING);
   End-If;
   &Files.move(&Source, &Target, &Options);
end-method;
```
