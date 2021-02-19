---
layout: post
title:  "Bean Lifecycle"
date:   2021-02-19 12:18:33 +0100
categories: jekyll update
---

The creation of a bean follows a process for the creation of the bean that consists in the steps below



## During Context Creation
    1 - The bean definition is created based on the configuration provided
    2 - BeanFactoryPostProcessors are called (In this step you still don't have any instance of the bean, only its configuration)
## Bean is Created

    3 - Instance of the bean is created.
    4 - Properties and Dependencies are defined
    5 - BeanPostProcessor::postProcessBeforeInitialization is executed (in this step you already have the information of the bean)
    6 - @PostConstruct method gets called
    7 - InitializingBean:afterPropertiesSet is called ( in this case you bean has to implement  org.springframework.beans.factory.InitializingBean)
    8 - @Bean(initMethod) is invoked ( this has to be defined in the @Bean)
    9 - BeanPostProcessor::postProcessAfterInitialization is called
    
    
 **After the creation of the bean, it will be available for you to be used. Until your context is destroyed or the lifecycle of the bean reaches the end**
 
 
 ## Bean Destroyed
 
    10 - @PreDestroy method gets called.
    11 - DisposableBean::destroy method gets called.
    12 - @Bean(destroyMethod) method gets called.