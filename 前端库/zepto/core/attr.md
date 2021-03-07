## attr

```
attr(name)  ⇒ string
attr(name, value)  ⇒ self
attr(name, function(index, oldValue){ ... })  ⇒ self
attr({ name: value, name2: value2, ... })  ⇒ self
```

Read or set DOM attributes. When no value is given, reads specified attribute from the first element in the collection. When value is given, sets the attribute to that value on each element in the collection. When value is null, the attribute is removed (like with [removeAttr](https://zeptojs.com/#removeAttr)). Multiple attributes can be set by passing an object with name-value pairs.

读取或者设置DOM节点的属性。

如果没有value参数，则是读取值。

如果传递了value参数，则是设置属性值。

如果传递的value参数为 null，则是删除该属性值。

如果传递的value值为对象，则修改多个属性。

To read DOM properties such as `checked` or `selected`, use [prop](https://zeptojs.com/#prop).

```
var form = $('form')
form.attr('action')             //=> read value
form.attr('action', '/create')  //=> set value
form.attr('action', null)       //=> remove attribute

// multiple attributes:
form.attr({
  action: '/create',
  method: 'post'
})
```



- property是DOM中的属性，是JavaScript里的对象；
- attribute是HTML标签上的特性，它的值只能够是字符串；id, class等



### source code

```
attr: function(name, value){
      var result
      return (typeof name == 'string' && !(1 in arguments)) ?
      // 如果name属性是字符串，且不存在第二个参数，则是读取
      // nodeType == 1, element节点
        (0 in this && this[0].nodeType == 1 && (result = this[0].getAttribute(name)) != null ? result : undefined) :
        // 存在第二个参数，是赋值
        this.each(function(idx){
          if (this.nodeType !== 1) return
          // 如果是对象，则遍历
          if (isObject(name)) for (key in name) setAttribute(this, key, name[key])
          // 如果不是对象，则继续判断下第二个属性是否是function
          else setAttribute(this, name, funcArg(this, value, idx, this.getAttribute(name)))
        })
    }
```

