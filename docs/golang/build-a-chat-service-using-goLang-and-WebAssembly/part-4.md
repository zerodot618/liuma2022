# 第四篇：使用 Gloang 和 Webassembly 构建一个聊天服务

- 外文地址：https://dev.to/taherfattahi/build-a-chat-service-using-golang-and-webassembly-part-4-5bka
- 作者：[Taher Fattahi](https://dev.to/taherfattahi)

为了与服务器通信，我们需要一个Websocket服务器-我更喜欢我们用nodejs来开发我们的Websocket服务器端。

源码地址：[Github Link](https://github.com/taherfattahi/golangChatWebAssembly)

## 什么是 WebSocket
WebSocket是一个简单易用、快速且经过测试的WebSocket客户端和服务器实现。

Websocket规范定义了一个在web浏览器和服务器之间建立连接的API。WebSocket用于创建实时游戏、聊天应用程序、显示股票数据等。

## NodeJS 和 WebSocket
NodeJS是一个基于 `Chrome V8 JavaScript` 引擎的 JavaScript 运行时。使用 NodeJS 很容易构建 WebSocket 服务器实现。

传统HTTP依赖于客户端请求，你必须将请求发送给服务器，然后从服务器获得响应，但在WebSocket中，你可以直接从服务器向客户端发送数据，我们可以双向调用。

让我一句话向你介绍 WebSocket：一种事件驱动的、对web友好的HTTP替代方案。Websocket 不需要每次都通过客户端请求从服务器获取数据。

## 使用 NodeJS 构建 WebSocker 服务器
作为先决条件，你的系统中应该安装Nodejs和NPM。如果你没有，从这里下载并安装: https://nodejs.org/zh-cn/download/

1. 一旦你准备好了NodeJs，打开终端并输入以下命令:

```
$ mkdir webSocketServer && cd webSocketServer
```
2. 创建 nodejs 项目环境:

```
$ npm init -y
```
3. 接下来，执行以下命令将ws库作为依赖项安装:

```
$ npm install ws
```
4. 创建一个 javascript 文件，例如 `webSocketServer.js`，并粘贴以下代码来创建一个web服务器:

```javascript
// 导入 ws 模块
const WebSocketServer = require('ws');

// 创建 一个 websocket 服务器
const wss = new WebSocketServer.Server({ port: 8080 })

// 使用 websocket 创建连接
wss.on('connection', ws => {
    console.log('new client connected');
    // 发生消息
    ws.on('message', data => {
        console.log(`Client has sent us: ${data}`)
    });
    // 处理当客户端从服务器断开连接时要做的事情
    ws.on('close', () => {
        console.log('the client has connected')
    });
    // 处理客户端连接错误
    ws.onerror = function () {
        console.log('Some Error occurred')
    }
});

console.log('The WebSocket server is running on port 8080');
```

这段代码将为你创建一个基本的WebSocket服务器，你可以在创建WebSocket服务器时提供任何你想要的端口。

5. 为了测试它，打开一个终端并输入:

```
$ node webSocketServer.js
```

## 参考资料
1. [piesocket.com](https://www.piesocket.com/blog/nodejs-websocket#:~:text=What%20is%20WebSocket%3F,%2C%20displaying%20stock%20data%2C%20etc.)

在[第5篇](./part-5.md)中，我们将使用 Golang WebAssembly 编写 WebSocket 客户端服务，并连接到WebSocket nodejs服务器端。