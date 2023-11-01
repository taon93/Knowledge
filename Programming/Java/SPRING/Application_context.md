## Introduction:
Application context is a way in which spring provides configuration information to the application - it brings together 
different spring functionalities such as components, beans, internalization, life cycle events etc. **It acts as a container
that holds and maintains lifecycle of the beans.**
## Bean creation and management:
Beans are defined using XML configuration document, annotations or JavaConfig classes. Definition specifies bean dependencies,
lifecycle (singleton, prototype, etc) and class type.
When **ApplicationContext** is created it creates beans as needed - **singleton** is default and it means that only one instance
of bean will be created, and it will be injected everywhere, when needed, **prototype** is another type of scope: for every been dependency
of this type, different instance is created. 
## Autowiring:
Spring supports autowiring: that means that beans will be automatically injected when needed, either by a constructor or by a setter method.
## Getting beans from the **ApplicationContext**:
```java
@SpringBootApplicaton
public static class MySpringApplication {
    public static main(String[] args){
        ApplicationContext ctx = ApplicationContext.run(MySpringApplication.class, args);
        MyController controller = ctx.getBean(MyController.class);
    }
}
```