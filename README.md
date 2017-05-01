初探nodejs
=========

## 两种启动服务器的简单测试

```js

var http = require('http');

http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Hello World\n');
}).listen(1337, '127.0.0.1');

console.log('Server running at http://127.0.0.1:1337/');

```

```js

var http = require('http');

var server = http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Hello Nodejs\n');
});

server.listen(1337, '127.0.0.1');

console.log('Server running at http://127.0.0.1:1337/');

```

参数：

*	请求体`req`---request
*	相应体`res`---response 

## 宿主环境的差异

浏览器中的环境变量，在全局中是可以拿到的。

但是nodejs是拿不到的。

## Node.js的模块与Commonjs规范

什么鬼，云里雾里啊！！！

## 模块的分类

*	核心模块--------http fs path）
*	文件模块--------var until = require('./util.js')
*	第三方模块------var promise = require('bluebird')

## 模块的流程

*	创建模块
*	导出模块
*	加载模块
*	使用模块

**student.js**

```js

function add(student) {
  console.log('Add student:' + student)
}

exports.add = add

```

**teacher.js**

```js

function add(teacher) {
  console.log('Add Teacher:' + teacher)
}

exports.add = add

```

**klass.js** 

```js

var student = require('./student')
var teacher = require('./teacher')

function add (teacherName, students) {
  teacher.add(teacherName)

  students.forEach(function(item, index) {
    student.add(item);
  })
}

exports.add = add

```

**index.js**

```js

var klass = require('./class')

klass.add('yby', ['白富美', '高富帅'])

```

在执行过程中遇到的报错信息：

* Uncaught SyntaxError: Unexpected token ILLEGAL （由于文件中某处的标点符号，出现多余或者缺失造成）
* SyntaxError: Unexpected reserved word  （代码中使用了保留字）

## Node.js API

io.js

node.js

两个阵营

## URL网址解析的好帮手

三种方法：

*	url.parse()  解析地址
*	url.format() 合并地址
*	url.resolve() 合并地址

例子：

```js

url.parse('http://imooc.com/course/list')

{
	protocol: 'http:',
	slashes: true,
	auth: null,
	host: 'imooc.com',
	port: null,
	hostname: 'imooc.com',
	hash: null,
	search: null,
	query: null,
	pathname: '/course/list',
	href: 'http://imooc.com/course/list'
}

url.parse('http://imooc.com:8080/course/list?from=yby&course=node#floor1')

{
	protocol: 'http:',
	slashes: true,
	auth: null,
	host: 'imooc.com:8080',
	port: 8080,
	hostname: 'imooc.com',
	hash: #floor1,
	search: ?form=yby&course=node,
	query: form=yby&course=node,
	pathname: '/course/list?from=yby&course=node',
	href: 'http://imooc.com:8080/course/list?from=yby&course=node#floor1'
}

url.parse('http://imooc.com:8080/course/list?from=yby&course=node#floor1', true)

// parse 的第二个参数来指定解析的参数是用query模块还是queryString模块，默认是false，设置为true，解析出来的参数是对象格式

url.parse('//imooc.com/course/list', true, true)

// parse 的第三个参数处理地址不确定协议的情况，从而正确的解析地址

```

*	protocol---地址使用的协议是什么
*	slashes---是否有协议的双斜线
*	auth---[nodejsAPI](https://nodejs.org/api/url.html#url_urlobject_auth)
*	host---域名
*	port---端口
*	hostname---主机名
*	hash---页面中的锚点
*	search---查询字符串参数
*	query---发送给http服务器的数据
*	pathname---访问资源路径名
*	path---访问资源路径
*	href---没别解析的完整连接

```js

url.format({
	protocol: 'http:',
	slashes: true,
	auth: null,
	host: 'imooc.com:8080',
	port: 8080,
	hostname: 'imooc.com',
	hash: #floor1,
	search: ?form=yby&course=node,
	query: form=yby&course=node,
	pathname: '/course/list?from=yby&course=node',
	href: 'http://imooc.com:8080/course/list?from=yby&course=node#floor1'
})

http://imooc.com:8080/course/list?from=yby&course=node#floor1

```

```js

url.resolve('http://imooc.com/', 'course/list')

http://imooc.com/course/list

```

## querystring.stringfy()

```js

querystring.stringify({name:'yby', course:['jade','node'],form:''})

// 'name=yby&course=jade&course=node&form='

querystring.stringify({name:'yby', course:['jade','node'],form:''},',')

// 'name=yby,course=jade,course=node,form='

querystring.stringify({name:'yby', course:['jade','node'],form:''},',',':')

// 'name:yby,course:jade,course:node,form:'

```

参数：

*	第一个参数：要传输的参数对象
*	第二个参数：告诉函数数据之间的分隔符是什么
*	第三个参数：告诉函数键值对之间的分隔符是什么
*	根据指定参数生成字符串

## querystring.parse()

```js

querystring.parse('name=yby&course=jade&course=node&form=')

// { name: 'yby', course: [ 'jade', 'node' ], form: '' }

querystring.parse('name=yby,course=jade,course=node,form=', ',')

// { name: 'yby', course: [ 'jade', 'node' ], form: '' }

 querystring.parse('name:yby,course:jade,course:node,form:', ',', ':')

// { name: 'yby', course: [ 'jade', 'node' ], form: '' }

```

参数：

*	第一个参数：要解析的字符串
*	第二个参数：告诉函数数据之间的分隔符是什么
*	第三个参数：告诉函数键值对之间的分隔符是什么
*	根据指定参数解析出一个数据对象

## querystring.escape() 和 querystring.unescape()  

转义和反转义函数

```js

querystring.escape('哈哈>')

// '%E5%93%88%E5%93%88%3E'

querystring.unescape('%E5%93%88%E5%93%88%3E')

// '哈哈>'


```

## http知识

大概流程：

*	http客户端发起请求，创建端口
*	http服务器在端口监听客户端请求
*	http服务器向客户端返回状态和内容


> 1.	浏览器搜索自身的DNS缓存（大概一分钟）
> 2.	搜索操作系统自身的DNS缓存（浏览器没有找到缓存或缓存已经失效）
> 3.	读取本地的HOST文件

> 4.	浏览器发起一个DNS的一个系统调用
>> 1). 宽带运营商服务器查看本身缓存

>> 2). 运营商服务器发送一个迭代DNS解析请求（运营商服务器把结果返回操作系统内核同时缓存起来，操作系统内核把结果返回浏览器，最终浏览器拿到了想要访问域名对应的IP地址）
>
> 5.浏览器获得域名对应的IP地址后，发起HTTP“三次握手”
>
> 6.  TCP/IP连接建立起来后，浏览器就可以向服务器发送HTTP请求了使用了比如说，用HTTP的GET方法请求一个根域里的一个域名，协议可以采用HTTP 1.0的一个协议
> 
> 7. 服务器接收到这个请求，根据路径参数，经过后端的一些处理之后，把处理后的一个结果的数据返回给浏览器，如果是慕课网的页面就会把完整的HTML页面代码返回给浏览器。
> 
> 8. 浏览器拿到了慕课网的完整的HTML页面代码，在解析和渲染这个页面的时候，里面的JS、css、图片静态资源，他们同样也是一个个HTTP请求都需要经过上面的主要的七个步骤。
> 
> 9.浏览器根据拿到的资源对页面进行渲染，最终把一个完整的页面呈献给用户

**http头和正文信息：**

`http头` 发送的一些附加的信息：内容类型、服务器发送相应的日期、HTTP状态码

`正文` 就是用户提交的表单数据

## 请求方法

* GET---读取数据
* POST---提交数据
* PUT---更新
* DELETE---删除
* HEAD---与GET方法相同，
* TRACE---
* OPRIONS---
* ...

## 状态码

* 1xx---指示信息
* 2xx---请求成功---200
* 3xx---重定向
* 4xx---客户端错误---400（客户端请求有语法错误） 401（请求未经过授权） 403（服务器端收到请求，但拒绝服务） 404（找不到，请求资源不存在）
* 5xx---服务器端错误--- 500（服务器端遇到未知错误） 503（服务器端当前不能处理该请求）

## HTTP模块

### HTTP概念进阶

*	什么是回调？
*	什么是同步和异步？
*	什么是I/O？
*	什么是单线程、多线程？
*	什么是阻塞、非阻塞？
*	什么是事件？
*	什么是事件驱动？
*	什么是基于事件驱动的回调？
*	什么是事件循环？

## http源码解读

*	什么是作用域？

```js

var globalVariable = 'This is global variable'

function globalFunction() {
  var localVariable = 'This is local variable'

  console.log('Visit global/local variable')
  console.log(globalVariable)
  console.log(localVariable)
}

globalFunction()

```

*	什么是上下文？

```js

var pet = {
  words: '...'
  speak: function() {
    console.log(this.words)
    console.log(this === pet)
  }
}

pet.speak()

function pet(words) {
  this.words = words

  console.log(this.words)
  console.log(this === global)
}

pet('...')

function Pet(words) {
  this.words = words
  this.speak = function() {
    console.log(this.words)
    console.log(this)
  }
}

var cat = new Pet('Miao')

cat.speak()

```

引入作用域和上下文，是为了引入call和apply连个方法

```js

var pet = {
  words: '...'
  speak: function(say) {
    console.log(say + ' ' + this.words)
  }
}

// pet.speak('Speak')

var dog = {
  words: 'Wang'
}

pet.speak.call(dog, 'Speak')

function Pet(words) {
  this.words = words
  this.speak = function() {
    console.log(this.words)
  }
}

function Dog(words) {
  Pet.call(this, words)
  // Pet.apply(this, arguments)
}

var dog = new Dog('Wang')

dog.speak()

```

## HTTP源码解读

不想读，不想读，不想读，重要的事情说三遍

## HTTP性能测试

使用Apache ab做测试[http://httpd.apache.org/docs/2.0/en/programs/ab.html](http://httpd.apache.org/docs/2.0/en/programs/ab.html)

## HTTP小爬虫

借助HTTP API

```js

var http = require('http')
var cheerio = require('cheerio')
var url = 'http://www.imooc.com/learn/348'

function filterChapters(html) {
  var $ = cheerio.load(html)
  var chapters = $('.chapter')

  // [{
  //   chapterTitle: '',
  //   video: [
  //     title: '',
  //     id: ''
  //   ]
  // }]

  var courseData = []
  chapters.each(function(item) {
    var chapter = $(this)
    var chapterTitle = chapter.find('strong').text()
    var videos = chapter.find('.video li')
    var chapterData = {
      chapterTitle: chapterTitle,
      videos: []
    }

    videos.each(function(item) {
      var video = $(this).find('.J-media-item')
      var videoTitle = video.text()
      var id = video.attr('href').split('/video/')[1]

      chapterData.videos.push({
        title: videoTitle,
        id: id
      })
    })

    courseData.push(chapterData)
  })

  return courseData
}

function printCourseInfo(courseData) {
  courseData.forEach(function(item) {
    var chapterTitle = item.chapterTitle
    console.log(chapterTitle + '\n')

    item.videos.forEach(function(video) {
      console.log('【' + video.id + '】' + video.title + '\n')
    })
  })
}

http.get(url, function(res) {
  var html = ''

  res.on('data', function(data) {
    html += data
  })

  res.on('end', function() {
    var courseData = filterChapters(html)

    printCourseInfo(courseData)
  })
}).on('error', function() {
  console.log('获取课程数据出错')
})

```

## 事件模块小插曲












