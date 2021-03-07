## addClass

```
addClass(name)  ⇒ self
addClass(function(index, oldClassName){ ... })  ⇒ self
```

Add class name to each of the elements in the collection. Multiple class names can be given in a space-separated string.

给集合中的每个对象添加类名。如果有多个类名，通过空格符来分开。



### source code

```
addClass: function(name){
      if (!name) return this
      return this.each(function(idx){
        if (!('className' in this)) return
        classList = []
        var cls = className(this), newName = funcArg(this, name, idx, cls)
        newName.split(/\s+/g).forEach(function(klass){
          if (!$(this).hasClass(klass)) classList.push(klass)
        }, this)
        // 把需要添加的类名放到classList中去，然后通过className函数加到节点上去
        classList.length && className(this, cls + (cls ? " " : "") + classList.join(" "))
      })
    }

// access className property while respecting SVGAnimatedString
// 对svg的类名有特殊的处理，别的节点的话，有value值就赋值，没有就是取值。
function className(node, value){
    var klass = node.className || '',
        svg   = klass && klass.baseVal !== undefined

    if (value === undefined) return svg ? klass.baseVal : klass
    svg ? (klass.baseVal = value) : (node.className = value)
}
 
function funcArg(context, arg, idx, payload) {
  return isFunction(arg) ? arg.call(context, idx, payload) : arg
}
```

