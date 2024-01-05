---
id: 591
title: 'Useful WinMerge Filter'
date: '2016-05-25T11:40:01+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=591'
permalink: /useful-winmerge-filter/
thrive_post_fonts:
    - '[]'
categories:
    - BuildTools
    - Uncategorized
tags:
    - winmerge
---

I was doing a comparison between two directories in winmerge, and it was getting clouded by “noise” from svn, class and jars.

The basic pattern matching is –

f: – match files

d: – match directories

Example –

```
## Ignore Java class and jar files 
f: \.class$ 
f: \.jar$ 

## Ignore subversion housekeeping folders 
d: \\.svn$ 
d: \\._svn$ 

```

Best Subversion Filter –

```

## Ignore Java class and jar files 
f: \.class$ 
f: \.jar$ 
f: \\.class$
f: \\.$
f: \\org.eclipse.$


## Ignore subversion housekeeping folders 
d: \\.svn$ 
d: \\._svn$ 
d: \\_svn$ ## SVN working copy ASP.NET 
d: \\cvs$ ## CVS control directory 
d: \\.git$ ## Git directory 
d: \\.bzr$ ## Bazaar branch 
d: \\.hg$ ## Mercurial repository
d: \\target
d: \\htdocs
d: \\logs
d: \\manual
d: \\cgi-bin
d: \\.$
d: \\org.eclipse.$
d: \\.apt_generated
d: \\.settings

```