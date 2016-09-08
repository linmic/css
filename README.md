# CNYES CSS / Sass Styleguide

*Heavily inspired / extended by [Airbnb CSS / Sass Styleguide](https://github.com/airbnb/css)*

## Terminology

### Rule declaration

A “rule declaration” is the name given to a selector (or a group of selectors) with an accompanying group of properties. Here's an example:

```css
.listing {
  font-size: 18px;
  line-height: 1.2;
}
```

### Selectors

In a rule declaration, “selectors” are the bits that determine which elements in the DOM tree will be styled by the defined properties. Selectors can match HTML elements, as well as an element's class, ID, or any of its attributes. Here are some examples of selectors:

```css
.my-element-class {
  /* ... */
}

[aria-hidden] {
  /* ... */
}
```

### Properties

Finally, properties are what give the selected elements of a rule declaration their style. Properties are key-value pairs, and a rule declaration can contain one or more property declarations. Property declarations look like this:

```css
/* some selector */ {
  background: #f1f1f1;
  color: #333;
}
```

## CSS

### Formatting

* Use soft tabs (2 spaces) for indentation
* Prefer dashes over camelCasing in class names.
* Do not use ID selectors
* When using multiple selectors in a rule declaration, give each selector its own line.
* Put a space before the opening brace `{` in rule declarations
* In properties, put a space after, but not before, the `:` character.
* Put closing braces `}` of rule declarations on a new line
* Put blank lines between rule declarations

**Bad**

```css
.avatar{
    border-radius:50%;
    border:2px solid white; }
.no, .nope, .not_good {
    // ...
}
#lol-no {
  // ...
}
```

**Good**

```css
.avatar {
  border-radius: 50%;
  border: 2px solid white;
}

.one,
.selector,
.per-line {
  // ...
}
```

### Comments

* Prefer line comments (`//` in Sass-land) to block comments in most cases, except detailed comments and mixin definitions. e.g.

    ```scss
    /*
     * This mixin is for setting border radius with cross-browser support.
     * @param {$radius}
     * /
    @mixin border-radius($radius) {
      -webkit-border-radius: $radius;
      -moz-border-radius: $radius;
      -ms-border-radius: $radius;
      border-radius: $radius;
    } 
    ```

* Prefer comments on their own line. Avoid end-of-line comments.
* Write detailed comments for code that isn't self-documenting:
  - Uses of z-index
  - Compatibility or browser-specific hacks

### Use shorten syntax whenever possible

**Bad**

```css
.block {
  border-top: 1px solid #ccc;
  border-right: 2px solid #ccc;
  border-bottom: 3px solid #ccc;
  border-left: 4px solid #ccc;
}
```

**Good**

```css
.block {
  border: 1px solid #ccc;
  border-width: 1px 2px 3px 4px;
}
```

### ID selectors

While it is possible to select elements by ID in CSS, it should generally be considered an anti-pattern. ID selectors introduce an unnecessarily high level of [specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) to your rule declarations, and they are not reusable.

For more on this subject, read [CSS Wizardry's article](http://csswizardry.com/2014/07/hacks-for-dealing-with-specificity/) on dealing with specificity.

### JavaScript hooks

Avoid binding to the same class in both your CSS and JavaScript. Conflating the two often leads to, at a minimum, time wasted during refactoring when a developer must cross-reference each class they are changing, and at its worst, developers being afraid to make changes for fear of breaking functionality.

We recommend creating JavaScript-specific classes to bind to, prefixed with `.js-`:

```html
<button class="btn btn-primary js-request-to-book">Request to Book</button>
```

### Border / Outline

Use `0` instead of `none` to specify that a style has no border / outline.

**Bad**

```css
.foo {
  border: none;
}
```

**Good**

```css
.foo {
  border: 0;
}
```

### `@supports`

It's always a better choice of feature detection over browser detection, hence the use of `@supports` are encouraged in almost all modern browsers, including Edge.

Even IEs don't support `@supports` you can still use it for better css solutions in well organised form.

### !important

Avoid this whenever possible.

## Sass

### Syntax

* Use the `.scss` syntax, never the original `.sass` syntax
* Order your regular CSS and `@include` declarations logically (see below)

### Prevent magic numbers/colors

Use a _vars.scss to declare all variables, try to put every reusable number/color here. More importantly, remember to use them.

### Ordering of property declarations

1. Property declarations

    List all standard property declarations, anything that isn't an `@include` or a nested selector.

    ```scss
    .btn-green {
      background: green;
      font-weight: bold;
      // ...
    }
    ```

2. `@include` declarations

    Grouping `@include`s at the begin point makes it easier to read the entire selector.

    ```scss
    .btn-green {
      @include transition(background 0.5s ease);
      background: green;
      font-weight: bold;  
      // ...
    }
    ```
3. `@extend` declarations

    Grouping `@extend`s at the end makes it works as you expected as in css order does matters.
    
    ```scss
    .btn-green {
      // ...
      background: green;
      font-weight: bold;  
      @extend %color-gray;
    }
    ```

4. Nested selectors

    Nested selectors, *if necessary*, go last, and nothing goes after them. Add whitespace between your rule declarations and nested selectors, as well as between adjacent nested selectors. Apply the same guidelines as above to your nested selectors.

    ```scss
    .btn {
      background: green;
      font-weight: bold;
      @include transition(background 0.5s ease);

      .icon {
        margin-right: 10px;
      }
    }
    ```
    
5. Refrerencing Parent Selectors: &
	
	Always use referencing parent selector / nested selectors to declare rules of own states, e.g. `:hover`.
	
    ```scss
	a {
	  color: #333;
	  
	  &:hover,
	  &.active {
	    text-decoration: underline;
	  }
		
	  &.focus {
	    outline: 0;
	  }
	}
	```
	
	- This should be placed at the very top over other non-parant-referencing selectors.
	- This doesn't really increase the nesting levels.
	- This is more readable and maintainable.

### Variables

Prefer dash-cased variable names (e.g. `$my-variable`) over camelCased or snake_cased variable names. It is acceptable to prefix variable names that are intended to be used only within the same file with an underscore (e.g. `$_my-variable`).

### Placeholders

Use placeholders to do the DRY principle over `@mixin` and `@extend` directives.

- It produces less code without code duplication.
- It's intuitive, you know what's gonna happen once you place the placeholder.
- You should really be careful not to produce duplicated properties, the same rule goes for `@include` and normal `@extend` as well.

    ```scss
    @mixin vertical-center {
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
    }
    
    .vertical-centered-image {
      @include vertical-centered-image;
      position: absolute;
      left: 0;
      top: 0;
      // duplicated `position` and `top` here
    }
    ```

### Mixins

If you need a function with custom parameters to do the DRY stuff, use mixin.

e.g. cross-browser *text-clamp*:

```scss
@mixin text-clamp($line_height, $lines, $bg-color: #f1f1f1) {
  position: relative;
  height: $line_height * $lines;
  display: block;
  display: -webkit-box;
  -webkit-line-clamp: $lines;
  -webkit-box-orient: vertical;
  overflow: hidden;

  &::after {
    content: '';
    text-align: right;
    bottom: 0;
    right: 0;
    width: 20%;
    display: block;
    position: absolute;
    height: $line_height;
    background: linear-gradient(to right, rgba($bg-color, 0), rgba($bg-color, 1) 75%);
  }

  @supports (-webkit-line-clamp: 1) {
    &::after {
      display: none;
    }
  }
}
```

### Extend directive

Do not use `@extend` to extend classes, only placeholders.

### Nested selectors

**Do not nest selectors more than three levels deep!**

```scss
.page-container {
  .content {
    .profile {
      // STOP!
    }
  }
}
```

When selectors become this long, you're likely writing CSS that is:

* Strongly coupled to the HTML (fragile) *—OR—*
* Overly specific (powerful) *—OR—*
* Not reusable


Again: **never nest ID selectors!**

If you must use an ID selector in the first place (and you should really try not to), they should never be nested. If you find yourself doing this, you need to revisit your markup, or figure out why such strong specificity is needed. If you are writing well formed HTML and CSS, you should **never** need to do this.

## WebFont Loader

WebFont Loader is a well-designed SDK for webfont loading management. The basic concept is to allow us to apply webfonts as soon as they are loaded. We should always employ this to prevent content blocking.

To achieve this, we need to make sure the loaded font can successfully override the default font. In this case, we don't use font's shorten syntax on any selectors other than `:root`, because it will specify font-family and might cause the webfont fail to be applied.

**Bad**

```scss
:root {
  // we firstly set a websafe font as our default font-family
  font: 400 16px/1.7 Arial, sans-serif;
}

.blah {
  h5.intend-to-be-replaced {
    // this might failed applying loaded webfont
    font: 400 18px/1.9 Arial, sans-serif;
  }
}

:global(.wf-opensans-n3-active) {
  :root {
    font-family: $font_family;
  }
}

```
**Good**

```scss
:root {
  // we firstly set a websafe font as our default font-family
  font: 400 16px/1.7 Arial, sans-serif;
}

.blah {
  // this nested class' font family will be successfully replaced with new one
  h5.intend-to-be-replaced {
    font-size: 18px;
    line-height: 1.9;
  }
}

:global(.wf-opensans-n3-active) {
  :root {
    font-family: $font_family;
  }
}
```

## CSS Module

### Prevent `:global` in most cases

The whole idea of CSS Module is to prevent global scope and enforced explicit dependencies. You should only use it on third party library's stylesheet and some legacy-handling styles.

### LocalIndentName

- dev: `[local]_[hash:base64:3]`
- prod: `_[hash:base64:3]`

It's essential to minimize the output stylesheet as best as we can without losing readibility for debugging in dev mode.

