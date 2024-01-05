---
id: 429
title: 'Blocking vs Non-blocking IO'
date: '2015-02-02T15:29:50+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=429'
permalink: /blocking-vs-non-blocking-io/
thrive_post_fonts:
    - '[]'
categories:
    - Uncategorized
---

**IO**

- Streams
- Blocking
- Call the thread, and the thread is blocked until the data is read or fully written

\[sourcecode lang=”java”\] InputStream input = … ; // get the InputStream from the client socket BufferedReader reader = new BufferedReader(new InputStreamReader(input));

String nameLine = reader.readLine();  
\[/sourcecode\]

**NIO**

- Buffer Oriented
- Non-blocking
- Call thread and it delegates – only returning when ready to send data
- Selectors – single thread monitors multiple input channels – register multiple channels with selector and it can select which one to use

\[sourcecode lang=”java”\] ByteBuffer buffer = ByteBuffer.allocate(48); int bytesRead = inChannel.read(buffer);

while(! bufferFull(bytesRead) ) {  
 bytesRead = inChannel.read(buffer);  
}  
\[/sourcecode\]

**References**

[http://tutorials.jenkov.com/java-nio/nio-vs-io.html#main-differences-between-java-nio-and-io  ](http://tutorials.jenkov.com/java-nio/nio-vs-io.html#main-differences-between-java-nio-and-io)  
[](http://en.wikipedia.org/wiki/Non-blocking_I/O_%28Java%29)