## General
- `.isEmpty()` vs `.isBlank()` - `isEmpty` returns true when String is `null` or `""`, `isBlank` returns true also when String consists only of whitespaces.
- **There is better method than `trim()` for removing whitespaces:** `strip()`, `stripLeading()`, `stripTrailing()`
- `StringBuilder` vs `StringBuffer` - `StringBuffer` is less efficient but enables multithreaded operations, API of the classes are identical
- You can print starting directory of the program by calling: `System.getProperty("user.dir")`

### String blocks:
They make it easier to write string literals that take more than one line, to make String block, start it and end it with `"""`:
```
String text = """
Some text, 
in multiple lines,
using String blocks.
"""
```
To escape endline in String block use `\`:
```
`String text = """Hello, my name is Hal. \
Please enter your name:""" 
```
This will create only one line of text. 

### Conversion characters in String.format of System.out.printf (after %)
- `d` -> decimal integer
- `x` -> Hexadecimal integer
- `o` -> octal integer
- `f` -> fixed-point floating point (15.9)
- `e` -> exponential floating point (1.59e+01)
- `g` -> general floating point - shorter of `f` & `e`
- `a` -> hexadecimal floating point
- `s` -> string
- `c` -> character
- `b` -> boolean
- `h` -> hash code
- `%` -> literally "%"
- `n` -> platform dependant line separator
You can use it in `System.out.printf()`, `String.format()` and in literal `.formatted()`:   
`String message = "Hello, %s. Next year, you'll be %d".formatted(name, age + 1);`