# $.camelCase

```
$.camelCase(string)  ⇒ string
```

Turn a dasherized string into “camel case”. Doesn’t affect already camel-cased strings.

把分隔符的字符串改为驼峰法，不影响已经是驼峰法的字符串。

```js
$.camelCase('hello-there') //=> "helloThere"
$.camelCase('helloThere')  //=> "helloThere"
```





### 源码解析

```
$.camelCase = camelize

var camelize = function(str){ 
	return str.replace(/-+(.)?/g, function(match, chr){ 
        return chr ? chr.toUpperCase() : '' 
    }) 
}
```

通过正则表达式来匹配-分隔符后面的第一个文字，，然后使用大写



```
(x)： 匹配 x 并且捕获匹配项。 这被称为捕获括号（capturing parentheses）。
```

