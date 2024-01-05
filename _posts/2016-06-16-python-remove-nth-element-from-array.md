---
id: 599
title: 'Python &#8211; remove nth element from array'
date: '2016-06-16T11:27:12+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=599'
permalink: /python-remove-nth-element-from-array/
thrive_post_fonts:
    - '[]'
categories:
    - Uncategorized
---

I was looking at sampling algorithms, and realised it was overkill for my task. Instead I found a useful numpy feature –

```
import numpy as np

my_array = np.array([1,2, 3, 4, 5, 6, 7, 8, 9, 10,11,12])
my_array = np.delete(x, np.arange(0, x.size, 3))

Output - 
[2 3 5 6 8 9 11 12]
```

Here we remove every 3rd element! Easy!