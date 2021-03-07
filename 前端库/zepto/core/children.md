## children

```
children([selector])  ⇒ collection
```

Get immediate children of each element in the current collection. If selector is given, filter the results to only include ones matching the CSS selector.

获取当前集合每个元素的元素节点（children就是childNodes中nodeType=1的节点）。如果传入了selector选择器，需要从children中过滤

```
$('ol').children('*:nth-child(2n)')
//=> every other list item from every ordered list
```



### source code

```
children: function(selector){
  return filtered(this.map(function(){ return children(this) }), selector)
}

function children(element) {
    return 'children' in element ?
      slice.call(element.children) :
      $.map(element.childNodes, function(node){ if (node.nodeType == 1) return node })
  }

this.map(function(){ return children(this) })
// 通过map函数和children函数来得到所有节点的children节点

// 如果传入了selector，还需要通过filtered函数来过滤
function filtered(nodes, selector) {
    return selector == null ? $(nodes) : $(nodes).filter(selector)
  }
```

