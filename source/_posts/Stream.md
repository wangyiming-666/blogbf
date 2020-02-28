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
console.log("弟"+counter+"次取数据",chunk);
})

rs.on("end",function(){
    console.log("done.....");
})
```

**对于on data事件的这样理解的 因为所有流都是事件的实例  所有不需要实例化**

**Events对象 直接使用**  

**data事件回调函数里面显示每次取得数据   每次最大读取64kb**

**end事件显示读取完后运行的代码**

**运行结果如下：**

```
弟13取数据 <Buffer 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 ... 65486 more bytes>
弟14取数据 <Buffer 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 ... 65486 more bytes>
弟15取数据 <Buffer 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 ... 65486 more bytes>
弟16取数据 <Buffer 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 61 ... 65486 more bytes>
done.....
```

**文件的可写流  单次最大写入16384字节   首先调用fs的可写流方法**

```js
var fs = require("fs");
var ws = fs.createWriteStream("abc.txt");
```

**然后写入数据**

```js
ws.write("abc",function(err){
if(err)console.log(err)
else console.log("write");
})
console.log(ws.writableHighWaterMark);

ws.end("写完了")
ws.on("finish",function(){
    console.log("done");
})
```

**end事件代表写完的标志   如果没有这个标志那么文件会在写到16384字节 并且会在写的文件的最后加入“写完了”**

**finish事件是在文件写完后运行的代码**

**运行结果如下**

```
$ node 文件可写流.js
16384
write
done
```

**文件内容**

```
abc写完了
```

**关于pipe（）    类似于一个管道 在数据特别大的时候会用得到**

```js
var fs = require("fs");
var rs = fs.createReadStream("abc.txt");
var ws = fs.createWriteStream("1M.txt");
ws.on("pipe",function(src) {
    console.log("有数据在流动");
})
rs.pipe(ws)
```

**运行效果如下**

```
$ node 管道.js
有数据在流动
```

