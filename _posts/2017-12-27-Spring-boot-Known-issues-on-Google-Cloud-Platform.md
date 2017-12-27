---
layout: post
title: Spring boot known issues on Google Cloud Platform
author: Author Name
---

Now a day, Spring boot is a powerful framework to build a RESTFul Web-services as well as Micro-services. However, I faced some issues when deploying on Google Appengine Standard.

> **Assume that we are using:**
> - Java 8
> - Maven 3.5.x
> - Spring-boot 1.5.x
> - Google Appengine Standard


## Application Logging? 
-----

By following this page https://cloud.google.com/appengine/docs/standard/java/logs/, We can implement custom application logs easier. However, It does not works when we use this package "org.springframework.boot:spring-boot-starter-web" because Spring-boot's default logging bridge conflicts with Jetty's logging system (Appengine use Jetty). 

To be able to capture the Spring-boot startup logs, you need to exclude "org.slf4j:jul-to-slf4j" dependency. The easiest way to do this is to set the dependency scope to provided, so that it won't be included in the WAR file:

```
<!-- Exclude any jul-to-slf4j -->
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>jul-to-slf4j</artifactId>
  <scope>provided</scope>
</dependency>
```
