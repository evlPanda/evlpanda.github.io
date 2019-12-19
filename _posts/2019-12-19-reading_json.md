---
layout: post
title:  "Reading JSON with PeopleCode"
date:   2019-12-19 12:34:56 +1000
categories: [PeopleCode, JSON]
blurb: This example reads JSON into an array of a complex data type.
---

Building on the example at [Cedar Hills](https://www.cedarhillsgroup.com/knowledge-base/kbarticles/JSON-parsing-using-peopleTools/) 
I refactored it into a complex data type of ```Animal```, contained in an ```Array of Animal```:

```
class Animal
   property string name;
   property string species;
   property boolean canBark;
   property string likesFoods;
   property string dislikesFoods;
end-class;
```

Example use:

```
Local JSON_EXAMPLE:Animals &Animals;
Local string &exampleJSON = GetHTMLText(HTML.EXAMPLE_JSON);
&Animals = create JSON_EXAMPLE:Animals(&exampleJSON);

Local any &Animal = &Animals.getByName("Barky");
Messagebox(0,"",0,0,"Barky is a " | &Animal.species);
Messagebox(0,"",0,0,"Barky likes to eat " | &Animal.likesFoods);
```

[Code is here at GitHub](https://github.com/evlPanda/JSON-Example)
