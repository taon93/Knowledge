## General
- **In Java variables are always passed by value**
- **JDK** Java Development Kit: software for programmers who want to write java programs
- **JRE** Java Runtime Environment: software for running java programs, JDK includes JRE.
- **SE** Standard edition

> Running java via commandline:   
> **javac**: compiles *.java files -> creates *.class programs - that are written in bytecode.    
> **java**: launches VM, executes bytecode from *.class files.
> **jshell**: commandline program for running java procedures
### JVM: 
**Java Virtual Machine** - environment on which Java programs run that is independent of the platform.
When it starts, it reserves part of RAM for java application.
JVM manages:
- Class area
- **Heap**
- **Stack**
- Program counter register
- Native method stack memory
---
**STACK MEMORY**
It is responsible for executing methods, contains primitives and object references of methods in the stack (objects are
stored on the heap). Each method is represented by one block of the memory, when the method ends, block pops from the stack.
----
**HEAP MEMORY**
Contains all the objects that exist in the application. Objects on the heap contain primitive values and references to other
objects on the heap. When object on the heap is no longer connected (directly or indirectly) to the stack memory it is garbage
eligible. 
---
**STRING POOL**
Strings are created on the string pool -
when new String is created (existing one is changed: which is basically the same),
compiler checks if there is another that has the same value and if so it only creates new reference.
----
### Garbage Collector:   
It is run by JVM and when it will be called is dependent on the JVM (I think that there is actual GC thread). Possible
situations when GC will be run: 1. When memory on the heap is low 2. When CPU is free 3. When **requested** (only requested) by the developer: running `System.gc()`
JVM will throw `OutOfMemoryException` when memory is full but there are no GC eligible objects on the heap.
### Serialization: 
Act of saving state of the object to some other representation (ie. JSON). Deserialization is reverse operation. To serialize an object it must implement Serializable interface.
You may use `ObjectOutputStream.writeObject()` for serialization and `ObjectInputStream.readObject` for deserialization.
```java
public class someClass {
  // ...
  public void someMethod() {
    // *********** SERIALIZATION *****************
    FileOutputStream fos=new FileOutputStream("Rectangle.ser");
    ObjectOutputStream oos=new ObjectOutputStream(fos);
    oos.writeObject(new Rectangle(4,5));
    oos.close();
    // ...
    // ********** DESERIALIZATION ****************
    FileInputStream fis=new FileInputStream("Rectangle.ser");
    ObjectInputStream ois=new ObjectInputStream(fis);
    Rectangle rect=(Rectangle)ois.readObject();
    ois.close();
    // ...
  }
  // ...
}

```
- `transient` fields of the class will not be serialized.   
- If we want to serialize Object consisting of objects of other classes, all of those classes must implement `Serializable` interface, or
must be marked as `transient` in the class declaration. Otherwise exception will be thrown (`NotSerializableException`).  
- Constructor is not called during deserialization

### Other
- Applet: java program that works on web page.
  To run applet, you need Java embedded web browser - you'll get the newest version of the
  applet when you visit its webpage - it's automatically used.

### Miscellaneous
- **Final classes cannot be subclassed**
- **Constructor can be called only from another constructor within the class (not any other method)**
- **Class with only private constructors cannot be subclassed**
- **Final methods cannot be overridden**
- **Uninitialized local primitives don't have value and cannot be read**
- Only Strings that are Strings literals are shared, not Strings that are the result of concatenation, substrings of String objects, all of them
are immutable though.
- String can be `null` or empty: `""` - when checking both, first check is string is not null: `str ~= null && str.lenght() != 0`, invoking method on
not an assigned object will throw an error.
- You can have only one public class in the source file (non-public classes are not limited).
- Local variables shadow class fields
- Object has methods: `requireNotNull(checkedField, exceptionMessage)` and `requireNotNullOrElse(checkedField, defaultValue)`
first method will throw an exception if checkedField is not set - has `null` value, second method will set checkedField with
default value if not set.

```java
class A {
    protected void method() {
    // ...do something
    }
}
class B extends A {
  void method() {
    // ... do something else
  }

  public static void main(String args[]) {
    B b = new B();
    b.method(); // COMPILATION ERROR!
  }
}
```
This example is invalid: as B::method has a default access modifier, which is stricter than protected from A::method
- Abstract classes cannot be instantiated
- Switch `default` case don't need to be at the end and will be invoked only if better match is not found: regardless of the order.
- Class with abstract method, must be abstract itself
- super keyword access overriden method.
- **You CANNOT invoke `this()` and `super()` in the same constructor (only one `this` or one `super` may be invoked in the constructor)**
- **If not specified otherwise (by calling super) during initialization of the subclass its parent `super()` (without arguments) is called - if 
it is not defined, this will result in compiler error.**
- **this() and super() must be invoked as the first thing in constructor**
- **final static variables must be initialized during declaration or in the static block**
- **Function arguments take precedence over member variables (class fields)**
- **If you override equals() you MUST override hashCode() -> a.equals(b) must also mean that a.hashCode() == b.hashCode() (but not vice versa)**
- Initialization block should always come after field definitions.
- Initialization block is executed before constructor call;
- Records are immutable and readable by the public.
- You must explicitly initialize local variables but fields of the class are initialized by defaults (`0`, `false` and `null`)
- From the compiler point of view nested packages don't have any relationship - `java.util` and `java.util.stream`
  have nothing to do with each other.
  Each of them is independent. 
- To print 1D array use `Arrays.toString(arr1)`, to print more complex arrays use `Arrays.deepToString(arr2)`, to compare arrays use: `Arrays.equals(arr1, arr2)` 
- Strings are immutable. StringBuilder wraps String and is mutable - creates String only upon calling `toString()`.
```
String str1 = "0"
for(int i = 0; i < 10; i++) str1 += i; // 11 Strings created: one used and rest GC eligible
```
> **Coupling**  
> Measure of how much a class is dependent on the other class, usually coupling should be minimized.    
> **Cohesion**
> Measure of how related are responsibilities of the class. Class should be highly cohesive: its responsibilities should
> be highly related to each other.
> **Encapsulation**
> Hiding implementation behind well-defined interface. 
----
> **Interfaces:**
> - variables of the interfaces are always `public static final`, if you define any different scope - it will result with compile time error.
> - interface methods are by default `public abstract`, after java 8 you may specify concrete default implementations for the interface.
> - interface can extend another interface but cannot extend the class. `interface SubInterface extends ExampleInterface {...}`.
----
> **`.euqals()** should satisfy 5 conditions: 
> 1. Reflexive: for any x `x.equals(x)` should yield `true`.
> 2. Symmetric: If `x.equals(y) == true` then (and only) `y.equals(x) == true`.
> 3. Transitive: If `x.equals(y) == true` and `y.equals(z) == true` then `x.equals(z)` also must be `true`. 
> 4. Consistent: If state is not changed in both of the objects then `x.equals(y)` should yield always the same result.
> 5. For any non-null reference of x, `x.equals(null)` should yield `false`.
----
> **Immutable Object: final class, final fields, no setters.**
----
> **Varargs** `someMethod(String format, Integer... args)` you may pass as many arguments of type Integer as you want,
method can have only one vararg argument, and it must be at the end of the declaration (last parameter). https://www.baeldung.com/java-varargs   
----
> **Abstract Classes**:   
> Are classes that cannot be instantiated but instead should be extended: they may be fully, partially or not implemented: some or all 
> of the methods don't have implementation. Use abstract classes if you have more defined ***"is-a"*** relation between subclasses extending 
> abstract class. Use interfaces when you want to point out that given class shares behaviour of the interface. **Abstract methods** don't have
> any body: `abstract void abstractMethod1(int value);` and may be declared only in abstract class (if defined in normal class it will result in compiler error)   
> **Diffrences between abstract classes and Interfaces:**   
> - Methods and members of the abstract class can have any scope, and members may be not static.
> - A concrete child of abstract class must implement every abstract method, abstract child doesn't have to do it. 
> - Child can extend only one abstract class but may implement multiple interfaces
> - Child can define abstract methods with same or less restricted visibility, all methods implemented from the interface must be 'public'
----
**SCOPE MODIFIERS** 
- Private: only within the class.
- Default: private + within the package
- Protected: default + subclasses that are outside the package
- Public: everywhere
- **The Overriding method must have the same or less strict modifiers than method from super class** SUPER -> STRICTER, SUB -> MORE LOSE
- Default class with public methods will act like an entire default class: you cannot access methods if you cannot access class in the first place.
- If there is subclass outside the package that has an instance of the superclass,
  it will not be able to access methods of superclass through this instance,
only through inheitance (TODO: check this: I think this should be specified for non-public superclass).
- Scope modifiers apply to the class, not the object: within the class, every field and method of the same class can be accessed:
also those invoked on a different object.
```
class Employee {
  ...
  private String name;
  public boolean equals(Employee other) {
    return this.name.equals(other.name); // Other is another object of Employee class
  }
}
```
- To pass copy of the object not actual reference (for example in getter), use `Object.clone()`
----
#### Type conversion in mathematical operations
- If either of the operands is of type double, the other one will be converted to a double.
- Otherwise, if either of the operands is of type float, the other one will be converted to a float.
- Otherwise, if either of the operands is of a type, long, the other one will be converted to a long.
- Otherwise, both operands will be converted to an int.
----
#### Operations hierarchy:
- same level is processed from left to right, except those that are right associative: `a += b += c` will give result: `b = b + c` and `a = a + b + c`
1. `[] . () (method call)`
2. `! ~ ++ -- (cast) new`
3. `* / %`
4. `+ -`
4. `>> << >>>`
5. `< <= >= > instanceof`
6. `&`
7. `^`
8. `|`
9. `&&`
10. `||`
11. `?:`
12. `= += -= *= /= %= &= |= ^= <<= >>= >>>=`
- 
#### Generics:
- In generics "extends" means both extends and implements - type is important - not actual implementation
- Class implementing a generic interface MUST also be generic
- Difference between <? extends T> and <? super T> https://stackoverflow.com/questions/4343202/difference-between-super-t-and-extends-t-in-java
- PECS: Producer extend, Consumer super
```java
/***************** CLASSES & INTERFACES************************/
public class GenericClass<X, T> implements GenericInterface<X> {...}
    // type 'T' must support comparison with other instance of itself via Comparator interface
interface SmallestLargest<T extends Comparable<T>> {...} 
    // Class generic declarations: After name of the class
public class ArrayList<E> extends AbstractList<E> {...} 
    // Using specific class implementing generic interface (specialization)
class GenericClass implements GenericInterface <String> {...} 
/********************** METHODS *******************************/
public <T> void takeThat(ArrayList<T> list)
```
## Exceptions:
- Exceptions usually implement "Chain of Responsibility" design pattern. 
- Method that throws exception must have it in the declaration `public void takeRisc() throws BadException`
There are three subclasses inheriting the main Exception class: **IOException**, **InterruptedException**
and **RuntimeException** - last one is not checked by compiler
- `finally{}` block that will run no matter if there was normal execution or exception:
```java
public void someMethod() throws SomeException {
...
}
... in another funciton:
try {
    someMethod();
}
catch (SomeException(e) { e.printStackTrace(); }
finally {
    doRegardlessIfSuccessOrFailure()
}
```
- `finally` is used mostly for clearing resources. 
- `finally` block will not finish if a) exception will be thrown in `finally` itself, b) JVM will crash.
- you can use `try` block without `catch` - it may be used if you want to do something in `finally` block even if exception 
occurs, without handling this exception. `try` is not allowed if both `catch` and `finally` are missing.
- Jf the try or catch blocks return something, Finally block will be invoked just before
return. 
- If method throws more than one exception, but they have a common superclass, then they may be caught in one block of this superclass
typeof exception - this is not mandatory: you may divide exception handlers to more `catch` blocs, but all exceptions must be properly 
handled or ducked. 
- You may handle more than one type of exceptions in one block, dividing each exception type with '|': `catch(IOException | SQLException e) {...}`.
- Try with resources: `try (BufferReader br = new BufferReader(new FileReader("FILE_PATH"))` `br` will be accessible in entire try block and then 
its resources will be automatically freed - regardless of success or failure of the `try` block itself, to use resource in the `try` block it must implement
`AutoClosable` interface (BufferReader does).
- Less generic exception handlers must be defined before more generic ones: otherwise there will be compilation error.
- You may freely throw `RuntimeException` but if `CheckedException` will be thrown without handling, it will result in compiler errror:
you may handle exception by providing `catch` block or ducking exception: specifing that methods `throws` exception.
- `Catch` blocks handle exceptions in order of writing: if there is more generic type of exception handler before more specific of the same type,
exception will be handled by a more generic one even though more specific is more fitting: more specific will never be used.
- You can duck exceptions by declaring that method throws an exception. But this exception must be caught somewhere down the method stack.
```java
myFunction() {
try { riskyFunction(); }
catch (RiskyException e) { e.printStacktrece(); ... }
}

//VVVVVVVVVVVVV  MAY BE CHANGED TO   VVVVVVVVVVVVVVVV

myFunction()  throws RiskyException {
    riskyFunction(); // Without try block
}
```
### Good exception handling practices:
1) Never hide exceptions: at least use `printStackTrace()`
2) Do not use exceptions for flow control (there is performance impact). 
3) Think about user and developer - what they need to get when exception will happen
4) Can calling do something about exception if not: make unchecked exception or error exception

### Exception hierarchy:
Throwable -> extended by: Error, Exception & RuntimeException -> Programmer defined exceptions.
1) `Error` : is used in situations when nothing can be done about error
2) `Exception` : exception can be handled
3) `RuntimeException` : unchecked exceptions - they are not checked during copilation.
----
### Multithreading and concurrency:

- A single thread is an instance of call/method stack: JVM is responsible for creating new threads and managing existing ones.
- Threads: Main program thread, threads created by developer - in the runtime, garbage collector thread.
- To run the thread, pass it to the object that implements a Runnable interface.  
- For Runnable implementation use mostly lambdas, and objects only when their states are needed.
- **For running threads you may use specified JDK API class: `ExecutorService` https://www.baeldung.com/java-executor-service-tutorial**
- Every object has `lock` mechanism, but most of the time it remains unlocked, only when Object is in `Synchronised` block it becomes
locked
- When object is locked it may be accessed only by one thread at the time. 
```
MyDuck duck; 
synchronised(duck) { // synchronised block example
    duck.doSomething();
    ...
}
class Duck {
    ...
    // synchronised method
    public synchronised void swim() { // object is locked when invoking swim method
        ... 
    }
}
```
- If an object has more than one synchronised method, then it means that each of those methods is blocked when a single thread runs in
any of them: If any of the synchronised method is accessed by the thread, any synchronised method of this object is locked for 
any thread except this one, until it will not leave synchronised scope.
- Locking and unlocking methods is handled by the JVM (you cannot tinker with this).
- There is one key for each object and one lock per class (for accessing static states and behaviours)
- **Atomic Variables** assure that setting and reading of this variable are done at once in one thread (without switching)
- **CAS** - compare & set - variable check if the value that the value that they want to set is correct (was not changed in the
meantime by another thread) and if so they atomically change it, they return boolean (set to false if operation does not succeed)
- The best way to work with thread is to use immutable objects and swap them instead of updating - by doing so; threads may work 
independent of others. 
- To work with collections in multithreading applications use `CopyOnWrite<Collection>` i.e. `CopyOnWriteMap` 
- Threads `join()` method: blocks execution of the code, until "joined" thread will not complete its execution:
```java
  ThreadExample thread1 = new ThreadExample();
  ThreadExample thread2 = new ThreadExample();
  ThreadExample thread3 = new ThreadExample();
  
  thread1.start();
  thread2.start();
  thread2.join(); // Will wait until thread2 is not finish.
  thread3.start();
```
- Threads `yield()` static method: changes state of currently picked thread from RUNNING to RUNNABLE - however, scheduler still may pick
this exact thread to run next (especially if this is high priority thread).
- Threads `sleep()` static method: puts currently running thread to sleep for the number of milliseconds.
- **Deadlock**: thread1 is holding resources (in synchronized block) of ObjectA and needs resources from ObjectB, thread2 holds resources
of ObjectB and needs ObjectA (also variations of this case).
- **Inter-Thread communication** 
  - `wait()` - thread will wait until it is "notified"
  - `notify()` - object notifies thread "waiting" for this object (only one)
  - `notifyAll()` - object notifies all threads "waiting" for this object
- `synchronised` block method blocks: 
  - All `synchronized` non-static methods within single object for synchronised non-static blocks and methods within the class. 
  - All `synchronized` static methods for all objects and class itself for synchronized static blocks and methods within the class.
- **States of the thread**: 
  1) NEW - thread is created but `start()` is not yet called.
  2) RUNNABLE - thread is eligible to run but not running yet.
  3) RUNNING - thread is currently picked by the scheduler and running.
  3) BLOCKED/WAITING - thread is alive but not eligible for running: it is waiting for something, ie another thread is in synchronised
  block that this thread wants to use.
  4) TERMINATED/DEAD - thread completed execution. After that, there is no reuse of this thread - it is dead.
- **Priority** of the thread: default priority is 5 (out of 10), if scheduler must pick between threads that are waiting, it will pick one
wiht highest priority first, if there are more threads with same priority, it will pick at random. You may change priority of the thread: 
```
ThreadExample thread1 = new ThreadExample();
thread1.setPriority(7);
thread1.start();
```
There are constants for priority: `Thread.MAX_PRIORITY()`, `Thread.MIN_PRIORITY`, `Thread.NORM_PRIORITY`.   
- **Multiple Runnables in one thread** - thread starts once and if it will end all tasks then it will die; thread can have more than one task though.

----
### Date & Time:
In Java there are two main classes that are used for tracking time: `Date` class, that represents point in time and `LocalDate` class,
which express days in different notations: for example, different calendars. 

----
### Streams:
- Stream consists of operations: 
  1. Source: creation or use of existing stream
  2. Intermediate Operations: each intermediate operation results in new stream
     - Stateful: Elements need to be compared against one another (sort, distinct etc.)
     - Stateless: No need of comparing with other elements (map, filter, etc.)
  3. Terminal Operation: consumes the stream: produces a result.
----

> **Functional Interface:** Interface that has SAM - *single abstract method* - every lambda may be treated as functional interface. Functional
> interface may be marked by `@FunctionalInterface` annotation: it will guard interface to have only one abstract method.   
> **Types of functional interfaces:** 
>- **`R Function<T, R>(T t)`:** given argument of type T returns type R - transforms arguments. (map)
>- **`boolean Predicate<T>(T t)`:** function specialization that assets parameter: given argument returns if predicate is met or not (filter)
>- **`T Operator<T>(T t)`:** function specialization that returns argument of same type as received. (String::toUpperCase)
>- **`T Supplier<T>()`:** function specialization that takes no arguments, but returns value of type T. (generate)
>- **`void Consumer<T>(T t)`:** function specialization reverse to supplier: takes argument are consumes it, returning void. (peek)

### Keywords and operators:
- `strictfp` methods tagged with this keyword will use strict floating-point operations to assure reproducible results on different VMs
- `instanceof` operator that checks if given object is an instance of checked class: 
```
// SubClass extends SuperClass
SubClass subClass = new SubClass(); 
if(subClass instanceof SubClass) // true 
{ doSomething(); }
if(subClass instanceof SuperClass) // true
{ doSomethingMoreGeneric(); }
if(subClass instanceof AnotherClass) // false
{ doSomethingStupid(); }
```
- `volatile` only applicable to instance variables, it's content will not be stored and retrived from thread cache but directly
from *main memory*

### TODO: check what is the difference between CopyOnWrite, Concurrent and Synchronised collections.
### TODO: Anonymus, nested and inner classes.
