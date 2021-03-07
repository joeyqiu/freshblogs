## $.isWindow

```
$.isWindow(object)  ⇒ boolean
```

True if the object is a window object. This is useful for iframes where each one has its own window, and where these objects fail the regular `obj === window` check.

检查传入的对象是否是window对象。对于iframes来说非常有用，每个iframe都有各自的window对象。而且这些对象在`obj === window`的比较中都会返回false



### source code

```
$.isWindow = isWindow

function isWindow(obj)     { return obj != null && obj == obj.window }
```

通过window === window.window来判断.