---
layout: post
title: Spring boot known issues on Google Cloud Platform
author: Thuan Nguyen
---

Now a day, Spring boot is a powerful framework to build a RESTFul Web-services as well as Micro-services. However, I faced some issues when deploying on Google Appengine Standard.

> **Assume that we are using:**
> - Java 8
> - Maven 3.5.x
> - Spring-boot 1.5.x
> - Google Appengine Standard


## Setup Application Logging
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

## Sending Email with GAE Mail API
-----
If you are going to use GAE Mail API, you should considering some following topics:
- Mail from: We can use any email address of the from **anything@[APP_NAME].appspotmail.com**
- Bounce notification: You need to implementing **Bounce notification** when mail is not delivered in case non-exist email address or exeeded 31.5 MB of message size
- You should remove SMTP configuration in Spring-boot **applications.yaml** file since Spring-boot will initial SMTP settings when starting up, so It may catch error
- You need to enabling **GAE Client API** dependence as following:

```
<dependency>
    <groupId>com.google.api-client</groupId>
	  <artifactId>google-api-client-appengine</artifactId>
	  <version>1.23.0</version>
</dependency>
<dependency>
    <groupId>com.google.appengine</groupId>
	  <artifactId>appengine-api-1.0-sdk</artifactId>
		<version>1.9.59</version>
</dependency>
```

For more detail, please read this document [Mail API Overview](https://cloud.google.com/appengine/docs/standard/java/mail/)
