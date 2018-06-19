---
layout: post
title: Stack Edit Post
author: Michael Nitschke
tags: Editors
categories: Editors
date: 2018-06-19 12:34:56 +1000

---


**markdown** |  ˈmɑːkdəʊn  |
noun
a reduction in price: *markdowns of up to 50 per cent  on  many items.*

I've started using a combination of [StackEdit](https://stackedit.io/), [Jekyll](https://jekyllrb.com), and [GitHub Pages](https://pages.github.com) to write this blog.

The primary reason is I much prefer using [markdown](https://en.wikipedia.org/wiki/Markdown) to write documents. It does everything I need and nothing more.

GitHub pages offers free hosting, and it works with Jekyll. Magic. :)

**Test code:**

{% highlight ruby %}
For &i = 1 to &something;
    %This.DoSomething();
    %This.DoSomethingElse();
End-For;
{% endhighlight %}

**Test code II:**
```Java
For &i = 1 to &something;
    %This.DoSomething();
    %This.DoSomethingElse();
End-For;
```

Inline ```&code.Highlight()```

What are the chances of...

```mermaid
graph TD
A --> B
```

I wonder if ==highlighting== works.

# Takeaways

## Code Highlighting

Only code highlighting like this works, and it requires spaces for initial indent (return keeps the indent).

```
{% highlight ruby %}
[code]
{% endhighlight %}
```
## Template
I've added this template in StackEdit to use instead of the provided Jekyll, html etc templates.:
```
---  
layout: post  
{{{files.0.content.yamlProperties}}}  
---  

{{{files.0.content.text}}}
```
## Publishing
Once I've published it as *exactly* this format, and to given directory, it's all golden. I can edit and 'Publish Now' to update the file.

    _posts/YYYY-MM-DD-Title-Name.markdown

## Other

- Unfortunately can't use UML. Which is a shame because that is just awesome in stacked. I guess I can screenshot the results from there.
- Have to use the longer ```{% highlight ruby %}``` tag. I'd prefer just ```{% code ruby %}``` or something similar and easier to remember, and more descriptive.


> Written with [StackEdit](https://stackedit.io/).
