---
layout: post
title:  "AOP Pointcut expressions"
date:   2021-03-04 12:18:33 +0100
categories: Spring Certification
---

When declaring a @Pointcut you need to define the expression to where how your advice will be executed.
For that you have the designator types below:

* `execution`
* `within`
* `args`
* `bean`
* `this`
* `target`
* `@annotation`
* `@args`
* `@within`
* `@target`


### execution

The execution is the most complex, and most complete of all the options, and matches a method execution.

The formula follows the order below

 `execution( [visibility modifier] [return type] [package].[class].[method]([arguments])) [throws exceptions]`
 
 * **[visibility modifiers]** - public/protected, if omitted all are matched.
    * negation `!` can be used
 * **[return type]** - void, primitive or Object type, cannot be omitted.
    * wildcard `*` can be used
    * negation `!` can be used
 * **[package]** - package in which class is located
    * wildcard `*` can be used to match all packages
    * wildcard `..` can be used to match all subpackages
 * **[class]** - Class name to match against.(matches the subclass of the class too)
    * may be ommitted
    * wildcard `*` can be used  
 * **[method]** - Name of the method.
    * wildcard `*` can be used to match partial name of the class
 * **[arguments]** - May be empty to match methods without argumens.
    * wildcard `*` can be used to match any type.
    * negation `!` can be used for **types**.
    * wildcard `..` can be used to match zero or more arguments.
 * **[throws exceptions]** match method that throws exceptions from given list.
    * negation `!` can be used

#### Example


All public method with return type different of int and under package com with class name MyBean method starting with say and 2 arguments one type Integer and other anytype.

```java
@Before("execution(public !int com..MyBean.say*(Integer, *))")
```



### within

within matches execution **within** especified class:

The formula follows the order below:

`within([package].[class])`

 * **[package]** - package in which class is located
    * wildcard `*` can be used to match all packages
    * wildcard `..` can be used to match all subpackages
 * **[class]** - Class name to match against.(matches the subclass of the class too)
    * may be ommitted
    * wildcard `*` can be used
    
#### Example


all method in class under package com and Starting with My ending with Bean
```java
@Before("within(com..My*Bean)")
```


### bean

matches execution of method with matching Spring Bean Name

`bean([beanName])`

* **[beanName]** name of the Spring Bean

#### Example

```java
@Before("bean(hello_child_bean)")
```

### this

matches the execution **against type of proxy**. Remember that spring creates a proxy for your beans. The `this` will be used to check those.

`this([type])`

* **[type]** - type of the proxy, matches if generated proxy is of specified type

#### Example

In this example lets use one interface and one implementation.

```java
@Before("this(com.spring.example.MyBean)")

//and

@Before("this(com.spring.example.MyBeanImpl)")
```

Using `this` only the interface will be called because it is refering to the proxy.


 
### target

Compared to this it is going to be executed in the target object.

* **[type]** - type of the target object.

`target([type])`

#### Example

```java
@Before("this(com.spring.example.MyBean)")

//and

@Before("this(com.spring.example.MyBeanImpl)")
```

In this example both will be executed.

### @annotation

Matches method annotated with specific annotation.

`@annotation([annotation_type])`

* **[annotation_type]** - type of annotation used to annotate method which should match pointcut expression.

#### Example

```java
@Before("@annotation(com.example.MyCustomAnnotation)")
```

### @args

This one it is not how it looks like. In fact the `args` matches argument of classes annotated with the specified type.
It is not that the annotation is specified in the argument itself but in the class of the argument. 

`@args([annotation_type])`

* **[annotation_type]** - type of annotation used to annotate method which should match pointcut expression.

#### Example

```java
@Before("@args(com.example.MyCustomAnnotation)")
```


### @within

Matches method execution inside class annotated with specified annotation.

`@within([annotation_type])`

* **[annotation_type]** - type of annotation used on top of class, inside which method execution should be matched.


#### Example

```java
@Before("@within(com.example.MyCustomAnnotation)")
```

### @target

Matched method execution inside proxied target class which is annotated with specific annotation.


`@target([annotation_type])` 

* **[annotation_type]** - type of annotation used on top of class, inside which method execution should be matched.


#### Example

```java
@Before("@target(com.example.MyCustomAnnotation)")
```



### Expressions

You can combine pointcuts together with the usage of logical operators.

* ! - negation
* || - or
* && - and

#### Example

```java
@Before("@within(com.example.MyCustomAnnotation) || @within(com.example.MySecondCustomAnnotation)")
```