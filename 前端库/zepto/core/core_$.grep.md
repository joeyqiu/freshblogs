## $.grep

```
$.grep(items, function(item){ ... })  ⇒ array
```

Get a new array containing only the items for which the callback function returned true.

遍历items，并返回一个新的数组，，该数组中的每项都是遍历函数为 true的项。



### source code

```
$.grep = function(elements, callback){
  return filter.call(elements, callback)
}

var filter = [].filter
```

其实就是用原生数组的filter方法来遍历