---
id: 561
title: 'Applying Eclipse formatting using Maven'
date: '2016-05-25T10:51:18+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=561'
permalink: /applying-eclipse-formatting-using-maven/
thrive_post_fonts:
    - '[]'
categories:
    - BuildTools
    - Uncategorized
tags:
    - eclipse
    - formatter-maven-plugin
    - maven
---

I had an issue recently where a branch had been formatted by eclipse and made a merge from Trunk to Branch reasonably difficult. These things happen – so the solution was to find a plugin that would allow me to format both HEAD and Branch consistently outside of eclipse.

The first step was to export my eclipse preferences from eclipse –

Preferences &gt; Java &gt; Code Style &gt; Formatter

<div class="wp-caption alignnone" id="attachment_566" style="width: 844px">![Eclipse Preferences](https://glenware.files.wordpress.com/2016/05/eclipsepreferences.png?resize=834%2C697)Eclipse Preferences

</div>The next step was to find a plugin that would apply these changes. I opted for [ formatter-maven-plugin](http://code.revelc.net/formatter-maven-plugin/examples.html#Setting_Source_Files)

Then it was a matter of adding it to both master poms –

```
<plugin>
     <groupId>net.revelc.code</groupId>
     <artifactId>formatter-maven-plugin</artifactId>
     <version>0.5.2</version>
     <configuration>
        <configFile>${session.executionRootDirectory}\eclipse-formatter.xml</configFile>
     </configuration>
     <dependencies>
        <dependency>
            <groupId>net.revelc.code</groupId>
            <artifactId>formatter-maven-plugin</artifactId>
            <version>0.5.2</version>
        </dependency> 
      </dependencies>
      <executions>
           <execution>
                <goals>
                    <goal>format</goal>
                </goals>
           </execution>
      </executions>
 </plugin>
```

Then run the plugin on both branch and trunk –

```
mvn formatter:format
```

Commit then continue with your merge