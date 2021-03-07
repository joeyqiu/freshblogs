# $.each

```
$.each(collection, function(index, item){ ... })  ⇒ collection
```

Iterate over array elements or object key-value pairs. Returning `false` from the iterator function stops the iteration.

遍历数组或者对象的key-value对。如果遍历函数返回false，则停止遍历。

```js
$.each(['a', 'b', 'c'], function(index, item){
  console.log('item %d is: %s', index, item)
})

var hash = { name: 'zepto.js', size: 'micro' }
$.each(hash, function(key, value){
  console.log('%s: %s', key, value)
})
```



### source code

```
$.each = function(elements, callback){
    var i, key
    if (likeArray(elements)) {
      for (i = 0; i < elements.length; i++)
        if (callback.call(elements[i], i, elements[i]) === false) return elements
    } else {
      for (key in elements)
        if (callback.call(elements[key], key, elements[key]) === false) return elements
    }

    return elements
  }
```



#### likeArray

```
function likeArray(obj) { 
	return typeof obj.length == 'number'
}

for (i = 0; i < elements.length; i++){
  if (callback.call(elements[i], i, elements[i]) === false) {
    return elements
  }
}
```



#### object

```
for (key in elements){
  if (callback.call(elements[key], key, elements[key]) === false) return elements
}
```

