---
id: 2112
title: 'Apache Log Rotation on Windows'
date: '2016-07-26T13:33:13+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=658'
permalink: /apache-log-rotation-on-windows/
thrive_post_fonts:
    - '[]'
thrive_share_count:
    - '{"facebook":0,"twitter":0,"plusone":0,"pinterest":null,"linkedin":0,"total":0,"last_fetch":1697828203,"url":"https://www.javabullets.com/apache-log-rotation-on-windows/"}'
categories:
    - Uncategorized
---

Thie example uses Apache 2.4, 64-bit version from apachehaus, and the rotatelogs.exe contained in this project –

https://www.apachehaus.com/cgi-bin/download.plx

Unzip the apache download and go to the bin folder, and confirm there is a rotatelogs.exe file. Install the server as either a service, or start from the bin folder, and confirm it works.

Now lets configure rotatelogs.exe for an initial test of every 10s –

```
 #
 # If you prefer a logfile with access, agent, and referer information
 # (Combined Logfile Format) you can use the following directive.
 #
 # CustomLog "logs/access_log" combined
 CustomLog "|C:/httpd-2.4/bin/rotatelogs.exe -l C:/httpd-2.4/logs/accesslog.%Y-%m-%d-%H-%M-%S.log 10" combined
```

The file names use the strftime format –

http://httpd.apache.org/docs/current/programs/rotatelogs.html

Restarting the The log directory then shows files with names like –

```
accesslog.2016-07-21-10-27-30.log
accesslog.2016-07-21-10-27-40.log
```

So lets change it to a daily rotate – or 60\*60\*24s = 86400s –

```
CustomLog "|C:/httpd-2.4/bin/rotatelogs.exe -l C:/httpd-2.4/logs/accesslog.%Y-%m-%d.log 86400" combined
```

The final change is to the error log – we dont need to rotate this daily as its smaller than the access\_log, but we can use rotatelogs to rotate on file size, in this case 10MB –

```
ErrorLog "logs/error_log"
```

Becomes –

```
ErrorLog "|C:/httpd-2.4/bin/rotatelogs.exe -l C:/httpd-2.4/logs/error_log.%Y-%m-%d.log 10MB"
```

## Issues

- Starts an extra external process – rotatelogs.exe – for each log
- Piping on windows is a bit ropy
- You cant configure monthly as an interval

## Alternatives

### mod\_log\_rotate – https://github.com/JBlond/mod\_log\_rotate

This is a Apache Shared Object – so operates the same as rotatelogs, but within the apache processes

Example Usage –

```
LoadModule log_rotate_module modules/mod_log_rotate.so
RotateLogs On
CustomLog logs/access-%Y%m%d-%H%M%S.log common
```

To rotate the logs monthly the advice is to use LogRotateWin, a windows version of the unix command logrotate (http://www.linuxcommand.org/man\_pages/logrotate8.html) –

### LogRotateWin – https://sourceforge.net/projects/logrotatewin/

This operates as a daily task, and processes the configuration file. To rotate monthly you can use the following

```
C:\httpd-2.4\logs\access_log {
 monthly
 dateext
 dateformat -%d-%m-%Y
 missingok
 nocompress
 rotate 30
}
```

References –

http://serverfault.com/questions/577908/what-are-the-drawbacks-to-using-mod-log-rotate-and-rotatelogs-exe-together  
https://www.digitalocean.com/community/tutorials/how-to-manage-log-files-with-logrotate-on-ubuntu-12-10  
https://sourceforge.net/p/logrotatewin/wiki/LogRotate/