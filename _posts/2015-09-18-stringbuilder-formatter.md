---
id: 458
title: 'StringBuilder Formatter'
date: '2015-09-18T07:59:34+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=458'
permalink: /stringbuilder-formatter/
thrive_post_fonts:
    - '[]'
categories:
    - Uncategorized
---

I was looking at some StringBuilder code and it was a series of appends – so I thought it was worth stream lining it with StringBuilder and Formatter –

> String myFirstString = “my First String”;
> 
> String mySecondString = “my Second String”;
> 
> StringBuilder sb = new StringBuilder();
> 
> sb.append(“Populate my first String “);
> 
> sb.append(myFirstString);
> 
> sb.append(” then add my second String “);
> 
> sb.append(mySecondString);

Output –

> “Populate my first String my First String then add my second String my second String”

Or using a String Formatter –

> String myFirstString = “my First String”;
> 
> String mySecondString = “my Second String”;
> 
> StringBuilder sb = new StringBuilder();
> 
> // bind formatter to StringBuilder
> 
> Formatter formatter = new Formatter(sb, Locale.UK);
> 
> formatter.format(“Populate my first String %s then add my second String %s”, myFirstString, mySecondString);

Output –

> “Populate my first String my First String then add my second String my second String”