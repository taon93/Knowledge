Thymeleaf is an framework to edit and post HTML documents from your java spring application, to post thymeleaf HTML document:
1. Add thymeleaf dependency
2. Create html document under resources/teplates 
3. Return name of the html template from the controller in the string format: for example to return books.html template
return "books" from the controller. 
You may add Model as an argument to the controller - this is context type of argument that may be accessible from thymeleaf template,
for example: to acces publishers DAO in the `books.html` tymeleaf template, invoke in the controller: `model.addAttribute("publishers", publisherService.getPublishers())`
where `model` is of type `Model`, `"publishers"` is the name of the attribute under which it will be available in the template. Access publishers in the template by invoking
for example: 
```html
<tr th:each="publisher : ${publishers}">
    <td th:text="${publisher.name}">template text </td>
    <td th:text="${publisher.yearOfCreation}">template text </td>
    <td th:text="${publisher.isbn}">template text </td>
</tr>
```
`th:text` will replace text of the document by one provided by thymeleaf, `th:each` - iterates over collection
HTML template to use thymeleaf has to have it included: in the `<html>` tag you have to define attribute: `xmlns:th="http://www.thymeleaf.org"'

