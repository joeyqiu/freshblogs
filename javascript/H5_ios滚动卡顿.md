# H5_ios滚动卡顿

在滚动方面，android的效果很好，然后在ios手机上会比较的卡顿

## 现象

ios中滚动卡顿，主要是因为使用了`overflow-y:auto`的原因，尽量避免使用overflow来滚动，而是使用页面自带的滚动效果。。
超出就会滚动，所以可以少在父元素上设置`overflow-y`

问题代码：

````
.wrapper {
  position: relative;
  width: 100%;
  height: 100%;
  background-color: #f5f5f5;
  font-family: PingFangSC-Regular;
  overflow-x: hidden;
  overflow-y: auto;
  -webkit-overflow-scrolling:touch;
}
````

修改后的代码:

````
.wrapper {
  position: relative;
  background-color: #f5f5f5;
  font-family: PingFangSC-Regular;
  -webkit-overflow-scrolling:touch;
}
````

## -webkit-overflow-scrolling:touch

`-webkit-overflow-scrolling:touch;`该属性在ios上可以使得滚动效果更加流畅，如果不可避免的需要使用overflow，记得使用该属性。

然后该属性会导致内部的fixed元素出现布局问题，所以fixed元素尽量放在外部。