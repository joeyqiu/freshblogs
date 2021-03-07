## $.fn

`Zepto.fn` is an object that holds all of the methods that are available on Zepto collections, such as [`addClass()`](https://zeptojs.com/#addClass), [`attr()`](https://zeptojs.com/#attr), and other. Adding a function to this object makes that method available on every Zepto collection.

Here’s an example implementation of Zepto’s [`empty()`](https://zeptojs.com/#empty) method:

```
$.fn.empty = function(){
  return this.each(function(){ this.innerHTML = '' })
}
```



### source code

```
zepto.Z.prototype = Z.prototype = $.fn

$.fn = {
    constructor: zepto.Z
    xxx
};
```

$.fn给每个zepto对象上添加方法和属性。