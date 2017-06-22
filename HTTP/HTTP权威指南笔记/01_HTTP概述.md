# HTTP -- 因特网的多媒体信使

HTTP使用的是可靠的数据传输协议，因此即使数据来自地球的另一端，它也能够确保数据在传输的过程中不会被损坏或产生混乱。这样，用户在访问信息时就不用担心其完整性了，就无需担心HTTP通信会在传输过程中被破坏、复制或产生畸变了。

# Web客户端和服务器

Web内容都是存储在Web服务器上的。Web服务器所使用的是HTTP协议，因此经常会被称为HTTP服务器。这些HTTP服务器存储了因特网中的数据，如果HTTP客户端发出请求的话，它们会提供数据。客户端向服务器发送HTTP请求，服务器会在HTTP响应中回送所请求的数据，如图1-1所示。HTTP客户端和HTTP服务器共同构成了万维网的基本组件。

![https://raw.githubusercontent.com/JerryIreya/Image/master/HTTP_Chapter1_1-1.png](https://raw.githubusercontent.com/JerryIreya/Image/master/HTTP_Chapter1_1-1.png)

浏览一个页面时(比如http://www.oreilly.com/index.html )，浏览器会向服务器www.oreilly.com发送一条HTTP请求(参见上图)。服务器会去寻找所期望的对象(在这个例子中就是/index.html)，如果成功，就将对象、对象类型、对象长度以及其他一些信息放在HTTP响应中发送给客户端。


# 资源

Web服务器是Web资源(Web resource)的宿主.Web资源是Web内容的源头。最简单的Web资源就是Web服务器文件系统中的静态文件。这些文件可以包含任意内容：文本文件、HTML文件、Word文件、JPEG图片文件、AVI电影文件，或所有其他你能想到的格式。

但资源不一定是静态文件。资源还可以是根据需要生成内容的软件程序。这些动态内容资源可以根据你的身份、所请求的信息或每天的不同时段来产生内容。

![https://raw.githubusercontent.com/JerryIreya/Image/master/HTTP_Chapter1_1-2.png](https://raw.githubusercontent.com/JerryIreya/Image/master/HTTP_Chapter1_1-2.png)

## 媒体类型

因特网上有数千种不同类型的数据类型，HTTP仔细地给每种要通过Web传输的对象都打上了名为MIME类型(MIME type)的数据格式标签。

Web服务器会为所有HTTP对象数据附加一个MIME类型(参见图1-3)。当Web浏览器从服务器取回一个对象时，会去查看相关的MIME类型，看看它是否知道应该如何处理这个对象。大多数浏览器都可以处理数百种常见的数据类型：显示图片、解析并格式化HTML文件、通过计算机声卡播放音视频文件，或者运行外部插件软件来处理特殊格式的数据。

![https://raw.githubusercontent.com/JerryIreya/Image/master/HTTP_Chatper1_1-3.png](https://raw.githubusercontent.com/JerryIreya/Image/master/HTTP_Chatper1_1-3.png)

MIME类型是一种文本标记，表示一种 **主要的对象类型(image)** 和一个 **特定的子类型(jpeg)** ，中间由一条斜杠来分割。

* HTML格式的文本文档由text/html类型来标记
* 普通的ASCII文本文档由text/plain类型来标记
* JPEG版本的图片为image/jpeg类型
* GIF格式的图片为image/gif类型
* 微软的PowerPoint演示文件为application/vnd.ms-powerpoint类型

## URI

每个Web服务器资源都有一个名字，这样客户端就可以说明它们感兴趣的资源是什么了。服务器资源名被称为 **统一资源标识符(Uniform Resource Identifier,URI)** 。URI就像因特网上的邮政地址一样，在世界范围内唯一标识并定位信息资源。

这是Joe的五金店商店的Web服务器上的一个图片资源的URI：

http://www.joes-hardware.com/specials/saw-blade.gif

图1-4显示了URI是怎样指示HTTP协议去访问Joe商店服务器上的图片资源的。给定了URI，HTTP就可以解析出对象。URI有两种形式，分别称为URL和URN。

![https://raw.githubusercontent.com/JerryIreya/Image/master/HTTP_Chapter1_1-4.png](https://raw.githubusercontent.com/JerryIreya/Image/master/HTTP_Chapter1_1-4.png)

## URL

**统一资源定位符(URL)** 是资源标识符最常见的形式。URL描述了一台特定服务器上某资源的特定位置。它们可以明确说明如何从一个精确、固定的位置获取资源。图1-4显示了URL如何精确地说明某资源的位置以及如何去访问它。

大部分URL都遵循一种标准格式，这种格式包括三部分：

* URL的第一部分被成为scheme，说明了访问资源所使用的协议类型。这部分通常就是HTTP协议(http://)
* 第二部分给出了服务器的因特网地址(比如www.baidu.com)
* 其余部分指定了Web服务器上的某个资源(比如，/specials/saw-blade.gif)

现在，几乎所有的URI都是URL

# 事务

我们来更仔细地看看客户端是怎样通过HTTP与Web服务器机器资源进行事务处理的。一个HTTP事务由一条(从客户端发往服务器的)请求命令和一个(从服务器发回客户端的)响应结果组成。这种通信是通过名为 **HTTP报文(HTTP message)** 的格式化数据块进行的，如图1-5所示。

![https://raw.githubusercontent.com/JerryIreya/Image/master/HTTP_Chapter1_1-5.png](https://raw.githubusercontent.com/JerryIreya/Image/master/HTTP_Chapter1_1-5.png)

## 方法

HTTP支持几种不同的请求命令，这些命令被称为 **HTTP方法(HTTP method)** .每条HTTP请求报文都包含一个方法。这个方法会告诉服务器要执行什么动作(获取一个Web页面、运行一个网关程序、删除一个文件等)。下表列出了5种常见的HTTP方法。

| HTTP方法 | 描述 |
| :------------- | :------------- |
| GET | 从服务器向客户端发送命名资源 |  
|PUT|将来自客户端的数据存储到一个命名的服务器资源中去
|DELETE|从服务器中删除命名资源
|POST|将客户端数据发送到一个服务器网关应用程序
|HEAD|仅发送命名资源响应中的HTTP首部

## 状态码

每条HTTP响应报文返回时都会携带一个状态码。状态码是一个三位数字的代码，告知客户端请求是否成功，或者是否需要采取其他动作。下表显示了几种常见的状态码：

| HTTP状态码 | 描述 |
| :------------- | :------------- |
| 200 | OK。文档正确返回 |
|302|Redirect(重定向)。到其他地方去获取资源
|404|Not Found(没找到)。无法找到这个资源

伴随着每个数字状态码，HTTP还会发送一条解释性的“原因短语”文本(例如：OK)。包含文本短语主要是为了进行描述，所有的处理过程使用的都是数字码。

## Web页面中可以包含多个对象

应用程序完成一项任务时通常会发布多个HTTP事务。比如，Web浏览器会发布一系列HTTP事务来获取并显示一个包含了丰富图片的Web页面。浏览器会执行一个事务来获取描述页面布局的HTML“框架”，然后发布另外的HTTP事务来获取每个嵌入式的图片、图像面板、Java小程序等。这些嵌入式资源甚至可能位于不同的服务器上，如图1-6所示。因此，一个“Web页面”通常并不是单个资源，而是一组资源的集合。

![https://raw.githubusercontent.com/JerryIreya/Image/master/HTTP_Chapter1_1-6.png](https://raw.githubusercontent.com/JerryIreya/Image/master/HTTP_Chapter1_1-6.png)

# 报文

现在我们来快速浏览一下HTTP请求和响应报文的结构。

HTTP报文是由一行一行的简单字符串组成的。HTTP报文都是纯文本，不是二进制代码，所以人们可以很方便地对其进行读写。图1-7显示了一个简单事务所使用的HTTP报文。

![https://raw.githubusercontent.com/JerryIreya/Image/master/HTTP_Chapter1_1-7.png](https://raw.githubusercontent.com/JerryIreya/Image/master/HTTP_Chapter1_1-7.png)

从Web客户端发往Web服务器的HTTP报文称为 **请求报文(request message)** 。从服务器发往客户端的报文称为 **响应报文(response message)** ,此外没有其他类型的HTTP报文。HTTP请求和响应报文的格式很类似。

HTTP报文包括以下三个部分：

* 起始行  
报文的第一行就是起始行，在请求报文中用来说明要做些什么，在响应报文中说明出现了什么情况。

* 首部字段  
起始行后面有零个或多个首部字段。每个首部字段都包含一个名字和一个值，为了便于解析，两者之间用冒号(:)来分割。首部以一个空行结束。添加一个首部字段和添加新行一样简单。

* 主体  
空行之后就是可选的报文主体了，其中包含了所有类型的数据。请求主体中包括了要发送给Web服务器的数据；响应主体中装载了要返回给客户端的数据。起始行和首部都是文本形式且都是结构化的，而主体则不同，主体中可以包含任意的二进制数据(比如图片、视频、音轨、软件程序)。当然，主体中也可以包含文本。

## 简单的报文实例

图1-8显示了可能会作为某个简单事务的一部分发送的HTTP报文。浏览器请求资源 http://www.joes-hardware.com/tools.html。

在图1-8中，浏览器发送了一条HTTP请求报文。这条请求的起始行中有一个GET命令，且本地资源为/tools.html。这条请求说明它使用的是1.0版的HTTP协议。请求本文没有主体，以为从服务器上GET一个简单的文档不需要请求数据。

服务器会回送一条HTTP响应报文。这条响应中包含了HTTP的版本号(HTTP/1.0)、一个成功状态码(200)、一个描述性的原因短语(OK)，以及一块响应首部字段，在所有这些内容之后跟着包含了所请求文档的响应主体。Content-Length首部说明了主体的长度，Content-Type首部说明了文档的MIME类型。

![https://raw.githubusercontent.com/JerryIreya/Image/master/HTTP_Chapter1_1-8.png](https://raw.githubusercontent.com/JerryIreya/Image/master/HTTP_Chapter1_1-8.png)

# 连接

概要介绍了HTTP报文的构成之后，我们来讨论一下报文是如何通过传输控制协议(Transmission Control Protocol,TCP)连接从一个地方搬移到另一个地方去的。


## TCP/IP

HTTP是个应用层协议。HTTP无需操心网络通信的具体细节；它把联网的细节都交给了通用、可靠的因特网传输协议TCP/IP。

TCP提供了：

* 无差错的数据传输
* 按序传输(数据总是按照发送的顺序到达)
* 未分段的数据流(可以在任意时刻以任意尺寸将数据发送出去)

因特网自身就是基于TCP/IP的，TCP/IP是全世界的计算机和网络设备常用的层次化分组交换网络协议集。TCP/IP隐藏了各种网络和硬件的特点及弱点，使各种类型的计算机和网络都能够进行可靠地通信。

只要建立了TCP连接，客户端和服务器之间的报文交换就不会丢失、不会被破坏，也不会在接收时出现错序了。

用网络术语来说，HTTP协议位于TCP的上层。HTTP使用TCP来传输其报文数据。与之类似，TCP则位于IP的上层(参见图1-9)

![https://raw.githubusercontent.com/JerryIreya/Image/master/HTTP_Chapter1_1-9.png](https://raw.githubusercontent.com/JerryIreya/Image/master/HTTP_Chapter1_1-9.png)

## 连接、IP地址及端口号

在HTTP客户端向服务器发送报文之前，需要用网际协议(Internet Protocol,IP)地址和端口号在客户端和服务器之间建立一条TCP/IP连接。

建立一条TCP连接的过程与给公司办公室的某个人打电话的过程类似。首先，要拨打公司的电话号码，这样就能进入正确的机构了。其次，拨打要联系的那个人的分机号。

在TCP中，你需要知道服务器的IP地址，以及与服务器上运行的特定软件相关的TCP端口号。

这就行了，但最初怎么获得HTTP服务器的IP地址和端口号呢？当然是通过URL了！URL就是资源的地址，所以自然能够为我们提供存储资源的机器的IP地址。我们来看几个URL：

* http://207.200.23.25:80/index.html
* http://www.netscape.com:80/index.html
* http://www.netscape.com:index.html

第一个URL使用了IP地址，以及端口号  
第二个URL使用了文本形式的域名，或者称为主机名。主机名就是IP地址人性化的别称。可以通过一种称为 **域名服务(Domain Name Services,DNS)** 的机制方便地将主机名转换为IP地址。  
最后一个URL没有端口号，HTTP的默认端口号是80

有了IP地址和端口号，客户端就可以通过TCP/IP和服务器进行通信了。图1-10显示了浏览器是怎样通过HTTP显示位于服务器中的某个HTML资源的。

![https://github.com/JerryIreya/Image/blob/master/HTTP_Chapter1_1-10.png?raw=true](https://github.com/JerryIreya/Image/blob/master/HTTP_Chapter1_1-10.png?raw=true)

步骤如下：

1. 浏览器从URL中解析出服务器主机名
2. 浏览器将主机名转换成IP地址
3. 浏览器将端口号从URL中解析出来
4. 浏览器建立一条与服务器的TCP连接
5. 浏览器向服务器发送一条HTTP请求报文
6. 服务器向浏览器回送一条HTTP响应报文
7. 关闭连接，浏览器显示文档
