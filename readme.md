# SASS 

Sass is a scripting language that is interpreted into Cascading Style Sheets 

## Sass Fundamelntals

**CSS Pitfalls**

- no variables 
- global - unintended behaviours

**preprocessors**

a preprocessor is a program 

​	that processes its input data 

​	to produce output that is used as input to another program. 

- sass / less / stylus 

**why to use ?**

- add stuff that should be right away in website ! :)
- faster to build and maintain 
- DRY 
- EASY TO setup

### 1.1 Preprocessing

```bash
sass input.scss output.css 
sas --watch input.scss output.css 
```

we can watch and output to dirs by using folder paths - seperate with `:`

```bash
sass --watch app/sass:public/stylesheets
```





## 2. Sass Basics - nesting

.scss - we work with that file format

```html
<div class="container">
    <div class="left-area">...</div>
    </div>   
</div>
```

```scss
.container {
    .left-area {
        
    }
}
```

**direct descendant**

```scss
.container {
    > .left-area {
        
    }
}
```

```scss
nav {
  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }

  li { display: inline-block; }

  a {
    display: block;
    padding: 6px 12px;
    text-decoration: none;
  }
}

```





**parent selector**

```html
<div class="container right-nav">
    
</div>
```

```scss
.container{
    &.right-nav
}
```

parent selector allow us to add css style based on parent :  

```scss
.button {
    color:#333;
    .theme-dark & {
        color:#fff;
    }
}


```

===

```css
.button {
    
}
.theme-dark .button {}
```

### 2. Nesting

```scss
button {
    padding:2px 10px;
    border:1px solid #c46;
    border-radius: 2px;

    &.btn-primary {
        color:white;
        background-color: #c46;
    }
    &.btn-secondary {
        background: #edbcc8;
        color:black;    
    }
    &:disabled {
        opacity: 0.5;
    }
}
```

transform to 

```css
button {
  padding: 2px 10px;
  border: 1px solid #c46;
  border-radius: 2px;
}
button.btn-primary {
  color: white;
  background-color: #c46;
}
button.btn-secondary {
  background: #edbcc8;
  color: black;
}
button:disabled {
  opacity: 0.5;
}
```

command to run

```bash
sass style.scss output.css
sass --watch style.scss output.css
```

### 3. Parent Selectors

`&` - parent selector --> refer to the outer selector 

```css
.alert {
  &:hover {
    font-weight:bold;
  }
}
```

convert to css

```css
.alert:hover {
  font-weight: bold;
}
```



```scss
.accordion {
  max-width: 600px;
  margin: 4rem auto;
  width: 90%;
  font-family: "Raleway", sans-serif;
  background: #f4f4f4;

  &__copy {
    display: none;
    padding: 1rem 1.5rem 2rem 1.5rem;
    color: gray;
    line-height: 1.6;
    font-size: 14px;
    font-weight: 500;

    &--open {
      display: block;
    }
  }
}

```

```css
.accordion {
  max-width: 600px;
  margin: 4rem auto;
  width: 90%;
  font-family: "Raleway", sans-serif;
  background: #f4f4f4;
}
.accordion__copy {
  display: none;
  padding: 1rem 1.5rem 2rem 1.5rem;
  color: gray;
  line-height: 1.6;
  font-size: 14px;
  font-weight: 500;
}
.accordion__copy--open {
  display: block;
}

```

## 4. Variables

CSS's @import allows for splitting CSS into smaller

`@import` - new http request 

bringing module from JS --- component way of thinking 

partials designed to be imported

>**src/sass**
>
>_layout.scss  -- partials
>
>_variable.scss  -- partials
>
>app.scss
>
>other.scss



Sass compiler will compile only app and other 

```scss
@import 'bootstrap'; // from library
@import '_layouit';   // partial in project 
```



```scss
$error_color : #f00 !default; // tell if error color is set somewhere else then skip it
.alert-error {
    $text_color: #ddd; // local variable - only nested things has access 
    background-color: $error_color;
    color: $text_color;
    text-shadow: 0 0 2px darken($text_color, 40%); // darken() - sass function 
}
```



in our app.scss

```css
@import '_variables';
body {
  h1 {
    ...
  }
}
```

### 5. Mixins

- Re-use of style
- function that return some style
- merged by @include 
- seperate from styles like variables 



```scss
@mixin alert-text {
  background-color:#f00;
  color:white;
  font-variant: small-caps;
}
.error-text {
  @include alert-text;
}
```

**args**

```scss
@mixin alert-text($color) {
  background-color:$color;
  color:white;
  font-variant: small-caps;
}
.error-text {
  @include alert-text(blue);
}
```



**default argument values**

```scss
@mixin alert-text($color: #ff3) {
  background-color:$color;
  color:white;
  font-variant: small-caps;
}
/*
** Two way of providing args
*/
.h1 {
  @include alert-text(blue);
}
.h2 {
  @include alert-text($color: green)
}
```

**passing a declaration block**

```scss
@mixin foo($color) {
  color:$color;
  .inner {
    @content
  }
}
.btn {
  @include foo(#c69) {
    color:red;
  }
}
```

css:

```css
.btn {
  color:#c69;
}
.btn .inner {
  color:red;
}
```

### 6. Sass Functions

built in functions

```scss
rgb($red, $green, $blue) // create color 
```

### 7. Control Flow - @if 

```scss
@mixin foo($size) {
	font-size: $size;
  @if $size > 20 {
    line-height: $size;
  }
}
```

