---
layout: post
title:  "How to create your spring context in integration test"
date:   2021-02-19 12:18:33 +0100
categories: Spring Certification
---

The creation of the `spring context` to execute your integration test in fundamental, in each case to turn it on you have to follow some steps

#### First add the spring-test dependency: 

```xml

<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>5.1.6.RELEASE</version>
    <scope>test</scope>
</dependency>

```

#### Second Add the @SpringRunner in your class


```java
    @RunWith(SpringRunner.class)
```

#### And finally add the context configuration in your class

```java
    @ContextConfiguration(classes = ApplicationConfiguration.class)
```


Note that in this case you can either define the ApplicationConfiguration used in your production context or your can create another one
specific for your tests.

