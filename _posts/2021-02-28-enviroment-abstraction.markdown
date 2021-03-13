---
layout: post
title:  "The Enviroment abstraction"
date:   2021-02-28 12:18:33 +0100
categories: Spring Certification
---

The `Environment` abstraction in spring has two roles:

* Profiles
* Properties

It is through the Environment abstraction where you can pull the information related to properties and profiles.

You also have to know that we have 2 types o` Environment `:

#### StandardEnvironment

It is available when you create a standalone context and in this Environment context you can access the information from:

* Property Files
* JVM properties
* System Environment variables

#### StandardServletEnvironment

In case of the `StandardServletEnvironment` it is created when you startup a servlet container, and because of that in this
enviroment you can access the following:

* Property Files
* JVM properties
* System Environment variables
* JNDI
* Servlet Config
* Servlet Context Parameters


#### Spring Boot

Spring Boot has way more different places where you can pull properties. The places are listed below below:


* Devtools properties from ~/.spring-boot-devtools.properties (when devtools is active)
* @TestPropertySource annotations on tests
* Properties attribute in @SpringBootTest tests
* Command line arguments
* Properties from SPRING_APPLICATION_JSON property
* ServletConfig init parameters
* ServletContext init parameters
* JNDI attributes from java:comp/env
* JVM system properties
* System Environment Variables
* RandomValuePropertySource - ${random.*}
* application-{profile}.properties and YAML variants - outside of jar
* application-{profile}.properties and YAML variants – inside jar
* application.properties and YAML variants - outside of jar
* application.properties and YAML variants - inside jar
* @PropertySource annotations on @Configuration classes
* Default properties - SpringApplication.setDefaultProperties

#### Code example

Below is an example of how you can get the Environment from the ApplicationContext:

```java

AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext();
context.getEnvironment();

```






