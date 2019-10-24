# Nodejs进程

<a name="2OQwO"></a>
## 进程和线程的区别介绍

详情请看[https://baijiahao.baidu.com/s?id=1611925141861592999](https://baijiahao.baidu.com/s?id=1611925141861592999)

1.CPU是什么？<br />CPU是一块超大规模的集成电路，是一台计算机的运算核心和控制核心。它的功能主要是解释[计算机指令](https://www.baidu.com/s?wd=%E8%AE%A1%E7%AE%97%E6%9C%BA%E6%8C%87%E4%BB%A4&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)以及处理计算机软件中的数据，<br />![image.png](https://cdn.nlark.com/yuque/0/2019/png/436565/1565079897374-9107c475-887a-42a5-a66c-053e411b7e60.png#align=left&display=inline&height=288&name=image.png&originHeight=288&originWidth=427&size=204018&status=done&width=427)<br />解释:<br />进程是什么？<br />是执行一段程序，即一旦程序被载入到内存中并准备执行，他就是一个进程，进程是表示资源分配的基本概念，又是调度运行的基本单位，是系统中的并发执行的单位<br />线程是什么？<br />单一进程中执行中每个任务就是一个线程，线程是进程中执行运算的最小单位。<br />1.一个线程只能属于一个进程，但是一个进程可以拥有多个线程，多线程处理就是允许一个进程中在同一时刻执行多个任务。<br />2.父和子进程使用进程间通信机制，用一进程的线程通过读取和写入数据到进程变量来通信<br />3.子进程不对任何其他子进程施加控制，进程的线程可以对同一进程的其他线程事假控制，子进程不能对父进程施加控制，进程中所有的线程都可以对主线程施加控制<br />进程和线程的共同点:<br />进程和线程都有ID/寄存器组，状态和优先权，信息快，创建后都可更改自己的属性，都可与父进程共享资源，都不能直接访问其他无关进程和线程的资源

<a name="6TZxL"></a>
## Nodejs充分利用多核CPU以及它的稳定性

详情请看[https://segmentfault.com/a/1190000007343993?utm_source=tag-newest](https://segmentfault.com/a/1190000007343993?utm_source=tag-newest)

<a name="UzE7X"></a>
### 简介
Nodejs是基于chrome浏览器的v8引擎构建的，他的模型与浏览器类似；<br />

<a name="PY8bt"></a>
### 单线程的好处
1.状态单一<br />2.没有锁<br />3.不需要线程间同步<br />4.减少系统上下文的切换<br />5.有效提高了CPU的使用率
<a name="StJ0j"></a>
### Node创建子进程的4种方式
1.spawn   创建一个子进程来执行命令<br />2.exec      创建一个子进程来执行命令，和spawn不同的是参数不同，他可以通过回调函数来获取子进程的状态<br />3.execFile 启动一个子进程来执行指定文件，注意：该文件的顶部必须声明符号(#!)用来制动进程类型<br />4.fork       和spwan类似，不同点在于创建node的子进程只需要指定要执行的js文件模块即可 <br />
<br />注意：**后面的3种方法都是spawn()的延伸应用。**<br />**<br />**
<a name="Kz6yx"></a>
## 深入了解Nodejs单线程模型


详情请看[https://www.jb51.net/article/118256.htm](https://www.jb51.net/article/118256.htm)<br />
<br />Nodejs采用事件驱动和异步I/O的方式，实现了一个单线程，高并发的运行时环境，而单线程就意味着同一时间只能做一件事。<br />1.高并发<br />高并发的解决方案就是多线程模型，<br />2.事件循环<br />      事件循环可以简单地理解为通过EVENT LOOP进行循环，EVENT QUEUE（事件队列）进行循环，就好比如要去相近的地方做三件事，如果是不使用事件驱动话就会跑6趟去做三件事，但是如果是事件驱动的话那么就可以通过两趟就把三件事请做完，可以很好的提高自己的效率<br />![image.png](https://cdn.nlark.com/yuque/0/2019/png/436565/1565081652857-7c1f1d5b-2dab-4b10-b10a-cce378ae7f75.png#align=left&display=inline&height=280&name=image.png&originHeight=280&originWidth=690&size=108427&status=done&width=690)<br />这个图是nodejs的运行原理，从左到右，从上到下，nodejs被分成四层，分别是应用层，V8引擎层，node api层和LIBUV层<br />应用层：   即Javascript交互层，常见的就是Node.js的模块，比如 http，fs<br />V8引擎层：  即利用V8引擎来解析Javascript语法，进而和下层API交互<br />NodeAPI层：  为上层模块提供系统调用，一般是由C语言来实现，和操作系统进行交互<br />LIBUV层： 即Event Loop，是Node.js实现异步的核心，由LIBUV库来实现，而LIBUV中的线程池是由操作系统内核接受管理的。<br />从上述理解来看，Node.js的单线程仅仅是指Javascript运行在单线程中，而并非Node.js是单线程，在Node中，无论是Linux平台还是Windows平台，内部都是通过线程池来完成IO操作，而LIBUV就是针对不同平台的差异性实现了统一调用<br />3.事件驱动

nodejs的核心是使用事件驱动模式实现了异步I/O<br />3.1事件队列：先进先出(FIFO)的数据结构

```java
/**
 * 定义事件队列
 * 入队：unshfit()
 * 出队：pop()
 * 空队列：length == 0
 */
eventQueue:[],
```
为了方便理解，我们规定：数组的第一个元素是队列的尾部，数组的最后一个元素是队列的头部， unshfit 就是在尾部插入一个元素，pop就是从头部弹出一个元素，这样就实现了一个简单的队列。<br />3.2接受请求

```java
/**
 * 接收用户请求
 * 每一个请求都会进入到该函数
 * 传递参数request和response
 */
processHttpRequest:function(request,response){
    
  //定义一个事件对象
  var event = createEvent({
    params:request.params, //传递请求参数
    result:null, //存放请求结果
    callback:function(){} //指定回调函数
  });
  
  //在队列的尾部添加该事件 
  eventQueue.unshift(event);
},
```
**
<a name="1blmc"></a>
## Process进程Api

- process.arch()       返回当前的系统类型，一个操作系统CPU架构的字符串（系统的处理器），x64代表64位，x84代表32位
- process.platform   返回当前系统的平台  分window和苹果
- process.events      事件对象
- process.argv()       返回一个数组,获取的命令行的参数
- process.argv0       获取nodejs启动时传入的argv[0]的原始值只读副本
- process.config      返回一个js对象，此对象描述了用于编译当前nodejs执行程序时涉及的配置项信息
- process.cpuUsage 获取到当前进程用的CPU时间和系统CPU时间的对象
- process..memoryUsage()  返回内存使用情况
- process.cwd()        返回nodejs进程的当前工作目录//路径
- process.version     返回node的版本
- process.pid()         当前的进程号
- process.ppid()         当前的进程号
- process.versions    返回nodejs及其依赖项的版本，是一个对象
- process.title          返回命令行参数
- process.env           返回系统的环境变量，指向当前shell的环境变量
- process.execPath   返回运行当前进程的可执行文件的绝对路径
- process.stderr        输出错误
```java
process.stderr.write( str )
```


- process.stdout       命令行的输出
```java
console.log = function(d) {
  process.stdout.write(d + '\n');
};
//实现了console.log()
```


- process.stdin         命令行的输入

 
```java
//会在使用 ctrl + c的时候触发次信号：        process.stdin.resume();
//使用Control+C键，可以触发SIGINT信号
process.on('SIGINT', function() {
  console.log('收到SIGINT信号，按Control+D键可以退出进程');
});
```

- process.nextTick    讲一个回调函数放在下次事件循环的顶部  效果更好，描述更准确
```java
process.nextTick(function () {
    console.log('Next event loop!');
});
```


- process.exit()        退出当前进程
```java
process.on('exit',function(code){
    fs.writeFileSync('/tmp/myfile','this must be saves on exit')
})//进程正常退出，其退出码code为0;非正常退出的码为1
```

- process.abort()    触发abort事件，会让nodejs进程立即结束，并生成一个core文件
- process.getgid()  获取进程的GID，GID为Gropld即组ID，用来标识用户组的唯一标识符
- process.setgid()   设置进程的GID
- process.getuid()   获取用户的ID
- process.setuid(id) 设置用户的ID
- process.initgroups(user,extra_group)    初始化group分组访问列表
- process.kill           向指定进程pid发送一个信息singnal
- process.hrtime      返回当前的好精度事件
- process.uptime     返回node程序已经运行的描述
- process.umask      设置或者读取进程文件的权限掩码
- process.chdir        改变工作目录，process.cwd()查看目录切换是否成功，process.chdir('/home/bbb')
- process.uncaughtException   当前进程抛出一个没有被捕捉的意外时，会触发uncaughtException事件。

```java
 process.on('uncaughtException', function (err) {
   console.error('An uncaught error occurred!');
   console.error(err.stack);
 });
//一个未定义的方法，用来制造异常
//nonexistentFunc();
```

- process.on('end',(code)=>{//code:程序退出码}
- process.mainModule   指向启动脚本的模块
