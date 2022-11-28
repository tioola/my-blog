---
layout: post
title:  "[1z0-829] 9-Collections and generics"
date:   2022-11-02 12:18:33 +0100
categories: 1Z0-829
---
_These are highlights from a Java 8 developer with years of experience trying to highlight only the important details that we easily miss throughout the time.
You might think that sometimes is silly to highlight some things but even those things might help you to remember the entire context._
---

* The diamond operator cannot be used as the type in a variable declaration. It can be used only on the right side of an assignment operation.

```java

List<> list = new ArrayList<Integer>(); // Does not compile

```

* List allows duplicates while set does not allow duplicates
* removeIf() method removes all elements that match a condition

```java

list.removeIf(s -> s.startsWith("A"));

```

* Check below to understand the equality of list

```java

var list1 = List.of(1,2);
var list2 = List.of(2,1);
var set1 = Set.of(1,2);
var set2 = Set.of(2,1);

System.out.println(list1.equals(list2)); // False because the elements are in a different order
System.out.println(set1.equals(set2)); // Prints true because set is not sensitive to order.
System.out.println(list1.equals(set1)); // Prints false because the types are different

```

* Unboxing nulls will result in a null pointer exception like the example below

```java

var heights = new ArrayList<Integer>();
heights.add(null);
int h = heights.get(0); // This will result in a NullPointerException since it will try to autobox and it won`t be possible.

```

* A linked list is special because
  * It has all the methods of a list
  * Additional methods to facilitate adding or removing from the beginning and/or end of the list
  * LinkedList is a good choice when you will be using it as a Deque
  * The main benefit of a LinkedList are that you can access, add to, and remove from the beginning and end of the list in constant time.
* The trade-off is that dealing with an arbitrary index takes linear time

* Arrays.asList(varargs)
  * Returns fixed size list backed by an array
  * Cannot add elements
  * Can replace elements
  * Cannot delete elements
* List.of(varargs)
  * Returns immutable list
  * Cannot add elements
  * Cannot replace elements
  * Cannot delete elements
* List.copyOf(collection)
  * Returns immutable list with copy of original collection`s values
  * Cannot add elements
  * Cannot replace elements
  * Cannot delete elements

* The code below actually compiles

```java

var list = new ArrayList<>();
// The type of the var is ArrayList<Object>. Since there isn`t a type specified for the generic. Java has to assume the ultimate superclass.


```

* The code below is very tricky.

```java

var list = new ArrayList<Integer>();

list.add(3);
list.add(2);
list.add(1);

list.remove(2); // This line passes a primitive which will remove number 1
list.remove(Integer.valueOf(2)); // But this is actually passing an Integer object which will remove the Integer 2
System.out.println(list);

// The result of the list will be 3
```

* HashSet
  * Store its elements in a table (keys are kash and values are objects)
  * Uses the hashCode() method of objects to retrieve them more efficiently.
* TreeSet
  * Stores its elements in a sorted tree structure.
  * Benefit is that the set is always in sorted order.
  * Adding and checking if an element exists takes longer compared to HashSet (specially if it is a large set)
* A Deque (like LinkedList) has the main benefit the ability to add and remove from the beginning and end of the queue.
* The trade-off is that a Deque is not as efficient as a "pure" queue.

* Queue methods
  * offer(E e) - Add to back
  * peek() - Read from front
  * poll() - Get and remove from front
* Deque methods
  * offerFirst(E e) - add to front
  * offerLast(E e) - add to back
  * peekFirst() - read from front
  * peekLast() - read from last
  * pollFirst() - get and remove from front
  * pollLast() - get and remove from last
* A Deque can be a LIFO or FIFO  and it is really important to determine which type will be used.
* Map
  * You need to know that a TreeMap is sorted.
  * There is a factory method create a Map as you can see below

* Note that poll() returns null if the list is empty, and pop() (and removeFirst()) raises a NoSuchElementException

```java
    // This is not the best way of doing it becaus you don`t know which one is the key and the value
    Map.of("key1","value1","key2","value2")

    // A more readable way and recommended is
    Map.ofEntries(
            Map.entry("key1","value1"),
            Map.entry("key2","value2"));
    
```
* For the **HashMap** java uses the hashCode of the key to determine the order so it is not sorted.
* On the other hand TreeMap sorts the **keys** 
* contains() method is on the collection interface but not the map interface.
* get() and getOrDefault() examples

```java

Map<String, String> map = new HashMap<>();
map.put("x", "myvalue");

System.out.println("This is my value => " + map.get("x"));
System.out.println("This is my value => " + map.get("y")); // this will print This is my value null
System.out.println("This is my value => " + map.getOrDefault("y", "no-value")); // this will print This is my value no-value

```

* Replacing Values

```java

Map<Integer, Integer>  map = new HashMap<>()

map.put(1,2);
map.put(3,4);

map.replaceAll((k,v) -> k + v);

// This will result in a map like 1=3,3=7

```

* Putting if absent

```java

Map<String, String> myItems = new HashMap<>();

myItems.put("key1", "value1");
myItems.putIfAbsent("key1", "newValue1"); // This will not replace
myItems.putIfAbsent("key2", "value2"); // This will replace

```

* Merge

```java

// The merge operation will receive a value for an specific key and will decide how to merge the value

BiFunction<String, String, String> mapper = (v1, v2) -> v1.length() > v2.length() ? v1 : v2;

Map<String, String> myItems = new HashMap<>();

myItems.put("key1", "value1");
myItems.put("key2", "value2");

myItems.merge("key1", "longervalue1", mapper); // new value will be longervalue1
myItems.merge("key2", "v2", mapper); // value won`t be replaced because it is smaller

// Also note that if the mapper returns null the key will be removed from it.
```

* Summary of collections and habilities
  * ArrayDeque (Deque)
    * Not Sorted
    * Do not Call hashCode
    * Do not call compareTo
  * ArrayList (List)
    * Not sorted
    * Do not Call hashCode
    * Do not call compareTo
  * HashMap (Map)
    * Not sorted
    * Call hashCode
    * do not call compareTo
  * HashSet (Set)
    * Not sorted
    * Call hashCode
    * do not call compareTo
  * LinkedList (List,Deque)
    * Not sorted
    * Call hashCode
    * do not call compareTo
  * TreeMap (Map)
    * Sorted
    * do not call hashCode
    * Calls compareTo
  * TreeSet (Set)
    * Sorted
    * do not call hashCode
    * Calls compareTo
* If your class implements Comparable, it can be used in data structures that require comparison
* Comparator and Comparable
  * Comparator uses method compare()
  * Comparable uses method compareTo()
  * Comparable must be implemented by class comparing
  * Comparator shouldn`t be implementd by class comparing
  * Comparable is not common to declare using lambda
  * Comparator is common to declare using lambda.
* Comparator has helper static methods for building a comparator.

```java

  Comparator<Animal> c = Comparator.comparing(Animal::getHeight)
          .thenComparingInt(Animal::getWeight);

```

* Remember that Collection.sort(Comparable) requires a comparable
* If your class does not implement comparable you have to use Collection.sort with comparator.

---

Generics

* A type can be named anything you want but the convention is to to use Single upper letters
* Type Erasure is the process in which the compiler replaces the generics with object and add the casts


```java

// This 

public class Crate<T> {
  private T contents;
  public T lookInCrate() {
    return contents;
  }
  public void packCrate(T contents) {
    this.contents = contents;
  }
}

// After Type Erasure becomes this

public class Crate {
  private Object contents;
  public Object lookInCrate() {
    return contents;
  }
  public void packCrate(Object contents) {
    this.contents = contents;
  }
}

// and This

  Robot r = crate.lookInCrate();

// After Type erasure becomes this

  Robot r = (Robot) crate.lookInCrate();

```

* The fact that there is type erasure makes no possible to have overload with generics like

```java

// This will not work because type Erasure will make both object
public class MyAnimal {
  protected void chew(List<Object> input) {}
  protected void chew(List<Double> input) {} // DOES NOT COMPILE
}

```

* When you are working with overridden methods that return generics, the return value must be covariant. In terms of generics this means that the return type of the class defined in the parent class.

```java

public class Animal {
    public List<CharSequence> play() {  }
    public CharSequence sleep() {  }
}
public class Dog extends Animal {
    public ArrayList<CharSequence> play() {  }
}
public class Cat extends Animal {
    public List<String> play() {  } // DOES NOT COMPILE
    public String sleep() {  }
}

```

* Generics can also be used directly in methods, you have to specify it before the return type. Normally they are used in static methods but can also be used in normal methods


```java

public class myUtilsClass {
    
    public static <T> List<T> separate(T myValue){...}
    
}

```

* Take a look at this tricky situation

```java

public class MyCrazyClass<T> {
    
    public <T> T crazyMethod(T t) {
      return t;
    }

  public static void main(String[] args) {
    MyCrazyClass<Animal> mcc = new MyCrazyClass<Animal>();// Here the generic is related to animal
    mcc.crazyMethod("hello crazy"); // Here the generic is related to String
    
    // Note that we declare 2 different generics with both times with the same letter but the one of the class is different of the one in the method.
    
        
  }
    
}




```
---

Generics wild cards

* Remember that you cannot assign subclasses of generics like

```java

List<Integer> numbers = new ArrayList<>();
List<Object> objects = numbers; // This does not compile


```

* To solve this issue you have to use the wildcard ?

```java

List<?> list

```


* **extends** explanation below

```java

// List<? extends Number> menas that any of these are legal assignments

List<? extends Number> foo3 = new ArrayList<Number>();  // Number "extends" Number (in this context)
List<? extends Number> foo3 = new ArrayList<Integer>(); // Integer extends Number
List<? extends Number> foo3 = new ArrayList<Double>();  // Double extends Number


```

* **super** explanation below

```java

List<? super Integer> foo3 means that any of these are legal assignments:

        List<? super Integer> foo3 = new ArrayList<Integer>();  // Integer is a "superclass" of Integer (in this context)
        List<? super Integer> foo3 = new ArrayList<Number>();   // Number is a superclass of Integer
        List<? super Integer> foo3 = new ArrayList<Object>();   // Object is a superclass of Integer

```

* Normally in the real world we use wildcards when receiving an array in a parameter and not as a local variable. Check the example below and notice the issue.

```java

        List<? extends Number> myNumbers = new ArrayList<>();
        Integer myInteger = 1;
        myNumbers.add(myInteger); // This will not compile since java won`t know what type it is
        //These makes lists with wild cards automatically immutable

```

* Remember that you should not use the **bounded wildcard** in the return type. The method below for example does not compile.

```java

<T> <? extends T> myMethod(List<? extends T> list) { // DOES NOT COMPILE
return list.get(0);
}

```