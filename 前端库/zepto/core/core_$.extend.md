# $.extend

```
$.extend(target, [source, [source2, ...]])  ⇒ target
$.extend(true, target, [source, ...])  ⇒ target
```

Extend target object with properties from each of the source objects, overriding the properties on target.

By default, copying is shallow. An optional `true` for the first argument triggers deep (recursive) copying.

把源对象上的属性扩展到目标对象，覆盖目标对象上的属性。默认情况下的拷贝是浅拷贝。第一个参数是true，则表示深拷贝。

```
var target = { one: 'patridge' },
    source = { two: 'turtle doves' }

$.extend(target, source)
//=> { one: 'patridge',
//     two: 'turtle doves' }
```





### source code

```
$.extend = function(target){
    var deep, args = slice.call(arguments, 1)
    if (typeof target == 'boolean') {
    // 如果第一个参数是true/false
      deep = target
      target = args.shift()
    }
    args.forEach(function(arg){ extend(target, arg, deep) })
    return target
  }
```



把source的属性全部覆盖到target上

如果deep是true，且target上对应的属性不是对象或数组，直接覆盖

```
function extend(target, source, deep) {
    for (key in source)
      if (deep && (isPlainObject(source[key]) || isArray(source[key]))) {
        if (isPlainObject(source[key]) && !isPlainObject(target[key]))
          target[key] = {}
        if (isArray(source[key]) && !isArray(target[key]))
          target[key] = []
        extend(target[key], source[key], deep)
      }
      else if (source[key] !== undefined) target[key] = source[key]
  }
```



####  deep copy

```
if (isPlainObject(source[key]) && !isPlainObject(target[key])){
	target[key] = {}
}
如果源属性是对象，目标属性不是，修改目标属性为对象
然后通过递归重新赋值
```





#### shallow copy

```
if (source[key] !== undefined) target[key] = source[key]
```





浅拷贝只是简单粗暴的，什么都改为target已有的属性。

深拷贝，会深入到