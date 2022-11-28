---
layout: post
title:  "[1z0-829] 8-Beyond Classes"
date:   2022-09-27 12:18:33 +0100
categories: 1Z0-829
---
_These are highlights from a Java 8 developer with years of experience trying to highlight only the important details that we easily miss throughout the time.
You might think that sometimes is silly to highlight some things but even those things might help you to remember the entire context._
---

* Lambda works with Interfaces that have excatly one abstract method.
* Remember that the parentheses are optional only when there is one parameter and it doesn`t have a type declared
* The @FunctionalInterface annotation tells the compiler that you intend for the code to be a functional interface.
* Method reference and lambda samples

```java

interface MyStringChecker {
    boolean check(String text, String prefix);
}

// Pay attention to the parameter order
MyStringChecker methodRef = String::startsWith;
MyStringChecker lambda =  (s,p) -> s.startsWith(p);

```

* Binary files generated when compiling a lambda will contain $$ as the name
* Sample of common functional interfaces
```java

class Samples {

    public static void main(String[] args) {
        
        Consumer<String> c1 = System.out::println;
        Consumer<String> c1 = s -> System.out.println(s);
        
        c1.accept("myString");
        
        BiConsumer<String, Integer> b1  = map::put;
        BiConsumer<String, Integer> b2 = (k, v) -> map.put(k,v);
        
        b1.accept("myString",1);
        
        Predicate<String> p1 = String::isEmpty;
        Predicate<String> p2 = x -> x.isEmpty();
        
        p1.test(""); // return true;
        
        BiPredicate<String, String> b1 = String::startsWith;
        BiPredicate<String, String> b2 = (string, prefix) -> start.startsWith(prefix);
        
        b2.test("prefix","pre"); // return true
        
        Function<String, Integer> f1 = String::length;
        Function<String, Integer> f2 = x -> x.length();       
        
        f1.apply("lennnngthhhhh");
        
        BiFunction<String,String,String> bf1 = String::concat;
        BiFunction<String,String,String> bf2 = (x,y) -> x.concat(y);
        
        bf1.apply("first","second");
    }
    
}

```

* UnaryOperator and BinaryOperator are special cases of a Function. They require all type parameters to be the same type.

```java

class Sample1 {

    public static void main(String[] args) {
        
        UnaryOperator<String> u1 = String::toUpperCase;
        UnaryOperator<String> u2 = x -> x.toUpperCase();
     
        u1.apply("haha");
        
        BinaryOperator<String> b1 = String::concat;
        BinaryOperator<String> b2 = (mystring, stringtoadd) -> mystring.concat(stringtoadd);
        
        b1.apply("baby","shark");
        
    }
    
}

```

* Convenience Methods

* Consumer.andThen()
* Function.andThen()
* Function.compose() // Note that andThen() and compose are equivalent, they are equivalente like x.andThen(y) is the same as y.compose(x)
* Predicate.and()
* Predicate.negate()
* Predicate.or()