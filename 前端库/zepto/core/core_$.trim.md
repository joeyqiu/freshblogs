## $.trim

```
$.trim(string)  ⇒ string
```

Remove whitespace from beginning and end of a string; just like String.prototype.trim().

去除字符串头尾的空白符，就像原生的trim方法



### source code

```
$.trim = function(str) {
    return str == null ? "" : String.prototype.trim.call(str)
  }
```

判断是否是null，如果不是，则用原生的trim方法