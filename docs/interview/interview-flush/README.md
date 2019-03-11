# 耗时6小时的同花顺面试

[[toc]]

## 面试前注意点
::: tip
本人文笔不行，只会讲大白话，粗人一个
:::

1. 一份漂亮的简历很重要。
好吧，虽然我改了五六次，但是我的简历还是很low。注意：千万不要给自己挖坑，不要写自己答不上来的东东。要在面试之前，把简历上的东西都弄懂，用人单位会问你引申出很多知识，这个最好也掌握。

2. 跳槽和离职理由要恰当。
这一点很关键，你如果连个理由都没有，万一你啥理由都没有就离职了。GG。公司归属感很重要滴。大厂宁可不招，也不会招进一个随时会走的人。理由：1，工资太低，想加薪；2，为了提升技术；3，换一个氛围；4，其他因素（譬如，小孩上学、另一伴的工作、父母要求等等）。个人感觉，不能说理由1。大厂会认为你是金钱主义者，下一家给的多，你就去，毫无归属感。推荐理由2、理由3和理由4,哈哈，虽然你的目标是加薪。

3. 注重平时积累（尤其是基础知识和原理）。
我19届的，今年才毕业，已经在滨江实习了7、8个月了。这几个月了，我疯狂做项目，疯狂敲代码。你要想着：比你优秀的人都还在努力。我一边做项目，一边会去做笔记，推荐有道云笔记。之前用过印象。哎，充满金钱味的笔记（买不起会员）。我遇到过的bug，我会记载下来。在网上看到的优秀文章，除了收藏那就是放进笔记中。优秀博客也是。上文提到的朋友也告诉我要用一句话概括自己今天所做的————我归纳为今日总结。面试前将笔记刷一遍，将面经刷一遍。重要的是前端面试题刷一遍（很多多会考到，没考到就会问到）。

## 面试前

   其实笔试和面试各1个小时，在路上竟然花了4个小时 :100: 。
下午4点约的面试，我早早就到了同花顺。好吧，那里附近挺荒凉的，没有滨江繁华（个人觉得），同花顺是一个独立的大楼。坐了这么久的地铁，尿急死的我就啪啪啪往里走，然后解手后，发现才1点多，是我太早了，突然想到一句话：准时是礼貌，早到是修养。不知道是我谁说的。

   于是，我就坐在一楼的沙发上继续地铁上的复习。我来之前查过，同花顺面试是笔试+面试。然后它笔试主要会考察js。所以我将小册里的《前端面试之道》打开，继续开始我的学习。好吧，我并没有看完。突然想到昨天我的朋友的话：你的胆子好大啊，第一家就是大公司。看不完，准备不完，碰巧微信里有同花顺的hr，那就联系他咯。他说可以面试，那我就去试一试咯。
   复习了一个小时，将近三点的时候，我给hr打了电话。天哪，他说我跑进来干嘛，wc，难道我要在外面吹风？是我不懂事，好伐。然后跟着他走进了电梯，hr有点难沟通，我无法扯开刚才尴尬的气氛，他问一句我答一句。出了电梯，他带我进了小房间，摆着一张张桌子，桌子上有电脑，竟然还是xp的。他让我坐着等他。等他走后，我张望着四周，奋笔疾书的人有三四个，还有些人跟我一样张望着，估计都是来面试的。我看着前面的电脑，里面还有没关掉的笔试网页，是IE的，天真的我以为也是在电脑上答，也是这套卷子，所以疯狂地在脑海里搜索着屏幕上题目的答案。呵呵，结果，hr拿了张纸，让我在纸上写。

## 面试中
### [笔试题](https://my.oschina.net/u/2332658/blog/466893)
笔试题在网上可以找到
> JavaScript包括哪些数据类型，请分别编写3种以上类型的判断函数如：isString（）？

简单类型：null、undefined、number、string、array
复杂类型：object
看不懂后者问的是啥。

参考答案:
字符串、数字、布尔、数组、对象、null、undefined
typeof, instanceof, isArray()?

> 编写一个JavaScript函数，实时显示当前时间，格式‘年-月-日 时：分：秒’?
```js
var date=new Date()
var year=date.getFullYear()
var month=date.getMonth()
var day=date.getDate()
var hour=date.getHours()
var minute=date.getMinutes()
var second=date.getSeconds()
console.log(year+'-'+month+'-'+day+'-'+hour+':'+minute+':'+second)
```
参考答案:
```js
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <input id="show" style="width:300px;"/>
  <script>
    function getTime(){
      var nowDate = new Date();
      var year = nowDate.getFullYear();
      var month = (nowDate.getMonth() + 1) > 10 ? nowDate.getMonth() + 1 : '0' + (nowDate.getMonth() + 1);
      var day = nowDate.getDate() > 10 ? nowDate.getDate() : '0' + nowDate.getDate();
      var hour = nowDate.getHours() > 10 ? nowDate.getHours() : (nowDate.getHours() == 0 ? 24 : '0' + nowDate.getHours());
      var minutes = nowDate.getMinutes() > 10 ? nowDate.getMinutes() : '0' + nowDate.getMinutes();
      var seconds = nowDate.getSeconds() > 10 ? nowDate.getSeconds() : '0' + nowDate.getSeconds();
      var str="当前时间为：" + year + "年" + month + "月" + day + "日" + " " + hour + ":" + minutes + ":" + seconds;
      document.getElementById("show").value = str;
    }
    window.setInterval("getTime()", 1000);
  </script>
</body>
</html>
```

> 如何显示/隐藏一个DOM元素？

```css
display:none/display:block
```
参考答案:
```css
显示：object.style.display="block";
隐藏：object.style.display="none";
```

> 如何添加html元素的事件处理，有几种方法。

@+事件名，忘记了。

参考答案:
 ```text
 答案：html的元素的事件就只用控件自带的的那么几个，如onclick,onmouserdown ,..等等都是调用脚本执行

            方法：1、在空间上写直接激发事件

                      2、在页面加载的时候就调用脚本激发控件的某个事件

                      3、在后台利用后台代码强行执行控件的事件。

   或：

（1） 为HTML元素的事件属性赋值

（2） 在JS中使用ele.on*** = function() {…}

（3） 使用DOM2的添加事件的方法 addEventListener或attachEvent
```

> 如何控制alert中的换行。

```html
\n或者\n\r
```
参考答案:

```text
\n alert("text\ntext");

alert("再打个招呼。这里演示了" + "\n" + "如何在消息框中添加折行。")
```
> 判断一个字符串中出现次数最多的字符，统计这个次数。

忘记了
参考答案:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
<script>
    var str = "abcdefgaddda";
    var obj = {};
    for(var i = 0; i < str.length; i++){
        var key = str[i];
        if(typeof obj[key] === 'undefined'){
            obj[key] = 1;
        }else{
            obj[key]++;
        }
    }
    var max = -1;
    var max_key = obj[key];
    for(var key in obj){
        if(max < obj[key]){
            max = obj[key];
            max_key = key;
        }
    }
    document.write("字符:" + max_key + ",出现次数最多为:" + max + "次")
</script>
</body>
</html>

```

> 判断字符串是否是这样组成的，第一个必须是字母，后面可以是字母、数字、下划线，总长度为5-20

参考答案
```js
var reg = /^[a-zA-Z][a-zA-Z_0-9]{4,19}$/
console.log(reg.test("11a__a1a__a1a__a1a__"))

```


> 请编写一个JavaScript函数parseQueryString，他的用途是把URL参数解析为一个对象，如：var url=“http://witmax.cn/index.php?key0=0&key1=1&key2=2”；

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
<script>
    function parseQueryString(url){
        var result = {};
        var arr = url.split("?");
        if (arr.length <= 1){
            return result;
        }else{
            arr = arr[1].split("&");
            for(var i=0; i<arr.length; i++){
                var a = arr[i].split("=");
                result[a[0]] = a[1];
            }
            return result;
        }
    }
    var url = "http://witmax.cn/index.php?key0=0&key1=1&key2=2";
    var ps = parseQueryString(url);
    document.write(ps['key0'] + '<br>' + ps['key1'] + '<br>' + ps['key2']);
</script>
</body>
</html>

```

> 在页面中有如下html：
```html
<div id="field">
<input type="text" value="User Name"/>
</div><span class="red"></span>
```
要求用闭包方式写一个JS从文本框中取出值并在标签span中显示出来。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <div id="firld">
    <input type="text" value="User Name"/>
  </div>
  <span id="red" class="red"></span>
  <script>
    var result = (function(){
      var value = document.getElementById("firld").children[0].value;
      var all = document.getElementsByTagName("span");
      for(var i = 0; i < all.length; i++){
        if(all[i].className == 'red'){
          all[i].innerHTML = value;
          break;
        }
      }
    })();
  </script>
</body>
</html>
```

> 在IE6.0下面是不支持position：fixed的，请写一个JS使用<div id="box"></div>固定在页面的右下角。



> 请实现，鼠标移到页面中的任意标签，显示出这个标签的基本矩形轮廓。



> js的基础对象有哪些，window和document的常用的方法和属性列出来

window属性：location
方法：alert()、
document属性：
方法：getElementById(),getElementsByName()


> JavaScript中如何对一个对象进行深度clone

```js
JSON.parse(JSON.stringify(obj))
```

> js中如何定义class，如何扩展protope？

```js
FUNCTION.protope.myName=function(){
}

```

> ajax是什么？ajax的交互模型？同步和异步的区别？如何解决跨域问题？

- ajax是javascript异步通讯机制

- 同步阻塞，只有一个任务完成后才能完成下一个任务
  异步非阻塞，等待一个任务的同时可以执行另一个任务

- ACAO、nigix


> 请给出异步加载js方案，不少于两种？

async、defer

有关链接：[异步加载js的几种方式](https://www.cnblogs.com/1314-/p/6561475.html)

> 多浏览器检测通过什么？

听都没听过

### 面试题
面试知识点
两个面试官人超级好，都挺帅的。感觉没比我大多少。注意：我不是gay。

> window.onload()在哪个周期中？

没听清楚，懵逼了。

网上答案：当文档内容完全加载完成会触发该事件。可以为此事件注册事件处理函数，并将要执行的脚本代码放在事件处理函数中，于是就可以避免获取不到对象的情况。

> 如何异步加载js？

上面笔试题碰到了，他竟然还问，我回答了js外部引用，这还是同步啊，服了自己了。

script中加入async和reffer。

> vue生命周期？

beforeCreate：此时获取不到prop和data中的数据；
created：可以获取到prop和data中的数据；
beforeMount：获取到了VDOM;
mounted：VDOM解析成了真实DOM;
beforeUpdate：在更新之前调用；
updated：在更新之后调用；
keep-alive：切换组件之后，组件放进activated，之前的组件放进deactivated；
beforeDestory：在组件销毁之前调用，可以解决内存泄露的问题，如setTimeout和setInterval造成的问题。
destory：组件销毁之后调用。

> 缓存

没回答上来

> socket-io与http请求的区别?

> generator如何执行？如何让generator自动next（不通过next.next.next）？

```js
function* f(){
    yeild()
    yeild()
    yeild()
    ...
}
f().next().next()
```

async await
中间加上await

> 遇到过的兼容性问题？

一直做的是PC端的项目，公司不考虑兼容性问题，基本都是跑在chrome浏览器上的。

> promise原理？

三种状态：pending、resolve和reject
三种状态中，除了pending可以改成后两者，其他都不能变，成功就往resolve去，失败就往reject去。

> koa和express？

koa和express原理和用法都是一样的，app.get/post/put要用下，路由分发方便。多了一generator


> 你有哪些优秀代码可以讲讲？哪些好项目？

我提到的是我做得的项目中的贡献。

> 你怎么学习？

github关注大佬动态，看他们博客。慕课网、Stack Overflow，v2、掘金等等。

> 会用哪些linux命令？

mkdir、cd、shutdowm，当时只想到常用的一些。

>看什么书？其中有什么你要说的

犀牛书和红皮书当字典查。generator不太懂其原理。

> 你为什么要从事前端？什么时候？未来展望？为什么要用vue？

- 喜欢前端啊
- 大二的时候
- 未来3-5年成为前端技术大佬，具体措施是将vue原理看懂的同时js底层深入。之后往服务端靠。
- 经朋友推荐，vue轻量级，有模板，上手快。

> echarts如何画图？

我不知道，但是我有一点点想法。
用canvas画，比如要画一个柱状图，先给他一个x轴和y轴，然后将每个柱子往上画，柱子之间的距离和粗细要进行计算
第一、echarts的实现，是通过canvas来实现的。由于canvas的限制，所以echarts在实现的时候多是绘制一些，规则的、可预期的、易于实现的东西。

第二、echarts的核心就是options的配置对象。一般而言，我们使用最多的是直角坐标图、极点图、以及地图。

第三、对于直角坐标，必须配置xAsix和yAxis；对于极点坐标，必须配置radiusAxis和angleAxis；

第四、对series系列的认识，它是一个数组。数组中的每一项代表着一个单独的系列，可以配置显示为散点图scatter、折线图line、柱状图bar等等功能。series的data一般是一个每一项都是数组的数组，类似于：[[],[],[]]这样的形式，里层数组一般代表xAsix、yAxis或者radiusAxis、angleAxis等等。

参考网上链接:[echarts绘图](https://blog.csdn.net/mapbar_front/article/details/78444477)

> 你用什么布局？

flex布局和grid布局

参考网上链接:[页面布局大全](https://www.cnblogs.com/best/p/6136165.html)

>把ui图给你，你要怎么操作？没有UI库呢？

先选UI库，将UI图拆分，拆分成一个个组件。选择布局，flex，grid布局。用html和css实现框架，完成细枝末节。

我的问题：
>技术栈？

>团建？打球？

- 一周一次团建，还没有技术分享会
- 楼上有足球场、旁边是篮球场

>996吗？

不是，弹性工作制度。
潜台词：会很迟。

> 如果我进来，我做什么？

你可以做公司的活动页，小程序的...忘记了。

## 面试后
面试完后千万记得要做笔记，就算没通过，也是一份面试经验，要把它当作学习。水滴石穿，如此垃圾的我，多面面也就会了-面试套路。