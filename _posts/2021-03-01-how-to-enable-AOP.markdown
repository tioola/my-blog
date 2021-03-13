---
layout: post
title:  "How to enable Aspects in your spring context"
date:   2021-03-01 12:18:33 +0100
categories: Spring Certification
---

The process of enabling aspects is pretty simple, you basically need a @Configuration class annotated with

### @EnableAspectJAutoProxy

For example your ApplicationConfiguration class like below:

```java

@Configuration
@ComponentScan
@EnableAspectJAutoProxy
public class ApplicationConfiguration {
}

```

Other than that you have to import the following dependency:


```xml
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-aspects</artifactId>
    </dependency>
```

And of course have your @Aspect annotated as a @Component or create your Aspect with a @Bean. otherwise your `spring-context` won't be able to find it.


