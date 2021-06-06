---
title: nodejs中fs的常用方法
categories: nodejs
tags: [nodejs,fs]
summary: nodejs 中 fs的一些常用方法总结、案例列举
---

> flag <string> 参见文件系统 flag 的支持。 默认值: 'w'。
>	- w 写入
>	- r 读取
>	- a 追回



```javascript

let fs = require('fs')

```

### open、write、close

>	打开文件
fs.open(path[, flags[, mode]], callback)
fs.openSync(path[, flags, mode])<br>
将 string 写入到 fd 指定的文件
fs.write(fd, string[, position[, encoding]], callback)
fs.writeSync(fd, buffer[, offset[, length[, position]]])<br>
关闭fd文件
fs.close(fd, callback)
fs.closeSync(fd)


```javascript
// 同步方法：
// let fd = fs.openSync('hello.txt','w')
// fs.writeSync(fd,'同步方法写入\n')
// fs.closeSync(fd)

// 异步方法：
// fs.open('hello.txt','a',function(err,fd){
// 	if (err) throw err;
// 	fs.write(fd,'异步方法写入\n',function(err){
// 		if(err) throw err;
// 		fs.close(fd,function(){
// 			if(err) throw err;
// 			console.log('异步写入成功，关闭')
// 		})
// 	})
// })
```

### createWriteStream、createReadStream

> 同步、异步、简单文件写入都不适合大文件的写入，性能较差，容易导致内存溢出<br>
	流式文件写入
	fs.createWriteStream(path[, options])<br>
	流式文件读取
	fs.createReadStream(path[, options])<br>

```javascript
// let fw = fs.createWriteStream('a.mp3',{flags:'w'})
// let fr = fs.createReadStream('b.mp3')
// fw.once('open',function(){
// 	console.log('流式文件写入打开')
// })
// fw.once('close',function(){
// 	console.log('流式文件写入关闭')
// })
// fr.once('open',function(){
// 	console.log('流式文件读取打开')
// })
// fr.once('close',function(){
// 	console.log('流式文件读取关闭')
// 	// 读取结束后，关闭写入流
// 	fw.close()
//  //  fw.end(); 都可以
// })
// // 读取可读流中的数据，必须为可读流绑定一个data事件，data事件绑定完毕，它会自动开始读取数据
// // data最长长度为65536
// fr.on('data',function(data){
// 	// 将读取到的数据写入到可写流中
// 	fw.write(data);
// 	// 读完后关闭可写流
// })
// rs.on('end',function(){
// 	console.log('读取结束,关闭之前执行')
// })
// rs.on('error',function(err){
// 	console.log(err)
// })
// ws.on('finish',()=>{
// 	console.log('写入完成')
// })

// 可读流打开了
// 可写流打开了
// 读取结束,关闭之前执行
// 可读流关闭了
// 写入完成
// 可写流关闭了
```

### 管道流 pipe

```
// 写
let ws = fs.createWriteStream('b.mp3')

// 读
let rs = fs.createReadStream("吴亦凡-翱翔.mp3")

// pipe可将可读流中的内容直接写入到可写流中
rs.pipe(ws)
```

### readFile、writeFile

> 简单文件读取
fs.readFile(path[, options], callback)
		读取的文件返回的data格式是BUffer格式
fs.readFileSync(path[, options])<br>
简单文件写入
fs.writeFile(file, data[, options], callback)
fs.writeFileSync(file, data[, options])

```javascript
// fs.readFile('hello.txt',function(err,data){
// 	console.log(data.toString())
// 	fs.writeFile('hello2.txt', data, function(err){
// 		if(err) throw err
// 	})
// })

// let data = fs.readFileSync('hello.txt')
// console.log(data.toString())
// fs.writeFileSync('hello3.txt',data)
```

### writeFile

> 简单文件写入 	{flag:'a'}追回的方式
	fs.writeFile(file, data[, options], callback)
  fs.writeFileSync(file, data[, options])

```javascript
// fs.writeFile('hello.txt', '简单方式写入\n',{flag:'a'}, function(err){
// 	if(err) throw err;
// })

// fs.writeFileSync('hello.txt','简单方式写入，同步方法\n',{flag:'a'})
```

### appendFile

> 追加数据到文件，如果文件尚不存在则创建文件
fs.appendFile(path, data[, options], callback)
fs.appendFileSync(path, data[, options])

```javascript
// fs.appendFile('hello.txt','appendFile的方式向文件中追回内容',function(err){
// 	if(err) throw err;
// })
```

### exists
> 通过检查文件系统来测试给定的路径是否存在
fs.exists(path, callback)	弃用
fs.existsSync(path)
		返回结果：true:存在，false:不存在

```javascript
let res = fs.existsSync('a23.mp3')
// console.log(res)
```

### stat

> 获取文件的状态
	fs.stat(path[, options], callback)
	fs.statSync(path[, options])
	返回一个对象，对象中保存当前文件状态的相关信息
		size:文件的大小
		isFile:是否是一个文件
		isDirectory:是否是一个目录

```javascript
// fs.stat('a.mp3', function(err,stats){
// 	console.log(stats)
// })
```

### unlink

> 删除文件或符号链接
  对空或非空的目录均不起作用。 若要删除目录，则使用 fs.rmdir()。
  fs.unlink(path, callback)
  fs.unlinkSync(path)

```javascript
// fs.unlinkSync('01.txt')
```

### readdir

> 读取目录的内容
fs.readdir(path[, options], callback)
fs.readdirSync(path[, options])
		返回的files是一个字符串数组，每个元素就是一个文件夹或文件的名字

```javascript
// fs.readdir('.', function(err,files){
// 	console.log(files)
// })

// let dir = fs.readdirSync('.')
// console.log(dir)
```

### truncate(截断文件)

> 截断文件，将文件截断为指定长度
	fs.truncate(path[, len], callback)
	fs.truncateSync(path[, len])

```javascript
// fs.truncateSync('02-1.txt',10)
```

### mkdir、rmdir

> 创建一个目录
	fs.mkdir(path[, options], callback)
	fs.mkdirSync(path[, options])
			options:
					recursive:指示是否要创建父目录 默认false;如果不使用这个参数创建一个已经存在的目录会报错<br>
	删除一个目录
	fs.rmdir(path[, options], callback)
	fs.rmdirSync(path[, options])

```javascript
// fs.mkdirSync('./目录11')
// fs.mkdir('./目录1/目录2/目录3', { recursive: true }, (err) => {
//   if (err) throw err;
// });

// fs.rmdirSync('./目录11')
```

### rename

> 把 oldPath 文件重命名为 newPath 提供的路径名; 可移动文件
  fs.rename(oldPath, newPath, callback)
  fs.renameSync(oldPath, newPath)
```javascript
// fs.renameSync('01-1.txt', './目录1/a.txt')
```

### watchFile

> 监视文件的修改，每当访问文件时都会调用 listener 回调。
  fs.watchFile(filename[, options], listener)
  options对象：
  		interval 属性，指示轮询目标的频率（以毫秒为单位）
  listener回调的两个参数：(两个参数都是stats对象)
  		curr：当前文件状态
  		prev：修改前文件状态

```javascript
// fs.watchFile('hello.txt',{interval:1000}, function(curr,prev){
// 	console.log(curr.size,prev.size)
// })

```


### 案例1

> 判断服务器上有没有upload目录，如果没有的话创建这个目录，如果有的话不去操作


```javascript
const fs = require('fs')
const path = './upload'
fs.stat(path,function(err,stats){
  // 判断有没有upload文件
  if(err){
    // 如果没有upload文件直接创建文件夹
    mkdir(path)
  }else{
    if( !stats.isDirectory() ){
      // 如果有upload文件，且不是文件夹，就删除upload文件，再创建文件夹
      fs.unlink(path,function(unlinkErr){
        if(unlinkErr) throw unlinkErr;
        mkdir(path)
      })
    }
  }
})
function mkdir(path){
  fs.mkdir(path,function(err){
    if(err) throw err;
    console.log('创建目录成功')
  })
}
```

### 案例2

> wwwroot文件夹下有images css js 以及index.html，找出wwwroot中所有的文件夹，并放到一个数组中

```javascript
const fs = require('fs')
let path = './wwwroot'
let arr = []
// 找出wwwroot中的所有文件
// fs.readdir('./wwwroot',function(err,files){
//   if(err) throw err;
//   files.forEach(v=>{
//     fs.stat('./wwwroot/'+v,function(statErr,data){
//       if(statErr) throw statErr;
//       console.log(v,data.isDirectory())
//       if( data.isDirectory() ){
//         arr.push(v)
//       }
//       console.log(arr)
//     })
//   })
//   console.log(arr)  // 返回空数组。原因：arr.push在stat的回调里面，执行顺序是在log arr后再执行arr.push
// })

// 正确写法1：使用递归的方法
// fs.readdir(path,(err,data)=>{
//   if(err){
//     console.log(err);
//     return
//   }
//   (function getDir(i){
//     if(i == data.length){
//       console.log(arr);
//       return;
//     }
//     fs.stat(path+'/'+data[i],(error,stats)=>{
//       if( stats.isDirectory() ){
//         arr.push(data[i])
//       }
//       getDir(i+1)
//     })
//   })(0)
// })

// 正确写法2：async await

// 1、定义一个异步方法判断是否是文件夹
async function isDir(path){
  return new Promise((resolve,reject)=>{
    fs.stat(path,(err,stats)=>{
      if(err){
        console.log(err);
        reject(err);
        return;
      }
      if( stats.isDirectory() ){
        resolve(true)
      }else{
        resolve(false)
      }
    })
  })
}
// 2、遍历目录
fs.readdir(path,async (err,files)=>{
  if(err){
    console.log(err);
    return
  }
  for(let i=0;i<files.length;i++){
    let dir = await isDir(path+'/'+files[i])
    if( dir ){
      arr.push(files[i])
    }
  }
  console.log(arr)
})

```
