---
layout: post
title:  "Transaction and self-invocation"
date:   2021-03-13 12:18:33 +0100
categories: Spring Certification
---

When we talk about `@Transaction` in spring we have to remember that, when we annotate a method of production code and a method of a test (junit)
the `@Transaction` annotation will behave completely different.

## Production code

Transaction rollback happens automatically when your method throws an unchecked exception, this is the default behavior but you can change it with the
following attributes of `@Transaction` annotation

* `rollbackFor` or `rollbackForClassName`
* `noRollbackFor` or `noRollbackForClassName`


For example if don't want to rollback for `IllegalArgumentException` you can do like below:

```java
@Transactional(noRollbackFor = IllegalArgumentException.class)
public void myMethod(){
    //Code to execute
}
```

If you want rollback for one specific transaction you can do like below

```java
@Transactional(rollbackFor = MyException.class)
public void myMethod(){
    //Code to execute my exeception
}
```

## Test code

In test when you annotate your test with the `@Transaction` annotation like the example below:

```java
@Test
@Transactional
public void myTest(){
    myBean.myMethodWithTransaction();
}
```

The `@Transaction` in the test method will **automatically rollback** my transaction. Since this is a test the most probable is that.


If you want to **prevent** your test transaction from being rolled back, what you can do is add the @Rollback(false)

```java
   
    @Rollback(false)
    @Test
    @Transactional
    public void myTest(){
        myBean.myMethodWithTransaction();
    }
``` 