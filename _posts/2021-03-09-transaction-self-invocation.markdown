---
layout: post
title:  "Transaction and self-invocation"
date:   2021-03-12 12:18:33 +0100
categories: Spring Certification
---

Self invocation happens when you invoke **from** your bean **within** your bean, for example:

```java

@Component
public class MyBean{
    
    public void executeTransactions(){   
        methodTransaction1();
        methodTransaction2();
  }
    @Transactional
    public void methodTransactionOne(){
        System.out.println("method transaction one");
   }
    @Transactional
    public void methodTransactionTwo(){
        System.out.println("method transaction two");
    }

}

```

The problem with the code above is that, once you are `self-invoking` in you own bean, the proxy is not called and therefore the transaction is not managed correctly.


To fix this problem you have to:


* Have dependency to spring-aspects
* Include aspectj-maven-plugin
* Configura support with @EnableTransactionManagement(mode = AdviceMode.ASPECTJ)