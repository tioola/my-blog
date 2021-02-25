---
layout: post
title:  "When and how are beans initialized"
date:   2021-02-19 12:18:33 +0100
categories: Spring Certification
---


Depending on the `scope` of your `@Bean` you will have a different behavior in terms of initialization, you can take as default like below:


 | Escope    | Behavior                                                               |
 |-----------|------------------------------------------------------------------------|
 | Singleton | Instance created and initialized during the startup of the application |
 | Prototype | Lazily created every time the bean is requested.                       |
 | Session   | Lazily created once a new  http session is created and bean requested  |
 | Request   | Lazily created once a new http request is created and bean requested   |
 | WebSocket | Lazily created once a new web socket is created  and bean requested    |
 
 
 
 In case you want to change the default behavior you can do it with the annotation `@Lazy` or you can also do it in
 a more general way using the attribute `lazyInit` from `@ComponentScan`
 
 As an example you could do like below
 
 
 
 ```java
    
 @Component
 @Lazy(true)
 public class MySingletonClass {
    
    public MySingletonClass(){
        System.out.println("Lazy creation");
    }
        
 }
```
