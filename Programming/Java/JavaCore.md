## General
###JVM: 
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
 
### Miscellaneous
- **Final classes cannot be subclassed**
- **Class with only private constructors cannot be subclassed**
- **Final methods cannot be overridden**

```
class A {
    protected void method() {
    ...do something
    }
}
class B extends A {
    void method() {
    ... do something else
    }
    public static void main(String args[]) {
        B b = new B();
        b.method(); // COMPILATION ERROR!
    }
```
This example is invalid: as B::method has a default access modifier, which is stricter than protected from A::method
- Abstract classes cannot be instantiated
- Class with abstract method, must be abstract itself
- super keyword access overriden method.
- **You CANNOT invoke this() and super() in the same constructor**
- **this() and super() must be invoked as the first thing in constructor**
- **final static variables must be initialized during declaration or in the static block**
- **Function arguments take precedence over member variables (class fields)**
- **If you override equals() you MUST override hashCode() -> a.equals(b) must also mean that a.hashCode() == b.hashCode() (but not vice versa)**

> **Immutable Object: final class, final fields, no setters.**
- Strings are immutable. StringBuilder wraps String and is mutable - creates String only upon calling `toString()`.
```
String str1 = "0"
for(int i = 0; i < 10; i++) str1 += i; // 11 Strings created: one used and rest GC eligible
```
> **Varargs** `someMethod(String format, Integer... args)` you may pass as many arguments of type Integer as you want,
method can have only one vararg argument, and it must be at the end of the declaration (last parameter). https://www.baeldung.com/java-varargs   

**SCOPE MODIFIERS** 
- Private: only within the class.
- Default: private + within the package
- Protected: default + subclasses that are outside the package
- Public: everywhere
- **The Overriding method must have the same or less strict modifiers than method from super class** SUPER -> STRICTER, SUB -> MORE LOSE
- Default class with public methods will act like entirely default class: you cannot access methods if you cannot access class in the first place.
- If there is subclass outside the package that has instance of hte superclass it will not be able to access methods of superclass through this instance,
only throug inheitance (TODO: check this: I think this should be specified to non public superclass).

----
#### Generics:
- In generics "extends" means both extends and implements - type is important - not actual implementation
- Class implementing a generic interface MUST also be generic
- Difference between <? extends T> and <? super T> https://stackoverflow.com/questions/4343202/difference-between-super-t-and-extends-t-in-java
- PECS: Producer extend, Consumer super
```
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
- Method that throws exception must have it in the declaration `public void takeRisc() throws BadException`
There are three subclasses inheriting the main Exception class: **IOException**, **InterruptedException**
and **RuntimeException** - last one is not checked by compiler
- `finally{}` block that will run no matter if there was normal execution or exception:
```
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
- Jf the try or catch blocks return something, Finally block will be invoked just before
return. 
- If method throws more than one exceptions, but they have a common superclass, then they may be caught in one block of this superclass
type of exception - this is not mandatory: you may divide exception handlers to more `catch` blocs, but all exceptions must be properly 
handled or ducked. 
- `Catch` blocks handle exceptions in order of writing: if there is more generic type of exception handler before more specific of the same type,
exception will be handled by a more generic one even though more specific is more fitting: more specific will never be used.
- You can duck exceptions by declaring that method throws an exception. But this exception must be caught somewhere down the method stack.
```
myFunction() {
try { riskyFunction(); }
catch (RiskyException e) { e.printStacktrece(); ... }
}

//VVVVVVVVVVVVV  MAY BE CHANGED TO   VVVVVVVVVVVVVVVV

myFunction()  throws RiskyException {
    riskyFunction(); // Without try block
}
```
----
### Multithreading and concurrency:

- Single thread is instance of call/method stack: JVM is responsible for creating new threads and managing existing ones.
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
meantime by another thread) and if so they atomicaly change it, they return boolean (set to false if operation does not succeed)
- The best way to work with thread is to use immutable objects and swap them instead of updating - by doing so; threads may work 
independent of others. 
- To work with collections in multithreading applications use `CopyOnWrite<Collection>` i.e. `CopyOnWriteMap` 

### TODO: check what is the difference between CopyOnWrite, Concurrent and Synchronised collections.