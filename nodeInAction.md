# node in action

## 前言

写一本关于nodejs的书籍是一项很有挑战性的工作。

## Part1 Node fundamentals（node 基本原理）

## Welcome to Node.js

这个章节覆盖以下内容：

1. node.js是什么
2. 服务器上的javascript
3. Node 的异步和事件性质
4. Node 设计用于的应用程序类型
5. Node示例程序

所以node.js是什么？它可能是你已经听到过的术语。或许已经使用node。也许你对此感到好奇。目前，Node 非常流行，年轻（2009年首次亮相）。这是 GitHub 上观看次数第二多的项目，它在 Google 群组中拥有相当多的追随者(http://groups.google.com/group/nodejs) 和 IRC 频道 (http://webchat.freenode.net/?channels=node.js)，并且拥有超过 15,000 个社区 mod-规则发布在 NPM（包管理器）(http://npmjs.org) 中。这一切都是为了说，这个平台背后有相当大的吸引力。

瑞安·达尔 (Ryan Dahl) 谈 Node：你可以在JSCONF Berlin 2009网站上观看Node的第一次演示，演讲者是创建者Ryan Dahl。链接是http://jsconf.eu/2009/video_nodejs_by_ryan_dahl.html。

官方网站（http://www.nodejs.org）将Node定义为“基于Chrome的JavaScript运行时平台，可轻松构建快速、可扩展的网络应用。Node.js使用事件驱动、非阻塞I/O模型，使其轻巧高效，非常适合跨分布式设备运行的数据密集型实时应用。”
在本章中，我们将探讨以下概念：

1. 为什么 JavaScript 对于服务器端开发很重要
2. 浏览器如何使用JavaScript处理I/O
3. Node在服务器端如何处理I/O
4. DIRTy应用程序的含义以及它们为什么适合Node
5. 一些基本Node程序的样例
6. 让我们先关注JavaScript...

#### **1.1 基于 JavaScript 构建**

无论好坏，JavaScript 都是世界上最流行的编程语言。如果你做过任何网络编程，这是不可避免的。 JavaScript，因为网络的纯粹覆盖范围，实现了 Java 的“一次编写，随处运行”的梦想
早在20世纪90年代。
2005 年 Ajax 革命前后，JavaScript 从一个“玩具”语言变成了一个人们用语言来编写真实且重要的程序。 一些值得注意的第一个是 Google 地图和 Gmail，并且今天有许多网络应用程序从 Twitter 到 Facebook 再到 GitHub。
自 2008 年底发布 Google Chrome 以来，由于浏览器供应商（Mozilla、Microsoft、Apple、Opera 和 Google）之间的激烈竞争，改进速度令人难以置信，JavaScript 性能已大幅提升。 这些现代的表现JavaScript 虚拟机正在真正改变您可以构建的应用程序类型。一个引人注目且坦率地说令人兴奋的例子是 jslinux，一台 PC在 JavaScript 中运行的模拟器，您可以在其中加载 Linux 内核，与终端会话，并编译 C 程序，所有这些都在您的浏览器中进行。
Node 使用 V8，即为 Google Chrome 提供支持的虚拟机，用于服务器端编程。 V8 为 Node 带来了巨大的性能提升，因为它省去了中间人，比起执行，更喜欢直接编译为本机机器代码字节码或使用解释器。 由于 Node 在服务器上使用 JavaScript，因此还有其他好处：

1. 开发人员可以用一种语言编写 Web 应用程序，这有助于减少在客户端和服务器开发之间进行上下文切换，并允许客户端和服务器之间的代码共享，例如表单重复使用相同的代码验证或游戏逻辑
2. JSON 是当今非常流行的数据交换格式，并且是 JavaScript 原生的
3. JavaScript 是各种 NoSQL 数据库（例如 CouchDB和 MongoDB），因此与它们交互是很自然的事情（例如，MongoDB 的 shell 和查询语言是 JavaScript； CouchDB的map/reduce是JavaScript）。
4. JavaScript 是一个编译目标，并且已经有许多语言可以编译它。
5. Node 使用一台符合 ECMAScript 标准的虚拟机 (V8)。5 换句话说，您不必等待所有浏览器都赶上来在 Node 中使用新的 JavaScript 语言功能

谁知道 JavaScript 最终会成为一种引人注目的用于编写服务器端应用程序的语言？ 然而，由于其广泛的覆盖范围、性能和其他特征前面提到，Node 已经获得了很大的关注。 JavaScript 只是这个难题的一小部分； Node 使用 JavaScript 的方式更加引人注目。 为了了解 Node 环境，让我们深入了解您最熟悉的 JavaScript 环境：浏览器。

#### **1.2 异步和事件驱动：浏览器**

  node提供一个事件驱动和异步平台给服务端js。它将js引入服务器的方式与浏览器引入客户端的方式非常相似。为了理解 Node 的工作原理，了解浏览器的工作原理非常重要。 两者都是事件驱动的（它们使用事件循环）并且在处理 I/O 时是非阻塞的（它们使用异步 I/O）。 让我们看一个例子来解释这意味着什么
事件循环和异步I/O 想了解更多关于事件循环和异步I/O，可以看Wikipedia相关的文章： http://en.wikipedia.org/wiki/Event_loop 、 http://en.wikipedia.org/wiki/Asynchronous_I/O
采用 jQuery 使用 XMLHttp-Request (XHR) 执行 Ajax 请求的常见片段

```js
$.post('/resource.json',function(data){
    console.log(data);
    });//script execution continues  
I/O 不会阻塞执行
```

该程序对resource.json执行HTTP请求。 当响应返回时，将调用包含参数数据的匿名函数（在本上下文中称为“回调”），该参数数据是从该请求接收到的数据
注意代码不是这样写的：

```js
vardata=$.post('/resource.json'); // I/O阻塞直到执行结束
console.log(data); 

```

浏览器中非阻塞 I/O 的示例
![1](./img/浏览器中的非阻塞I:O.png)
在此示例中，假设 resources.json 的响应在准备就绪时将存储在 data 变量中，并且在此之前 console.log 函数不会执行。I/O 操作（Ajax 请求）将“阻止”脚本继续执行，直到准备就绪。由于浏览器是单线程的，如果此请求需要 400 毫秒返回，则该页面上发生的任何其他事件都会等到那时才执行。您可以想象如果动画暂停或用户尝试以某种方式与页面交互，用户体验会很差。
  值得庆幸的是，事实并非如此。 当 I/O 发生在浏览器中时，它发生在事件循环之外（主脚本执行之外），然后当 I/O 完成时发出一个“事件”，由函数处理通常称为“回调”）如图1.1所示
  I/O 异步发生，不会“阻止”脚本执行，从而允许事件循环响应页面上正在执行的任何其他交互或请求。 这使得浏览器能够响应客户端并处理页面上的大量交互。
  记下这一点，然后切换到服务器

#### **1.3 异步和事件驱动：服务器**

  在大多数情况下，您可能熟悉服务器端编程的传统 I/O 模型，例如 1.2 节中的“阻塞”jQuery 示例。 这是 PHP 中的示例

```js
$result=mysql_query('SELECT*FROMmyTable'); // 执行会暂停直到DB请求完成
print_r($result);

```

  此代码执行一些 I/O，并且该进程将被阻止继续执行，直到所有数据返回。对于许多应用来说，这个模型很好并且很容易遵循。可能不明显的是，该进程具有状态或内存，并且在 I/O 完成之前基本上不执行任何操作。这可能需要 10 毫秒到几分钟不等，具体取决于 I/O 操作的延迟。意外原因也可能导致延迟：

1. 磁盘正在执行维护操作，暂停读/写
2. 由于负载增加，数据库查询速度变慢
3. 由于某种原因，今天从 sitexyz.com 提取资源的速度很慢。
   如果程序在 I/O 上阻塞，当有更多请求需要处理时服务器会做什么？通常，您会在这种情况下使用多线程方法。一种常见的实现是每个连接使用一个线程，并为这些连接设置一个线程池。您可以将线程视为计算工作空间，处理器在其中处理一项任务。在许多情况下，线程包含在进程内并维护自己的工作内存。每个线程处理一个或多个服务器连接。虽然这听起来像是一种委派服务器工作的自然方式（至少对于长期这样做的开发人员来说是这样），但管理应用程序中的线程可能很复杂。此外，当需要大量线程来处理许多并发服务器连接时，线程会消耗操作系统资源。线程需要 CPU 来执行上下文切换，以及额外的RAM.
   为了说明这一点，让我们看一个比较 NGINX 和 Apache 的基准测试（如图 1.2 所示，来自 http://mng.bz/eaZT）。 NGINX (http://nginx.com/)，如果你不熟悉的话，它是一个像 Apache 一样的 HTTP 服务器，但它没有使用阻塞 I/O 的多线程方法，而是使用具有异步 I/O 的事件循环。 （如浏览器和 Node）。 由于这些设计选择，NGINX 通常能够处理更多请求和连接的客户端，使其成为响应速度更快的解决方案。
   在 Node 中，I/O 几乎总是在主事件循环之外执行，从而使服务器保持高效和响应能力，就像 NGINX 一样。 这使得进程更难成为 I/O 限制，因为 I/O 延迟不会使您的服务器崩溃，也不会像阻塞一样使用资源。 它允许服务器轻量级执行服务器通常执行的最慢的操作。
   事件驱动和异步模型以及广泛使用的 JavaScript 语言的这种组合有助于打开数据密集型实时应用程序的令人兴奋的世界。
   ![2](./img/aoache:nginx%20benchmark.png)

#### **1.4 脏应用程序**

实际上，Node 设计的应用程序类型有一个缩写词：DIRT。它代表数据密集型实时应用程序。由于 Node 本身在 I/O 方面非常轻量级，因此它擅长将数据从一个管道转移或代理到另一个管道。它允许服务器保持多个连接打开，同时处理许多请求并保持较小的内存占用。它被设计为响应式的，就像浏览器一样。实时应用程序是网络的一个新用例。 现在，许多网络应用程序几乎可以即时提供信息，实现在线白板协作、实时定位接近的公共交通巴士以及多人游戏等功能。无论是通过实时组件增强现有应用程序还是全新类型的应用程序，网络都正在朝着更具响应性和协作性的环境发展。然而，这些新型 Web 应用程序需要一个能够几乎立即响应大量并发用户的平台。Node 擅长于此，不仅适用于 Web 应用程序，还适用于其他 I/O 密集型应用程序
  使用 Node 编写的 DIRTy 应用程序的一个很好的例子是 Browserling（brow-serling.com，如图 1.3 所示）。该网站允许在浏览器内使用其他浏览器。这对于前端开发者来说非常有用，因为它使他们不必安装大量浏览器和操作系统来测试。Browserling 利用名为 StackVM 的node驱动项目，该项目管理使用 QEMU（快速仿真器）模拟器创建的虚拟机 (VM)。QEMU 模拟运行浏览器所需的 CPU 和外设。
   ![3](./img/1.3.png)
  Browserling浏览器让虚拟机运行测试浏览器，然后将键盘和鼠标输入数据从用户浏览器中传递到模拟浏览器，而模拟浏览器又流式传输模拟浏览器的重绘区域，并将它们重新绘制在用户浏览器的canvas上。如图1.4说明的。Browserling 还提供了一个使用 Node 的补充项目，称为 Testling(testling.com)，它允许您从命令行针对多个浏览器并行运行测试套件
  ![4](./img/1.4.png)
  （1） 在浏览器中，用户的鼠标和键盘事件通过 WebSocket 实时传递到 Node.js，Node.js 又将它们传递到模拟器
  （2）受用户交互影响的模拟浏览器的重绘区域通过 Node 和 WebSocket 流回，并绘制在浏览器的画布上。
  Browserling和Testling是 DIRTy 应用程序的好例子，当您坐下来编写第一个 Node 应用程序时，用于构建此类可扩展网络应用程序的基础设施正在发挥作用。 让我们看看 Node 的 API 如何提供这个开箱即用的工具。

#### 1.5 DIRTy by default

   Node 是从头开始构建的，具有事件驱动和异步模型。JavaScript 从来没有标准 I/O 库，而这对于服务器端语言来说是常见的。JavaScript 的“宿主”环境始终决定这一点。JavaScript 最常见的宿主环境（大多数开发人员都习惯使用的环境）是浏览器，它是事件驱动和异步的。
   Node 尝试通过重新实现通用主机对象来保持浏览器和服务器之间的一致性，例如这些：
   Timer API (for example, setTimeout)
   Console API (for example, console.log)
  Node 还包括一组用于多种类型网络和文件 I/O 的核心模块。其中包括 HTTP、TLS、HTTPS、文件系统 (POSIX)、数据报 (UDP) 和 NET (TCP) 模块。 核心有意设计得小、低级且不复杂，仅包括基于 I/O 的应用程序的构建块。 第三方模块构建在这些块的基础上，为常见问题提供更好的抽象

 ** Platform vs. framework **
 Node 是 JavaScript 应用程序的平台，不要与框架混淆。 将 Node 视为 Rails 或 JavaScript 的 Django 是一种常见的误解，但实际上它的级别要低得多。 但是，如果您对 Web 应用程序的框架感兴趣，我们将在本书后面讨论一个流行的 Node 框架，称为 Express

 经过所有这些讨论，您可能想知道 Node 代码是什么样的。 让我们看几个简单的例子：

** 1.5.1 简单的异步示例 **

 在 1.2 节中，我们查看了使用 jQuery 的 Ajax 示例

```js
$.post('/resource.json',function(data){
    console.log(data);
    });//script execution continues  
```

让我们在 Node 中做类似的事情，但我们将使用文件系统 (fs) 模块从磁盘加载 resource.json。 请注意该程序与前面的 jQuery 示例有多么相似：

```js
varfs=require('fs');fs.readFile('./resource.json',function(er,data){console.log(data);}) 
```

在此程序中，我们从磁盘读取resource.json 文件。 当读取所有数据时，将调用匿名函数（也称为“回调”），其中包含参数 er、如果发生任何错误以及 data（文件数据）
该进程在幕后循环，能够处理可能发生的任何其他操作，直到数据准备好为止。 我们之前讨论过的所有事件和异步优势都会自动发挥作用。 这里的区别在于，我们不是使用 jQuery 从浏览器发出 Ajax 请求，而是访问 Node 中的文件系统以获取grabresource.json。 后一个动作如图 1.5 所示
![5](./img/1.5.png)

　**1.5.2 Hello World HTTP server**

  Node 的一个非常常见的用例是构建服务器。 Node 使得创建不同类型的服务器变得非常简单。 如果您习惯于让服务器托管您的应用程序（例如托管在 Apache HTTP 服务器上的 PHP 应用程序），这可能会感觉很奇怪。在 Node 中，服务器和应用程序是相同的。

```js
varhttp=require('http');http.createServer(function(req,res){res.writeHead(200,{'Content-Type':'text/plain'});res.end('HelloWorld\n');}).listen(3000);console.log('Serverrunningathttp://localhost:3000/');

varhttp=require('http');varserver=http.createServer();
server.on('request',function(req,res){res.writeHead(200,{'Content-Type':'text/plain'});res.end('HelloWorld\n');})
server.listen(3000);
console.log('Serverrunningathttp://localhost:3000/');
```

每当请求发生时，就会触发 回调函数function(req,res) ，并写出“HelloWorld”作为响应。 此事件模型类似于侦听浏览器中的 onclick 事件。 单击可能在任何时候发生，因此您设置一个函数来执行一些逻辑来处理该问题。 在这里，Node 提供了一个函数，每当请求发生时都会做出响应。

　** 1.5.3 Streaming data **

Node在streams和streaming方面也很强大。 您可以将streams视为类似于数组，但数据不是分布在空间上，而是可以将streams视为分布在时间上的数据。 通过逐块引入数据，开发人员能够在数据进来时对其进行处理，而不是等待所有数据到达后再采取行动。 这是流式传输 resources.json 的方式

```js

varstream=fs.createReadStream('./resource.json')
stream.on('data',function(chunk){console.log(chunk)})
stream.on('end',function(){console.log('finished')})
```

每当新的数据块准备就绪时，就会触发数据事件，而当所有数据块都已加载时，就会触发结束事件。块的大小可能会有所不同，具体取决于数据类型。 这种对读取流的低级访问使您可以在读取数据时有效地处理数据，而不是等待所有数据都缓冲在内存中。
可读和可写的流可以连接起来形成管道，就像使用 | （pipe）一样。 shell 脚本中的（管道）运算符。 这提供了一种在数据准备好后立即写出数据的有效方法，而无需等待完整的资源被读取然后写出
让我们使用之前的 HTTP 服务器来说明将图像流式传输到客户端

```js
varhttp=require('http');
varfs=require('fs');
http.createServer(function(req,res){res.writeHead(200,{'Content-Type':'image/png'});fs.createReadStream('./image.png').pipe(res);}).listen(3000);
// Piping from a readable stream to a writable stream
console.log('Serverrunningathttp://localhost:3000/');
```

在此单行代码中，数据从文件 (fs.createReadStream) 中读入，并在传入时将其发送 (.pipe) 到客户端 (res)。事件循环能够在数据被读取时处理其他事件。
Node 跨多个平台（包括各种 UNIX 和 Windows）提供了这种默认的 DIRTy 方法。 底层异步 I/O 库 (libuv) 专为提供统一体验而构建，无论父操作系统如何，这使得程序可以更轻松地跨设备移植，并在需要时在多个设备上运行

#### 1.6 总结

与任何技术一样，Node 并不是灵丹妙药。 它只是帮助您解决某些问题并开启新的可能性。 Node 的有趣之处之一是它将系统各个方面的人们聚集在一起。 许多人作为 JavaScript 客户端程序员来到 Node； 其他人是服务器端程序员； 其他人是系统级程序员。 无论您适合哪里，我们希望您了解 Node 适合您的堆栈中的位置

回顾：
基于 JavaScript 构建
异步和事件驱动
设计给数据密集型实时应用

在第 2 章中，我们将构建一个简单的 DIRTy Web 应用程序，以便您可以了解Node用程序的工作原理

# 第二章 Building a multiroom chat application

  在第 1 章中，您了解了使用 Node 的异步开发与传统的同步开发有何不同。 在本章中，我们将通过创建一个小型事件驱动的聊天应用程序来实际了解 Node。 如果本章中的细节让您难以理解，请不要担心； 我们的目的是揭开 Node 开发的神秘面纱，并让您预览完成本书后您将能够做什么。

  本章假设您有 Web 应用程序开发经验，对 HTTP 有基本了解，并且熟悉 jQuery。 当您阅读本章时，您将本章假设您有 Web 应用程序开发：

（1）浏览该应用程序以了解其工作原理

（2）查看技术要求并执行初始应用程序设置

（3）为应用程序的 HTML、CSS 和客户端 JavaScript 提供服务

（4）使用 Socket.IO 处理与聊天相关的消息传递

（5）将客户端 JavaScript 用于应用程序的 UI

### 应用概述

  您将在本章中构建的应用程序允许用户通过在简单的表单中输入消息来相互在线聊天，如图 2.1 所示。输入消息后，该消息将发送给同一聊天室中的所有其他用户。

   启动应用程序时，系统会自动为用户分配一个访客名称，但他们可以通过输入命令来更改它，如图 2.2 所示。 聊天命令以斜线 (/) 开头。

  类似地，用户可以输入命令来创建一个新的聊天室（如果已经存在则加入它），如图2.3所示。 加入或创建房间时，新的房间名称将显示在聊天应用程序顶部的水平栏中。 该房间还将包含在聊天消息区域右侧的可用房间列表中。

  用户变更到新房间后，系统会确认变更，如图2.4所示。

  虽然该应用程序的功能故意是简单的，但它展示了创建实时 Web 应用程序所需的重要组件和基本技术。 该应用程序展示了 Node 如何同时提供传统 HTTP 数据（如静态文件）和实时数据（聊天消息）。 它还展示了 Node 应用程序是如何组织的以及如何管理依赖关系。

  现在让我们看看实现该应用程序所需的技术。

#### 应用要求和初始设置

您将创建的聊天应用程序需要执行以下操作：

（1）提供静态文件（例如 HTML、CSS 和客户端 JavaScript）

（2）在服务器上处理与聊天相关的消息传递

（3）在用户的网络浏览器中处理与聊天相关的消息传递

  对于服务端静态文件，您要使用 Node 的内置 http 模块。 但是，当通过 HTTP 提供文件服务时，仅发送文件内容通常是不够的； 您还应该包括正在发送的文件类型。 这是通过使用文件的正确 MIME 类型设置 Content-Type HTTP header 来完成的。 要查找这些 MIME 类型，您将使用名为 mime 的第三方模块。

  要处理与聊天相关的消息传递，您可以使用 Ajax 轮询服务器。 但为了使该应用程序尽可能响应，您将避免使用传统的 Ajax 作为发送消息的方式。 Ajax 使用 HTTP 作为传输机制，而 HTTP 并不是为实时通信而设计的。 当使用 HTTP 发送消息时，必须使用新的 TCP/IP 连接。 打开和关闭连接需要时间，并且数据传输的大小较大，因为每个请求都会发送 HTTP 头。此应用程序不会采用依赖于 HTTP 的解决方案，而是更喜欢 Web-Socket (http://en.wikipedia. org/wiki/WebSocket），被设计为双向轻量级通信协议，支持实时通信。

  由于大多数情况下只有兼容 HTML5 的浏览器才支持 WebSocket，因此应用程序将利用流行的 Socket.IO 库 (http://socket.io/)，该库提供了许多后备方案，包括使用 Flash，因此应使用 WebSocket 不可能。 Socket.IO 透明地处理回退功能，不需要额外的代码或配置。 第 13 章更深入地介绍了 Socket.IO。

  在我们开始实际进行设置应用程序的文件结构和依赖项的初步工作之前，让我们更多地讨论一下 Node 如何让您同时处理 HTTP 和 WebSocket——这也是它成为实时应用程序的最佳选择的原因之一

#### Serving HTTP and WebSocket

  尽管此应用程序将避免使用 Ajax 来发送和接收聊天消息，但它仍将使用 HTTP 来传递在用户浏览器中进行设置所需的 HTML、CSS 和客户端 JavaScript。

![1692593790633](image/nodeInAction/1692593790633.png)

  Node 可以使用单个 TCP/IP 端口轻松处理同时服务的 HTTP 和 WebSocket，如图 2.5 所示。 Node 附带一个提供 HTTP 服务功能的模块。 有许多第三方 Node 模块，例如 Express，它们构建在 Node 的内置功能之上，使 Web 服务变得更加容易。我们将在第 8 章中深入讨论如何使用 Express 构建 Web 应用程序。 然而，在本章的应用中，我们将坚持基础知识。

  现在您已经大致了解了应用程序将使用的核心技术，让我们开始充实它



#### Creating the application file structure

  要开始构建教程应用程序，请为其创建一个项目目录。 主应用程序文件将直接位于该目录中。 您需要添加一个 lib 子目录，其中将放置一些服务器端逻辑。 您需要创建一个公共子目录来放置客户端文件。 在 public 子目录中，创建一个 javascripts 子目录和一个 stylesheets 目录。

  您的目录结构现在应如图 2.6 所示。 请注意，虽然我们在本章中选择以这种特定方式组织文件，但 Node 并不要求您维护任何特定的目录结构；应用程序文件可以以对您有意义的任何方式组织。

  现在您已经建立了目录结构，您将需要指定应用程序的依赖项。

  在这种情况下，应用程序依赖项是需要安装以提供应用程序所需功能的模块。 举例来说，您正在创建一个应用程序，需要访问使用 MySQL 数据库存储的数据。Node 没有提供允许访问 MySQL 的内置模块，因此您必须安装第三方 模块，这将被视为依赖项。

#### Specifying dependencies

  尽管您可以在不正式指定依赖项的情况下创建 Node 应用程序，但花时间指定它们是一个好习惯。 这样，如果您希望其他人使用您的应用程序，或者您计划在多个地方运行它，那么设置就变得更加简单。

  应用程序依赖项是使用 package.json 文件指定的。 该文件始终放置在应用程序的根目录中。 package.json 文件由遵循 CommonJS 包描述符标准 (http://wiki.commonjs.org/wiki/Packages/1.0) 的 JSON 表达式组成，并描述您的应用程序。 在 package.json 文件中，您可以指定很多内容，但最重要的是应用程序的名称、版本、应用程序功能的描述以及应用程序的依赖项。

  清单 2.1 显示了一个包描述符文件，它描述了教程应用程序的功能和依赖项。 将此文件保存为教程应用程序的根目录中的 package.json。

  如果这个文件的内容看起来有点令人困惑，别担心...您将在下一章中了解有关 package.json 文件的更多信息，并在第 14 章中深入了解。
