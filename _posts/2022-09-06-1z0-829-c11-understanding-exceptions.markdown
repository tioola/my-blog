---
layout: post
title:  "[1z0-829] 11-Understanding Exceptions"
date:   2022-11-17 12:18:33 +0100
categories: 1Z0-829
---
_These are highlights from a Java 8 developer with years of experience trying to highlight only the important details that we easily miss throughout the time.
You might think that sometimes is silly to highlight some things but even those things might help you to remember the entire context._
---

Exceptions

* Remember to check if the exception is a checked exception because if it is and there is no **throws** declaration, you will face a compilation issue.
* REMEMBER that an exception which inherits **Throwable** but not RuntimeException or Error is also checked.
* There are three ways to print an exception. You can let Java print it out, print just the message, or print where the stack trace comes from.

```java
try{
    myMethod()
}catch(Exception e){
    System.out.println(e);
    System.out.println(e.getMessage());
    e.printStackTrace();
}


```

* Remember that java will always try to protect you against impossible compilation

```java

String myvar = "mystring";
Integer number = (Integer) myvar; // it Does not compile 

```


* There is a rule for the catch blocks order. You have to always follow **subclass > superclass**

```java

try{
    myIoExceptionMethod();
        }catch(IOException ex){ //subclass
            // do something
        }catch(Exception ex2){ //superclass
           // do something
        }

//If this were with the IOException first. it would throw a compilation error.

```

* On the other hand we have the multi catch block which can handle more than one Exception at the same time

```java

try {
    System.out.println(Integer.parseInt(args[1]));
} catch (ArrayIndexOutOfBoundsException | NumberFormatException e) {
    System.out.println("Invalid");
}

```

* Note that in the example above, one exception MUST NOT be a superclass of the other exception. The multicatch block is only used for different types of exceptions. So the example below would never work.

```java

 try {
            
  }catch(IOException | Exception exception){
      
  }

```


* Remember that while declaring a multicatch block there must be only one variable name.

```java

catch(Exception1 e | Exception2 e | Exception3 e) // DOES NOT COMPILE
catch(Exception1 e1 | Exception2 e2 | Exception3 e3) // DOES NOT COMPILE
catch(Exception1 | Exception2 | Exception3 e)

```

* The finally block should always be the last one

* We now have the try-with-resources mechanism which is responsible to close a resource automatically. You can check the example below:

```hava
try (FileInputStream is = new FileInputStream("mytext.txt")) {
    // read file with is variable
 } catch (IOException e) {
 e.printStackTrace();
 }

```

* You can also declare multiple Autoclosable in the try-with-reasources


```java

try (var first = new FileInputStream("mytext.txt");
     var second = new FileInputStream("secondfile.txt")) {
    // first inputstream
    // seond inputstream
 } catch (IOException e) {
 e.printStackTrace();
 }

```

* One important thing to mention is that resources are closed in the reverse order.
* to use try-with-resources, the resource has to implement Autoclosable.
* Remember that in try-with-resource catch is optional only if the close() method of AutoClosable does not throw any exception
* Closeable interface can also be used in try-with-resources. This interface was created for backwards compatibility
* You can also declare your auto-closable resources before the try catch and use them there they will be closed.

```java

var first = new FileInputStream("mytext.txt");
var second = new FileInputStream("secondfile.txt")
try (first;
     second) {
    // first inputstream
    // seond input stream
} catch (IOException e) {
    e.printStackTrace();
}

```

* IMPORTANT, when using a variable in the try-with-resources. you must make sure that this class is effectively final.

```java


        var first = new FileInputStream("mytext.txt");
        var second = new FileInputStream("secondfile.txt");
        try (first;
             second) {
            // first inputstream
            // seond input stream
        } catch (IOException e) {
            e.printStackTrace();
        }

        first = null; // doing this here will cause a compilation error

```

* Suppressed exceptions are exceptions that are suppressed since one exception was already thrown. you can check the example below:

```java

class MyAutoClosable implements AutoCloseable {

    @Override
    public void close(){
        throw new IllegalStateException("My supressed exception ");
    }
}

public class MyTest {
    public static void main(String[] args) {
        try (var myautoc = new MyAutoClosable()) { // The autoclosable will also cause an exception and this exception will be suppressed.
            throw new IllegalStateException("My exception");// This is the first exception will be the main
        } catch (Exception e) {
            for(var ie: e.getSuppressed()){
                System.out.println(ie.getMessage());// Here the exception thrown by the Autoclosable will be returned in the suppressed. 
            }
        }


    }

}


```

---

Date ,Locale and NumberFormat

* For the date format, if you want to define your format but also need to scape a string you can do it using the scape quotes ''

```java

var f = DateTimeFormatter.ofPattern("MMMM dd, yyyy 'at' hh:mm");

```

* Locale.getDefault() will return the default locale of the machine.
* You have pre defined locales like Locale.GERMANY, but you can also create locales that do not exists 

```java
// this locale is valid

Locale myfakelocale = new Locale("xx","YY");

// You can also create a locale using a builder

final Locale build = new Locale.Builder()
        .setRegion("YY")
        .setLanguage("xx")
        .build();


```
* The NumberFormat also receives a locale which differentiate the final format

```java
        
int milionaire = 1_200_000;


var us = NumberFormat.getInstance(Locale.US);
System.out.println(us.format(milionaire)); // 1,200,000

var gr = NumberFormat.getInstance(Locale.GERMANY);
System.out.println(gr.format(milionaire)); // 1.200.000

```

* Remember that we have the CompactNumberFormat which can be used to format number in a compact manner (as the name says)

```java

var myFormatter = NumberFormat.getCompactNumberInstance(Locale.getDefault(), Style.SHORT)

myFormatter.format(1_144_034);// will result 1M


```


* Resource bundle files should be created with {name}_{locale}. Samples of files could be
  * Names_en.properties
  * Names_pt.properties
  * Names_pt_BR.properties
* To get the resource bundle you could do something like below

```java

        final Locale locale = new Locale("pt", "BR");
        final ResourceBundle zoo = ResourceBundle.getBundle("Names", locale);
        
```

* The order of the resource bundle to get the information depends if you define the location or if you use your default location.
* A locale consists of a required lowercase language code and optional uppercase country code.


```java


// Imagine that you have Names_en.properties and Names.properties

final Locale locale = new Locale("pt", "BR");
final ResourceBundle zoo = ResourceBundle.getBundle("Names", locale);

// In this case it will get the default bundle  


```

