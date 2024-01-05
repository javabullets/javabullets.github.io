---
id: 439
title: 'Retrofit &#8211; RESTful Services made Easy'
date: '2015-05-29T08:55:46+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=439'
permalink: /retrofit-restful-services-made-easy/
thrive_post_fonts:
    - '[]'
thrive_share_count:
    - '{"facebook":0,"twitter":0,"plusone":0,"pinterest":0,"linkedin":0,"total":0,"last_fetch":1522566725,"url":"https://www.javabullets.com/retrofit-restful-services-made-easy/"}'
categories:
    - Uncategorized
---

This is more of a bookmark post. But Ive been looking at some RESTful API’s recently and trying to decide the best approach – which was most likely Springs REST template. I then found Retrofit[Retrofit](http://square.github.io/retrofit/ "Retrofit") as an alternative.

Its syntax is pretty simple, and it basically maps the JSON response to the Java object, and all you are required to do is define the interface. Similar to Spring Data.

The example given on the retrofit website demonstrates this –

`public interface GitHubService {<br></br>  @GET("/users/{user}/repos")<br></br>  List listRepos(@Path("user") String user);<br></br>}`

Then to call the code –

`RestAdapter restAdapter = new RestAdapter.Builder()<br></br>    .setEndpoint("https://api.github.com")<br></br>    .build();`

GitHubService service = restAdapter.create(GitHubService.class);

List repos = service.listRepos("octocat");

I’ll definitely consider this in future projects