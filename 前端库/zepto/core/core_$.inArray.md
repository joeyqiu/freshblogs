## $.inArray

```
$.inArray(element, array, [fromIndex])  ⇒ number
```

Get the position of element inside an array, or `-1` if not found.

得到元素在数组中的位置，如果找不到则返回-1.



### source code

```
var emptyArray = []

$.inArray = function(elem, array, i){
  return emptyArray.indexOf.call(array, elem, i)
}
```

就是通过array的indexOf方法来处理