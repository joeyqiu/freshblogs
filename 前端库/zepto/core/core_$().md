# $()

Zepto对象的构造函数实现，其实是通过zepto.init方法来实现的

```
var Zepto = (function(){
    //xxx
    
    $ = function(selector, context){
      return zepto.init(selector, context)
    }
  
    return $
})()

// If `$` is not yet defined, point it to `Zepto`
window.Zepto = Zepto
window.$ === undefined && (window.$ = Zepto)
```



zepto.init的方法如下：

```
zepto.init = function(selector, context) {
    var dom
    // If nothing given, return an empty Zepto collection
    if (!selector) return zepto.Z()
    // Optimize for string selectors
    else if (typeof selector == 'string') {
      selector = selector.trim()
      // If it's a html fragment, create nodes from it
      // Note: In both Chrome 21 and Firefox 15, DOM error 12
      // is thrown if the fragment doesn't begin with <
      if (selector[0] == '<' && fragmentRE.test(selector))
        dom = zepto.fragment(selector, RegExp.$1, context), selector = null
      // If there's a context, create a collection on that context first, and select
      // nodes from there
      else if (context !== undefined) return $(context).find(selector)
      // If it's a CSS selector, use it to select nodes.
      else dom = zepto.qsa(document, selector)
    }
    // If a function is given, call it when the DOM is ready
    else if (isFunction(selector)) return $(document).ready(selector)
    // If a Zepto collection is given, just return it
    else if (zepto.isZ(selector)) return selector
    else {
      // normalize array if an array of nodes is given
      if (isArray(selector)) dom = compact(selector)
      // Wrap DOM nodes.
      else if (isObject(selector))
        dom = [selector], selector = null
      // If it's a html fragment, create nodes from it
      else if (fragmentRE.test(selector))
        dom = zepto.fragment(selector.trim(), RegExp.$1, context), selector = null
      // If there's a context, create a collection on that context first, and select
      // nodes from there
      else if (context !== undefined) return $(context).find(selector)
      // And last but no least, if it's a CSS selector, use it to select nodes.
      else dom = zepto.qsa(document, selector)
    }
    // create a new Zepto collection from the nodes found
    return zepto.Z(dom, selector)
  }
```

传入selector和context两个参数，第一个是选择器，第二个是上下文



### selector为空

```
if (!selector) return zepto.Z()

zepto.Z = function(dom, selector) {
	return new Z(dom, selector)
}

function Z(dom, selector) {
  var i, len = dom ? dom.length : 0
  for (i = 0; i < len; i++) this[i] = dom[i]
  this.length = len
  this.selector = selector || ''
}

传入选择器为空，返回一个空的Z对象。
所有zepto对象，都是Z对象的实例。
```



### selector === 'string'

```
else if (typeof selector == 'string') {
    selector = selector.trim()
    // If it's a html fragment, create nodes from it
    // Note: In both Chrome 21 and Firefox 15, DOM error 12
    // is thrown if the fragment doesn't begin with <
    if (selector[0] == '<' && fragmentRE.test(selector))
    dom = zepto.fragment(selector, RegExp.$1, context), selector = null
    // If there's a context, create a collection on that context first, and select
    // nodes from there
    else if (context !== undefined) return $(context).find(selector)
    // If it's a CSS selector, use it to select nodes.
    else dom = zepto.qsa(document, selector)
}
```



#### String: selector[0] == '<' && fragmentRE.test(selector)

```
fragmentRE = /^\s*<(\w+|!)[^>]*>/

如果是标签开头的字符串，如：'<p>xxx';

dom = zepto.fragment(selector, RegExp.$1, context)
selector = null

例子：$('<p>asd');

selector = '<p>asd';
RegExp.$1 = p

zepto.fragment返回一个<p>标签数组
```



zepto.fragment接收一个html字符串和标签名（可选）

````
zepto.fragment = function(html, name, properties) {
    var dom, nodes, container

    // A special case optimization for a single tag
    // 从html标签中识别出单个标签名，把dom赋值为节点
    if (singleTagRE.test(html)) dom = $(document.createElement(RegExp.$1))

    if (!dom) {
      if (html.replace) html = html.replace(tagExpanderRE, "<$1></$2>")
      if (name === undefined) name = fragmentRE.test(html) && RegExp.$1
      if (!(name in containers)) name = '*'

      container = containers[name]
      container.innerHTML = '' + html
      dom = $.each(slice.call(container.childNodes), function(){
        container.removeChild(this)
      });
      // 把container中的子节点都提取出来，变成数组赋值给dom
    }

    if (isPlainObject(properties)) {
      nodes = $(dom)
      $.each(properties, function(key, value) {
        if (methodAttributes.indexOf(key) > -1) nodes[key](value)
        else nodes.attr(key, value)
      })
    }

    return dom
  }
  
singleTagRE = /^<(\w+)\s*\/?>(?:<\/\1>|)$/,
containers = {
  'tr': document.createElement('tbody'),
  'tbody': table, 'thead': table, 'tfoot': table,
  'td': tableRow, 'th': tableRow,
  '*': document.createElement('div')
}
````



#### String: context !== undefined

````
return $(context).find(selector)

如果传递的上下文不为空，则直接从该上下文中查找
````



#### String: else

````
dom = zepto.qsa(document, selector)
````



````
/**
   * 1.如果可以通过ID查找，则通过getElementByID查找
   * 2.如果不是元素或者document节点，documentFragment节点，返回空
   * 3. isSimple, 用getElementByClassName和getElementbyTagName试一下
   * 4. not simple, 用querySelectorAll试一下
   */
  zepto.qsa = function(element, selector){
    var found,
        maybeID = selector[0] == '#',
        maybeClass = !maybeID && selector[0] == '.',
        nameOnly = maybeID || maybeClass ? selector.slice(1) : selector,
        isSimple = simpleSelectorRE.test(nameOnly)
    return (element.getElementById && isSimple && maybeID) ? 
      ( (found = element.getElementById(nameOnly)) ? [found] : [] ) :
      (element.nodeType !== 1 && element.nodeType !== 9 && element.nodeType !== 11) ? [] :
      slice.call(
        isSimple && !maybeID && element.getElementsByClassName ? 
          maybeClass ? element.getElementsByClassName(nameOnly) : 
          element.getElementsByTagName(selector) : 
          element.querySelectorAll(selector) 
      )
  }
````



### isFunction(selector)

如果传递的选择器是一个函数，执行。。。

```
else if (isFunction(selector)) return $(document).ready(selector)

function isFunction(value) { return type(value) == "function" }
var readyRE = /complete|loaded|interactive/;

ready: function(callback){
    // need to check if document.body exists for IE as that browser reports
    // document ready when it hasn't yet created the body element
    // IE的兼容
    if (readyRE.test(document.readyState) && document.body) callback($)
    else document.addEventListener('DOMContentLoaded', function(){ callback($) }, false)
    return this
}

大部分情况下，都是在DOMContentLoaded的时候执行传入的函数
```



### isZ(selector)

如果传入的是个Z对象，直接返回该对象

```
else if (zepto.isZ(selector)) return selector

zepto.isZ = function(object) {
  return object instanceof zepto.Z
}
```



### isArray(selector)

```
dom = compact(selector)

isArray = Array.isArray || function(obj) {return obj instanceof Array};

/**
* 遍历array，得到其中 != null 的数据项
*/
function compact(array) {
  return filter.call(array, function(item){
    return item!=null
  });
}
```



### isObject(selector)

如果传递一个对象，改成一个数组

```
dom = [selector];
selector = null;

function isObject(obj) { return type(obj) == 'object' }
```



### fragmentRE.test(selector)

```
else if (fragmentRE.test(selector)){
        dom = zepto.fragment(selector.trim(), RegExp.$1, context), selector = null
}
```

觉得这种情况比较少，应该是会用selector.toString()得到字符串



### context !== undefined

```
else if (context !== undefined) return $(context).find(selector)
```



### else dom = zepto.qsa(document, selector)



得到dom对象之后，通过Z构造函数，生成一个Z对象