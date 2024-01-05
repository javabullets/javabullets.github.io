---
id: 1005
title: 'Auto-Generating Spring Security: Accessing the In-memory Database'
date: '2016-11-23T13:07:48+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=1005'
permalink: /auto-generating-spring-security-accessing-the-in-memory-database/
thrive_post_fonts:
    - '[]'
categories:
    - Java
---

I came across a blog post on [Spring Framework guru’s](https://springframework.guru/using-the-h2-database-console-in-spring-boot-with-spring-security/) which uses the h2 database console, and thought it would be useful to combine the console with my own spring security tutorials –

- [Can Spring Security be auto-generated?](https://www.javabullets.com/can-we-auto-generate-spring-security/)
- [Auto-generating Spring Security Tutorial – Memory Realms](https://www.javabullets.com/2016/10/09/auto-generating-spring-security-tutorial-memory-realms/)
- [Auto-generating Spring Security Tutorial – Default JDBC Realms](https://www.javabullets.com/auto-generating-spring-security-tutorial-default-jdbc-realms/)
- [Auto-generating Spring Security Tutorial – Custom JDBC Realms](https://www.javabullets.com/auto-generating-spring-security-tutorial-custom-jdbc-realms/)

I’ve updated the [parkrunpb](https://github.com/farrelmr/parkrunpbreboot) project on github to replace hsqldb with h2database. Ive also introduced a new class WebConfiguration.java, which registers the h2 database servlet

Start the application –

**mvn spring-boot:run**

## Access The Console

You can access the console through -http://localhost:8080/console

![console2](https://glenware.files.wordpress.com/2016/11/console2.png?w=680&resize=657%2C231)

You then make sure the JDBC URL is –

**jdbc:h2:mem:testdb**

And login –

![console3](https://glenware.files.wordpress.com/2016/11/console3.png?resize=1181%2C415)

The layout shows the tables we loaded in schema.sql on the right (CUSTOM\_AUTHORITIES, CUSTOM\_USERS and PARKRUNCOURSE)

## Combine it with spring security

The next step is to combine with Spring Security, so I’ll use the configuration from the previous tutorial –[Auto-generating Spring Security Tutorial – Custom JDBC Realms](https://www.javabullets.com/auto-generating-spring-security-tutorial-custom-jdbc-realms/)

We start with our class –

```
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Autowired
    private DataSource dataSource;

    @Autowired
    public void configureGlobal(AuthenticationManagerBuilder auth)
            throws Exception {
         auth
             .jdbcAuthentication()
                 .dataSource(dataSource)
                   .usersByUsernameQuery(
                   "select username, password, enabled from custom_users where username = ?")
                   .authoritiesByUsernameQuery(
                   "select username, authority from custom_authorities where username = ?");
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/webjars/**","/about.html","/rest/**").permitAll()
                .antMatchers("/admin/**").hasAnyRole("CUSTOM_ADMIN")
                .anyRequest().authenticated()
            .and()
                .formLogin()
                    .loginPage("/login")
                    .defaultSuccessUrl("/admin/admin.html")
                    .failureUrl("/login")
                    .permitAll()
             .and()
                .logout()
                    .logoutSuccessUrl("/")
                    .permitAll()
                    ;                    
    }    
}
```

We then add to the configure method –

```
        http.authorizeRequests().antMatchers("/").permitAll().and()
                .authorizeRequests().antMatchers("/console/**").permitAll();
         http.csrf().disable();
        http.headers().frameOptions().disable();
```

The method then becomes –

```
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/webjars/**","/about.html","/rest/**").permitAll()
                .antMatchers("/admin/**").hasAnyRole("CUSTOM_ADMIN")
                .anyRequest().authenticated()
            .and()
                .formLogin()
                    .loginPage("/login")
                    .defaultSuccessUrl("/admin/admin.html")
                    .failureUrl("/login")
                    .permitAll()
             .and()
                .logout()
                    .logoutSuccessUrl("/")
                    .permitAll()
                    ;
                    
        http.authorizeRequests().antMatchers("/").permitAll().and()
                .authorizeRequests().antMatchers("/console/**").permitAll();
         http.csrf().disable();
        http.headers().frameOptions().disable();
                    
    }
```

This means the normal security from the original tutorial is applied to the application, but we have a special rule for the console

You can then test the application as before with the username/password customadmin/customadmin. You could also insert or update courses,