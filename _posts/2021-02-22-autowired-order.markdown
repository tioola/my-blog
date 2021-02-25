---
layout: post
title:  "@Autowired matching order"
date:   2021-02-22 12:18:33 +0100
categories: Spring Certification
---

The `@Autowired` follows the following order to match your bean candidates

    1-Match exactly by type
    2-If more than one candidate by type match by `@Primary`
    3-If there is no `@Bean` marked with @Primary then check for @Qualifier
    4-If no bean marked with @Qualifier check if the bean name match
    
One example of name matching is for example, imagine that you have 3 beans of the same type


```java
    @Configutation
public class BeansConfiguration{
    
    @Bean
    public Bean1 bean1Red(){
        return new Bean1("red");
   }
    @Bean
    public Bean1 bean2Blue(){
        return new Bean1("blue");
   }

    @Bean
   public Bean1 bean3Green(){
        return new Bean1("green");
   }

}
```

Since you don't have any qualifier how to you tell spring which one should be injected?

You can do that with the name of the property for example

```java

public class BeanToInject{ 

    @Autowired
    public Bean1 bean1Red;
}


```

In the example above the bean red will be injected.