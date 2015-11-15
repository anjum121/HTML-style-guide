
> Sass creates Syntactically Awesome Style sheets, or at least thats what it is supposed to do.
When used effectively, Sass helps to build scalable and DRY CSS. When used incorrectly however, Sass can actually increase file size and add unnecessary or duplicate code.


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
     background-image:url(/img/image-one.jpg");
 }

 .image-two {
     @extend %bg-image;
     background-image:url(/img/image-two.jpg");
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
    background-image:url(/img/image-one.jpg") ;
}

.image-two {
    background-image:url(/img/image-two.jpg") ;
}
```






###### Attribute order
----

HTML attributes should come in this particular order for easier reading of code.

* `class`
* `id` , `name`
* `data-*`
* `src` , `for`, `type`, `href`, `value`
* `title`, `alt`
*



```javascript
<a class="..." id="..." data-toggle="modal" href="#">
  Example link
</a>

<input class="form-control" type="text">

<img src="..." alt="...">
```


###### Reducing markup
----

Whenever possible, avoid superfluous parent elements when writing HTML. Many times this requires iteration and refactoring, but produces less HTML. Take the following example:
```javascript
<!-- Not so great -->
<span class="avatar">
  <img src="...">
</span>

<!-- Better -->
<img class="avatar" src="...">
```


###### Forms
---
* Wrap radio and checkbox inputs and their text in <label>s. No need for for attributes here—the wrapping automatically associates the two.
* Form buttons should always include an explicit type. Use primary buttons for the `type="submit"` button and regular buttons for `type="button"`



###### Tables
---
* Make use of `<thead>`, `<tfoot>`, `<tbody>`, and `<th>` tags (and scope attribute) when appropriate

 ```javascript
<table summary="This is a chart of invoices for 2011.">
  <thead>
    <tr>
      <th scope="col">Table header 1</th>
      <th scope="col">Table header 2</th>
    </tr>
  </thead>
  <tfoot>
    <tr>
      <td>Table footer 1</td>
      <td>Table footer 2</td>
    </tr>
  </tfoot>
  <tbody>
    <tr>
      <td>Table data 1</td>
      <td>Table data 2</td>
    </tr>
  </tbody>
</table>
```

