## To add bootstrap to the project use: 
`npm install bootstrap@5.2.2`
`gem install bootstrap -v 5.2.2`
via package menager, or paste this lines in your ***file.html***:
`<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-Zenh87qX5JnK2Jl0vWa8Ck2rdkQ2Bzep5IDxbcnCeuOxjzrPF/et3URy9Bv1WTRi" crossorigin="anonymous">` in html part
`<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-OERcA2EqjJCMA+/3y+gxIOqMEjwtxJY7qPCqsdltbNJuaOe923+mo//f6V8Qbsw3" crossorigin="anonymous"></script>` in body part
For more information see: https://getbootstrap.com/

## Bootstrap containers and brakepoints: 
Classes specifying a grid system in bootstrap.
`container` : wraps the grid system.
`row` : rows in your grid.
`column` : columns in your rows.
In container padding is set dynamically, based on the size of the window on which it is displayed on.  
Bootstrap is built on breakpoints: when display reaches a threshold of some pixel width, style changes.  
You may set breakpoint that will be minimal size - when size of the display will reach this point, it will  
no longe change settings of the style to fit the window - the content of the container will take 100% width of the window:  
examples:
`container-lg`, `container-md`, `container-sm`, `container-fluid` - sets width of the continer to always be 100% of the window.  
Breakpoints are last before value in class name: col-**lg**-3, row-cols-**md**-4 etc.

### Columns:
Bootstrap is based on 12 colum system: display is by default divided into 12 columns. By default columns in the row  
are divided equally but you may change it, for example using `col-3`, `col-5` and `col-4` you are saying that row  
should be divided on three parts of ratio 1/4, 5/12 and 1/3. Columns come with default padding. 
- `col-auto` Sets column to fit the content.  
- `col` Default column type: takes as much space as it can in the row, for example for: `col-9`, `col` and `col`, second  
and third column will divide between themselves remaining space of 1/4 of the row.
****
Columns may take diffrent part of the screeen based on its size for example: `<div class="col-lg-4 col-8">` column will  
take 1/3 of the screen if it is large or above and 2/3 if it is smaller.  
Larger breakpoint always override smaller, **if breakpoint is not specified then default is `col-12`**  
If columns size add up to more than 12 then they will go to another row

**Offset**  
Offset works as columns but this is just empty space, it is used to move columns to the side, or create empty space. 

### Rows: 
You may specify how many columns fit every row by using: `row-cols-n` where n is number of columns in the row.   
You may specify gaps between rows by using `gy-n` where n is a value from 1 to 5.
- `gy` gap between rows
- `gx` gap between columns
- `g` gap between both columns and rows
****
**In rows everything must live inside a column.**

### Tables
You may style table by using `class="table"` inside table tag, it will style table in bootstrap way  
Color classes: they may be asigned to table itself or for separate tags: **thread, tbody, tr, th**
- table-denger: red
- table-succes: green
- table-primary: blue
- table-secondary: gray
- table-warning: yellow
- table-info: cyan
- table-light: white
- table-dark: black

****
You may specify styling of element that is hooverd on by: `table-hoover` - if you want to have this type of styling  
in specific column regardless of hoover you may do: `<tr class="table-active">`

You may create stripe pattern by: **table-striped / table-striped-columns**  

You may style borders of the table via **table-border/table-borderless**  
You may shrink table via: **table-sm**  

Via wrapping table in `<div class="responsive">` you will make it responsive: instead of overflow there will be slider  
`table-group-divider` is a class that may be put in thread, tbody or tfoot of the table: it puts divider between sections.  

### Forms: 

`form-control` class added to the input of the form styles the input putting highlight when focused and applying other things  
it has versions like `form-control-sm/lg` to change the size of input field. 
`form-label` class styles the label

**Floating Labels:**
```
<form>
    <div class="form-floating">
        <input type="someType" id="someId" class="form-control" placeholder="Placeholder is mandatory">
        <label for="someId" class="form-label">Label displayed inside floating lagbel</label>
    </div>
    ... rest of the form
</form>
```
**Input must precede label, placeholder must be present**

### Validating FORM:

```
<form novalidate>
    <input type="email" class="form-control" required>
    <div class="invalid-feedback">Invalid email</div>
    <div class="valid-feedback">Correct email</div>
    <button>Submit</button>
</form>
```
Add event listener of the form submiting, if .checkValidity on form fail, prevent submitting the form, else
add to the class list 'was-validated' - will display if correct values where inputted upon submitting.
