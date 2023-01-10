## General
- **Final classes cannot be subclassed**
- **Class with only private constructors cannot be subclassed**
- **Final methods cannot be overridden**
- **Overriding method must have same or less strict modifiers than method from super class** SUPER -> STRICTER, SUB -> MORE LOSE
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
This example is invalid: as B::method has default access modifier, which is stricter than protected from A::method
- Abstract classes cannot be instantiated
- Class with abstract method, must be abstract itself
- super keyword access overriden method.
- **You CANNOT invoke this() and super() in the same constructor**
- **this() and super() must be invoked as the first thing in constructor**
- **final static variables must be initialized during declaration or in the static block**
- **Function arguments take precedence over member variables (class fields)**
- **If you override equals() you MUST override hashCode() -> a.equals(b) must also mean that a.hashCode() == b.hashCode() (but not vice versa)

## Generics:
- In generics "extends" means both extends and implements - type is important - not actual implementation
- Class implementing generic interface MUST also be generic
- Diffrence between <? extends T> and <? super T> https://stackoverflow.com/questions/4343202/difference-between-super-t-and-extends-t-in-java
- PECS: Producer extend, Consumer super
```
/***************** CLASSES & INTERFACES************************/
public class GenericClass<X, T> implements GenericInterface<X> {...}
interface SmallestLargest<T extends Comparable<T>> {...} // type 'T' must support comparison with other instance of itself via Comparator interface
public class ArrayList<E> extends AbstractList<E> {...} // Class generic declarations: After name of the class
class GenericClass implements GenericInterface <String> {...} // Using specific class implementing generic interface (specialization)
/********************** METHODS *******************************/
public <T> void takeThat(ArrayList<T> list)
```
## Exceptions:
- Method that throws exception, must have it in the declaration `public void takeRisc() throws BadException`
