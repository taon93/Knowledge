- **DDL: *Data Definition Language:*** It specifies how the DataBase is structured, how tables are organized, what are the constraints of the columns,
and database schemas
- **DML: *Data Manipulation Language:*** It is used to query operations on the data: such as - insert, update, delete. 
- **Database Constraints:** rules applied to the data, for example, min value, max value, not null, unique, primary key, foreign key.
- **Database schema:** logical structures in data, specifies among others: what are the privileges of the given user, organizing database objects into
logical groups, gives more control over data access and security. 
### Miscellaneous
- query root: class `@Entity` from which query originates, is synonymous to the `FROM` keyword in SQL.
- unwraping `SessionFactory` from `EntityMenagerFactory`: 
```java
private static SessionFactory unwrapSessionFactory(EntityMenagerFactory emf) {
    return emf.unwrap(SessionFactory.class);
}
```
- hibernate requires for entity to have no args constructor.
- Useful properties in `application.properties` for debugging hibernate:
```java
// shows sql queries invoked via hibernate
spring.jpa.properties.hibernate.show_sql=true
// format showed queries: 
spring.jpa.properties.hibernate.format_sql=true
// Showes binded parameters (object -> SQL table)
logging.level.org.hibernate.type.descriptor.sql=true
// Shows database 'URL' under which H2 database is avaliable
spring.h2.console.enabled=true
```
### TESTING
- `@DataJpaTest` - by default, spring testing environment gives entire context to the test class: it is suboptimal for larger projects with 
greater number of moving pieces. To work around this, you may give testing class only part of Spring context: splice: `@DataJpaTest` is
an annotation which extracts part of the context related to the JPA and Data, and uses this part for test suite. This is class annotation.
By default `@DataJpaTest` will not scan packages for beans discovery: to do, so you must add additional annotation: `@ComponentScan` you may add parameter
with specification: which exact packages should be scanned: this may be done, for example, for bootstrapping the database. 
- Test methods are wrapped inside JPA transaction
