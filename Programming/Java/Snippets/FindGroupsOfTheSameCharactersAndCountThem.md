**REGEX**: Znajdywanie grup powtarzającymi się takimi samymi znakami w String'u,
a potem iterowanie po grupach:
```
Pattern SAME_DIR = Pattern.compile("(.)\\1*"); // Najistotniejsza część

SAME_DIR.matcher(stringToMatch).results()... // zwraca Stream kolejnych matchów, żeby wyciągnąć String: match.group()        
// PRZYKŁAD
String result = SAME_DIR.matcher(stringToMatch).results()
    .map(match -> String.format("Char %c repeats %d", match.group().charAt(0), match.group().lenght()))
    .join("\n");
/* 
Dla stringToMatch == "aaaacc4444udll" 
result == "Char a repeats 4
Char c repeats 2
Char 4 repeats 4
Char u repeats 1
Char d repeats 1
Char l repeats 2"
*/
```