### Golden rule
> Every line of code should appear to be written by a single person, no matter the number of contributors.
------------------

###### Syntax
----

* Use soft tabs with two spaces—they're the only way to guarantee code renders the same in any environment.
* Nested elements should be indented once (two spaces).
* Always use double quotes, never single quotes, on attributes.
* Don’t omit optional closing tags (e.g. `</li>` or `</body>`).

```javascript
<!DOCTYPE html>
<html>
  <head>
    <title>Page title</title>
  </head>
  <body>
    <img src="images/company-logo.png" alt="Company">
    <h1 class="hello-world">Hello, world!</h1>
  </body>
</html>
```

###### HTML5 doctype
----

Enforce standards mode and more consistent rendering in every browser possible with this simple doctype at the beginning of every HTML page.


```javascript
<!DOCTYPE html>
<html>
  <head>
  </head>
</html>
```


###### IE compatibility mode
----

Internet Explorer supports the use of a document compatibility `<meta>` tag to specify what version of IE the page should be rendered as. Unless circumstances require otherwise, it's most useful to instruct IE to use the latest supported mode with edge mode.

```javascript
<meta http-equiv="X-UA-Compatible" content="IE=Edge">
```


###### CSS and JavaScript includes
----

Per HTML5 spec, typically there is no need to specify a type when including CSS and JavaScript files as `text/css` and `text/javascript` are their respective defaults.
```javascript
<!-- External CSS -->
<link rel="stylesheet" href="code-guide.css">

<!-- In-document CSS -->
<style>
  /* ... */
</style>

<!-- JavaScript -->
<script src="code-guide.js"></script>
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

