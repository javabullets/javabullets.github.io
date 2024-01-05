---
id: 681
title: 'To c or not to c'
date: '2016-08-10T11:11:07+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=681'
permalink: /to-c-or-not-to-c/
thrive_post_fonts:
    - '[]'
categories:
    - Uncategorized
---

Ive edited httpd.conf files for years now, and hadnt given much thought to why some “IfModule” constructs have a .c and some don’t. But then I was editing a config and it was a mix of .c and non-.c constructs, so it niggled me as to why.

Consider the following –  
\[sourcecode\]LoadModule headers\_module modules/mod\_headers.so\[/sourcecode\]

This is split into two parts –

- Module Identifier – headers\_module – This is the module name. If in doubt google the module and get the name from the documentation
- Source File – mod\_headers.c – This is the file name with a c extension

We can then look at the “IfModule” documentation which gives the syntax –  
\[sourcecode\] &lt;IfModule \[!\]module-file|module-identifier&gt; … &lt;/IfModule&gt;  
\[/sourcecode\]

Putting this together we can use either the module identifier(headers\_module) –

\[sourcecode\] &lt;IfModule mod\_headers.c&gt;  
 &lt;filesMatch "\\.(ico|jpe?g|png|gif|swf)$"&gt;  
 Header set Cache-Control "max-age=2592000, public"  
 &lt;/filesMatch&gt;  
 &lt;filesMatch "\\.(css)$"&gt;  
 Header set Cache-Control "max-age=604800, public"  
 &lt;/filesMatch&gt;  
 &lt;filesMatch "\\.(js)$"&gt;  
 Header set Cache-Control "max-age=216000, private"  
 &lt;/filesMatch&gt;  
&lt;/IfModule&gt;  
\[/sourcecode\] Or the source file(mod\_headers.c) –

\[sourcecode\] &lt;IfModule headers\_module&gt;  
 &lt;filesMatch "\\.(ico|jpe?g|png|gif|swf)$"&gt;  
 Header set Cache-Control "max-age=2592000, public"  
 &lt;/filesMatch&gt;  
 &lt;filesMatch "\\.(css)$"&gt;  
 Header set Cache-Control "max-age=604800, public"  
 &lt;/filesMatch&gt;  
 &lt;filesMatch "\\.(js)$"&gt;  
 Header set Cache-Control "max-age=216000, private"  
 &lt;/filesMatch&gt;  
&lt;/IfModule&gt;  
\[/sourcecode\] ## Which is better?

None are better – the best option is to choose one approach and stick to it. I prefer the module-file name as it is easier to reconcile to my modules directory.

## References

[http://httpd.apache.org/docs/current/mod/mod\_headers.html](http://httpd.apache.org/docs/current/mod/mod_headers.html)

<http://httpd.apache.org/docs/2.4/mod/core.html#ifmodule>