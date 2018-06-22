---
title: 前端面试
date: 2018-03-10 21:15:56
tags: [面试,js,知识点]
description: Xiao丶メ北er
categories: 面试题库
---

面试题，总得来说前者理论性的东西问的更多一点，可以很清楚的了解你的实力，后者问的开放性的问题比较多一点，大部分都是问什么什么你用过吗，而不是去问是什么原理

<!--more-->
# Q:rem布局的原理

A：使用rem布局实质上都是通过改写html的font-size基准值，来实现不同设备之间的适配。
rem是CSS3的长度单位，相对文档跟元素html，比如设置font-size=100px,那么1rem=100px

[移动端页面开发适配 rem布局原理](http://www.tuicool.com/articles/FjMfuyM)

> 网易的做法：
> 
> 1) 将布局适口设置为视觉适口，不进行缩放，即理想适口。
> 
```
<meta name="viewport"content="initial-scale=1,maximum-scale=1, minimum-scale=1”>
```
> 
> 
> 2) 以设计稿分辨率为基准，取100px为font-size的参照，那么body元素的宽度就可以设置为`width:6.4rem（640/100）`，当我们将布局视口设置为320时，于是html的 `font-size=deviceWidth / 6.4`。
> 
> 3) 通过`document.documentElement.clientWidth`获取 `deviceWidth`；
> 
> 4) 当页面的`dom ready`后设置`html font-size`,
> 
> 
```
document.documentElement.style.fontSize =document.documentElement.clientWidth / 6.4 + ‘px’
```
> 
> 5) 通过 `mediaQuery` 设置字体大小，字体大小不可以使用rem,原因是误差太大。
> 
> 以640的设计稿为例最终的设置html `font-size`代码如下，布局时拿设计稿标注的尺寸除以100，就是rem的值，相当简单啊
> 
> 
```
var deviceWidth = document.documentElement.clientWidth;
if(deviceWidth > 640) deviceWidth = 640;
document.documentElement.style.fontSize = deviceWidth / 6.4 + 'px';
这里if(deviceWidth > 640) deviceWidth = 640;
```
>  是因为当deviceWidth大于640时物理分辨率已经大于1280（取决于 dpr ），应该去访问pc的网站；

```
var documentElement = document.documentElement;

    if (documentElement.clientWidth >= 750) {
        documentElement.style.fontSize = '54px';
    } else {
        documentElement.style.fontSize = "${documentElement.getBoundingClientRect().width / 10}px";
    }


    (function () {
        document.addEventListener('DOMContentLoaded', function () {
            var deviceWidth = document.documentElement.clientWidth;
            document.documentElement.style.fontSize = deviceWidth / 10 + 'px';
        }, false);
        window.onresize = function () {
            var deviceWidth = document.documentElement.clientWidth;
            document.documentElement.style.fontSize = deviceWidth / 10 + 'px';
        };

        console.log(document.width)
    })();
```

> 淘宝的做法：
> 
> 1）判断head中是否设置了viewport，如果有设置，按照已有viewport 设置缩放比；
> 
> 2）如果没有设置meta viewport，判断是否设置dpr，如果有，通过dpr计算缩放scale。
> 
> 3）如果 dpr &scale都没有设置，那么就通过设备的dpr设置起缩放 scale，
> 
> 4）得到scale之后 ，如果meta 的viewport不存在，那么就创建一个meta[name=“viewport”],将scale配置进去。
> 
> 5）动态改写html的font-size
> 
> 关于具体的实现 淘宝提供了一个开源的方案[lib-flexible](https://github.com/amfe/lib-flexible)；
> 
> 总结：
>
> 使用rem布局，实质都是通过动态改写html的font-size基准值，来实现不同设备下的良好统一适配；
>
> 网易与淘宝不同 的地方是 ，网易将布局视口设置成了 视觉视口，淘宝将布局视口设置成了物理像素大小，通过 scale缩放嵌入了 视觉视口中；
>
> 容器元素的字体大小都不使用rem，需要额外的media查询；


# Q:常见的布局方式

A：1.静态布局（Static Layout）2.弹性布局（Flex）3.自适应布局（Adapive Layout）4.流式布局（Liquid Layout）5.响应式布局（Responsive Layout）[web前端开发之几种布局方式之响应式布局](http://blog.csdn.net/gertYY/article/details/52764527)

> 1）静态布局（Static Layout）
> 
>```
>即传统Web设计，对于PC设计一个Layout，在屏幕宽高有调整时，使用横向和竖向的滚动条来查阅被遮掩部分； 
意思就是不管浏览器尺寸具体是多少，网页布局就按照当时写代码的布局来布置； 
对于移动设备，单独设计一个布局，使用不同的域名如wap.或m.。
>```
>
> 2）弹性布局（Flex）
> 
> 弹性布局是CSS3引入的强大的布局方式，用来替代以前Web开发人员使用的一些复杂而易错hacks方法（如使用float进行类似流式布局）。
>
> 其中flex-flow是flex-direction和flex-wrap属性的简写方式，语法如下：
>
> flex-flow: <flex-direction> || <flex-wrap>
>
> flex-direction: row（初始值） | row-reverse | column | column-reverse
>
> flex-wrap: nowrap（初始值） | wrap | wrap-reverse
>
> flex-direction定义了弹性项目在弹性容器中的放置方向，默认是row，即行内方向（一般而言是由左往右，但注意这个和书写模式有关）。
>
> flex-wrap定义是否需要拆行以使得弹性项目能被容器包含。*-reverse代表相反的方向。
>
> 两者结合起来即flex-flow属性就确定了弹性容器在main axis和cross axis两个方向上的显示方式，下面的例子很直观的说明了各个属性值的区别：
> 
> 3）自适应布局（Adapive Layout）
> 
> 自适应布局（Adaptive）的特点是分别为不同的屏幕分辨率定义布局。布局切换时页面元素发生改变，但在每个布局中，页面元素不随窗口大小的调整发生变化。 
> 
> 你可以把自适应布局看作是静态布局的一个系列。 
> 
> 就是说你看到的页面，里面元素的位置会变化而大小不会变化；
> 
> 4）流式布局（Liquid Layout）
> 流式布局（Liquid）的特点（也叫”Fluid”) 是页面元素的宽度按照屏幕进行适配调整，主要的问题是如果屏幕尺度跨度太大，那么在相对其原始设计而言过小或过大的屏幕上不能正常显示。
>  
>你看到的页面，元素的大小会变化而位置不会变化——这就导致如果屏幕太大或者太小都会导致元素无法正常显示。
> 
> 5）响应式布局（Responsive Layout）
> 
> 做手机网站必加的一句头部(head)代码
> 
```
<meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no">  
name="viewport"  
 名称=视图
width=device-width
 页面宽度=设备宽度(可以理解为获取你手机的屏幕宽度)
initial-scale - 初始的缩放比例  
minimum-scale - 允许用户缩放到的最小比例   
maximum-scale - 允许用户缩放到的最大比例  
user-scalable - 用户是否可以手动缩放
```

# Q:怎么使用媒体查询

```
@media screen and (max-width:720px) and (min-width:320px){
	body{
	
		background-color:red;
	
	}
}


@media screen and (max-width:320px){
	body{
	
		background-color:blue;
	
	}

}
```

# Q:移动端优化方法

A: 1.了解你的用户 2.让你的内容更有价值和针对性 3.缩短页面加载时间 4.触摸的设计 5.创建流体布局 6．使用压缩工具 7．不要隐藏图像，CSS或JavaScript 8.功能最大化 9.优化本地搜索 10．查询指南和数据库

> 原文链接：http://codecondo.com/10-ways-to-make-sure-your-website-is-optimized-for-mobile-devices/
> 
> 作者：刘妮娜译 链接：http://network.51cto.com/art/201708/546823.htm 来源：51CTO.com原创稿件 著作权归作者所有，转载请联系作者获得授权。


# Q:js模块化

A：Javascript模块化编程，已经成为一个迫切的需求。理想情况下，开发者只需要实现核心的业务逻辑，其他都可以加载别人已经写好的模块。

> 作者：阮一峰
> 
> [Javascript模块化编程（一）：模块的写法](http://www.ruanyifeng.com/blog/2012/10/javascript_module.html)
> 
> [Javascript模块化编程（二）：AMD规范](http://www.ruanyifeng.com/blog/2012/10/asynchronous_module_definition.html)
> 
> [Javascript模块化编程（三）：require.js的用法](http://www.ruanyifeng.com/blog/2012/11/require_js.html)


# Q:怎么实现延迟加载

A：js的延迟加载有助与提高页面的加载速度，以下是延迟加载的几种方法：

> 1.使用setTimeout延迟方法的加载时间
> 
> 延迟加载js代码，给网页加载留出更多时间
> 
> ```
> <script type="text/javascript" >
  function A(){
    $.post("/lord/login",{name:username,pwd:password},function(){
      alert("Hello");
    });
  }
  $(function (){
    setTimeout('A()', 1000); //延迟1秒
  })
</script>
> ```
> 
> 2.让js最后加载
> 
> 例如引入外部js脚本文件时，如果放入html的head中,则页面加载前该js脚本就会被加载入页面，而放入body中，则会按照页面从上倒下的加载顺序来运行JavaScript的代码~~~ 所以我们可以把js外部引入的文件放到页面底部，来让js最后引入，从而加快页面加载速度
> 
> 3.上述方法2也会偶尔让你收到Google页面速度测试工具的“延迟加载javascript”警告。所以这里的解决方案将是来自Google帮助页面的推荐方案。
> 
> ```
//这些代码应被放置在</body>标签前(接近HTML文件底部)
<script type="text/javascript">
  function downloadJSAtOnload() {
    var element = document.createElement("script");
    element.src = "defer.js";
    document.body.appendChild(element);
  }
  if (window.addEventListener)
    window.addEventListener("load", downloadJSAtOnload, false);
  else if (window.attachEvent)
    window.attachEvent("onload", downloadJSAtOnload);
  else window.onload = downloadJSAtOnload;
</script>
> ```
> 这段代码意思是等到整个文档加载完后，再加载外部文件“defer.js”。 
> 
> 1）.复制上面代码

> 2）.粘贴代码到HTML的标签前 (靠近HTML文件底部)

> 3）.修改“defer.js”为你的外部JS文件名

> 4）.确保你文件路径是正确的。例如：如果你仅输入“defer.js”，那么“defer.js”文件一定与HTML文件在同一文件夹下。
> 
> 注意：这段代码直到文档加载完才会加载指定的外部js文件。因此，不应该把那些页面正常加载需要依赖的javascript代码放在这里。而应该将JavaScript代码分成两组。一组是因页面需要而立即加载的javascript代码，另外一组是在页面加载后进行操作的javascript代码(例如添加click事件或其他东西)。这些需等到页面加载后再执行的JavaScript代码，应放在一个外部文件，然后再引进来。

# Q:你对闭包的理解
# Q:localStorage、sessionStorage、cookie之间的区别，各自的储存容量、怎么选择相应的存储机制

上交所初试之笔试题：
# 什么是“use strait”有什么好处有什么坏处
> ECMAscript 5添加了第二种运行模式："严格模式"（strict mode）。顾名思义，这种模式使得Javascript在更严格的条件下运行。
> 
> 设立"严格模式"的目的，主要有以下几个：
> 
> 1. 消除Javascript语法的一些不合理、不严谨之处，减少一些怪异行为;
> 
> 2. 消除代码运行的一些不安全之处，保证代码运行的安全；
> 
> 3. 提高编译器效率，增加运行速度；
> 
> 4. 为未来新版本的Javascript做好铺垫。
> 
> 注：经过测试 IE6,7,8,9 均不支持严格模式。
> 
> 缺点：
> 
> 现在网站的 JS 都会进行压缩，一些文件用了严格模式，而另一些没有。这时这些本来是严格模式的文件，被 merge 后，这个串就到了文件的中间，不仅没有指示严格模式，反而在压缩后浪费了字节。

# 解释下javascript中this是怎么工作的
> JavaScript 只有函数能提供作用域，this 表达了这个作用域的上下文。这个上下文是一个对象，默认是 global 对象：
> 
> function foo() {
> 
>   // 在终端输出 this 对象
>   console.log(this)
> }
> 
> foo() // 输出： global 对象 在浏览器中即 window 对象
> 
> 但是也可以在函数被运行的时候动态指定（call,apply, bind）：
> 
> var bar = { name: 'bar' }
> 
> foo.call(bar) // 输出： { name: 'bar' }
> 
> 存在一个特殊的情形，就是使用 new 关键字调用一个构造函数的时候。这会为这个函数自动创建一个设置在原型末端的上下文对象。
> 
> new foo() // 输出： {} 在构造函数内部创建一个对象
> 
> 作者：管斌瑞
> 链接：https://www.zhihu.com/question/19624483/answer/25745246
> 来源：知乎
> 著作权归作者所有，转载请联系作者获得授权。

# 有一个未知长度的数组a，如果它的长度为0就把数字1添加到数组里，否则按照先进先出的队列规则让第一个元素出队

```
a.length === 0 ? a.push(1) : a.shift();

注：shift() 方法用于把数组的第一个元素从其中删除，并返回第一个元素的值。
```

# 描述下`reset`css文件的作用和使用它的好处
重置浏览器的css默认属性；浏览器的品种不一样，那么对默认样式的解释不一样，通过reset可以达到显示一致的效果

# 说说前端怎么如何解决异步回调地狱

A：什么是“回调地狱”？我们很难一眼就看懂异步JavaScript，或者是使用回调函数的JavaScript程序。

> 作者：ssshooter
> 链接：https://segmentfault.com/a/1190000009644973
> 来源：SegmentFault
> 著作权归作者所有，转载请联系作者获得授权。

# 请解释Function.prototype.bind的作用
> bind() 方法的主要作用就是将函数绑定至某个对象，bind() 方法会创建一个函数，函数体内this对象的值会被绑定到传入bind() 函数的值。

> 例如，在 f() 函数上调用 bind() 方法并传入参数 obj ，即 f.bind(obj) ，这将返回一个新函数, 新函数会把原始的函数 f() 当做 obj 的方法来调用,就像 obj.f() 似的，当然这时 f() 函数中的 this 对象指向的是 obj 。

# 描述以下变量的区别：null，undefined，该如何检测他们
## null表示"没有对象"，即该处不应该有值。典型用法是：
（1） 作为函数的参数，表示该函数的参数不是对象。
（2） 作为对象原型链的终点。

## undefined表示"缺少值"，就是此处应该有一个值，但是还没有定义。典型用法是：
（1）变量被声明了，但没有赋值时，就等于undefined。
（2) 调用函数时，应该提供的参数没有提供，该参数等于undefined。
（3）对象没有赋值的属性，该属性的值为undefined。
（4）函数没有返回值时，默认返回undefined。

## 判断undefined: 
复制代码 代码如下:

```
<span style="font-size: small;">var tmp = undefined; 
if (typeof(tmp) == "undefined"){ 
alert("undefined"); 
}</span>
```

## 判断null: 
复制代码 代码如下:

```
<span style="font-size: small;">var tmp = null; 
if (!tmp && typeof(tmp)!="undefined" && tmp!=0){ 
alert("null"); 
}　</span>
```

## 判断NaN: 
复制代码 代码如下:


```
<span style="font-size: small;">var tmp = 0/0; 
if(isNaN(tmp)){ 
alert("NaN"); 
}</span>
```

# 说说类的创建、继承和闭包

# 有一个长度为100的数组，请以优雅的方式求出该数组的前10个元素之和

```
var a = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15],
 sum = 0;
 sum = a.slice(0, 10).reduce(function(pre, current) {
 　　return pre + current;
 });
  
 console.log(sum); //55
```

# 下面的代码会输出什么：

```
var test = (function(a) {
    this.a = a;
    return function(b) {
        return this.a + b;
    }
} (function(a, b) {
    return a;
}(1, 2))); 

console.log(test(4)); //输出什么？？？？
```

最后发现这大部分都是阿里的面试题= =
[阿里前端笔试题目](http://www.cnblogs.com/beidan/p/5285742.html)