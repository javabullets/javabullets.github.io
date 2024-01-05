---
id: 1340
title: 'Keeping Documentation Up-To-Date'
date: '2017-04-04T14:55:27+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=1340'
permalink: /keeping-project-documentation-up-to-date/
publicize_twitter_user:
    - glenwareltd
post_views_count:
    - '6'
thrive_post_fonts:
    - '[]'
categories:
    - Java
---

I returned to a project wiki on confluence after a few months break, and was looking through the documentation wondering if it was still relevent. The code and requirements had changed in places, so I knew some was out of date – however the documentation of the new areas looked good. There were also other design artifacts that seemed important at the time, but now I felt had questionable value.

## Problem

In my experience the problem is that we create lots documentation at the start of a project to set direction, but as the project continues it becomes easier to see how the requirements slot into the overall design. This means we are less likely to update the documentation in these areas as they are covered at a high level in the existing documentation. This means the information can slip through the cracks.

## Solutions?

I’ve had a look into solutions to this problem –

- Aims – What do you need to document? From a developer perspective you likely need – 
    - Onboarding
    - UML artifacts – Class Diagrams, Sequence Diagrams
    - Server setups might also be relevent
- Pruning – At the end of a project, or phase, have a task to prune the information. The degree to which you “prune” the information depends on your project. If you are using a wiki you could reference the access log to see when the pages were last used, and there are some tools that can auto-archive but I have not used these.
- Autogenerate content – This is especially important for documenting software – but look to use tools to generate your design documentation
- Fresh Eyes – We only see the true value of documentation when we onboard someone new, or return to a project after a break. Then we get to see the cracks in the documentation, and what is not of value

## Other?

I would be interested to read how other people manage project documentation, especially in small teams