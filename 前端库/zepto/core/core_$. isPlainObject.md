## $.isPlainObject

```
$.isPlainObject(object)  ⇒ boolean
```

True if the object is a “plain” JavaScript object, which is only true for object literals and objects created with `new Object`.

如果传入的是一个空对象，则返回true。这边的空对象指的是对象字面量或者通过`new Object`方式创建的对象

```
$.isPlainObject({})         // => true
$.isPlainObject(new Object) // => true
$.isPlainObject(new Date)   // => false
$.isPlainObject(window)     // => false
```



### source code

```
function isPlainObject(obj) {
    return isObject(obj) &&
    	   !isWindow(obj) &&
    	   Object.getPrototypeOf(obj) == Object.prototype
}

function isWindow(obj)     { return obj != null && obj == obj.window }
function isObject(obj)     { return type(obj) == "object" }
```

也就是原型链上不存在继承的情况，或者说直接继承自默认的Object对象