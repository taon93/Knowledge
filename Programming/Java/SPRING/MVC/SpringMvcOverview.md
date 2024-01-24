## Controller vs RestController Annotation

`@RestController` annotation is a convinience annotation which consists of `@Controller` annotation
but adds additional functionalities to it:
```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Controller
@ResponseBody
public @interface RestController {
    @AliasFor(annotation = Controller.class)
    String value() default "";
}
```
> **TODO**   
Basically it says that JSON will be returned (in oposition to normal Controller - which returns HTML) - I think that it
may be changed - but I will need further reading on the matter


### @RequestMapping

It is generally good idea to use this annotation on the class level - to define endpoint scope of this class, 
methods handling singles REST functions should be marked by fitting concreate annotation:
```java
@GetMapping(<endpoint>)
@PostMapping(<endpoint>)
@DeleteMapping(<endpoint>)
        ...
```

