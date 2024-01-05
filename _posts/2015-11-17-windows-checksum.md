---
id: 465
title: 'Windows Checksum'
date: '2015-11-17T12:01:45+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=465'
permalink: /windows-checksum/
thrive_post_fonts:
    - '[]'
categories:
    - Uncategorized
---

I’m using checksums more to confirm I’m deploying the correct files from nexus

```
CertUtil -hashfile myfile.txt MD5
MD5 hash of file myfile.txt:
36 1c 16 f6 5e a6 da b3 2c 10 b6 5b cc 51 f7 2f
CertUtil: -hashfile command completed successfully.
```