## ObjectMapper

ObjectMapper object is used for deserialization and serialization of JSON in the spring boot application
it creates basic rules of serializing objects to the JSON, and deserializing JSON to java POJO. 

### Defining the ObjectMapper: 
It is usually defined in `@Configuration` class as a bean and injected as dependency via spring cotext:

**Example with setting time and date serialization/deserialization rules:**
```java
@Configuration
public class JacksonConfiguration {
    @Bean
    public ObjectMapper objectMapper() {
        ObjectMapper mapper = new ObjectMapper();
        mapper.registerModule(new JavaTimeModule()); // essential to correctly serializing/deserializing LocalDateTime/LocalDate objects
        mapper.setDateFormat(new SimpleDateFormat("yyyy-MM-dd HH:mm:ss")); // sets formatting for Date Time serialization - how it will be displayed in JSON
        mapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false); // will not fail if encounter unknown properties in JSON 
        mapper.configure(DeserializationFeature.READ_DATE_TIMESTAMPS_AS_NANOSECONDS, false); // Will treat date time formats in milliseconds, not nanoseconds, important for compatibility reasons
        return mapper;
    }
}
```
