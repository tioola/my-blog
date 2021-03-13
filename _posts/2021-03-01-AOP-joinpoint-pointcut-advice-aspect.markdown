---
layout: post
title:  "What is the difference between a **pointcut**, **join point**, **advice** and an **aspect**"
date:   2021-03-01 12:18:33 +0100
categories: Spring Certification
---

### Join Point

Join Point is a point in execution so in Spring a join point can be for example a `method`


```java

interface myCart{

    OrderReceipt placeOrder(Order order); // This is a Join Point

    Order addItem(Item item); // This is a Join Point

    Order removeItem(Item item); // This is a Join Point

}

```

### A Point cut

A `point cut` is the predicate used to match a join point.
 
You can define a point cut with the `@Pointcut` annotation for example:

```java

@Component
@Aspect
public class SavingsAspects {

    @Pointcut("within(com.spring.exemple.savings.*)")
    public void repositoriesOfSavings() {    
    }

}

```

Examples of Point cut

#### execution  Match Method execution

`execution (* com.spring.examples.addItem(..))`

#### within (Match Execution of given type or types inside package)

`within (com.spring.examples.*)`

#### @within (Match execution of type annotated with annotation)

`@within (com.spring.examples.PerformanceLogger)`

#### @annotation (Match join points where the subject of the join point has the given annotation)

`@annotation(com.spring.example.Transactional)`

#### bean (Match by spring bean name)

`bean(my_bean)`

#### args (Match by method arguments)

`ầrgs(String, String, int)`

#### @args (Match by runtime type of the method arguments that have annotations of the given type)

`@args(com.spring.example.AuditArgument)`

#### this (Match by bean reference being an instance of the given type)

`this(com.spring.example.Order)`

#### target (Match by target object being an instance of the given type)

`target(com.spring.example.Order)`

#### @target (Match by class of the executing object having an annotation of the given type)

`@target(com.spring.example.Order)`




### Advice

Advice is the method the additional behavior that will be executed and when for example:



#### @Before

It is executed before the method call and are normally used for:

* Authorization
* Logging
* Data validation

```java
@Component
@Aspect
public class MyAspect {
    
    @Before("within(com.spring.exemple.savings.*") // Advice
    public void beforeMyRepository(JoinPoint joinPoint){
        System.out.println("additional behavior of the advice");
   }

}

```

**Note that you could refer to the @Pointcut declared aboved like that**

```java
@Component
@Aspect
public class MyAspect {
    
    @Before("repositoriesOfSavings()") // Advice
    public void beforeMyRepository(JoinPoint joinPoint){
        System.out.println("additional behavior of the advice");
   }

}

```

#### @After

It is executed after the method call and normally used for:

* Logging
* Resource Cleanup

```java
@Component
@Aspect
public class MyAspect {
    
    @After("repositoriesOfSavings()") // Advice
    public void afterMyRepository(JoinPoint joinPoint){
        System.out.println("additional behavior of the advice");
   }

}

```

#### @AfterThrowing

After throwing an exception it is executed, normally it is used for:

* Logging
* Error Handling.


```java
@Component
@Aspect
public class MyAspect {
    
    @After("repositoriesOfSavings()") // Advice
    public void afterThrowing(JoinPoint joinPoint, Exception exception){
        System.out.println("additional behavior of the advice");
   }

}

```

#### @AfterReturning

It is executed after the method returns the value, it is normally used for:

* Logging
* Data Validation for method result.

```java
@Component
@Aspect
public class MyAspect {
    
    @After("repositoriesOfSavings()") // Advice
    public void afterReturning(JoinPoint joinPoint, Object returning){
        System.out.println("additional behavior of the advice");
   }

}

```

#### @Around


It is the one that englobes all of the items above and it is normally used for:

* Transactions
* Distributed Call tracing
* Authorization, Security

```java

@Aspect
@Component
public class LogPerformanceAspect {

    private Logger logger = Logger.getLogger("performance.log");

    @Around("@annotation(com.spring.examples.annotations.LogPerformance)")
    public Object logPerformance(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        long startTime = System.currentTimeMillis();
        try {
            return proceedingJoinPoint.proceed();
        } finally {
            long finishTime = System.currentTimeMillis();
            Duration duration = Duration.ofMillis(finishTime - startTime);

            logger.info(String.format("Duration of %s was %s", proceedingJoinPoint.getSignature(), duration));
        }
    }
}


```
#### ProceedingJoinPoint.

The proceeding joinpoint as you can se above is related to the around and through it you can not only get the parameters of the method execution but decide whether the execution will continue or not.



### Weaving

Weaving is how your aspects are applied.


* Compile Time Weaving : byte code modified during the compilation
* Load time Weaving : byte code modified when classes are loaded by class
* Runtime weaving : used by Spring AOP , proxy is created and used instead of the original.