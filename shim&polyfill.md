# shim 与 polyfill
在前端，这两个词shim和polyfill经常提及，是关于Javascript解决浏览器兼容性问题。
> Polyfill 可以理解为“腻子”，就是装修时，可以把缺损的地方补平。对应到代码代码中，帮你把差异抹平，不支持变得支持，比如，判断该当前浏览器是否有某个功能，没有的话，就写一些补丁代码。

例如，低版本IE不支持标准的XMLHttpRequest，但支持ActiveXObject方法。对此有以下两种解决方案。  
jQuery的方式，把两种封装成$.ajax函数，使用时只要熟悉$.ajax方法即可。
```JS
// 伪代码
$.ajax = function(url) {
  if (isIE) {
    ActiveXObject(url)
  } else {
    XMLHttpRequest(url)
  }
}

```
另一种方式，判断浏览器是否支持XMLHttpRequest，如果不支持就是ActiveXObject实现。
```JS
// 伪代码
if (!XMLHttpRequest) {
  XMLHttpRequest = function (url) {
    ActiveXObject(url)
  }
}

```
**这两种方式都能解决浏览器兼容性问题，但是两种不同的思维。后者解决方案更先进些。**  
jQuery没有遵循标准，这里有个学习成本的问题，我们需要学习这个函数，而后者使用的是标准的API,不存在学习成本问题。若当不需要兼容低版本IE时，后者只需要移除兼容代码即可，而前者需要则不能，只能重构代码。遵循标准代码在维护性方面更好。  
后者可以按需加载，只在低版本浏览器上兼容代码。

**根据上面的例子，我们再来理解shim和polyfill的概念。**  
shim是一个库，它将新的PAI引入到旧的环境，靠旧环境中的技术实现。ployfill是shim的一种，它的API遵循API标准。polyfill通常先检查浏览器是否支持某个API,若不支持，就使用旧有的技术实现。  
理解了概念后，polyfill的思想就能指导我们如何去设计API。
比如，旧版本浏览器不支持ES6的Array.prototype.find方法，我们像在项目中使用此方法，只要polyfill去实现即可。
应该如下实现：
```JS
if (!Array.prototype.find) {
  Array.prototype.find = function() {
    // ...
  }
}
```
而不是封装一个新的方法
```JS
function arrayFind() {
  if (Array.prototype.find) {
    // ...
  } else {
    // ...
  }
}
```

## 参考地址
1. https://www.aliyun.com/jiaocheng/773254.html
2. https://segmentfault.com/q/1010000006796739
