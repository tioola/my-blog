---
layout: post
title:  "Understanding the @ComponentScan"
date:   2021-02-22 12:18:33 +0100
categories: Spring Certification
---

### Simple component scan

`@ComponentScan` is used to inform spring which classes should be scanned.
 If you for example have your configuration ApplicationConfiguration like below all the classes from the package and subpackages
 of `ApplicationConfiguration.java` will be scanned.
 
```java

@ComponentScan
public class ApplicationConfiguration {
}

```


### Advanced Component Scanning

In case we need an advanced component scanning like for example: only scanning classes ending with Bean and excluding classes ending with repository
we can easily do that like the example below:


```java

@ComponentScan(
basePackages = "com.spring.professional.exam.tutorial.module01.question15.advanced.beans",
includeFilters = @ComponentScan.Filter(type = FilterType.REGEX, pattern = ".*Bean"),
excludeFilters = @ComponentScan.Filter(type = FilterType.REGEX, pattern = ".*(Controller|Service).*")
)
public class ApplicationConfiguration {
}

```