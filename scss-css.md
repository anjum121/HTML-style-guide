
> Sass creates Syntactically Awesome Style sheets, or at least thats what it is supposed to do.
When used effectively, Sass helps to build scalable and DRY CSS. When used incorrectly however, Sass can actually increase file size and add unnecessary or duplicate code.

###### Spacing
* Use soft-tabs with a two space indent. Spaces are the only way to guarantee code renders the same in any person’s environment.
* Put spaces after `: `in property declarations.
* Put spaces before `{` in rule declarations.
* Put line breaks between rulesets.
* When grouping selectors, keep individual selectors to a single line.
* Place closing braces of declaration blocks on a new line.
* Each declaration should appear on its own line for more accurate error reporting.



###### Formatting
* Use hex color codes `#000` unless using `rgba()` in raw CSS (SCSS' `rgba()` function is overloaded to accept hex colors as a param, e.g., `rgba(#000, .5)`).
* Use `//` for comment blocks (instead of `/* */`).
* Avoid specifying units for zero values, e.g., `margin: 0;` instead of `margin: 0px;`.
* As a rule of thumb, avoid unnecessary nesting in SCSS. At most, aim for three levels. If you cannot help it, step back and rethink your overall strategy .

Here are some good examples that apply the above guidelines:

```javascript
// Example of good basic formatting practices
.styleguide-format {
  color: #000;
  background-color: rgba(0, 0, 0, .5);
  border: 1px solid #0f0;
}

// Example of individual selectors getting their own lines (for error reporting)
.multiple,
.classes,
.get-new-lines {
  display: block;
}

// Avoid unnecessary shorthand declarations
.not-so-good {
  margin: 0 0 20px;
}
.good {
  margin-bottom: 20px;
}
```




###### Structure Your Sass
----


* Getting your site structure correct from the beginning is vital for any new Sass project.
* Using partials allows to break the CSS up into smaller more manageable blocks of code that are easier to maintain and develop.
* Partial files are created using an underscore and are not output as separate CSS files
* Each partial should be imported using a master Sass file (global.scss) in the root of the Sass folder.

```javascript
vendor/
base/
|
|-- _variables.scss
|-- _mixins.scss
|-- _placeholders.scss

framework/
modules/
main.scss
```

###### Use Sass Variables More Effectively
----

Variables are one of the more straightforward features of Sass but are still on occasion used incorrectly. Creating a site-wide naming convention is essential when working with Variables. Without one, they become harder to understand and re-use.

Bad examples
```javascript
$link: #ffa600;
$listStyle: none;
$radius: 5px;
```

Good examples
```javascript
$orange: #ffa600;
$grey: #f3f3f3;
$blue: #82d2e5;

$link-primary: $orange;
$link-secondary: $blue;
$link-tertiary: $grey;

$radius-button: 5px;
$radius-tab: 5px;
```


###### Reduce Mixin Usage
----

A mixin is a great way to include sections of code multiple times within a site. However, including a mixin is the same as copying and pasting the styles throughout the CSS file
```javascript
@mixin rounded-corner($arc) {
    -moz-border-radius: $arc;
    -webkit-border-radius: $arc;
    border-radius: $arc;
}
```

This `rounded-corner` mixin can be used in any situation simply by changing the value of $arc, making it a worthwhile mixin:

```javascript
.tab-button {
     @include rounded-corner(5px);
}

.cta-button {
     @include rounded-corner(8px);
}
```


###### Placeholders
----

Unlike mixins, placeholders can be used multiple times without adding any duplicate code. This makes them a much friendlier option for outputting DRY CSS:

```javascript
 %bg-image {
     width: 100%;
     background-position: center center;
     background-size: cover;
     background-repeat: no-repeat;
 }

 .image-one {
     @extend %bg-image;
     background-image:url(/img/image-one.jpg);
 }

 .image-two {
     @extend %bg-image;
     background-image:url(/img/image-two.jpg);
 }
```


compiled CSS:


```javascript
.image-one, .image-two {
    width: 100%;
    background-position: center center;
    background-size: cover;
    background-repeat: no-repeat;
}

.image-one {
    background-image:url(/img/image-one.jpg) ;
}

.image-two {
    background-image:url(/img/image-two.jpg) ;
}
```






###### Pixels vs. ems
----

Use px for font-size, because it offers absolute control over text. Additionally, unit-less line-height is preferred because it does not inherit a percentage value of its parent element, but instead is based on a multiplier of the font-size.


