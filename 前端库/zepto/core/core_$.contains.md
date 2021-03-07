## $.contains

```
$.contains(parent, node)  ⇒ boolean
```

Check if the parent node contains the given DOM node. Returns false if both are the same node.

检查父节点是否包含目标节点。如果是相同节点，返回false



### source Code

```
$.contains = document.documentElement.contains ?
    function(parent, node) {
      return parent !== node && parent.contains(node)
    } :
    function(parent, node) {
      while (node && (node = node.parentNode))
        if (node === parent) return true
      return false
    }
```



IE提供contains方法，所以直接通过parent.contains(node)来判断

```
function(parent, node) {
	return parent !== node && parent.contains(node)
}
```

否则不断通过子节点获取父节点，并判断两者是否相同来比较

```
function(parent, node) {
      while (node && (node = node.parentNode))
        if (node === parent) return true
      return false
    }
```

