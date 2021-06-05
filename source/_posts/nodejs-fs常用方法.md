---
title: nodejs中fs的常用方法
categories: nodejs
tags:[nodejs,fs]
---

```javascript
let fs = require('fs')

/**
flag <string> 参见文件系统 flag 的支持。 默认值: 'w'。
	- w 写入
	- r 读取
	- a 追回
 */


/**
 * 打开文件
 * fs.open(path[, flags[, mode]], callback)
 * fs.openSync(path[, flags, mode])
 * 
 * 将 string 写入到 fd 指定的文件
 * fs.write(fd, string[, position[, encoding]], callback)
 * fs.writeSync(fd, buffer[, offset[, length[, position]]])
 * 
 * 关闭fd文件
 * fs.close(fd, callback)
 * fs.closeSync(fd)
 */

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


/**
 * 简单文件写入
 * fs.writeFile(file, data[, options], callback)
 * fs.writeFileSync(file, data[, options])
 */

// fs.writeFile('hello.txt', '简单方式写入\n',{flag:'a'}, function(err){
// 	if(err) throw err;
// })

// fs.writeFileSync('hello.txt','简单方式写入，同步方法\n',{flag:'a'})


/**
 * 同步、异步、简单文件写入都不适合大文件的写入，性能较差，容易导致内存溢出
 * 
 * 流式文件写入
 * fs.createWriteStream(path[, options])
 * 流式文件读取
 * fs.createReadStream(path[, options])
 */

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
// })
// // 读取可读流中的数据，必须为可读流绑定一个data事件，data事件绑定完毕，它会自动开始读取数据
// // data最长长度为65536
// fr.on('data',function(data){
// 	// 将读取到的数据写入到可写流中
// 	fw.write(data);
// 	// 读完后关闭可写流
// })


/**
 * 简单文件读取
 * fs.readFile(path[, options], callback)
 * 		读取的文件返回的data格式是BUffer格式
 * fs.readFileSync(path[, options])
 * 
 * 简单文件写入
 * fs.writeFile(file, data[, options], callback)
 * fs.writeFileSync(file, data[, options])
 */

// fs.readFile('hello.txt',function(err,data){
// 	console.log(data.toString())
// 	fs.writeFile('hello2.txt', data, function(err){
// 		if(err) throw err
// 	})
// })

// let data = fs.readFileSync('hello.txt')
// console.log(data.toString())
// fs.writeFileSync('hello3.txt',data)


/**
 * 通过检查文件系统来测试给定的路径是否存在
 * fs.exists(path, callback)	弃用
 * fs.existsSync(path)
 * 		返回结果：true:存在，false:不存在
 */
let res = fs.existsSync('a23.mp3')
// console.log(res)

/**
 * 获取文件的状态
 * fs.stat(path[, options], callback)
 * fs.statSync(path[, options])
 * 返回一个对象，对象中保存当前文件状态的相关信息
 * 		size:文件的大小
 * 		isFile:是否是一个文件
 * 		isDirectory:是否是一个目录
 */

// fs.stat('a.mp3', function(err,stats){
// 	console.log(stats)
// })

/**
 * 删除文件或符号链接
 * 对空或非空的目录均不起作用。 若要删除目录，则使用 fs.rmdir()。
 * fs.unlink(path, callback)
 * fs.unlinkSync(path)
 */

// fs.unlinkSync('01.txt')


/**
 * 读取目录的内容
 * fs.readdir(path[, options], callback)
 * fs.readdirSync(path[, options])
 * 		返回的files是一个字符串数组，每个元素就是一个文件夹或文件的名字
 */

// fs.readdir('.', function(err,files){
// 	console.log(files)
// })

// let dir = fs.readdirSync('.')
// console.log(dir)

/**
 * 截断文件，将文件截断为指定长度
 * fs.truncate(path[, len], callback)
 * fs.truncateSync(path[, len])
 */

// fs.truncateSync('02-1.txt',10)

/**
 * 创建一个目录
 * fs.mkdir(path[, options], callback)
 * fs.mkdirSync(path[, options])
 * 		options:
 * 				recursive:指示是否要创建父目录 默认false;如果不使用这个参数创建一个已经存在的目录会报错
 * 删除一个目录
 * fs.rmdir(path[, options], callback)
 * fs.rmdirSync(path[, options])
 */

// fs.mkdirSync('./目录11')
// fs.mkdir('./目录1/目录2/目录3', { recursive: true }, (err) => {
//   if (err) throw err;
// });

// fs.rmdirSync('./目录11')

/**
 * 把 oldPath 文件重命名为 newPath 提供的路径名; 可移动文件
 * fs.rename(oldPath, newPath, callback)
 * fs.renameSync(oldPath, newPath)
 */
// fs.renameSync('01-1.txt', './目录1/a.txt')

/**
 * 监视文件的修改，每当访问文件时都会调用 listener 回调。
 * fs.watchFile(filename[, options], listener)
 * options对象：
 * 		interval 属性，指示轮询目标的频率（以毫秒为单位）
 * listener回调的两个参数：(两个参数都是stats对象)
 * 		curr：当前文件状态
 * 		prev：修改前文件状态
 */

// fs.watchFile('hello.txt',{interval:1000}, function(curr,prev){
// 	console.log(curr.size,prev.size)
// })

```