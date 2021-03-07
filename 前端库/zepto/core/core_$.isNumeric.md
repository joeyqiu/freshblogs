## $.isNumeric

```
$.isNumeric(value)  ⇒ boolean
```

True if the value is a finite Number or a String representing a number.

判断传入的值是否是一个数字



### source code

```
$.isNumeric = function(val) {
    var num = Number(val), type = typeof val
    return val != null && type != 'boolean' &&
      (type != 'string' || val.length) &&
      !isNaN(num) && isFinite(num) || false
  }
```

