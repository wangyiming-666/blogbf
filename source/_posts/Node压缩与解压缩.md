---
title: Node压缩与解压缩
date: 2020-02-28 11:07:03
tags: 学习心得
categories: Node.js
---

引zlib模块  和fs模块

```js
var fs = require("fs");
var zlib = require("zlib");
```

然后创建写入和写出的流

```js
var rs = fs.createReadStream("1M.txt");
var z = zlib.createGzip();  //压缩模块的压缩模式
var ws = fs.createWriteStream("1M.txt.gz");
rs.pipe(z).pipe(ws);
```

通过pipe（）创建流引入到压缩模块的压缩模式然后写入到压缩文件

解压缩模式和上面类似

```js
var rs = fs.createReadStream("1M.txt.gz");
var unz = zlib.createUnzip(); //压缩模块的解压缩模式
var ws = fs.createWriteStream("1M1.txt");
rs.pipe(unz).pipe(ws);
```

通过pipe（）创建流引入到压缩模块的解压缩模式写入到解压缩的文件

顺带一提node的os模块  可以查看系统信息以及内存

先引入os模块

```js
var os = require("os"); //引入os模块
var cups = os.cpus(); //os模块的cpus（）方法
console.log(cups);  //这里可以查看cpu的信息
var m = os.freemem();//freemem()用来查看内存信息
console.log(m/1024/1024/1024);//这里是内存信息
```

