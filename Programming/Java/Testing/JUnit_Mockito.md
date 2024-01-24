## Running tests with Maven: 
1. Everything: `mvn test`
2. One Test Class: `mvn -Dtest=<TestClassName> test`
3. One test case: `mvn -Dtest=<TestClassName>#<TestName> test`

### Dependencies
1. `junit-jupiter-api` : main module where all core annotations such as `@Test`, assertions and lifecycle methods are located. 
2. `junit-jupiter-engine` : engine implementation - required to run and execute the tests
3. `junit-platform-suite` : The `@Suite` support provided by this module makes `JUnitPlatform` obsolete
4. `junit-jupiter-params` : Support for parameterized tests (not mandatory)
```xml
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter-engine</artifactId>
    <version>${junit.jupiter.version}</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter-api</artifactId>
    <version>${junit.jupiter.version}</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter-params</artifactId>
    <version>${junit.jupiter.version}</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.junit.platform</groupId>
    <artifactId>junit-platform-suite</artifactId>
    <version>${junit.platform.version}</version>
    <scope>test</scope>
</dependency>
```
----
### Plugin    
- Maven Surefire Plugin : executes unit tests
----
### Java JUnit Test Class:
```java
// same package as tested class package.
class TestedClassTest { // It is good practice to name Test Class with <nameOfTheTestedClass>Test
    @Test // JUnit Annotation for test
    void someTestCase() {
        // Test body
    }
    // ... Other tests in the test class. 
}
```

> When using JUnit you are not using platform itself but rather APIs which communicate with platform: 
> 1. Jupiter: default api
> 2. Vintage: Api used to run older versions of JUnit
> 3. EXT: extendable: Jupiter + Other plugins added by 3rd party

### Test lifecycle: 
Order of test execution is specified by JUnit platform not by the order of writing. That's why it is generally not advisable
to check a state of test class field against many test-cases if this state is changed in them. Every change
to the field will be preserved across proceeding tests. **In JUnit 5 - this is no longer valid: for each testcase execution, new 
class instance is created so every test will be fed by defaults.**
### Test lifecycle hooks: 
1. `@BeforeAll` - Initialize before whole test class
2. `@AfterAll` - Teardown after whole test class
3. `@BeforeEach` - Initialize before each test method 
4. `@AfterEach` - Teardown after each test method
`@BeforeAll` & `@AfterAll` methods must be static and cannot be dependent on the object itself - non-static fields.    

By default, JUnit initializes every test class before every test case execution but this may be changed by annotation: `@TestInstance(<option>)`

### Assertions: 
1. `assertSame(<objRef1>, <objRef2>)` - checks if two references point to the same object
2. `assertNotSame(<objRef1>, <objRef2>)` - checks if two references don't point to the same object. 
3. `assertEquals(<objRef1>, <objRef2>)` - checks if two references are equal (`equals()` is called).
4. `assertNull(<objRef>)` - checks if reference is not set (`null`)
5. `assertNotNull(<objRev>)` - checks if reference is set    
6. `assertArrayEquals(<arr1>, <arr2>)` - checks if arrays contain same elements. 
7. `assertThrows(<exception.class>, <executable>)` - checks if exception of the checked class will be thrown if executable will be executed:
`assertThrows(ArithmeticException.class, () -> 10 / 0)`
8. `assertTimeout(<duration>, <executable>)` - assertion made for performance testing purposes: will fail if `<executable>` method will take to long - over `<duration>`
9. `assertAll(<optionalStringMsg>, ...<executable>)` - this assertion will pass only if all assertions in vararg `<executable>` would be true. 
```java
List<Order> orders = cart.getOrders();
assertAll(
        () -> assertNotNull(orders),
        () -> assertEquals(1, orders.size()),
        () -> assertFalse(orders.isEmpty())
    );
```
`assertAll` will check all assertions even if some of them fail, then it will prompt all failed ones.

----

### Assumptions: 
The test part will be executed only if an assumption is met. 
- `assumingThat(<predicate>, <executable>)` - `<executable>` will be checked only if `<predicate>` is met

### Other annotations: 
- `@Disabled` - test (or test class) will be ignored: will not cause entire suite to fail.
- `@DisplayName(<Name>)` - change displayed name of the test (or test class) to one specified by `<Name>`.
- `@ReapetedTest(<Number>)` - repeats test method `<Number>` of times.
- `@Tag(<TagName>)` - Enables to filter tests: provides categories for tests, `<TagName>` is of type `String`, this annotation
can be used for method or class. 


#### Parametrized tests: 
**jupiter-params** dependency is mandatory.     
- `@ParametrizedTest` - annotation for marking test as parametrized test. Arguments may be used in parametrized tests: 
- `@ValueSource` - annotation for specifying arguments passed to the parametrized test. 
- `@EnumSource(<Enum.class>)` - source of the params in parametrized test will be enum, there is additional alternative:
  `@EnumSource(value=<Enum.class>, names={EnumValuesList})` - that takes only part of available enum values.
- `@MethodSource(<MethodName>)` - source from the method: method must be static and return collection/stream of`<Arguments>`
(`Arguments` abstraction is not necessary if you're providing only one argument). If `<MethodName>` is not provided, JUnit 
will check for method with name same as the test method. 
```java
private static Stream<Arguments> provideStringsForIsBlank() {
    return Stream.of(
      Arguments.of(null, true),
      Arguments.of("", true),
      Arguments.of("  ", true),
      Arguments.of("not blank", false)
    );
}

@ParameterizedTest
@MethodSource("provideStringsForIsBlank")
void isBlank_ShouldReturnTrueForNullOrBlankStrings(String input, boolean expected) {
    assertEquals(expected, Strings.isBlank(input));
}
```
If method providing values is outside test class, `<MethodName>` should be specified by fully qualified name (`<package>.<class>#<methodName>`)
- `@CsvSource` - This is good method for matching input and output, values are provided in format: `values={"input1,output1", "input2,output2", "input3,output3"...}`, commas are default delimiter between `input` and `output`
but this may be changed by additional argument: `delimiter='<delimiter>'`
```java
@ParameterizedTest
@CsvSource(value = {"test:test", "tEst:test", "Java:java"}, delimiter = ':')
void toLowerCase_ShouldGenerateTheExpectedLowercaseValue(String input, String expected) {
    String actualValue = input.toLowerCase();
    assertEquals(expected, actualValue);
}
```
If you want to use delimiter in the value of CSV, you must escape it wrapping it in `''`: 
`{"input1, output1", "'input, with comma', output2", "'third, input, with, comma', 'output, with comma'"}`, 
you may also use directly CSV file to use in parametrized test-cases, giving path to the file in annotation: 
`@CsvFileSource(resoruces=<FileNameAndPath.csv>)`, `resoruces` argument default path starts in `test/java/resources`

### Extensions:
`@ExtendWith(<NameOfTheClass>.class)` - extends annotated test class or test method by additional extensions,
defined in the `<NameOfTheClass>` class, this class must implement predefined interface

### Dynamic Tests: 
For dynamic tests use annotation `@TestFactory` - this is method annotation, this method should return `Collection<DynamicTest>`/`Stream<DynamicTest>`,
each dynamic test takes two arguments: `String` defining test name and `Executable`
```java
@TestFactory
Collection<DynamicTest> dynamicTestsWithCollection() {
    return Arrays.asList(
      DynamicTest.dynamicTest("Add test",
        () -> assertEquals(2, Math.addExact(1, 1))),
      DynamicTest.dynamicTest("Multiply Test",
        () -> assertEquals(4, Math.multiplyExact(2, 2))));
}
```

## Mockito: 
### Dependency: 
`mockito-junit-jupiter`

### Usage: 
- `mock(<ClassToMock>.class)` : this method creates `Mock` object of the given `<ClassToMock>`
- By default, mockito uses nice mocks - they will not complain if called when not expected and will return some default value (empty array, 0 or other)
- `when(<mockObj>.<mockedMethod>).thenReturn(<returnValue>)` - when `<mockObj>` `<mockedMethod>` is called return `<returnValue>`
- Other version of `when(...)`, `thenReturn(...)` is `given(...)`, `willReturn(...)`
- `verify(<mockObj>, <optionalArgs>).<mockedMethodName(arguments)>` - verify that `<mockedMethodName>` with proper `<arguments>` will be called
on the `<mockObj>`, in optional arguments you may pass for example: `times(<number>)` - to check if method will be executed correct `<number>` of times.
Other `<optionalArgumenst>`:   
  - `atLeastOnce()`
  - `atLeast(<number>)`
  - `atMost(<number>)`
  - `never()`
- other versions of `verify` method are: `verifyZeroInteractions`, `verifyNoMoreInteractions`
- If you want to check order of mocked functions calls you may wrap mocked object in `InOrder` object, by calling static function `inOrder`: 
```
InOrder inOrder = inOrder(<mockedObj>);
inOrder.verify(<mockedObj>).<mockedMethodName(agruments)>;
inOrder.verify(<mockedObj>).<otherMockedMethodName(arguments)>;
// etc
```
- other version of `verify(<mockObj>, <optionalArgs>).<mockedMethodName(arguments)>` is: `then(<mockObj>).should(<optionalArgs>).<mockedMethodName(arguments)>;`
- `verify` vs `when` - when is not checking if method was called only specifies behaviour when method will be called.
#### Argument matchers: 
**You cannot mix matchers and normal/real arguments** `someMethod(any(), "Test")` will return true: first argument is a matcher second is `String` literal.   
They are used if you don't know what arguments would be sent to the mocked method: 
- `any()` - sets behaviour for method call with any kind of arguments: `given(cartHandler.canHandleCart(any())).willReturn(false)` - when `cartHandler` mocks method `canHandleCart` will be called with `any()` arguments, 
it will return `false`. You may limit accepted arguments by providing class as an argument to the method: `any(<CheckedClass>.class)`. There are specialized verisons of any, such as: `anyInt()`.
- `argThat(<functionalInterface>)` - you can use this matcher to acquire control over sent arguments: `given(cartHandler.canHandleCart(argThat(cart -> cart.getOrders().size() > 2))).willReturn(true);` - mock will return true only for carts with more than 2 orders. 
- `willThrow(<ExceptionType>.class)` - mock will throw exception when resolved: `given(cartHandler.canHandleCart(cart).willThrow(IllegalStateException.class);`
- `willDoNothing()` - mocked method will do nothing upon calling: may be used for example on `void` methods.
- Methods like `willDoNothing()` and `willThrow(...)` may be chained to depict a situation in which method for example will do nothing upon first call but then it is expected for it to throw exception: `willDoNothing().willThrow(...)`
#### Argument captor:
Used to check with what arguments mock was called.    
Initialization: `ArgumentCaptor<<CapturedArgumentClass>> argCapt = ArgumentCaptor.forClass(<CapturedArgumentClass>.class);`    
To use argument captor just use it as an argument in mocked method and call its method: `capture()`: 
`<mockedMethod>(argCaptor.capture())`. After capturing, argCaptor argument value/values may be checked in assertions using methods `getValue()`/`getAllValues()`:
`assertThat(argCapt.getValue().getOrder().size(), equalTo(1));`. `getAllValues()` returns `List`, which elements may be accessed using `get(<index>)`

### Other methods: 
- `doAnswer()` - let's you create additional logic to the mocked function - enables to create logic between passed arguments and returned value
- `willCallRealMethod()` - calls real (not mocked) method (works also for default implementation of the interfaces).

### Annotations: 
To enable annotation within the class, class must be marked by the annotation: `@ExtendWith(MockitoExtension.class)`    
- `@InjectMocks` : Creates instance of the class and inject to it objects specified as mocks by `@Mock` annotation. 
```java
@InjectMocks
private CartService cartService; // will inject to cartService any relevant mocks
```
- `@Mock` : Creates and wires mock of the given class: akts like a `mock()` method.
```java
@Mock
private CartHandler cartHandler; // cartHandler will be mock.
```
- `@Captor` : Creates and wires ArgumentCaptor object of the given type 
```java
@Captor
private ArgumentCaptor<Cart> argumentCaptor;
```
- `@Spy` : Creates and wires Spy object of the given type

### Spy objects: 
Object that may be treated as partial mock: some of its methods are mocked and some of them are derived from full-class implementation.    
Spies are created using method: `spy(<SpiedClass>.class)`/`spy(<SpiedClassObject>)`.