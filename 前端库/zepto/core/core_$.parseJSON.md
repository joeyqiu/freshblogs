## $.parseJSON

```
$.parseJSON(string)  ⇒ object
```

Alias for the native `JSON.parse` method.

原生`JSON.parse`方法的别名



### source code

```
if (window.JSON) $.parseJSON = JSON.parse
```

