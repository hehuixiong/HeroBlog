---
title: Web前端性能优化
date: 2022-02-26 14:00:00
cover: https://cdn.ithhx.cn/img-blog/20220223164807.png
top_img: https://cdn.ithhx.cn/img-blog/20220223164807.png
categories:
  - 前端优化
tags:
  - 优化
---

# 前端性能优化
[1、前端性能优化指标](https://www.h5w3.com/32655.html)
[2、雅虎前端优化总结](https://www.h5w3.com/32655.html)

## 1、前置知识
面试题：输入url到页面最终呈现都发生了什么？
- url解析：判断输入是关键字搜索还是url访问，对url进行解析
- dns域名解析获取ip地址
  * 缓存查找：浏览器缓存（chrome://net-internals/#dns地址查看）、系统缓存(hosts)、路由器缓存、isp缓存
  * 向本地DNS服务器发送查询报文"query zh.wikipedia.org"
  * 本地DNS服务器检查自身缓存，存在返回，不存在向根域名服务器发送查询报文"query zh.wikipedia.org"，得到顶级域 .org 的顶级域名服务器地址
  * DNS服务器向 .org 域的顶级域名服务器发送查询报文"query zh.wikipedia.org"，得到二级域 .wikipedia.org 的权威域名服务器地址
  * DNS服务器向 .wikipedia.org 域的权威域名服务器发送查询报文"query zh.wikipedia.org"，得到主机 zh 的A记录，存入自身缓存并返回给客户端
- 使用IP建立TCP链接（三次握手）
  * 第一次握手： 建立连接时，客户端发送SYN标记的数据包（syn=j）到服务器，并进入SYN_SENT状态，等待服务器确认；
  * 第二次握手： 服务器收到SYN标记的数据包，必须确认客户的SYN（ack=j+1），同时自己也发送一个SYN包（syn=k），即SYN+ACK包，此时服务器进入SYN_RECV状态；
  * 第三次握手： 客户端收到服务器的SYN+ACK包，向服务器发送确认包ACK(ack=k+1），此包发送完毕，客户端和服务器进入ESTABLISHED（TCP连接成功）状态，完成三次握手。
- 发送http请求，服务器响应，缓存判断（强缓存和协商缓存）
  * 请求：发送命令+发送请求头信息+空白行+请求体（post）
  * 响应：响应状态 + 响应头+空白行+响应体
  * 强缓存：cache-control（max-age）、Expires
  * 协商缓存：返回Etag、Last-modified和请求IF-none-match、IF-modified-since
- 浏览器解析渲染页面
  * 解析HTML，构建dom树，词法分析和语法分析
  * 解析css，生成css规则树，从右往左解析
  * 合并DOM树和CSS规则树，生成render树
  * 布局render树，根据render节点的类型，确定元素大小和位置
  * 绘制render树，绘制页面像素信息
  * 浏览器将各层的信息发送给GUI，GUI将各层合成，展示在屏幕上
  * 细化流程：构件dom树、构建sytle Rules、样式计算、创建布局树、分层、绘制、分块和光栅化、合成和显示
    - 渲染是在渲染进程执⾏的，渲染进程分为渲染主线程、光栅线程、合成线程等
    - 从分块阶段开始，包括分块、光栅化、合成这三步是在⾮主渲染线程执⾏
    - 重排、重绘、合成：开发中尽量减少重排重绘
      * 重排：改变了 DOM 元素的⼏何位置属性，⽐如宽度、⾼度，那么就会触发重新布局（Layout 阶段），及之后的⼦阶段；重排需要更新完整的流⽔线，开销也⽐较⼤
      * 重绘：通过CSS 或 JS 改变了⾮ DOM 元素的⼏何位置属性，⽐如背景⾊、边框⾊等；那么会跳过布局、分层阶段，直接到绘制阶段，执⾏效率⽐重排⾼⼀些
      * 合成：CSS3 动画，⽐如transform，直接在合成线程上合成动画操作，效率⽐较⾼
- 连接结束关闭TCP链接（四次挥手）
  * 第一次挥手是浏览器发完数据后，发送FIN请求断开连接，进入FIN_WAIT_1状态
  * 第二次挥手是服务器收到FIN报文，返回ACK报文段表示同意，进入FIN_WAIT_2状态
  * 第三次挥手是服务器发送FIN报文请求关闭连接，进入LAST_ACK状态
  * 第四次挥手是浏览器收到FIN报文段，向服务器发送ACK报文段，进入TIME_WAIT状态。服务器接收到ACK报文关闭连接，浏览器等待一段时间后，表示服务器已关闭连接，也关闭连接。

## 2、性能检测工具
原理：就是在合适的时机，打上合适的时间戳，或者暴露出事件。然后通过这些时间戳之间的差值，得出⼀个耗时时间。这个耗时时间就可以反映出我们⻚⾯的相关性能。工具如下：

- window全局作用域API：[performance](https://developer.mozilla.org/zh-CN/docs/Web/API/Performance)
- 性能检测对象：[PerformanceObserver.observe()](https://developer.mozilla.org/zh-CN/docs/Web/API/PerformanceObserver/observe)
- 前端框架：[web-vitals](https://www.npmjs.com/package/web-vitals)

### 1、performance
**Performance**接口可以获取到当前页面中与性能相关的信息。它是 High Resolution Time API 的一部分，同时也融合了Performance Timeline API、[Navigation Timing API](https://developer.mozilla.org/en-US/docs/Web/API/Navigation_timing_API)、 [User Timing API](https://developer.mozilla.org/en-US/docs/Web/API/User_Timing_API) 和 [Resource Timing API](https://developer.mozilla.org/en-US/docs/Web/API/Resource_Timing_API)
  * User Timing API ：⽤户⾃⼰定义在代码中通过调⽤ performance.mark（key） ⽅法定义的时间点。
  * Navigation Timing API ： 资源请求的时间戳，它⾥⾯包含的是我们从请求开始到整个⻚⾯的完全显示的各个阶段的时间点，具体时间点如下：

![](https://cdn.ithhx.cn/img-blog/20220223145900.png)


||key值|value值解释|
| :----- | :----- | :----- |
||navigationStart|当前浏览器窗⼝的前⼀个⽹⻚关闭，发⽣unload事件时的时间戳。如果没有前⼀个⽹⻚，就等于fetchStart（也就是输⼊URL开始，第⼀步就是卸载上个⻚⾯）|
||redirectStart|第⼀次重定向开始时的时间戳，如果没有重定向，或者上次重定向不是同源的，则为0|
||redirectEnd|最后⼀次重定向完成，也就是Http响应的最后⼀个字节返回时的时间戳，如果没有重定向，或者上次重定向不是同源的，则为0|
||fetchStart|浏览器准备通过HTTP请求去获取⻚⾯的时间戳。在检查应⽤缓存之前发生|
||domainLookupStart|域名查询开始时的时间戳。如果使⽤持久连接，或者从本地缓存获取信息的，等同于fetchStart|
||domainLookupEnd|域名查询结束时的时间戳。如果使⽤持久连接，或者从本地缓存获取信息的，等同于fetchStart|
||connectStart|HTTP请求开始向服务器发送时的时间戳，如果是持久连接，则等同于fetchStart|
||connectEnd|浏览器与服务器之间的连接建⽴时的时间戳，连接建⽴指的是所有握⼿和认证过程全部结束|
||requestStart|浏览器向服务器发出HTTP请求时（或开始读取本地缓存时）的时间戳|
||responseEnd|浏览器从服务器收到（或从本地缓存读取）最后⼀个字节时（如果在此之前HTTP连接已经关闭，则返回关闭时）的时间戳|
||domLoading|当前⽹⻚DOM结构开始解析时，也就是document.readyState属性变为“loading”、并且相应的readystatechange事件触发时的时间戳|
||domInteractive|当前⽹⻚DOM结构结束解析|
||domContentLoadedEventStart|当前⽹⻚DOMContentLoaded事件发⽣时，也就是DOM结构解析完毕、所有脚本开始运⾏时的时间戳|
||domContentLoadedEventEnd|当前⽹⻚DOMContentLoaded事件发⽣时，也就是DOM结构解析完毕、所有脚本运⾏完成时的时间戳|
||domComplete|当前⽹⻚DOM结构⽣成时，也就是Document.readyState属性变为“complete”|
||loadEventStart|当前⽹⻚load事件的回调函数开始时的时间戳。如果该事件还没有发⽣，返回0|
||loadEventEnd|当前⽹⻚load事件的回调函数结束时的时间戳。如果该事件还没有发⽣，返回0|

```javascript
//window.performance
{
  Performance: {
    eventCounts: EventCounts {size: 36},
    memory: MemoryInfo {totalJSHeapSize: 14794652, usedJSHeapSize: 12567868, jsHeapSizeLimit: 2172649472},
    navigation: PerformanceNavigation {type: 0, redirectCount: 0},
    onresourcetimingbufferfull: null,
    timeOrigin: 1630237780077,
    timing: PerformanceTiming {
      connectEnd: 1630237780080
      connectStart: 1630237780080
      domComplete: 1630237780410
      domContentLoadedEventEnd: 1630237780355
      domContentLoadedEventStart: 1630237780351
      domInteractive: 1630237780351
      domLoading: 1630237780126
      domainLookupEnd: 1630237780080
      domainLookupStart: 1630237780080
      fetchStart: 1630237780080
      loadEventEnd: 1630237780410
      loadEventStart: 1630237780410
      navigationStart: 1630237780077
      redirectEnd: 0
      redirectStart: 0
      requestStart: 1630237780081
      responseEnd: 1630237780083
      responseStart: 1630237780081
      secureConnectionStart: 0
      unloadEventEnd: 0
      unloadEventStart: 0
      [[Prototype]]: PerformanceTiming
  }
    [[Prototype]]: Performance,
  }
}

```

### 2、performanceObserver
PerformanceObserver.observe() ：指定监测的 entry types 的集合。 当 performance entry 被记录并且是指定的 entryTypes 之⼀的时候，性能观察者对象的回调函数会被调⽤。

```javascript
var observer = new PerformanceObserver(callback);
//首个参数是性能观察者参数列表、第二个参数是观察者对象
var observer = new PerformanceObserver(function(list, obj) {
  var entries = list.getEntries();
  for (var i=0; i < entries.length; i++) {
    // Process "mark" and "frame" events
  }
});
//当记录一个指定类型的性能条目时，性能监测对象的回调函数将会被调用。
observer.observe({entryTypes: ["mark", "frame"]});
```

### 3、web-vitals

前端框架，⽬前只能统计'**CLS**' | '**FCP**' | '**FID**' | '**LCP**' | '**TTFB**'。如果需要扩充的话，就可以使⽤上⾯的Performance 进⾏更改

```javascript
import {getCLS, getFID, getLCP} from 'web-vitals';

getCLS(console.log);
getFID(console.log);
getLCP(console.log);
```

## 3、性能指标

### 1、白屏时间FP
输入URL开始，到页面开始有变化，只要有任意像素点的变化，就算是白屏时间完结
```javascript
function getFP() {
  new PerformanceObserver((entryList, observer) => {
    let entries = entryList.getEntries();
    for (let i = 0; i < entries.length; i++) {
      if (entries[i].name === 'first-paint') {
        console.log('FP', entries[i].startTime);
      }
    }
  }).observe({entryTypes: ['paint']});
};
```

### 2、首次内容绘制时间FCP
指的是⻚⾯上绘制了第⼀个元素的时间

FP与FCP的最⼤的区别就在于：FP 指的是绘制像素，⽐如说⻚⾯的背景⾊是灰⾊的，那么在显示灰⾊背景时就记录下了 FP 指标。但是此时 DOM 内容还没开始绘制，可能需要⽂件下载、解析等过程，只有当 DOM 内容发⽣变化才会触发，⽐如说渲染出了⼀段⽂字，此时就会记录下 FCP 指标。因此说我们可以把这两个指标认为是和⽩屏时间相关的指标，所以肯定是最快越好。
```javascript
function getFP() {
  new PerformanceObserver((entryList, observer) => {
    let entries = entryList.getEntries();
    for (let i = 0; i < entries.length; i++) {
      if (entries[i].name === 'first-contentful-paint') {
        console.log('FCP', entries[i].startTime);
      }
    }
  }).observe({entryTypes: ['paint']});
};
```

### 3、首页时间FIRSTPAGE
当onload事件触发的时候，也就是整个⾸⻚加载完成的时候
```javascript
function getFirstPage() {
  console.log('FIRSTPAGE', (performance.timing.loadEventEnd - performance.timing.fetchStart));
};
```

### 4、最大内容绘制LCP
⽤于记录视窗内最⼤的元素绘制的时间，该时间会随着⻚⾯渲染变化⽽变化，因为⻚⾯中的最⼤元素在渲染过程中可能会发⽣改变，另外该指标会在⽤户第⼀次交互后停⽌记录。
```javascript
function getLCP() {
  new PerformanceObserver((entryList, observer) => {
      let entries = entryList.getEntries();
      const lastEntry = entries[entries.length - 1];
      console.log('LCP', lastEntry.renderTime || lastEntry.loadTime);
  }).observe({entryTypes: ['largest-contentful-paint']});
}
```

### 5、首次可交互时间TTI
FCP指标后，首个长任务执行时间点，其后无长任务或2个get请求。
1. 从 FCP 指标后开始计算
2. 持续 5 秒内⽆⻓任务（执⾏时间超过 50 ms）且⽆两个以上正在进⾏中的 GET 请求
3. 往前回溯⾄ 5 秒前的最后⼀个⻓任务结束的时间

![](https://cdn.ithhx.cn/img-blog/20220223154201.png)

```javascript
function getTTI() {
  let time = performance.timing.domInteractive - performance.timing.fetchStart;
  console.log('TTI', time);
};
```

### 6、网络请求耗时TTFB
网络请求耗时(TTFB): responseStart - requestStart

### 7、首次输入延迟FID
从用户第一次与页面交互到浏览器实际能够开始处理事件的时间，在 FCP（首次内容绘制） 和 TTI （首次可交互时间）之间⽤户⾸次与⻚⾯交互时响应的延迟，eg：点击输入框后，因渲染等引起的延迟
```javascript
function getFID() {
  new PerformanceObserver((entryList, observer) => {
    let firstInput = entryList.getEntries()[0];
    if (firstInput) {
        const FID = firstInput.processingStart - firstInput.startTime;
        console.log('FID', FID);
    }
    }).observe({type: 'first-input', buffered: true});
 }
```

### 8、累计位置偏移CLS
⻚⾯渲染过程中突然插⼊⼀张巨⼤的图⽚或者说点击了某个按钮突然动态插⼊了⼀块内容等等相当影响⽤户体验的⽹站。这个指标就是为这种情况⽽⽣的，计算⽅式为：位移影响的⾯积 * 位移距离。如下图： 0.25 * 0.75 = 0.1875 。CLS 推荐值为低于 0.1
```javascript
function getCLS() {
  try {
    let cumulativeLayoutShiftScore = 0;
    const observer = new PerformanceObserver((list) => {
        for (const entry of list.getEntries()) {
            // Only count layout shifts without recent user input.
            if (!entry.hadRecentInput) {
                cumulativeLayoutShiftScore += entry.value;
            }
        }
    });
    observer.observe({type: 'layout-shift', buffered: true});
    document.addEventListener('visibilitychange', () => {
        if (document.visibilityState === 'hidden') {
              // Force any pending records to be dispatched.
              observer.takeRecords();
              observer.disconnect();
              console.log('CLS:', cumulativeLayoutShiftScore);
        }
    });
  } catch (e) {
      // Do nothing if the browser doesn't support this API.
  }
};
```

### 9、使用方法
1. 问题：这几个指标怎么使用
    - vue：定义公用方法类，common.js，mounted阶段页面进行挂载，$nextTick()里对响应方法进行使用
    - react：hooks useEffect()中使用react useEffect(() => {}, []);
    - 公司内部使用打点系统：使用echars绘制，使用均值统计、百分位数统计、样本分布统计输出性能
    - 自己使用：使用谷歌lighthouse插件检查性能
2. ⾕歌的标准，关注：LCP（最大内容绘制时间）、FID（首次输入延迟）、CIS（累计位移偏移）
||Good|Poor|Percentile|
| :----- | :----- | :----- | :----- |
|Largest Contentful Paint|<=2500ms|>4000ms|75|
|First Input Delay|<=100ms|>300ms|75|
|Cumulative Layout Shift|<=0.1|>0.25|75|
  * LCP 代表了页面的速度指标，虽然还存在其他的一些体现速度的指标。一是指标实时更新，数据更精确，二是代表着页面最大元素的渲染时间，通常来说页面中最大元素的快速载入能让用户感觉性能还挺好。
  * FID 代表了页面的交互体验指标，毕竟没有一个用户希望触发交互以后页面的反馈很迟缓，交互响应的快会让用户觉得网页挺流畅。
  * CLS 代表了页面的稳定指标，尤其在手机上这个指标更为重要。因为手机屏幕挺小，CLS 值一大的话会让用户觉得页面体验做的很差。

## 4、优化方法
### 1、LCP
#### 1、影响元素
影响白屏时间，相关因素如下：
  * body前是否存在阻塞的script标签，以及是否存在⻓时间执⾏的任务，即JS包⼤⼩问题以及是否启⽤了JS异步加载。
  * ⽹速问题
#### 2、提升方法
  * 提⾼带宽（⽹速）
  * 需要使⽤webpack进⾏tree-shaking
  * 使⽤路由懒加载，只有在使⽤的时候在进⾏路由加载
  * 部署CDN，缩短⽤户与节点之间的距离（⽹速）
  * 建⽴缓存，提⾼下次加载速度。
  * 开启gzip压缩。
  * 不要在头部添加任何script标签，或使用js异步加载defer。
  * 对于少量⼩图标（单个尽量不要超过10K的），我们可以使⽤url-loader打包。或者使⽤将图标转化为字体库，异步进⾏加载。
  * 对于⼤图标的话，需要做到在展示的时候再去加载。也就是当图⽚出现到浏览器窗⼝的时候再去加载，⽽不是⾸屏的图⽚全部加载。
### 2、CIS
#### 1、提升方法
  * 如果经常需要变动的元素，脱离⽂档流，或者是占据位置，只是隐藏。
  * 对于位移等操作，使⽤动画代替，使⽤transform
  * 在定义图⽚的时候，就应该给出具体的宽⾼。
### 3、FID
对于⽤户可操作时间，影响⼀个是注册的事件是否可以被执⾏（说的通俗点就是JS脚本是否加载完毕），以及是否存在⻓任务。那么我们就可以有以下解决⽅案：
  * 对⽂件进⾏懒加载，不要⼀次性把所有的JS加载出来。这就需要使⽤路由懒加载，在跳转到某个路由的时候，再去加载他的脚本资源。这样就可以保证JS加载速度的优化。
  * 不要在响应事件⾥有过多的运算，导致卡顿。如果确有需要，应当开启webWorker，新起线程运算。
### 4、bigpipe框架
bigPipe是由facebook提出来的⼀种动态⽹⻚加载技术。它将⽹⻚分解成称为pagelets的⼩块，然后分块传输到浏览器端，进⾏渲染。它可以有效地提升⾸屏渲染时间。bigpipe的适⽤是服务端进⾏渲染，然后将⼀块⼀块的⽂件传递给前端。

> TIP
> 面试：做了哪些性能相关的工作？
> 如何统计性能、发现问题、处理问题、提升效果

## 5、实战
整体优化思路及解析：
1. 从浏览器输入url到页面各阶段做了什么，进行性能优化
2. 根据前端性能指标进行优化
3. 框架特有的性能优化点：小程序分包、vue路由按需加载等
4. 优化方法：开发规范、技术架构设计、系统架构设计

### 1、浏览器加载优化
1. DNS预解析、预链接
```html
<!-- 开启隐式预解析：默认情况，浏览器对a标签中与当前域名不在同一域的相关域名进行预获取且缓存结果，对于https失效 -->
<meta http-equiv="x-dns-prefetch-control" content="on">
<!-- 只解析域名，不进行资源下载 -->
<link rel="dns-prefetch" href="http://www.baidu.com" />
<!-- 将会做 DNS 解析，TLS 协商和 TCP 握手 -->
<link  rel="preconnect" href="//baidu.com">
```
2. http请求阶段
    - 减少http请求合理利用时序：资源合并（雪碧图）、使用promise.all并发请求
    - 减少资源体积：减少cookie信息、图片格式优化、gzip静态资源压缩、webpack打包压缩
    - 合理利用缓存：cdn、http缓存（强缓存和协商缓存）、本地缓存（localStorage、sessionStorage）
3. 浏览器渲染阶段：下载css并解析、下载js文件并解析会影响页面首屏渲染
    - 减少重排重绘，尽量使用css动画，或者添加will-change属性
    - script脚本放在body元素中⻚⾯内容的后⾯，避免JS阻碍html解析，减少⽩屏时间
    - css文件尽量放在head中，尽快下载和解析
    - 使用预解析和异步加载：prefetch、prerender、preload、async、defer
    - 服务器端渲染ssr
    - 资源按需引入：路由懒加载，组件库按需引入
```html
<!-- 在浏览器空闲时下载资源 -->
<link rel="prefetch" href="https://css-tricks.com/a.png">
<!-- 浏览器会提前完成所有的资源加载，执行，渲染并保存在内存里 -->
<link rel="prerender" href="https://css-tricks.com">
<!-- 提前下载资源，影响资源加载顺序，后置下载资源前置下载 -->
<link rel="preload" href="https://fonts.gstatic.com/s/sofia/v8/bjl.woff2" as="font" crossorigin="anonymous">

<!-- ⽂件加载完成后，会执⾏此脚本，执⾏顺序⽆法保证，先加载完成的先执⾏ -->
<script src="./static/demo1.js" async></script>
<!-- 延迟执⾏脚本，解析完</html>后执⾏，执⾏顺序不变 -->
<script src="./static/defer-demo1.js" defer></script>
```
### 2、性能指标监测
分析工具：lighthouse、web-vitals

底层api：performance

指标解析：
  * FP 首次绘制（白屏时间）、LCP 最大内容渲染：路由懒加载、缓存、脚本异步加载

  * TTI首次可交互时间：performance.timing.domInteractive -performance.timing.fetchStart

  * FID首次输入延迟：路由懒加载、建少js计算逻辑

  * CIS累计位置偏移：css动画设置位移、图片设置具体宽高

### 3、优化方法

#### 1、开发规范
  * css开发规范：雪碧图、图片格式优化、尽量减少重排和重绘，使用动画
  * js开发规范：promise并发、预解析和懒加载、开发细节（for循环缓存对象、尽量少使用闭包、递归的边界条件）

#### 2、技术框架
  * 路由懒加载
  * 组件按需引入：babel插件转换
  * webpack打包优化配置：资源压缩、资源拆分部署至cdn（externals）
  * 小程序：分包加载、setData操作优化、限频接口调用优化等

#### 3、架构优化
  * cdn预热
  * nginx缓存配置、gzip压缩开启
  * ssr及预渲染
  * 后端bigpipe引入：动态网页加载技术
