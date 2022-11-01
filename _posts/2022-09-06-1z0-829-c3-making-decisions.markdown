---
layout: post
title:  "[1z0-829] 3-Making decisions"
date:   2022-09-06 12:18:33 +0100
categories: 1Z0-829
---
_These are highlights from a Java 8 developer with years of experience trying to highlight only the important details that we easily miss throughout the time.
You might think that sometimes is silly to highlight some things but even those things might help you to remember the entire context._
---

* Pattern matching was introduced in Java 16 to reduce boilerplate. It is a technique of controlling program flow that only executes a section of code that meets certain criteria.
* With pattern matching you can avoid casting after an instance of for example:


```java

//This code is the old version
void compare(Number number){
    
    if(number instanceof Integer){
        Integer castedNumber = (Integer) number;
        System.out.println(castedNumber.compareTo(5));
    }    
    
}


//The new version with Pattern matching would be like below:

void compare(Number number){
    if(number instaceof Integer castedNumber){
        System.out.println(castedNumber.compareTo(5));
    }    
}

// You can also include conditions that can be used to validate before executing the block

void compare(Number number){
    if(number instaceof Integer castedNumber && castedNumber.compareTo(5) > 0){
        System.out.println("Block of code executed");
    }
}


```


* When using the pattern matching for catching it has to be explicitly from a subtype. The example below would give an error because you are declaring the same type of the parameter.

```java

// This is declared as Integer but shouldn`t... it only accepts SUBTYPES
void compare(Integer number){
    if(number instaceof Integer castedNumber && castedNumber.compareTo(5) > 0){
        System.out.println("Block of code executed");
    }
}   


```

* The **Flow Scope** has a really tricky situation. Take a look at the code below:

```java

// The example below in a common sense would not compile:


public class FlowScope {

    void printOnlyIntegers(Number number) {

        if((number instanceof Integer data)){
            return;
        }

        System.out.println(data.intValue());
    }


}

//And that is correct. this code will not compile because data is out of scope:


// But if you change the code to the example below:

public class FlowScope {

    void printOnlyIntegers(Number number) {

        if(!(number instanceof Integer data)){
            return;
        }

        System.out.println(data.intValue());
    }


}

// This surprisenly will compile, and the reason is that the compiler translate the example above like below


    void printOnlyIntegers(Number number) {

        if(number instanceof Integer data){
            System.out.println(data.intValue());
        }else {
            return;
        }
    }

// Very wierd but this is how this works


```

* Starting from Java 14 you can apply the switch case with multiple values like below:

```java

switch(myVar) {
    case 1,2: System.out.println("firstExample");
    case 3: System.out.println("Second example");        
}

```

* Remember the data types supported by the switch statement
  * int and Integer
  * byte and Byte
  * short and Short
  * char and Character
  * String
  * enum values
  * var (if the type resolves to one of the preceding types)

* The **switch expression** was introduced in Java 14 to reduce the boilerplate of the java code and also permit us to assign the return to a variable, check the example below:

```java

var result = switch(animal) {
        case 0 -> "Aligator";
        case 1 -> "Cow";
        default -> "Invalid";
    
        };

```

* The switch expressions do NOT need the break statement since only one branch is executed;

* There are some new rules for the switch expression
  * All of the branches of a switch expression that do not throw an exception must return a consistent data type (if the switch expression returns a value)
  * If the switch expression returns a value, then every branch that isn`t an expression must yield a value
  * A default branch is required unless all cases are covered or no value is returned.

* The yield keyword is equivalent to a return statement within a switch expression and is used to avoid ambiguity about whether you meant to exit the block or method around the switch expression.

* Watch semicolons in switch expressions

* Remember that this for represents an infinit loop and therefore the code compiles.

```java

for( ; ; )

```

* Remember that the variables of a for loop must always be of the same type. the example below does not compile

```java

//Never declare different types:
for(long a = 0, int b = 1; a<5; a++)

```

* Labeling the loops and conditions. Check the examples below to remember how it works since this is not very recommended to use in a real world scenario

```java

        // This is an example of optional labels.

        // This code is an example of break ... this will break the execution after the first 2 is reached
        FIRST_LABEL: for(int i = 0; i< 5; i++) {

                SECOND_LABEL: for(int x = 0; x<5; x++){
                        if(x == 2) {
                                break FIRST_LABEL;
                        }
                        System.out.println("break"+x);
                }

        } 
    
    // The output will be
    /**
     * break0
       break1
     */

    // This code is an example of continue using label this will stop the execution after printing 1
    FIRST_LABEL: for(int i = 0; i< 5; i++) {
    
        SECOND_LABEL: for(int x = 0; x<5; x++){
            if(x == 2) {
                continue FIRST_LABEL;
            }
            System.out.println("CONTINUE"+x);
        }
    
    } 
        
    // The output will be     
        /**
         * CONTINUE0
         CONTINUE1
         CONTINUE0
         CONTINUE1
         CONTINUE0
         CONTINUE1
         CONTINUE0
         CONTINUE1
         CONTINUE0
         CONTINUE1
         */

        

```

* Pay attention in that anything after the break and continue will not compile.

* var CAN be declared in a for look like the example below


```java

for(var a : myNumbers){
    
    //Iterate
        }

```

* Remember that you cannot use the flow scope with || like the example below

```java

if(o instanceof Long bat || bat <= 20)

```

* In a do while don`t forget that if you declare something inside of it the scope won`t be until the end, for example

```java

int myIt = 0;
do {
    int snake=1;
    myIt++;
    
    
        } while ( myIt == 10 && snake ==1)// Snake is out of sope

```

* Remember that Long is not compatible with a switch statement
* **var** is also supported in while loops

---
Todo`s

* Question 28 of chapter 3 has to be reviewed
