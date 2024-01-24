Klasa Arrays ma dwie (tak naprawdę więcej) metod equals: jedna jest
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