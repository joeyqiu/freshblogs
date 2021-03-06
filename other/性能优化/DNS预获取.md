## DNS预获取 dns-prefetch提升页面载入速度

DNS Prefetch，即DNS预获取，是前端优化的一部分。一般来说，在前端优化中与 DNS 有关的有两点： **一个是减少DNS的请求次数，另一个就是进行DNS预获取** 。

DNS 作为互联网的基础协议，其解析的速度似乎很容易被网站优化人员忽视。现在大多数新浏览器已经针对DNS解析进行了优化，典型的一次DNS解析需要耗费 20-120 毫秒，减少DNS解析时间和次数是个很好的优化方式。DNS Prefetching 是让具有此属性的域名不需要用户点击链接就在后台解析，而域名解析和内容载入是串行的网络操作，所以这个方式能 **减少用户的等待时间，提升用户体验** 。

默认情况下浏览器会对页面中和当前域名（正在浏览网页的域名）不在同一个域的域名进行预获取，并且缓存结果，这就是隐式的 DNS Prefetch。如果想对页面中没有出现的域进行预获取，那么就要使用显示的 DNS Prefetch 了。

目前大多数浏览器已经支持此属性，支持版本如下：

- – Safari: 5+
- – Chrome: All
- – Firefox: 3.5+
- – Opera: Unknown
- – IE: 9+ (called “Pre-resolution” on blogs.msdn.com)

其中 Chrome 和 Firefox 3.5+ 内置了 DNS Prefetching 技术并对DNS预解析做了相应优化设置。所以 即使不设置此属性，Chrome 和 Firefox 3.5+ 也能自动在后台进行预解析 。

目前很多大型站点也应用了这一优化，例如：

淘宝：

![](./images/1.png)

支付宝：

![](./images/2.png)

网易：

![](./images/3.png)

DNS Prefetch 应该尽量的放在网页的前面，推荐放在 `<meta charset="UTF-8">` 后面。具体使用方法如下：

```
<meta http-equiv="x-dns-prefetch-control" content="on">
<link rel="dns-prefetch" href="//www.zhix.net">
<link rel="dns-prefetch" href="//api.share.zhix.net">
<link rel="dns-prefetch" href="//bdimg.share.zhix.net">
```

 

需要注意的是，虽然使用 DNS Prefetch 能够加快页面的解析速度，但是也不能滥用，因为有开发者指出 禁用DNS 预读取能节省每月100亿的DNS查询 。

如果需要禁止隐式的 DNS Prefetch，可以使用以下的标签：

```
<meta http-equiv="x-dns-prefetch-control" content="off">
```