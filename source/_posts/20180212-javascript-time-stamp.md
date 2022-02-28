---
title: JavaScript时间戳总结
date: 2018-02-12 14:22:00
cover: https://cdn.jsdelivr.net/gh/hehuixiong/image-blog/20220223163912.png
top_img: https://cdn.jsdelivr.net/gh/hehuixiong/image-blog/20220223163912.png
categories:
  - JavaScript
tags:
  - js
---

javascript时间戳经常用到，现在总结一下。

# 一、时间戳函数
```javascript
<script>   
function getLocalTime(nS) {   
   return new Date(parseInt(nS) * 1000).toLocaleString().replace(/:\d{1,2}$/,' ');   
}   
alert(getLocalTime(1293072805));   
</script> 
```
也可以用如下，想取几位就几位，注意，空格也算！

```javascript
<script>   
function getLocalTime(nS) {   
    return new Date(parseInt(nS) * 1000).toLocaleString().substr(0,14)}   
alert(getLocalTime(1293072805));   
</script>   
```

如果想弹出：2018-02-12 14:00:00这个格式的也好办
```javascript
<script>   
function getLocalTime(nS) {   
   return new Date(parseInt(nS) * 1000).toLocaleString().replace(/年|月/g, "-").replace(/日/g, " ");    
}   
alert(getLocalTime(1177824835));   
</script>
```

另外，也可以这么写：
```javascript
function   formatDate(now)   {   
  var   year=now.getYear();   
  var   month=now.getMonth()+1;   
  var   date=now.getDate();   
  var   hour=now.getHours();   
  var   minute=now.getMinutes();   
  var   second=now.getSeconds();   
  return   year+"-"+month+"-"+date+"   "+hour+":"+minute+":"+second;   
}
  var   d=new   Date(1230999938);   
  alert(formatDate(d));
```

# 二、知识普及

### 1、当前系统区域设置格式(toLocaleDateString和toLocaleTimeString)
```javascript
例子:(new Date()).toLocaleDateString() + " " + (new Date()).toLocaleTimeString() 
结果: 2018年2月12日 16:13:11 
```

### 2.普通字符串(toDateString和toTimeString)
```javascript
例子: (new Date()).toDateString() + " " + (new Date()).toTimeString() 
结果:Tue Jan 29 2008 16:13:11 UTC+0800 
```

### 3.格林威治标准时间(toGMTString)
```javascript
例子: (new Date()).toGMTString() 
结果:Tue, 29 Jan 2008 08:13:11 UTC 
```

### 4.全球标准时间(toUTCString)
```javascript
例子: (new Date()).toUTCString() 
结果:Tue, 29 Jan 2008 08:13:11 UTC 
```

### 5.Date对象字符串(toString)
```javascript
例子: (new Date()).toString() 
结果:Tue Jan 29 16:13:11 UTC+0800 2008

Date对象构造函数 
Date对象具有多种构造函数。 
new Date() 
new Date(milliseconds) 
new Date(datestring) 
new Date(year, month) 
new Date(year, month, day) 
new Date(year, month, day, hours) 
new Date(year, month, day, hours, minutes) 
new Date(year, month, day, hours, minutes, seconds) 
new Date(year, month, day, hours, minutes, seconds, microseconds) 
Date对象构造函数参数说明 
milliseconds - 距离JavaScript内部定义的起始时间1970年1月1日的毫秒数 
datestring - 字符串代表的日期与时间。此字符串可以使用Date.parse()转换 
year - 四位数的年份，如果取值为0-99，则在其之上加上1900 
month - 0(代表一月)-11(代表十二月)之间的月份 
day - 1-31之间的日期 
hours - 0(代表午夜)-23之间的小时数 
minutes - 0-59之间的分钟数 
seconds - 0-59之间的秒数 
microseconds - 0-999之间的毫秒数 
Date对象返回值 
如果没有任何参数，将返回当前日期 
如果参数为一个数字，将数字视为毫秒值，转换为日期 
如果参数为一个字符串，将字符串视为日期的字符串表示，转换为日期 
还可以使用六个构造函数精确定义，并返回时间 
示例 
var d1 = new Date(); 
document.write(d1.toString()); 
var d2 = new Date("2009-08-08 12:12:12); 
document.write(d2.toString()); 
var d3 = new Date(2009, 8, 8); 
document.write(d3.toString()); 
Date做为JavaScript的一种内置对象，必须使用new的方式创建。 
Date对象在JavaScript内部的表示方式是，距1970年1月1日午夜(GMT时间)的毫秒数(时间戳)，我们在这里也把Date的内部表示形式称为时间戳。可以使用getTime()将Date对象转换为Date的时间戳，方法setTime()可以把Date的时间戳转换为Date的标准形式。 
Date函数使用语法 
date.方法名(参数1,参数2,...); 
Date.方法名(); 
date代表一个日期对象的实例，Date代表日期对象，date.方法名调用的为对象的成员函数 
Date.方法名调用的为对象的静态函数 
示例 
var d=new Date(); 
var d2=Date.UTC(); 
JavaScript_Date函数按功能分类 
日期获取类函数 
Date() 函数 -- Date对象的构造函数 
getDate() 函数 -- 返回date对象中的月份中的天数(1-31) 
getDay()函数 -- 返回date对象中的星期中的天数(0-6) 
getFullYear() 函数 -- 返回date对象中的四位数年份 
getHours()函数 -- 返回date对象中的小时数(0-23) 
getMilliseconds() 函数 -- 返回date对象中的毫秒数(0-999) 
getMinutes() 函数 -- 返回date对象中的分钟数(0-59) 
getMonth() 函数 -- 返回date对象中的月份数(0-11) 
getSeconds() 函数 -- 返回date对象中的秒数(0-59) 
getTime() 函数 -- 返回date对象的时间戳表示法(毫秒表示) 
getTimezoneOffset() 函数 -- 返回本地时间与用UTC表示当前日期的时间差，以分钟为单位 
getUTCDate() 函数 -- 返回date对象中用世界标准时间(UTC)表示的月份中的一天(1-31) 
getUTCDay() 函数 -- 返回date对象中用世界标准时间(UTC)表示的周中的一天(0-6) 
getUTCFullYear() 函数 -- 返回date对象中用世界标准时间(UTC)表示的四位年份 
getUTCHours() 函数 -- 返回date对象中用世界标准时间(UTC)表示的小时数(0-23) 
getUTCMilliseconds() 函数 -- 返回date对象中用世界标准时间(UTC)表示的毫秒数(0-999) 
getUTCMinutes() 函数 -- 返回date对象中用世界标准时间(UTC)表示的分钟数(0-59) 
getUTCMonth() 函数 -- 返回date对象中用世界标准时间(UTC)表示的月份数(0-11) 
getUTCSeconds() 函数 -- 返回date对象中用世界标准时间(UTC)表示的秒数(0-59) 
getYear() 函数 -- 返回date对象的年份(真实年份减去1900) 
Date.UTC()函数 -- 返回date对象距世界标准时间(UTC)1970年1月1日午夜之间的毫秒数(时间戳) 
日期设置类函数 
setDate() 函数 -- 设置date对象中月份的一天，并返回date对象距1970年1月1日午夜之间的毫秒数(时间戳) 
setFullYear() 函数 -- 设置date对象中的年份，月份和天，并返回date对象距1970年1月1日午夜之间的毫秒数(时间戳) 
setHours() 函数 -- 设置date对象的小时，分钟，秒和毫秒，并返回date对象距1970年1月1日午夜之间的毫秒数(时间戳) 
setMilliseconds() 函数 -- 设置date对象的毫秒数，并返回date对象距1970年1月1日午夜之间的毫秒数(时间戳) 
setMinutes() 函数 -- 设置date对象的分钟，秒，毫秒，并返回date对象距1970年1月1日午夜之间的毫秒数(时间戳) 
setMonth() 函数 -- 设置date对象中月份，天，并返回date对象距1970年1月1日午夜之间的毫秒数(时间戳) 
setSeconds() 函数 -- 设置date对象中月份的一天，并返回date对象距1970年1月1日午夜之间的毫秒数(时间戳) 
setTime() 函数 -- 使用毫秒数设置date对象，并返回date对象距1970年1月1日午夜之间的毫秒数(时间戳) 
setUTCDate() 函数 -- 设置date对象中用世界标准时间(UTC)表示的月份的一天，并返回date对象距1970年1月1日午夜之间的毫秒数(时间戳) 
setUTCFullYear() 函数 -- 设置date对象中用世界标准时间(UTC)表示的年份，月份和天，并返回date对象距1970年1月1日午夜之间的毫秒数(时间戳) 
setUTCHours() 函数 --- 设置date对象中用世界标准时间(UTC)表示的小时，分钟，秒和毫秒，并返回date对象距1970年1月1日午夜之间的毫秒数(时间戳) 
setUTCMilliseconds() 函数 -- 设置date对象中用世界标准时间(UTC)表示的毫秒数，并返回date对象距1970年1月1日午夜之间的毫秒数(时间戳) 
setUTCMinutes() 函数 -- 设置date对象中用世界标准时间(UTC)表示的分钟，秒，并返回date对象距1970年1月1日午夜之间的毫秒数(时间戳) 
setUTCMonth() 函数 -- 设置date对象中用世界标准时间(UTC)表示的月份，天，并返回date对象距1970年1月1日午夜之间的毫秒数(时间戳) 
setUTCSeconds() 函数 -- 设置date对象中用世界标准时间(UTC)表示的秒，毫秒，并返回date对象距1970年1月1日午夜之间的毫秒数(时间戳) 
setYear() 函数 -- 设置date对象的年份(真实年份减去1900) 
日期打印类函数 
toDateString() 函数 -- 返回date对象的日期部分的字符串表示 
toGMTString() 函数 -- 返回date对象的格林威治时间(GMT)的字符串表示 
toLocaleDateString函数 -- 返回date对象的日期部分的本地化字符串 
toLocaleTimeString函数 -- 返回date对象的时间部分的本地化字符串 
toTimeString()函数 -- 返回date对象的时间部分的字符串 
toUTCString函数 -- 返回date对象的世界标准时间(UTC)的字符串表示 
日期解析类函数 
Date.parse() 函数 -- 解析一个日期的字符串，并返回该日期距1970年1月1日午夜之间的毫秒数(时间戳) 
JavaScript_Date函数按照字母分类 
Date() 函数 -- Date对象的构造函数 
getDate() 函数 -- 返回date对象中的月份中的天数(1-31) 
getDay()函数 -- 返回date对象中的星期中的天数(0-6) 
getFullYear() 函数 -- 返回date对象中的四位数年份 
getHours()函数 -- 返回date对象中的小时数(0-23) 
getMilliseconds() 函数 -- 返回date对象中的毫秒数(0-999) 
getMinutes() 函数 -- 返回date对象中的分钟数(0-59) 
getMonth() 函数 -- 返回date对象中的月份数(0-11) 
getSeconds() 函数 -- 返回date对象中的秒数(0-59) 
getTime() 函数 -- 返回date对象的时间戳表示法(毫秒表示) 
getTimezoneOffset() 函数 -- 返回本地时间与用UTC表示当前日期的时间差，以分钟为单位 
getUTCDate() 函数 -- 返回date对象中用世界标准时间(UTC)表示的月份中的一天(1-31) 
getUTCDay() 函数 -- 返回date对象中用世界标准时间(UTC)表示的周中的一天(0-6) 
getUTCFullYear() 函数 -- 返回date对象中用世界标准时间(UTC)表示的四位年份 
getUTCHours() 函数 -- 返回date对象中用世界标准时间(UTC)表示的小时数(0-23) 
getUTCMilliseconds() 函数 -- 返回date对象中用世界标准时间(UTC)表示的毫秒数(0-999) 
getUTCMinutes() 函数 -- 返回date对象中用世界标准时间(UTC)表示的分钟数(0-59) 
getUTCMonth() 函数 -- 返回date对象中用世界标准时间(UTC)表示的月份数(0-11) 
getUTCSeconds() 函数 -- 返回date对象中用世界标准时间(UTC)表示的秒数(0-59) 
getYear() 函数 -- 返回date对象的年份(真实年份减去1900) 
Date.parse() 函数 -- 解析一个日期的字符串，并返回该日期距1970年1月1日午夜之间的毫秒数(时间戳) 
setDate() 函数 -- 设置date对象中月份的一天，并返回date对象距1970年1月1日午夜之间的毫秒数(时间戳) 
setFullYear() 函数 -- 设置date对象中的年份，月份和天，并返回date对象距1970年1月1日午夜之间的毫秒数(时间戳) 
setHours() 函数 -- 设置date对象的小时，分钟，秒和毫秒，并返回date对象距1970年1月1日午夜之间的毫秒数(时间戳) 
setMilliseconds() 函数 -- 设置date对象的毫秒数，并返回date对象距1970年1月1日午夜之间的毫秒数(时间戳) 
setMinutes() 函数 -- 设置date对象的分钟，秒，毫秒，并返回date对象距1970年1月1日午夜之间的毫秒数(时间戳) 
setMonth() 函数 -- 设置date对象中月份，天，并返回date对象距1970年1月1日午夜之间的毫秒数(时间戳) 
setSeconds() 函数 -- 设置date对象中月份的一天，并返回date对象距1970年1月1日午夜之间的毫秒数(时间戳) 
setTime() 函数 -- 使用毫秒数设置date对象，并返回date对象距1970年1月1日午夜之间的毫秒数(时间戳) 
setUTCDate() 函数 -- 设置date对象中用世界标准时间(UTC)表示的月份的一天，并返回date对象距1970年1月1日午夜之间的毫秒数(时间戳) 
setUTCFullYear() 函数 -- 设置date对象中用世界标准时间(UTC)表示的年份，月份和天，并返回date对象距1970年1月1日午夜之间的毫秒数(时间戳) 
setUTCHours() 函数 --- 设置date对象中用世界标准时间(UTC)表示的小时，分钟，秒和毫秒，并返回date对象距1970年1月1日午夜之间的毫秒数(时间戳) 
setUTCMilliseconds() 函数 -- 设置date对象中用世界标准时间(UTC)表示的毫秒数，并返回date对象距1970年1月1日午夜之间的毫秒数(时间戳) 
setUTCMinutes() 函数 -- 设置date对象中用世界标准时间(UTC)表示的分钟，秒，并返回date对象距1970年1月1日午夜之间的毫秒数(时间戳) 
setUTCMonth() 函数 -- 设置date对象中用世界标准时间(UTC)表示的月份，天，并返回date对象距1970年1月1日午夜之间的毫秒数(时间戳) 
setUTCSeconds() 函数 -- 设置date对象中用世界标准时间(UTC)表示的秒，毫秒，并返回date对象距1970年1月1日午夜之间的毫秒数(时间戳) 
setYear() 函数 -- 设置date对象的年份(真实年份减去1900) 
toDateString() 函数 -- 返回date对象的日期部分的字符串表示 
toGMTString() 函数 -- 返回date对象的格林威治时间(GMT)的字符串表示 
toLocaleDateString函数 -- 返回date对象的日期部分的本地化字符串 
toLocaleTimeString函数 -- 返回date对象的时间部分的本地化字符串 
toTimeString()函数 -- 返回date对象的时间部分的字符串 
toUTCString函数 -- 返回date对象的世界标准时间(UTC)的字符串表示 
Date.UTC()函数 -- 返回date对象距世界标准时间(UTC)1970年1月1日午夜之间的毫秒数(时间戳)
```

# 三、Javascript的时间戳和php的时间戳转换
js的时间戳通常是13位，php的时间戳是10位,转换函数如下：
```javascript
var nowtime = (new Date).getTime();/*当前时间戳*/   
/*转换时间，计算差值*/   
function comptime(beginTime,endTime){   
  var secondNum = parseInt((endTime-beginTime*1000)/1000);//计算时间戳差值      

  if(secondNum>=0&&secondNum<60){   
      return secondNum+'秒前';   
  }   
  else if (secondNum>=60&&secondNum<3600){   
      var nTime=parseInt(secondNum/60);   
      return nTime+'分钟前';   
  }   
  else if (secondNum>=3600&&secondNum<3600*24){   
      var nTime=parseInt(secondNum/3600);   
      return nTime+'小时前';   
  }   
  else{   
      var nTime = parseInt(secondNum/86400);   
      return nTime+'天前';   
  }   
}   

t = comptime("1324362556",nowtime);//timestamp为PHP通过ajax回传的时间戳   

alert(t); 
```