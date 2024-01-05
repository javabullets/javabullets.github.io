---
id: 2259
title: 'Data Hiding using JsonIgnore and Spring Data JPA'
date: '2017-06-15T20:19:12+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://www.javabullets.com/?p=2259'
permalink: /data-hiding-using-jsonignore-spring-data-jpa/
thrive_post_fonts:
    - '[]'
image: /wp-content/uploads/2017/06/mayra-carreno-62377.jpg
categories:
    - Java8
    - JPA
    - 'Spring Boot'
    - 'Spring Data'
tags:
    - '@JsonIgnoreProperties'
    - JsonIgnore
    - 'Spring Boot'
    - 'Spring Data'
    - 'spring data jp'
---

# Data Hiding using JsonIgnore and Spring Data JPA

Data Hiding using JsonIgnore and Spring Data JPA is achieved using two approaches –

- @JsonIgnore and @JsonIgnoreProperties
- Repository Detection Strategies

This post considers @JsonIgnore and @JsonIgnoreProperties

# Code

The code is available at –

- <https://github.com/farrelmr/introtospringdatarest/tree/4.0.0>
- <https://github.com/farrelmr/introtospringdatarest/releases/tag/4.0.0>

# Code Changes

I’ve added an extra table to for this example –

```

@Entity
public class Secrets {
@Id
@GeneratedValue(strategy=GenerationType.IDENTITY)
private long id;
private String mySecrets;
public String getMySecrets() {
return mySecrets;
}
public void setMySecrets(String mySecrets) {
this.mySecrets = mySecrets;
}
}
```

With its associated repository –

```

@PreAuthorize("hasRole('ROLE_USER')")
public interface SecretsRepository extends CrudRepository<Secrets, Long> {
}
```

# Running The Code

I have left the security from the last tutorial, [Securing Spring Data REST with PreAuthorize](https://www.javabullets.com/securing-spring-data-rest-preauthorize/), in place – but we can run this code using –

```
mvnw spring-boot:run
```

We can then call rest/profile to see the two exposed repositories –

```

curl -u user:user -X GET http://localhost:8080/rest/profile
{
"_links" : {
"self" : {
"href" : "http://localhost:8080/rest/profile"
},
"secrets" : {
"href" : "http://localhost:8080/rest/profile/secrets"
},
"parkrunCourses" : {
"href" : "http://localhost:8080/rest/profile/parkrunCourses"
}
}
}
```

And calling the secrets REST end point-

```

curl -u user:user -X GET http://localhost:8080/rest/secrets/1
{
"mySecret" : "I want to hide this",
"_links" : {
"self" : {
"href" : "http://localhost:8080/rest/secrets/1"
},
"secret" : {
"href" : "http://localhost:8080/rest/secrets/1"
}
}
}
```

This posts looks at techniques I can use to not expose the SecretRepository

# @JsonIgnore and @JsonIgnoreProperties

The purpose of @JsonIgnore, and @JsonIgnoreProperties is to hide attributes from the Jackson parser by instructing it to Ignore these fields

Usage is simply a matter of tagging the attribute with the [@JsonIgnore](http://fasterxml.github.io/jackson-annotations/javadoc/2.5/com/fasterxml/jackson/annotation/JsonIgnore.html)

```

@Entity
public class Secret {
//
@JsonIgnore
private String mySecret;
//
}
```

Or we can achieve the same using [@JsonIgnoreProperties](http://fasterxml.github.io/jackson-annotations/javadoc/2.5/com/fasterxml/jackson/annotation/JsonIgnoreProperties.html) annotation –

```

@JsonIgnoreProperties({"mySecret"})
@Entity
public class Secret {
//
private String mySecret;
//
}
```

With either of these changes we can then call our secrets REST end point, and the mySecret field is no longer exposed –

```

curl -u user:user -X GET http://localhost:8080/rest/secrets/1
{
"_links" : {
"self" : {
"href" : "http://localhost:8080/rest/secrets/1"
},
"secret" : {
"href" : "http://localhost:8080/rest/secrets/1"
}
}
}
```

# Conclusion

@JsonIgnore or @JsonIgnoreProperties simply hides the field from the Jackson parser. This is good for hiding small pieces of information. The downside is we still have an exposed end point due to the default Repository Detection Strategies.