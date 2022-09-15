---
title: "使用Console技巧提高JS调试效率"
date: 2021-08-01 20:20:12
cover: https://s-gz-2804-blog-image.oss.dogecdn.com/20210830174244.png
photos: https://s-gz-2804-blog-image.oss.dogecdn.com/20210830174244.png
categories:
  - Console调试
tags:
  - console
---
其实 JavaScript 给到我们很多调试工具来调试代码，那问问你自己，你又知道多少呢？用到多少呢？

大部分前端开发在 JavaScript 调试代码的常规用法都是直接console.log，直接输出某一个变量或者返回数据里面的对象数据。当然毋庸置疑这样输出来调试是没有问题的。但是不是最优雅的方式来调试代码，其实还有更好的办法。作为一个有追求的技术人才，有更好的调试方式为什么不去使用呢？

![](https://s2.ax1x.com/2019/10/11/ubREy4.png)
我们先来了解清楚浏览器的console。浏览器的console对象有提供自带的调试控制台。console对象只能在浏览器的 JavaScript 中使用，也就是说客户端应用可用而服务端应用不可用。它的作用或者效果会根据不同的浏览器而不同，但是基础使用方式和功能是基本一致的。不过console是可以在任何前端语言或者框架中使用。
***

# console.log
最常用的使用方式就是console.log，对前端开发工程师来说就是家常便饭了。以下是一个简单的使用例子。

```javascript
function sayHello(name) {
  console.log(name)
}

sayHello('Indrek')
```

> 以上sayHello方法接收一个名字，然后在控制台输出出来。

![](https://s2.ax1x.com/2019/10/12/ujmEr9.png)

现在我们开始玩以下更有趣的调试方法。加入我们现在想知道sayHello这个方法被调用了多少次，这样我们应该怎么调试呢？其实有一个很简单的办法就是使用console.count().
***

# console.count
count()方法会输出某一个标示被调用了几次。如果没有穿任何参数，count()默认为使用默认标示defaut。

```javascript
function sayHello(name) {
  console.count()
  console.log(name)
}

sayHello('Indrek')
sayHello('William')
sayHello('Kelly')
```

> 以上代码就会在控制台输出以下结果：

![](https://s2.ax1x.com/2019/10/12/ujm7ZR.png)
上面的例子实现了统计某一个方法被调用的次数，那如果我们想统计每个同名字(name)的在这个方法里面被调用了多少次呢？要调试这种其实也很简单，只要直接吧name直接传入count就可以了。

```javascript
function sayHello(name) {
  console.count(name)
}

sayHello('Indrek')
sayHello('William')
sayHello('Kelly')
sayHello('Indrek')
```
‍(∩｀-´)⊃━☆ﾟ.*・｡ﾟ 巴拉巴拉！就是那么简单，我们就可以跟踪同名的参数在sayHello方法里面被调用的次数了！

![](https://s2.ax1x.com/2019/10/13/uj1xhR.png)
***

# console.warn

这个控台答应方法会输出一个警告信息。在你开发 APIs 或者开发工具的时候使用。console.warn这个方法在你需要警告用户的时候特别实用，例如漏掉了某个参数或者是让开发者知道你的 API/插件包的版本已经失效的时候使用。

```javascript
function sayHello(name) {
  if (!name) {
    console.warn('No name given')
  }
}

sayHello()
```

> 上面的代码检测了sayHello方法的参数是否漏传。如果name参数没有传，一个警告消息就会被打印到控制台中。让开发者可以思考问题出在哪里。

![](https://s2.ax1x.com/2019/10/13/uj85Q0.png)
***

# console.table
如果是我们在调试数组或者对象时，console.table是一个非常实用的调试方法来在控制台打印数据。数组里面的每一个元素都会在表格的行里面展示。以下是的水果名数组作为一个例子，如果我们把这个数组传入console.table，我们会看到一个含有这个水果名数据以表格的方式在控制台被打印出来。

```javascript
const fruits = ['kiwi', 'banana', 'strawberry']

console.table(fruits)
```

> 我们一起来围观以下在控制台里面的展示效果

![](https://s2.ax1x.com/2019/10/13/ujGlkQ.png)

看到了这个，你会不会灵光一闪想到 mmp，如果是一个很大的数组这种表格化的展示方式是多么的实用啊！对的！例如一个上百个数据的数组，我们使用这种调试方法来打印就很方便了。为了可以让我们用双眼见证这个说法的真实性，我们用代码说话吧！

```javascript
const fruits = [
  'Apple',
  'Watermelon',
  'Orange',
  'Pear',
  'Cherry',
  'Strawberry',
  'Nectarine',
  'Grape',
  'Mango',
  'Blueberry',
  'Pomegranate',
  'Carambola',
  'Plum',
  'Banana',
  'Raspberry',
  'Mandarin',
  'Jackfruit',
  'Papaya',
  'Kiwi',
  'Pineapple',
  'Lime',
  'Lemon',
  'Apricot',
  'Grapefruit',
  'Melon',
  'Coconut',
  'Avocado',
  'Peach',
]
```
我们使用console.table来打印一下上面这个大数组试试看吧。

![](https://s2.ax1x.com/2019/10/13/ujGvNQ.png)

> 这种展示方式简直就是一目了然！这样妈妈再也不用担心我们调试数据的时候蒙圈了！՞༘✡ (๑ •̀ㅂ•́)و✧

但是问题少年们，我们可是有梦想的工程师，如果是用来调试对象会是怎么样呢？来吧亲自动手丰衣足食，上代码！

```javascript
const pets = {
  name: 'Simon',
  type: 'cat',
}

console.table(pets)
```
注意了兄弟姐妹们，现在我们打印的是对象不是数组。在控制台的表格现在有两个键值name和type。之前是 0，1，2，3，4…
![](src="https://s2.ax1x.com/2019/10/13/ujtOHK.png")

这种方式可以替代普遍使用的直接用 log 打印对象数据，表格化的展示相对还是更加清晰的。问题少年再次发问，如果我们想多个对象一起打印呢？

```javascript
const pets = {
  name: 'Simon',
  type: 'cat',
}

const person = {
  firstName: 'Indrek',
  lastName: 'Lasn',
}

console.table(pets, person)
```
与预想一致，两个不同键值的对象被才分成两个表格在控制台打印出来了。
![](https://s2.ax1x.com/2019/10/13/ujNQuq.png)

如果我们不想分开两个表格打印，可否在一个表格显示呢？可以的！只要把两个对象放入一个数组就 ok 了。

```javascript
const pets = {
  name: 'Simon',
  type: 'cat',
}

const person = {
  firstName: 'Indrek',
  lastName: 'Lasn',
}

console.table([pets, person])
```
现在我们看到两个对象在一个表格里面展示了，键值被放在表格的头部了，因为键值在两个对象里面是不一样的。
![](https://s2.ax1x.com/2019/10/13/ujNtC4.png)
***

# console.group
当我们是在调试集合（sets）或者是关联数据（linked-data），可以使用嵌套组来优化你的控制台输出。使用console.group()来创建一个嵌套的组。

```javascript
console.log('This is the first level')
console.group()
console.log('Level 2')
console.group()
console.log('Level 3')
console.warn('More of level 3')
console.groupEnd()
console.log('Back to level 2')
console.groupEnd()
console.log('Back to the first level')
```
以下是一个嵌套的层级提示输出，在调试关联或者层级数据的时候特别实用。
![](https://s2.ax1x.com/2019/10/13/ujNy5D.png)

> 使用console.groupCollapsed()可以把所有嵌套的层级收起来，使用鼠标点击时可以展开查看。

***

# 总结
作为一名优秀的程序员，我们应该尽量在合适的场景或者合适的情况下运用在提供到给我的调试工具。所以这一篇文章提到的调试方式，我们应该在开发调试的过程中多合理运用，习惯后我们会发现调试起来会更加敏捷和高效。
***

> #通过技术悟出人生道理# 💭
> “人生无常，写的了一行是一行
> Code now or never” ～ 络擎·NetEngine