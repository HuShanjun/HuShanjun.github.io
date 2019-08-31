#  nodejs 多进程篇

## 子进程创建模块 child_process

1. spawn() : 启动一个子进程用来执行命令
2. exec() : 启动一个子进程来执行命令，通过回调函数获取子进程的情况
3. execFile(): 启动一个子进程用来执行命令
4. fork(): 与spqwn类似，但是必须指定js文件或模块

## 用例

### 一. spawn 启动子进程

1. 执行操作系统命令

```js

try {
    let cp = require('child_process');
    let ll = cp.spawn('ls', ['-l']);
    ll.stdout.on('data', function(data){
        console.log('ll.stdout : \n',data.asciiSlice());
    });
    ll.stderr.on('data', function(data){
        console.log('ll.stderr : \n',data.asciiSlice());
    });
    ll.on('exit', function(code, signal){
        console.log('child process exit => code : '+code, ' signal : ', signal);
    });
} catch (e) {
    console.log('error ', e.message);
}

```

2. 启动一个node文件
```js
try {
    let cp = require('child_process');
    let worker = cp.spawn('node', [`${__dirname}/work.js`]);
    
    worker.stdout.on('data', function(data){
        console.log('worker.stdout : \n',data.asciiSlice());
    });
    worker.stderr.on('data', function(data){
        console.log('worker.stderr : \n',data.asciiSlice());
    });
    worker.on('exit', function(code, signal){
        console.log('child process exit => code : '+code, ' signal : ', signal);
    });
} catch (e) {
    console.log('error ', e.message);
}

```

### exec 创建子进程
1. 启动linux命令

```js
try {
    let cp = require('child_process');
    let ll = cp.exec('ls -l', function(err, stdout, stderr){

    });
    
    ll.stdout.on('data', function(data){
        console.log('ll.stdout : \n',data);
    });
    ll.stderr.on('data', function(data){
        console.log('ll.stderr : \n',data.asciiSlice());
    });
    ll.on('exit', function(code, signal){
        console.log('child process exit => code : '+code, ' signal : ', signal);
    });
} catch (e) {
    console.log('error ', e.message);
}
```
2. 启动node文件
```js
try {
    let cp = require('child_process');
    let worker = cp.exec('node ./work.js', function(err, stdout, stderr){

    });
    
    worker.stdout.on('data', function(data){
        console.log('worker.stdout : \n',data);
    });
    worker.stderr.on('data', function(data){
        console.log('worker.stderr : \n',data.asciiSlice());
    });
    worker.on('exit', function(code, signal){
        console.log('child process exit => code : '+code, ' signal : ', signal);
    });
} catch (e) {
    console.log('error ', e.message);
}

```
### execFile ：使用方法同 exec
### fork 启动子进程只能启动通过node启动js文件
```js
try {
    let cp = require('child_process');
    let worker = cp.fork('work.js');

    worker.on('exit', function(code, signal){
        console.log('child process exit => code : '+code, ' signal : ', signal);
    });
} catch (e) {
    console.log('error ', e.message);
}
```

