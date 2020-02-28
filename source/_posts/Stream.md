---
title: Node Stream
date: 2020-02-28 09:08:53
tags: 学习心得
categories: Node.js
---

**所有的stream都是EventEmitter的实例 意味着stream可以使用事件的所有方法**

**data：每次读取都会产生这个事件，取到部分流（chunk）到数据**

**end：读取完会产生的事件**

**首先创建一个对象使用对象的createReadStream( )方法**

```javascript
var fs = require("fs");
var rs =  fs.createReadStream("abc.txt");
```

**然后声明一个变量存储提取的次数counter** 

```js
var counter = 0;
rs.on("data",function(chunk){
counter++;
console.log("弟"+counter+"取数据",chunk);
})

rs.on("end",function(){
    console.log("done.....");
})
```

**对于on data事件的这样理解的 因为所有流都是事件的实例  所有不需要实例化**

**Events对象 直接使用**  

**data事件回调函数里面显示每次取得数据**

**end事件显示读取完后运行的代码**