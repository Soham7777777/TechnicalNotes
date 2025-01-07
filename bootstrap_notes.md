# Bootstrap notes:
---


## Setup:
- Download css and js files form website and unzip it, also download icons zip from website

- attach it to the html like this:
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
        <link rel="stylesheet" href="./bootstrap-5.3.3-dist/css/bootstrap.css">
        <link rel="stylesheet" href="./bootstrap-icons-1.11.2/font/bootstrap-icons.min.css">
    </head>
    <body>
        
    
        <script src="./bootstrap-5.3.3-dist/js/bootstrap.bundle.js"></script>
    </body>
    </html>
    ```

## Grid System:
---

- `.container` is the building block for grid system which is made with flexbox.
                    						X-small small   medium  large   X-large     XX-large   
                                        	<5	76px  ≥576px  ≥768px  ≥992px  ≥1200px    ≥1400px
                .container 	        100% 	540px 	720px 	960px 	1140px 	1320px
                .container-sm 	    100% 	540px 	720px 	960px 	1140px 	1320px
                .container-md 	    100% 	100% 	720px 	960px 	1140px 	1320px
                .container-lg 	    100% 	100% 	100% 	960px 	1140px 	1320px
                .container-xl 	    100% 	100% 	100% 	100% 	1140px 	1320px
                .container-xxl 	    100% 	100% 	100% 	100% 	100% 	1320px
                .container-fluid 	100% 	100% 	100% 	100% 	100% 	100%

- columns can be used as : `col-{breakpoint}-{size}`
    - the breakpoint can be None, which mean for all break points otherwise columns will be stacked on each other before that specified `{breakpoint}`.
    - the size can be 1 to 12 or `auto`
    - `col-{breakpoint}-auto` classes will size columns based on the natural width of their content.
    
- columns are made with flexbox so wrapping tham with `row justify-content-center` will align tham.

- columns will be on auto-layout if there is **no** explicit numbers like `col-3`

- if the only `col-md-{size}` is available than breakpoints with smaller than `md` will have stacked columns on top of each other.
    ```html
      <div class="container text-center">
        <div class="row">
          <div class="col-md bg-primary">first</div>
          <div class="col-md bg-success">second</div>
          <div class="col-md bg-primary">second</div>
          <div class="col-md bg-success">second</div>
          <div class="col-md bg-primary">second</div>
          <div class="col-md bg-success">second</div>
        </div>
      </div>
    ```
- `row-cols-{breakpoint}-{size}` classes can be used to quickly create layouts which will follow this rule: `{size} columns in 1 row`
    - `{size}` can be `auto` which means size of column will be variable based on its content.
    - `{breakpoint}` can be none which means for all break points, otherwise columns will stacked on each other before `{breakpoint}`.
    ```html
    <div class="container text-center">
    <div class="row row-cols-3">
      <div class="col-md bg-primary">first</div>
      <div class="col-md bg-success">second</div>
      <div class="col-md bg-primary">second</div>
      <div class="col-md bg-success">second</div>
      <div class="col-md bg-primary">second</div>
      <div class="col-md bg-success">second</div>
    
      <div class="col-md bg-primary">first</div>
      <div class="col-md bg-success">second</div>
      <div class="col-md bg-primary">second</div>
      <div class="col-md bg-success">second</div>
      <div class="col-md bg-primary">second</div>
      <div class="col-md bg-success">second</div>
    
      <div class="col-md bg-primary">first</div>
      <div class="col-md bg-success">second</div>
      <div class="col-md bg-primary">second</div>
      <div class="col-md bg-success">second</div>
      <div class="col-md bg-primary">second</div>
      <div class="col-md bg-success">second</div>
    
      <div class="col-md bg-primary">first</div>
      <div class="col-md bg-success">second</div>
      <div class="col-md bg-primary">second</div>
      <div class="col-md bg-success">second</div>
      <div class="col-md bg-primary">second</div>
      <div class="col-md bg-success">second</div>
    
      <div class="col-md bg-primary">first</div>
      <div class="col-md bg-success">second</div>
      <div class="col-md bg-primary">second</div>
      <div class="col-md bg-success">second</div>
      <div class="col-md bg-primary">second</div>
      <div class="col-md bg-success">second</div>
    </div>
  </div>
  
  <hr>
  
  <div class="container text-center">
    <div class="row row-cols-3">
      <div class="col bg-primary">first</div>
      <div class="col bg-success">second</div>
      <div class="col bg-primary">second</div>
      <div class="col bg-success">second</div>
      <div class="col bg-primary">second</div>
      <div class="col bg-success">second</div>
  
      <div class="col bg-primary">first</div>
      <div class="col bg-success">second</div>
      <div class="col bg-primary">second</div>
      <div class="col bg-success">second</div>
      <div class="col bg-primary">second</div>
      <div class="col bg-success">second</div>
  
      <div class="col bg-primary">first</div>
      <div class="col bg-success">second</div>
      <div class="col bg-primary">second</div>
      <div class="col bg-success">second</div>
      <div class="col bg-primary">second</div>
      <div class="col bg-success">second</div>
  
      <div class="col bg-primary">first</div>
      <div class="col bg-success">second</div>
      <div class="col bg-primary">second</div>
      <div class="col bg-success">second</div>
      <div class="col bg-primary">second</div>
      <div class="col bg-success">second</div>
  
      <div class="col bg-primary">first</div>
      <div class="col bg-success">second</div>
      <div class="col bg-primary">second</div>
      <div class="col bg-success">second</div>
      <div class="col bg-primary">second</div>
      <div class="col bg-success">second</div>
    </div>
  </div>
  
  <hr>
  
  <div class="container text-center">
    <div class="row row-cols-3">
      <div class="bg-primary">first</div>
      <div class="bg-success">second</div>
      <div class="bg-primary">second</div>
      <div class="bg-success">second</div>
      <div class="bg-primary">second</div>
      <div class="bg-success">second</div>
  
      <div class="bg-primary">first</div>
      <div class="bg-success">second</div>
      <div class="bg-primary">second</div>
      <div class="bg-success">second</div>
      <div class="bg-primary">second</div>
      <div class="bg-success">second</div>
  
      <div class="bg-primary">first</div>
      <div class="bg-success">second</div>
      <div class="bg-primary">second</div>
      <div class="bg-success">second</div>
      <div class="bg-primary">second</div>
      <div class="bg-success">second</div>
  
      <div class="bg-primary">first</div>
      <div class="bg-success">second</div>
      <div class="bg-primary">second</div>
      <div class="bg-success">second</div>
      <div class="bg-primary">second</div>
      <div class="bg-success">second</div>
  
      <div class="bg-primary">first</div>
      <div class="bg-success">second</div>
      <div class="bg-primary">second</div>
      <div class="bg-success">second</div>
      <div class="bg-primary">second</div>
      <div class="bg-success">second</div>
    </div>
  </div>
  ```
- The first section will show normal `.row` with 30 `.col-md` after `md` breakpoint, but before that breakpoint `col` is not defined so it will display 3 cols in 1 row

- the second section will show 3 cols in 1 row at every breakpoint, shows that `col` does not matter until a breakpoint is given.

- the third section will be same as 2nd section

- we can give `justify-content-{position}` and `align-items-{position}` to rows which will align the whole row across main-axis or cross-axis respectively.

- we can also give `align-self-{position}` to columns to align tham across row's cross-axis.

- If more than 12 columns are placed within a single row, each group of extra columns will, as one unit, wrap onto a new line.

- we can break columns in new line by specifying `w-100`:
    ```html
    <div class="container text-center">
      <div class="row">
        <div class="col-6 col-sm-3">.col-6 .col-sm-3</div>
        <div class="col-6 col-sm-3">.col-6 .col-sm-3</div>
    
        <!-- Force next columns to break to new line -->
        <div class="w-100"></div>
    
        <div class="col-6 col-sm-3">.col-6 .col-sm-3</div>
        <div class="col-6 col-sm-3">.col-6 .col-sm-3</div>
      </div>
    </div>
    ```
- we can also break at specific break point:
    ```html
    <div class="container text-center">
      <div class="row">
        <div class="col-6 col-sm-4">.col-6 .col-sm-4</div>
        <div class="col-6 col-sm-4">.col-6 .col-sm-4</div>
    
        <!-- Force next columns to break to new line at md breakpoint and up -->
        <div class="w-100 d-none d-md-block"></div>
    
        <div class="col-6 col-sm-4">.col-6 .col-sm-4</div>
        <div class="col-6 col-sm-4">.col-6 .col-sm-4</div>
      </div>
    </div>
    ```
- In the above snippent, the `w-100` will be applite at `md` and above breakpoints, before `md` there will be `display: none;`.

- The `.col-*` classes can also be used outside a `.row` to give an element a specific width. Whenever column classes are used as non-direct children of a `row`, the paddings are omitted.

- Margins can be applied by `m{direction}-{breakpoint}-{size}` on columns.
    - The `size` can have `auto` which will take every space it can get.
    
- The reordering can be applied on columns by `order-{breakpoint}-{value}`
    - the value can be 1 to 5 or `first` or `last`.

- The offsetting (shiffting columns to right) can be applied by `offset-{breakpoint}-{value}` on columns.
    - the columns will be shiffted right by `{value}` units at (and above) `{breakpoint}`.
    
- Gutters can be applied on `row` by `g{direction}-{breakpoint}-{value}`. The horizontal gutters are horizontal paddings inside column, The gutters will be applied on all columns while padding can only be given indevidually.

- Vertical gutters are space between two rows.

- The horizontal gutters can overflow, thus the solution is either to apply `overflow-hidden` or matching padding to the wrapper of row (here container). Same way we can prevent overflow of verticle gutters by giving `overflow-hidden` on wrapper of row.

- Apply `mx-0` to row to get edge-edge design.
