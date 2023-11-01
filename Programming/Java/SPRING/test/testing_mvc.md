### Mock MVC
In spring mvc we may use MockMVC to mock incoming requests with outgoing responses in the controller,
it is defined in the package: `org.springframework.test.web.servlet`. Requests are defined in subpackage:
`request.MockMvcRequestBuilders`. Response matchers are defined in the: `result.MockMvcResultMatchers` - we may match result message and status
MockMvc instance may be used in the following way:   
```java
mockMvc.perform(<HttpRequest>(<controllerEndpoint>) // what kind of HTTP method is called and on what enpoint
        .accept(<typeOfRequest>) // what kind of request is accepted
        .andExpect(status(<expectedStatusOfTheResponse>))) // what is expected status of the response
        // and possible other parts
```
EXAMPLE: 
```java
final var result = mockMvc.perform(put(CREATOR_COLLECTION_ENDPOINT + preservedCollection.id())
        .contentType(MediaType.APPLICATION_JSON)
        .content(mapper.writeValueAsString(colUpdateDTO))
```


-`@WebMvcTest(<ClassUnderTest.class>)` - 
- `@MockBean`
- `@Mock`