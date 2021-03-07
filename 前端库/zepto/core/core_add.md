## add

```
add(selector, [context])  ⇒ self
```

Modify the current collection by adding the results of performing the CSS selector on the whole document, or, if context is given, just inside context elements.

给当前集合添加元素，这个元素是通过传入的css选择器和context来得到的.



这个add方法是每个zepto对象都能够使用的，通过添加到$.fn对象上，通过

```
zepto.Z.prototype = Z.prototype = $.fn
```

使每个zepto对象都继承自$.fn

### source code

```
add: function(selector,context){
  return $(uniq(this.concat($(selector,context))))
}
```



1. 先通过`$(selector,context)`来查找想要添加的zepto对象
2. 通过this.concat来添加，this.concat是调用add方法的zepto对象上的



```
concat = emptyArray.concat
$.fn = {
  concat: function(){
      var i, value, args = []
      for (i = 0; i < arguments.length; i++) {
        value = arguments[i]
        args[i] = zepto.isZ(value) ? value.toArray() : value
        // 转成数组
      }
      return concat.apply(zepto.isZ(this) ? this.toArray() : this, args)
      // 数组相加
    }
}
```



3. 通过`uniq`方法来过滤其中重复的数据

```
uniq = function(array){ 
	return filter.call(array, function(item, idx){
		return array.indexOf(item) == idx
	})
}
```

