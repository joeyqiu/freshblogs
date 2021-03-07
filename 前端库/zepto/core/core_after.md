## after,insertAfter,prepend,prependTo,before,insertBefore,append,appendTo

### after

```
after(content，要插入的内容)  ⇒ self
```

Add content to the DOM after each elements in the collection. The content can be an HTML string, a DOM node or an array of nodes.

在集合的元素后面添加DOM节点，传入的内容可以是一个html字符串，DOM节点或者是节点数组

```
$('form label').after('<p>A note below the label</p>')
```



### insertAfter

```
insertAfter(target，目标节点)  ⇒ self
```

Insert elements from the current collection after the target element in the DOM. This is like [after](https://zeptojs.com/#after), but with reversed operands.

把元素从当前集合插入到目标元素的后面

```
$('<p>Emphasis mine.</p>').insertAfter('blockquote')
```





### append

```
append(content)  ⇒ self
```

Append content to the DOM inside each individual element in the collection. The content can be an HTML string, a DOM node or an array of nodes.

集中的每个节点后面插入content

```
$('ul').append('<li>new list item</li>')
```



### appendTo

```
appendTo(target)  ⇒ self
```

Append elements from the current collection to the target element. This is like [append](https://zeptojs.com/#append), but with reversed operands.

把内容插入到目标节点中

```
$('<li>new list item</li>').appendTo('ul')
```



### before

```
before(content)  ⇒ self
```

Add content to the DOM before each element in the collection. The content can be an HTML string, a DOM node or an array of nodes.

集中的每个节点前面插入content

```
$('table').before('<p>See the following table:</p>')
```



### insertBefore

```
insertBefore(target)  ⇒ self
```

Insert elements from the current collection before each of the target elements in the DOM. This is like [before](https://zeptojs.com/#before), but with reversed operands.

把内容插入到目标节点前面

```
$('<p>See the following table:</p>').insertBefore('table')
```



### prepend

```
prepend(content)  ⇒ self
```

Prepend content to the DOM inside each element in the collection. The content can be an HTML string, a DOM node or an array of nodes.

集中的每个节点中插入content，并放在前面

```
$('ul').prepend('<li>first list item</li>')
```

### prependTo

```
prependTo(target)  ⇒ self
```

Prepend elements of the current collection inside each of the target elements. This is like [prepend](https://zeptojs.com/#prepend), only with reversed operands.

把内容插入到目标节点中，并放在前面

```
$('<li>first list item</li>').prependTo('ul')
```



### source code

```
adjacencyOperators = [ 'after', 'prepend', 'before', 'append' ],

function traverseNode(node, fun) {
  fun(node)
  for (var i = 0, len = node.childNodes.length; i < len; i++)
    traverseNode(node.childNodes[i], fun)
}
  
// Generate the `after`, `prepend`, `before`, `append`,
  // `insertAfter`, `insertBefore`, `appendTo`, and `prependTo` methods.
  adjacencyOperators.forEach(function(operator, operatorIndex) {
    var inside = operatorIndex % 2 //=> prepend, append
	// after, inside= false
    // prepend, inside = true
    // before, inside = false
    // append, inside = true
        
    $.fn[operator] = function(){
      // arguments can be nodes, arrays of nodes, Zepto objects and HTML strings
      // 这边通过map函数，遍历所有传入参数，并转成节点
      var argType, nodes = $.map(arguments, function(arg) {
            var arr = []
            argType = type(arg)
            if (argType == "array") {
              arg.forEach(function(el) {
                if (el.nodeType !== undefined) return arr.push(el)
                else if ($.zepto.isZ(el)) return arr = arr.concat(el.get())
                arr = arr.concat(zepto.fragment(el))
              })
              return arr
            }
            return argType == "object" || arg == null ?
              arg : zepto.fragment(arg)
          }),
          parent, copyByClone = this.length > 1
      if (nodes.length < 1) return this

      return this.each(function(_, target){
        parent = inside ? target : target.parentNode
		// after: parent = target.parentNode
        // prepend: parent = target
        // before: parent = target.parentNode
        // append: parent = target

        // convert all methods to a "before" operation
        target = operatorIndex == 0 ? target.nextSibling :
                 operatorIndex == 1 ? target.firstChild :
                 operatorIndex == 2 ? target :
                 null

        var parentInDocument = $.contains(document.documentElement, parent)

        nodes.forEach(function(node){
          if (copyByClone) node = node.cloneNode(true)
          else if (!parent) return $(node).remove()

          parent.insertBefore(node, target)
          // after   -> parent.insertBefore(node, xxx.nextSibling)
          // prepend  -> parent.insertBefore(node, xxx.firstChild)
          // before   -> parent.insertBefore(node, xxx.target)
          // append   -> parent.insertBefore(node, null)
          
          if (parentInDocument) traverseNode(node, function(el){
            if (el.nodeName != null && el.nodeName.toUpperCase() === 'SCRIPT' &&
               (!el.type || el.type === 'text/javascript') && !el.src){
              var target = el.ownerDocument ? el.ownerDocument.defaultView : window
              target['eval'].call(target, el.innerHTML)
              // 如果是script节点，则需要执行其中的script代码
            }
          })
        })
      })
	
	// after    => insertAfter
    // prepend  => prependTo
    // before   => insertBefore
    // append   => appendTo
    $.fn[inside ? operator+'To' : 'insert'+(operatorIndex ? 'Before' : 'After')] = function(html){
    // 获取传入的html，并得到$(html)对应的zepto节点，然后操作对应append等操作，相当于反着执行了一次
      $(html)[operator](this)
      return this
    }
    
}
```

