## $.map

```
$.map(collection, function(item, index){ ... })  ⇒ collection
```

Iterate through elements of collection and return all results of running the iterator function, with `null` and `undefined` values filtered out.

遍历集合，并返回一个新的集合，，新集合都是遍历函数的返回结果，其中过滤了null和undefined项。



### source code

```
$.map = function(elements, callback){
    var value, values = [], i, key
    if (likeArray(elements)){
    // 数组
      for (i = 0; i < elements.length; i++) {
        value = callback(elements[i], i)
        // 得到遍历函数的返回值
        if (value != null) values.push(value)
        // 过滤器中为null的值
      }
    }
    else{
    // 考虑是对象
      for (key in elements) {
        value = callback(elements[key], key)
        if (value != null) values.push(value)
      }
    }
    return flatten(values)
    // flatten转成数组格式的
}

function flatten(array) {
	return array.length > 0 ? $.fn.concat.apply([], array) : array
}
```

