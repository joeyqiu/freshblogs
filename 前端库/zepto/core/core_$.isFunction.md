## $.isFunction

```
$.isFunction(object)  ⇒ boolean
```

True if the object is a function.



判断传入的对象是否是一个函数



### source code

```
function isFunction(value) { return type(value) == "function" }

$.isFunction = isFunction

function type(obj) {
    return obj == null ? String(obj) :
      class2type[toString.call(obj)] || "object"
  }

var class2type = {};

// 给class2type赋值
$.each("Boolean Number String Function Array Date RegExp Object Error".split(" "), 		function(i, name) {
    class2type[ "[object " + name + "]" ] = name.toLowerCase()
})
```

其实就是通过typeof来判断类型