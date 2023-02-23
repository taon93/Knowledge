# CORE
## Spring Properties: 
Main properties are defined in `application.properties` but they may be overridden, appended by properties specified as active properties of the project.
## IoC container:
> ***IoC: Inversion of Control, aka Dependency Injection*** - *design pattern in which, objects define dependencies that
> they need but those dependencies are managed and controlled by the other entity, which injects them to the object during its creation*

Interface that represents container holding all spring beans - IoC objects that are instantiated, managed and configured
by framework itself and injected into the objects that need them as dependencies. Spring supplies several implementations 
of this interface. The Configuration of the beans may be changed by introducing changes to XML files holding the 
configuration or by marking beans with annotations (@Annotation).
### Configuration metadata:
Beans managed by container correspond to actual objects used in the application. They are usually used in one of logical
layers of the application: service, persistence (repositories or DAO's - data access objects), presentation 
(ie: Web controllers) or infrastructure. 
### Using Spring Container:
You can retrieve beans from the context by calling: `T getBean(String nameOfTheBean, Class<T> requiredType)`, for example:
```aidl
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml") // constructors arguments are paths to XML configuration files
PetStroreService service = context.getBean("petStore", PetStoreService.class); // nameOfTheBean: "petStore" coresponds with <bean id="petStore... in XML configuration file
```
Using methods that call beans is not advisable - by doing so you craete dependency to Spring API itself.

### Bean definition:
In context container beans are represened by `BeanDefinition` objects, they contain (among others):
-   A package-qualified class name: actual implementation of the defined bean
-   Bean behavioral configuration elements: how bean should behave in the container: scope, lifecycle callbacks etc.
-   References to other beans that are necessary for the bean to work: collaborators aka dependencies
-   Other settings for the created bean object ie: size limit of the pool, number of connections used in the bean to  
manage connection pool.

> *Side note*: **lifecycle callbacks**: functions that are called before or after certain model methods.

There is possibility to register existing objects as beans by the user via `Application Context`'s `BeanFactory`, accessed 
through `getBeanFactory()`, which returns (by default) `DefaultListableBeanFactory` implementation and then calling 
`registerSingleton(yourObject)` or `registerBeanDefinition(yourObject)`. By using those methods, you should be careful 
and register object as soon as possible, for other beans dependant upon registered bean (and the bean itself) to be 
properly instantiated and autowired.

***Beans usually have one name,*** but more may be introduced—then they work as aliases. Names are defined in XML by `id`
(there may be only one) or `name` (there may be many) attributes, or by combination of those two. If bean doesn't have any 
explicit name/id - it is generated for it (but you no longer can access such a bean by `getBean(...)`).

### Instantiating beans: 
A bean definition is essentially a recipe for creating one or more objects. The container looks at the recipe for a named
bean when asked and uses the configuration metadata encapsulated by that bean definition to create (or acquire) an actual object.
Type of the bean instantiated is supplied by `class` attribute. If bean is instantiated using factory method instead of
constructor, you must provide `factory-bean` attribute, that specifies name of the bean in the container (current of ancestor)
that contains instance method that is to be invoked to create the object, which method will be invoked is specified by another
attribute: `factory-method`.

**Creating bean through factory: configuration**
```aidl
<!-- the factory bean, which contains a method called createInstance() -->
<bean id="serviceLocator" class="examples.DefaultServiceLocator">
    <!-- inject any dependencies required by this locator bean -->
</bean>

<!-- the bean to be created via the factory bean -->
<bean id="clientService"
    factory-bean="serviceLocator"
    factory-method="createClientServiceInstance"/>
```
**Respective class implementation:**
```aidl
public class DefaultServiceLocator {

    private static ClientService clientService = new ClientServiceImpl();

    public ClientService createClientServiceInstance() {
        return clientService;
    }
}
```
Bean runtime type may be determined by calling: `BeanFactory.getType` method for specified bean name.

## Dependencies
There are two types of dependency injection in Spring: 
- Constructor based - beans are invoked during call of specific args constructor/static factory bean, constructor arguments
are specified in configuration as Spring dependencies by using `<constructor-arg ref="beanIdRef"/>`
```aidl
public class ExampleBean {

    // Number of years to calculate the Ultimate Answer
    private final int years;

    // The Answer to Life, the Universe, and Everything
    private final String ultimateAnswer;

    public ExampleBean(int years, String ultimateAnswer) {
        this.years = years;
        this.ultimateAnswer = ultimateAnswer;
    }
}

//*******************************************Transpones to 

<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg type="int" value="7500000"/>
    <constructor-arg type="java.lang.String" value="42"/>
</bean>

//******************************************************OR

<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg index="0" value="7500000"/>
    <constructor-arg index="1" value="42"/>
</bean>


```
- Setter based- beans are invoked after calling no-args constructor or no-args static factory to instantiate object.

**It is a good rule of thumb to use constructor-based DI for mandatory dependencies and setter based on optional dependencies.**  

Singleton beans are pre-instantiated by default - during creation of the context itself, if the bean is not set to be
pre-instantiated, it is created when it is requested by the first time - creation of the bean may lead to graf of other 
beans being created. Each collaborating bean is fully configured before being injected as dependency to another bean.

1.4.3 Using depends-on


