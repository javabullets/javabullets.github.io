---
id: 668
title: 'Simple Apache Performance Improvements'
date: '2016-07-28T10:33:50+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=668'
permalink: /simple-apache-performance-improvements/
thrive_post_fonts:
    - '[]'
categories:
    - Uncategorized
---

This post to collect together a number of simple performance improvements using apache. It demonstrates using mod\_expires, mod\_deflate, mod\_cache and mod\_headers.

## .htaccess or httpd.conf

httpd.conf provides the core apache configuration, while .htaccess provides a directory specific configuration. The preference is to use httpd.conf where possible, because using htaccess results in a search for htaccess on every sub-directory of a request, for each request. Depending on your hosting this decision may be out of your hands.

## mod\_deflate

mod\_deflate tells apache to compress responses from your application server. The options are –

- AddOutputFilterByType – To use this option you must enable mod\_filter –  
    LoadModule filter\_module modules/mod\_filter.so
- AddOutputFilterByType DEFLATE text/html text/xml text/css text/plain
- Extension –

```
<filesMatch "\.(js|html)$">
 SetOutputFilter DEFLATE
</filesMatch>
```

If you use AddOutputFilterByType then you need to make sure you have the mime-types defined

Other directives –

DeflateCompressionLevel – 1 to 9 – the higher the value, the greater compression but the cost is higher CPU

You can also enable mod\_deflate logging to see the compression ratios on your files.

Putting it together –

```
LoadModule filter_module modules/mod_filter.so
LoadModule deflate_module modules/mod_deflate.so

<IfModule mod_deflate.c>
# List of mime types - 
AddOutputFilterByType DEFLATE text/html text/xml text/css text/plain
AddOutputFilterByType DEFLATE image/svg+xml application/xhtml+xml application/xml
AddOutputFilterByType DEFLATE application/rdf+xml application/rss+xml application/atom+xml
AddOutputFilterByType DEFLATE text/javascript application/javascript application/x-javascript application/json
AddOutputFilterByType DEFLATE application/x-font-ttf application/x-font-otf
AddOutputFilterByType DEFLATE font/truetype font/opentype

# May tune as a result of load testing
DeflateCompressionLevel 9

# Browser Specific rules -
BrowserMatch ^Mozilla/4 gzip-only-text/html
BrowserMatch ^Mozilla/4\.0[678] no-gzip
BrowserMatch \bMSIE !no-gzip !gzip-only-text/html
BrowserMatch \bOpera !no-gzip
</IfModule>
```

## mod\_expires

This module sets the Expires HTTP header and max-age of Cache-Control in HTTP header.

The format is –

```
ExpiresByType mime-type "access plus y years m months d days h hours"
```

A typical configuration would be –

```
LoadModule expires_module modules/mod_expires.so

<IfModule mod_expires.c>
ExpiresActive on
ExpiresDefault "access plus 30 days"

ExpiresByType image/jpg "access plus 30 days"
ExpiresByType image/png "access plus 30 days"
ExpiresByType image/gif "access plus 30 days"
ExpiresByType image/jpeg "access plus 30 days"

ExpiresByType text/css "access plus 1 days"

ExpiresByType image/x-icon "access plus 30 days"

ExpiresByType application/pdf "access plus 30 days"
ExpiresByType audio/x-wav "access plus 30 days"
ExpiresByType audio/mpeg "access plus 30 days"
ExpiresByType video/mpeg "access plus 30 days"
ExpiresByType video/mp4 "access plus 30 days"
ExpiresByType video/quicktime "access plus 30 days"
ExpiresByType video/x-ms-wmv "access plus 30 days"
ExpiresByType application/x-shockwave-flash "access 30 days"

ExpiresByType text/javascript "access plus 30 days"
ExpiresByType application/x-javascript "access plus 30 days"
ExpiresByType application/javascript "access plus 30 days"
</IfModule>
```

## mod\_headers

This plugin customises HTTP request and response headers, and we can use it to set the max-age on Cache-Control –

```
LoadModule headers_module modules/mod_headers.so

<ifModule mod_headers.c>
<filesMatch "\.(ico|jpe?g|png|gif|swf)$">
Header set Cache-Control "max-age=2592000, public"
</filesMatch>
<filesMatch "\.(css)$">
Header set Cache-Control "max-age=604800, public"
</filesMatch>
<filesMatch "\.(js)$">
Header set Cache-Control "max-age=216000, private"
</filesMatch>
<filesMatch "\.(x?html?|php)$">
Header set Cache-Control "max-age=600, private, must-revalidate"
</filesMatch>
</ifModule>

<ifModule mod_headers.c>
Header unset Last-Modified
</ifModule>
```

## ETag

An ETag is a unique ID for a resource and is configured per server. The problem is that in some clustered environments you can end up caching the same resource due to them having different ETags. A decision on disabling ETag’s should be made on an environment basis, as disabling them forces the browser to rely on Cache-Control and Expires headers.

The syntax to disable is –

```
LoadModule headers_module modules/mod_headers.so

FileETag None
<ifModule mod_headers.c>
Header unset ETag
</ifModule>
```

## References

- [htaccess](http://httpd.apache.org/docs/current/howto/htaccess.html)
- [mod\_deflate](http://httpd.apache.org/docs/current/mod/mod_deflate.html)
- [mod\_expires](http://httpd.apache.org/docs/current/mod/mod_expires.html)
- [mod\_headers](http://httpd.apache.org/docs/current/mod/mod_headers.html)