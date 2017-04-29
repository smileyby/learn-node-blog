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