## $.type

```
$.type(object)  ⇒ string
```

Get string type of an object. Possible types are: null undefined boolean number string function array date regexp object error.

得到对象的类型，可能的类型：null, undefined, boolean, number, string , function ,array ,date, regexp, object, error

For other objects it will simply report “object”. To find out if an object is a plain JavaScript object, use `isPlainObject`

对于别的对象，可能只返回object类型.



### source code

```
$.type = type

function type(obj) {
  return obj == null ? String(obj) :
      class2type[toString.call(obj)] || "object"
}

// 给class2type赋值
$.each("Boolean Number String Function Array Date RegExp Object Error".split(" "), function(i, name) {
    class2type[ "[object " + name + "]" ] = name.toLowerCase()
  })
```





