# TO attach a js file in django admin is a bit tricky

first you need ot overwrite the admin templates

then you have to write the js

As django already using jquery , you can't include your new one(as it cause confilict)
so django provided a way to use the django jquery

```javascript
window.addEventListener("load", () => {
make sure to use window load eventlistner in case you are including other js libraries
(function ($) {
// now inside this function you can happily use $ (as we use in normal jquery)
// or you can simply use django.jQuery
})(django.jQuery);

});
````

# Adding other libraris like jquery-confirm or select2

### you need to edit the source code of these library to work inside django admin
### as this libraries have the dependency on jquery whe have to modify it otherwise it will give
`Uncaught TypeError: $ is not a function error`

####  Uncaught TypeError: $ is not a function
just add 

```javascript
var jQuery = django.jQuery
```
