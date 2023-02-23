### Creation:
- Empty: `Optional<WrappedClass> optionalOfWrappedClass = Optional.empty();`
- Preinitialized: `Optional<WrappedClass> optionalOfWrappedClass = Optional.of(new WrappedClass(...));`
- Optional that can hold `null` value: `Optional<WrappedClass> optionalOfWrappedClass = Optional.ofNullable(new WrappedClass(...));`
when filled with null it will return `Optional.empty`
### Usage: 
- get value: `.get()`
- if value is present consume it: `.ifPresent(Consumer<? super WrappedClass>)`
- check if value is present: `.isPresent()`
- get or default: `.orElse(<defaultObjectOfWrappedClass>)`
- get or use supplier: `.orElseGet(Supplier<? extends WrappedClass>)`
- `or(Supplier<? extends WrappedClass>)` - gets wrapped object if present or returns object returned from supplier. `or` vs `orElseGet` - first returns optional second value. 
- get or throw an exception: `.orElseThrow(Supplier<Exception>)`, if no argument specified throws: `NoSuchElementException`
- `filter(Predicate)` : if optional is filled and value stored meets predicate, returns optional with value, if not `Optional.empty` is returned
- 'flatMap()' : maps to other optional value: function inside the flatMap must return optional - flattens structure of Optionals.
- `map` : maps optional value to other value and wraps this value inside the optional
- `stream` - returns stream consisting of one or zero elements: dependent on presence of the optional value. 