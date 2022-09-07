---
layout: post
title:  "[1z0-829] 1st Builduing blocks highlights"
date:   2022-09-06 12:18:33 +0100
categories: 1Z0-829
---

These are highlights from a Java 8 developer with years of experience trying to highlight only the important details that we easily miss throughout the time.
You might think that sometimes is silly to highlight some things but even those things might help you to remember the entire context.

* An object is often referred to as an instance.
* A reference is a variable that points to an object (in heap)
* Variables hold the state of the program and methods operate on that state
* A top-level type is a data structure that can be defined idependently **within** a source file
  * The majority of time we work with one class inside of the source file. This class will be always the top-level Type
* You can put two types in the same file. When you do so **at most one** of the top-level types in the file is allowed to be public
* If you do have a public type it needs to match the filename.
* Each source file can contain only one public class
* If the java class is an entry point fo the programa it must contain a valid main() method
* Remember tha these a valid declarations for an array:
```java
String[] args
String options[]
String... friends
```
* Example to compile a java class
```shell
javac Zoo.java
```
* Example to Run a compiled java class
```shell
java Zoo
```
* Example how to compile and run at the same time (Single-File Single Source code) this does not work for multiple source codes.
```shell
java Zoo.java
```
* For compilation you cannot use wild card to include subdirectories
* if you want to send the compiled files to another directory you have to use the **-d** option like below
```shell
java -d classes packagea/ClassA.java packageb/ClassB.java
```
* To run the program you can specify the classpath which can be specified in the following ways:
```shell
java -cp classes packageb.ClassB
java -classpath classes packageb.ClassB
java --class-path classes packageb.ClassB
```
* To create a jar file you can use the commands below, both result in the same execution
```shell
jar --create --verbose --file myNewFile.jar
jar -cvf myNewJar.jar -C dir .
```

* The import statement doesn`t bring in child packages, fields or methods; It imports only classes directly under the package.
* You can only have one wildcard (*) for the import. Something like below is not allowed
```java
import java.nio.*.*;
```
* You cannot import methods only class names so the example below is incorrect
```java
import java.nio.file.Paths.*;
```
* When a class name is found in multiple packages java gives a class error
* If you need to use classes with the same Name you have to use the full qualified name like below
```java
public class Conflicts {
  java.util.Date date;
  java.sql.Date sqlDate;
}
```
* You can read values of already initialized fields on a line initializing a new field if you respect the correct declaration order.
```java
public class Name {
  String firstName = "Jose";
  String lastName = "Silva";
  String completeName = firstName + lastName;
}
```
* You cannot refer to a Variable before it is declared. The example below would not work
 ```java
public class Name {
  String firstName = "Jose";
  String completeName = firstName + lastName;
  String lastName = "Silva";
}
```
* The inner block is used to limit the scope of the variables
```java

public void method1() {
        // Inner block
        {
            String myName = "Jose";
            doSomethingWithName(myName);
        }    
        // Can`t access name here.
}

```

* Fields and instance initializer blocks are run in the order in which they appear in the file
* Numbers and underscore characters works but there are some rules
  * You can add underscores anywhere except at:
    * the beginning of a literal
    * the end of a literal
    * right before a decimal point

```java
  double notAtStart = _1000.00; // Does not compile
  double notAtEnd = 1000.00_; //Does not compile
  double notByDecimal = 1000_.00; // Does not compile
  double legalButWierd = 1_00_0.0_0; // Compiles
  double reallyReallyWierd = 1___________1; // Also compiles  
```
* Primitive types will give you a compiler Error if you attempt to assign them null;
* Text blocks are used to avoid the ugly way of concatenating strings like the example below

```java
  String myTextBlock = """
  This is a Text block
  
  Made to simulate
  
  A big text
  
"""  
```

* In text blocks you have to categorize two types of spaces:
  * **Essential whitespaces** are spaces that are part of your string
  * **Incidental whitespaces** are spaces that are there to make the code easier to read\
  * In the code below imagine a vertical line drawn on the leftmost non-whitespace charactore in your text block: Everything to the left of it is incidental whitespace and everything to the right is essential whitespace.

```java
  String myTextBlock = """
  *
 * *
* * *  
"""  
```

* Declaring variables in the same line requires them to be of the same type some examples to use as a guidance:

```java
  boolean b1,b2; //Correct declaration
  String s1 = "1",s2; //Also correct
  double d1, double d2; //Incorrect you should not use twice the double , will no compile
```

* var can only be used in method local variable never on instance
* var belive it or not is not a reserved word
* A var cannot be initialized with null
* Var cannot be used in multiple assignments like
```java
var fall = 2, autumn = 2
```
* instance primivite values and local primitive values are different in terms of default: For instance it defaults but for local it doesn`t causing an error
* Importing classes by class name takes precedents over wildcards therefore if you have 2 classes like below
```java
package abc;
class Myclass{}

package dfg;
class Myclass{}


// You can import like below in any class
import abc.*;
import dfg.Myclass;

//It will take always the Myclass of package dfg because it does not use wildcard


```

* Although float can be declared with an F, it never prints the f in the sysout
* double should be assigned with an F prefix