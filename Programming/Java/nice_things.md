1. **REGEX**: Znajdywanie grup powtarzającymi się takimi samymi znakami w Stringu 
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

2. Klasa Arrays ma dwie (tak naprawdę więcej) metod equals: jedna jest 
dziedziczona z Object: `.equals(Object o)` który działa jak operator `==` druga jest statyczną implementacją klasy: `Arrays<T>.equals(T arr1, T arr2)` 
i porównuje elementy wewnątrz tablicy i długość tablic. Trzeba na to uważać - często kolekcje używają wersji `.equals(Object o)`, co może spowodować, że 
identyczne tablice będą traktowane jako różne, bo mają inne adresy. Rozwiązaniem jest używanie `List` lub wrappera na daną listę, który nadpisze metody 
`.hashCode()` i `equals(Object o)`. Przykład: zamiast `int[]`: 
```
record Row(int[] row) {
    @Override
    public int hashCode() {
        return Arrays.hashCode(row);
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Row row1 = (Row) o;
        return Arrays.equals(row, row1.row);
    }
}
```