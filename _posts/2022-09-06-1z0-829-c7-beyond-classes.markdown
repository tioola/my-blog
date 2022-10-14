---
layout: post
title:  "[1z0-829] 7-Beyond Classes"
date:   2022-09-27 12:18:33 +0100
categories: 1Z0-829
---
_These are highlights from a Java 8 developer with years of experience trying to highlight only the important details that we easily miss throughout the time.
You might think that sometimes is silly to highlight some things but even those things might help you to remember the entire context._
---


* Interface variables are referred to as constants because they are assumed to be public static and final
* Interface methods also include implicite public and abstract
```java
//     abstract is implicit and not necessary
public abstract interface MyInterface {
    
    //public and abstract are implicit and not necessary
    public abstract int myMethod(int myVar);
    
    //public, static and final are also implicit and not necessary
    public static final int MY_CONSTANT = 2;
}

```

* For obvious reasons interfaces cannot be marked as final
* Remember that covariant return type is: When we override a method the return type can be a subtype f the type of the overridden method for example

```java

public class Producer {
    public Object produce(String input) {
        Object result = input.toLowerCase();
        return result;
    }
}

public class IntegerProducer extends Producer {
    @Override
    public Integer produce(String input) {
        return Integer.parseInt(input);
    }
}


```

* Unlike a class which can extend only one class, an interface can extend multiple interfaces.
* It is quite obvious but the code below would never compile
```java

public  interface MyClass {
    
    private int count = 4;// Does not compile - private
    protected void counter();// Does not compile - protected
    
}

```
* Some default method rules to remember
  * A default method may be declared only within an interface
  * A default method must be marked with the default keyword and include a method body
  * A default method is implicitly public
  * A default method cannot be marked abstract, final or static
  * A default method may be overridden by a class that implements the interface
  * If a Class inherits two or more default methods with the same method signature, then the class must override the method.
  * default methods cannot be abstract or final
* You can use the syntax below to call a hidden default method.

```java

public interface EcoMode {
    
    default int getSpeed(){
        return 3;
    }
    
}

public interface SportMode {
    
    default int getSpeed() {
        return  10;
    }
    
}


public class MyCar implements EcoMode, SportMode {
    
    // YOU NEED TO OVERRIDE HERE OTHERWISE YOU WILL HAVE A COMPILATION ERROR
    public int getSpeed(){
        return EcoMode.super.getSpeed();// This is how you call the default method of Eco Mode
    }
    
}

```

* Without an explicit reference to the name of the interface, the code will not compile. example below

```java

public class Bunny implements Hop {
    
    public void printDetails() {
        System.out.println(Hop.getJumpHeight());
    }
    
}

```

* private and private static methods are allowed in interfaces
* default and private non-static methods can access abstract methods declared in the interface.
* Enum Methods
  * Enum.values() - Return all enums
  * Enum.name() - enum name as string
  * Enum.ordinal() - order of the enum 
  * DayOfWeek.valueOf("MONDAY") will return DayOfWeek.MONDAY

* DayOfWeek.valueOf("MONDAY") works, while DayOfWeek.valueOf("monday") does not work, it is case sensitive
* Remember how to use enums correctly in Switch case
```java

    public void myMethod(DayOfWeek dow){
        var message = switch (dow){
            case DayOfWeek.MONDAY -> "mymessage";// does not compilem ,if you had used only MONDAY, it would work fine
            case 0 -> "second message"; // Does not compile 
        };
    }

```
* All enum constructors are implicitly private.
* Remember that you can have abstract methods in the Enum. You would need to implement them in each ENUM value like the example below:
```java

public enum Season {
  WINTER {
    public String getHours() { return "10am-3pm"; }
  },
  SPRING {
    public String getHours() { return "9am-5pm"; }
  },
  SUMMER {
    public String getHours() { return "9am-7pm"; }
  },
  FALL {
    public String getHours() { return "9am-5pm"; }
  };
  public abstract String getHours();
}

```

---
Sealed classes


* When using the sealed classes you have to remember the keywords below and how each of them behaves.
  * **sealed** : Indicates that a class or interface my only be extended/implemented by named classes or interfaces.
  * **permits** : Used with the sealed keyword to list the classes and interfaces allowed.
  * **non-sealed** : Applied to a class or interface that extends a sealed class, indicating that it can be extended by unspecified classes.

* Check the straightforward example below of how it works:

```java

public sealed class Motorcycle permits Honda, Yamaha{}

// Classes extending sealed classes must be sealed, non-sealed or final
public final class Honda extends Motorcycle {}

public non-sealed class Yamaha extends Motorcycle{}

//This classes does not compile since Motorcycle does not permit it.
public final class Shineray extends Motorcycle {} 

```

* sealed must be before class keyword
* sealed classes NEEDS TO BE DECLARED AND COMPILED IN THE SAME PACKAGE AS ITS DIRECT SUBCLASSES otherwise it won`t work.
* if you have permits in your sealed class but the subclass does not extend it, it will not compile like the example below

```java

// This does not compile once honda does not extend it
public sealed class Motorcyle permits Honda {}

public final class Honda {}

```


* sealed classed can omit permits in case of 2 top level classes in the same file and nested classes.

```java

public sealed class Snake {}

final class Cobra extends Snake {}

//or

public sealed class Snake {
    final class Cobra extends Snake{}
}

```

* Interfaces can also be sealed. Interfaces are implicitly abstract and cannot be marked final. For this reason, interfaces that extend a sealed interface can only be marked sealed or non-sealed. They cannot be marked final.
* Sealed class Rules
  * Sealed classes are declared with the sealed and permits modifiers
  * Sealed class must be declared in the same package or named module as their direct subclasses
  * Direct subclasses of sealed classes must be marked final, sealed or non-sealed.
  * The permits clause is optional if the sealed class and its direct subclasses are declared within the same file or the subclasses are nested within the sealed class
  * Interfaces can be sealed to limit the classes that implement them or the interfaces that extend them.
* You can use sealed classes in switch cases without the default

---
Records

* A record is a special type of data-oriented class in which the compiler inserts boilerplate code for u.
* Records are immutable
* You can`t extend or inherit a record
* A record can implement a regular or sealed interface
* You can create your own constructor but you have to set all fields since they are all final.
* **Compact constructor**  is a special type of constructor used for records to process validation and transformation succinctly. It takes no parameters and implicitly sets all fields.

```java

public record MyNewRecord(int id, String name) {

  public MyNewRecord {
    if (id < 0) throw new IllegalArgumentException("Id cannot be negative");
    // This refers to the input parameters.
    name = name.concat("built");
  }

}


```

* While compact constructors can modify the constructor parameters, they cannot modify the fields of the record. For example this does not compile:

```java

public record MyRecord(int id, String name) {
    
    public MyRecord {
        this.name = name.concat("cc");// This does not compile
    }
  
}

```

* **The first line of an overloaded constructor must be an explicit call to another constructor via this()**. Unlike compact constructors, you can only transform the data on the first line. After the first line, all of the fields will already be assigned and the object is immutable.

```java

public record MyRecord(int id, String name){
    
    public MyRecord(String name){
        this(0,name);
        name = "changeName"; // No effect
        this.name = "changeName"; // Does not compile
      
    }
    
}

```

* You cannot add instance fields outside the record declaration. 
* Records do not support instance initializers. All Initialization for the fields of a record must happen in a constructor.


--- 
Nested Classes

* Because they are not top level types they can use any access level (public, protected, package or private)
* It can access members of the outer class, including private members like the example below:

```java

public class MyClassTest {

    private String name = "MTC";

    protected class MyInnerTestClass {
        public String getWhatName(){
            return name;
        }
    }

}

```

* It is quite odd but the way you instantiate an inner class is like below:

```java


        final MyClassTest myClassTest = new MyClassTest();

        final MyClassTest.MyInnerTestClass myInnerTestClass = myClassTest.new MyInnerTestClass();

```

* Nested classes can now have static members
* Referencing nested inner classes (don`t do it in real life)

```java

 public class A {
    private int x = 10;
    
    class B {
        private int x = 20;
        
        class C {
            private int x =30;
            
            public void all(){
              System.out.println(x);//30
              System.out.println(this.x);//30
              System.out.println(B.this.x);//20
              System.out.println(A.this.x);//10
            }
        }
    }
    
 }

```

* Remember that inner class requires an instance therefore must be initialized inside of the outer class or using instance.new YourInnerClass like the example above.

---

static Nested Class

* Unlike an inner class, a static nested class can be instantiated without an instance of the enclosing class. The trade-off, though, is that it can`t access instance variables or methods declared in the outer class.
* A local class is a nested class defined within a method. Like local variables.

```java

public class LocalClass {
    
    public void localClass() {
        class MyCalculator {
            public void sum(int a, int b){
                System.out.println(a + b);
            }
        }
        
        var myCalculatorInstance = new MyCalculator();
         myCalculatorInstance.sum(1,2);
    }
    
}


```

* You can use outer variables only in case they are final or effective final
* Anonymous class is basically creating a class without any name .
```java

public interface MyInterface() {
    int myMethod();
}

public class MyClass {
    
    int myOtherMethod() {
        var myAnonnymousClass = new MyInterface() {
            public int myMethod() {
                return 10;
            }
        };
    }
    
}

```

---
Polymorphism 

* Casting a reference from a subtype to a supertype doesn`t require an explicit cast
* Casting a reference from a supertype to a subtype requres an explicit cast.
* At runtime, an invalid cas of a reference to an incompatible type results in a ClassCastException being thrown
* The compiler disallows casts to unrelated types