# Info-Tech CSS / Sass Styleguide

*A mostly reasonable approach to CSS and Sass*

## Table of Contents

  1. [Terminology](#terminology)
    - [Rule Declaration](#rule-declaration)
    - [Selectors](#selectors)
    - [Properties](#properties)
  1. [CSS](#css)
    - [Formatting](#formatting)
    - [Comments](#comments)
    - [ID Selectors](#id-selectors)
    - [JavaScript hooks](#javascript-hooks)
    - [Border](#border)
    - [Media Queries](#media-queries)
  1. [Sass](#sass)
    - [Syntax](#syntax)
    - [Ordering](#ordering-of-property-declarations)
    - [Variables](#variables)
    - [Mixins](#mixins)
    - [Extend directive](#extend-directive)
    - [Nested selectors](#nested-selectors)
  1. [Colours](#colours)
    - [Global colours](#global-colours)
    - [Colour variants](#colour-variants)

## Terminology

### Rule declaration

A “rule declaration” is the name given to a selector (or a group of selectors) with an accompanying group of properties. Here's an example:

```css
.listing {
  font-size: 1em;
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
* Prefer dashes over camelCasing in class names
* Use ID selectors sparingly (see [ID Selectors](#id-selectors))
* When using multiple selectors in a rule declaration, put all selectors on the same line
* Put a space before the opening brace `{` in rule declarations
* In properties, put a space after, but not before, the `:` character
* Put closing braces `}` of rule declarations on a new line
* Put blank lines between rule declarations

**Bad**

```css
.avatar{
    border-radius:50%;
    border:2px solid white; }
.no, 
.nope, 
.not_good {
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

.one, .selector, .per-line {
  // ...
}
```

### Comments

* Prefer line comments (`//` in Sass-land) to block comments.
* Prefer comments on their own line. Avoid end-of-line comments.
* Write detailed comments for code that isn't self-explanatory:
  - Uses of `z-index`
  - Compatibility or browser-specific hacks
  - Uses of `!important`
* Use comments to denote sections of related styles/selectors

### ID selectors

While it is possible to select elements by ID in CSS, this should be done sparingly.

* ID selectors are primarily for adding structure to your CSS
* Can be used to wrap page-specific styles
* Should **not** be used to apply actual styles to an element

**Bad**

```css
#ugly-button {
  // ...
}
```

**Good**

```css
#my-page-wrapper {
  .page-element {
    // ...
  }
}
```

### JavaScript hooks

Avoid binding to the same class in both your CSS and JavaScript. Conflating the two often leads to, at a minimum, time wasted during refactoring when a developer must cross-reference each class they are changing, and at its worst, developers being afraid to make changes for fear of breaking functionality.

We recommend creating JavaScript-specific classes to bind to, prefixed with `.js-`. Alternatively, sometimes it makes more sense to bind JavaScript events to data selectors. Here are examples of both.

```html
<button class="btn btn-primary js-request-to-book">Request to Book</button>

<div data-chart data-chart-type="pie" data-chart-series="[1, 2, 3, 4]"></div>
```
```javascript
$(document).on('click', '.js-request-to-book', function() {
  // ...
};

$('[data-chart]').each(function() {
  // ...
});
```

### Border

Use `none` instead of `0` to specify that a style has no border.

**Bad**

```css
.foo {
  border: 0;
}
```

**Good**

```css
.foo {
  border: none;
}
```

### Media Queries

* Nest media queries within selectors, rather than around them
* Order media queries from small screen sizes to large

**Bad**

```scss
@media only screen and (max-width: $screen-sm-max) {
  .thing {
    // ...
  }
  .other-thing {
    // ...
  }
}
```

**Good**

```scss
.thing {
  @media only screen and (max-width: $screen-sm-max) {
    // ...
  }
  @media only screen and (max-width: $screen-md-max) {
    // ...
  }
}
```

## Sass

### Syntax

* Use the `.scss` syntax, never the original `.sass` syntax
* Order your regular CSS, modifiers, `@include` declarations, and media queries logically (see below)

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

    Grouping `@include`s at the end makes it easier to read the entire selector.

    ```scss
    .btn-green {
      background: green;
      font-weight: bold;
      @include transition(background 0.5s ease);
      // ...
    }
    ```

3. Element modifiers

    Modifiers should always immediately follow the the standard property declarations and `@includes`s.
    
    ```scss
    .btn-green {
      background: green;
      font-weight: bold;
      @include transition(background 0.5s ease);
      
      &:hover {
        // ...
      }
    }
    ```

4. Media queries

    Viewport-specific styling should follow, and should always be nested within the selector (see [Media Queries](#media-queries)).
    
    ```scss
    .btn {
      background: green;
      font-weight: bold;
      @include transition(background 0.5s ease);

      &:hover {
        // ...
      }
      
      @media only screen and (min-width: $screen-sm-min) {
        // ...
      }
    }
    ```

5. Nested selectors

    Nested selectors, _if necessary_, go last, and nothing goes after them. Add whitespace between your rule declarations and nested selectors, as well as between adjacent nested selectors. Apply the same guidelines as above to your nested selectors.

    ```scss
    .btn {
      background: green;
      font-weight: bold;
      @include transition(background 0.5s ease);

      &:hover {
        // ...
      }
      
      @media only screen and (min-width: $screen-sm-min) {
        // ...
      }

      .icon {
        margin-right: 10px;
      }
    }
    ```

### Variables

* Prefer dash-cased variable names (e.g. `$my-variable`) over camelCased or snake_cased variable names.
* Variables should be listed at the top of the file

### Mixins

Mixins should be used to DRY up your code, add clarity, or abstract complexity--in much the same way as well-named functions. They should be listed after variables within a Sass file. Each project has a standard set of mixins, which can be referenced below.

_Coming soon:_
* infotech.com mixins
* DDR mixins
* Software reviews mixins

### Extend directive

`@extend` should be avoided because it has unintuitive and potentially dangerous behavior, especially when used with nested selectors. Even extending top-level placeholder selectors can cause problems if the order of selectors ends up changing later (e.g. if they are in other files and the order the files are loaded shifts). Gzipping should handle most of the savings you would have gained by using `@extend`, and you can DRY up your stylesheets nicely with mixins.

### Nested selectors

**Do not nest selectors more than three levels deep!**

```scss
#page-container {
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

## Colours

### Global colours

Most projects have a file a single colours file which defines all globally used colours as Sass variables. When writing styles, you should always use the variables rather than the HEX values.

White and black are the two exceptions that have no variables. The text literals (`white` and `black`) should be used instead.
```scss
.foo {
  border: black;
  background: white;
}
.foo-two {
  border: $green;
  background: $orange;
}
```

### Colour variants

Colour variants should be grouped logically, and ordered from light to dark. Within the colours file, colours should be listed alphabetically. For a given colour, there should be a lighter, light, regular, dark, and darker variant. If additional variants are needed beyond these five, a new variable/colour name should be used.

```scss
// Blue
$blue-lighter: #7CADD4;
$blue-light: #5191C5;
$blue: #2576B7;
$blue-dark: #1E5E92;
$blue-darker: #16476E;

// Info-Tech Blue
$infotech-blue-lighter: #7F919F;
$infotech-blue-light: #546C7F;
$infotech-blue: #29475F;
$infotech-blue-dark: #21394C;
$infotech-blue-darker: #192B39;
```
