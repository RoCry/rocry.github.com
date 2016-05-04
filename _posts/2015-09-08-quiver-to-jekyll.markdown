---
layout: post
title: Quiver to Jekyll
category: Lifehacks
tags: jekyll, lifehacks, quiver
---

[Quiver](http://happenapps.com/)

Quiver is a notebook built for programmers. It lets you easily mix text, code and Markdown within one note, edit code with an awesome code editor, live preview Markdown and LaTeX cells, and find any note instantly via the full-text search.



* * *


# Quiver Data Format


{% highlight json %}
{
  "title": "02 - Cells",
  "cells": [
    {
      "type": "text",
      "data": "For example, this is a <b>text cell</b> with <i>some <u>formatting</u> applied</i>."
    },
    {
      "type": "text",
      "data": "<img src=\"/assets/quiver_export/8524433B-4ACF-42CB-B977-76D9EB701ADA.jpg\">"
    },
    {
      "type": "code",
      "language": "javascript",
      "data": "void hello()\n{\n    console.log(\"Hello World!\");\n}"
    },
    {
      "type": "markdown",
      "data": "## Markdown Cell\n\nThis is a markdown cell. You can use common markdown syntax here.\n\nBasic formatting of *italic* and **bold** is supported.\n\nSo is `inline code`.\n\nAnd lists.\n\n### Ordered list\n\n1. Item 1\n2. A second item\n3. Number 3\n4. Ⅳ\n\n### Unordered list\n\n* An item\n* Another item\n* Yet another item\n* And there's more...\n\n### Quote\n\n> Here is a quote.\n\nCustom CSS options can be set in the *Preferences* panel.\n"
    }
  ]
}
{% endhighlight %}

As you see, most of the cells are pretty straightforward, except the cell about image.

So what I need to do is copy the source of images to where my jekyll assets are, “~/Desktop/blog/assets” for example. Then I convert the html syntax in text cell to markdown, replace the source url to right one. Done!


What I have done is add text cell parsing for this script:&nbsp;[https://github.com/bradley-curran/quiver2jekyll](https://github.com/bradley-curran/quiver2jekyll)&nbsp;



_You need to do it yourself, because I don’t have time to make the script more generally..._



Enjoy~



