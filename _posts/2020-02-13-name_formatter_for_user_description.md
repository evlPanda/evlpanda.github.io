---
layout: post
title:  "A Name Formatter for User Description Field"
date:   2020-02-13 01:23:45 +1000
categories: PeopleCode
blurb: Example of Name Formatting.

---
Example of formatting names, where we have a list of first names (sometimes many), a last name (sometimes long), and a short field to fit it all into.

Update: repository for formatting names here: [NameFormatter](https://github.com/evlPanda/NameFormatter)

In this example we have a list of first names like "John Paul Ringo George" and a last name like "Moukhametzakirova", to fit into a 30 character field. This Class just formats it nicely.

Nothing exciting, just a nice example of using Arrays. Might use again one day.


**MN_CREATE_USER:Utils:Formatter**

```
class Formatter
   method formatUserDescription(&firstNames_ As string, &lastName As string) Returns string;
private
   method fullName(&Names As array of string, &lastName As string) Returns string;
end-class;

method formatUserDescription
   /+ &firstNames_ as String, +/
   /+ &lastName as String +/
   /+ Returns String +/
   Local array of string &FirstNames = Split(&firstNames_, " ");
   Local integer &userDescriptionLength = 30;
   Local integer &i;
   For &i = &FirstNames.Len To 1 Step - 1
      If Len(%This.fullName(&FirstNames, &lastName)) <= &userDescriptionLength Then
         Return %This.fullName(&FirstNames, &lastName);
      End-If;
      &FirstNames.Pop();
   End-For;
   Return Left(&lastName, &userDescriptionLength);
end-method;


/* private */

method fullName
   /+ &Names as Array of String, +/
   /+ &lastName as String +/
   /+ Returns String +/
   Return &Names.Join(" ", "", "") | " " | &lastName;
end-method;
```

Bonus: The unit test.

```
import MN_UNIT_TEST:Base;
import MN_CREATE_USER:Utils:Formatter;

class test_Utils_Formatter extends MN_UNIT_TEST:Base
   method test_Utils_Formatter();
   method run();
private
   Constant &longestFirstName = "Adolph Blaine Charles David Earl Frederick Gerald Hubert Irvin John Kenneth Lloyd Martin Nero Oliver Paul Quincy Randolph Sherman Thomas Uncas Victor William Xerxes Yancy";
   Constant &longestLastName = "Wolfeschlegelsteinhausenbergerdorff";
   instance MN_CREATE_USER:Utils:Formatter &_Formatter;
   method test_formatUserDescription_lengthOK();
   method test_formatUserDescription_firstNamesTooLong();
   method test_formatUserDescription_lastNameTooLong();
   method test_formatUserDescription_nameIsExactly30();
   method test_formatUserDescription_nameIsExactly29();
   method test_formatUserDescription_nameIsExactly31();
end-class;

method test_Utils_Formatter
   %Super = create MN_UNIT_TEST:Base("MN_CREATE_USER:Utils:Formatter");
end-method;

method run
   /+ Extends/implements TTS_UNITTEST:TestBase.Run +/
   %This.test_formatUserDescription_lengthOK();
   %This.test_formatUserDescription_firstNamesTooLong();
   %This.test_formatUserDescription_lastNameTooLong();
   %This.test_formatUserDescription_nameIsExactly30();
   %This.test_formatUserDescription_nameIsExactly29();
   %This.test_formatUserDescription_nameIsExactly31();
   %This.passClass();
end-method;


/* private */

method test_formatUserDescription_lengthOK
   &_Formatter = create MN_CREATE_USER:Utils:Formatter();
   %This.description = ".formatUserDescription() User description is already OK.";
   %This.expectedResult = "John Smith";
   %This.actualResult = &_Formatter.formatUserDescription("John", "Smith");
   %This.test();
end-method;

method test_formatUserDescription_firstNamesTooLong
   &_Formatter = create MN_CREATE_USER:Utils:Formatter();
   %This.description = ".formatUserDescription() First names are too long/many.";
   %This.expectedResult = "Adolph Blaine Charles Smith";
   %This.actualResult = &_Formatter.formatUserDescription(&longestFirstName, "Smith");
   %This.test();
end-method;

method test_formatUserDescription_lastNameTooLong
   &_Formatter = create MN_CREATE_USER:Utils:Formatter();
   %This.description = ".formatUserDescription() Last name is too long.";
   %This.expectedResult = Left(&longestLastName, 30);
   %This.actualResult = &_Formatter.formatUserDescription(&longestFirstName, &longestLastName);
   %This.test();
end-method;

method test_formatUserDescription_nameIsExactly30
   &_Formatter = create MN_CREATE_USER:Utils:Formatter();
   %This.description = ".formatUserDescription() Last name is exactly 30 chars.";
   %This.expectedResult = "John Paul Ringo George Smithey";
   %This.actualResult = &_Formatter.formatUserDescription("John Paul Ringo George", "Smithey");
   %This.test();
end-method;

method test_formatUserDescription_nameIsExactly29
   &_Formatter = create MN_CREATE_USER:Utils:Formatter();
   %This.description = ".formatUserDescription() Last name is 29 chars.";
   %This.expectedResult = "John Paul Ringo George Smithe";
   %This.actualResult = &_Formatter.formatUserDescription("John Paul Ringo George", "Smithe");
   %This.test();
end-method;

method test_formatUserDescription_nameIsExactly31
   &_Formatter = create MN_CREATE_USER:Utils:Formatter();
   %This.description = ".formatUserDescription() Last name is 31 chars.";
   %This.expectedResult = "John Paul Ringo Smitheey";
   %This.actualResult = &_Formatter.formatUserDescription("John Paul Ringo George", "Smitheey");
   %This.test();
end-method;
```

