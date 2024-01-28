## Creation of mapper:
**Important** to create mappstruct mapper you must first create bean in configuration class - for it to be 
linked to the spring context and can be easily injected
```java
@Configuration
public class MapperConfig {

    @Bean
    @Primary
    public TransactionMapper transactionMapper() {
        return Mappers.getMapper(TransactionMapper.class);
    }
}

```