
# Web安全第一章第一节 Web介绍

> 日期：2018年4月16日  杭州

## 什么是Web
&nbsp;&nbsp;Web（World Wide Web）即全球广域网，也称为万维网，它是一种基于超文本和HTTP的、全球性的、动态交互的、跨平台的分布式图形信息系统。是建立在Internet上的一种网络服务，为浏览者在Internet上查找和浏览信息提供了图形化的、易于访问的直观界面，其中的文档及超级链接将Internet上的信息节点组织成一个互为关联的网状结构。

## 背景
&nbsp;&nbsp;1989年CERN（欧洲粒子物理研究所）中由Tim Berners-Lee领导的小组提交了一个针对Internet的新协议和一个使用该协议的文档系统，该小组将这个新系统命名为Word Wide Web，它的目的在于使全球的科学家能够利用Internet交流自己的工作文档。
&nbsp;&nbsp;这个新系统被设计为允许Internet上任意一个用户都可以从许多文档服务计算机的数据库中搜索和获取文档。1990年末，这个新系统的基本框架已经在CERN中的一台计算机中开发出来并实现了,1991年该系统移植到了其他计算机平台，并正式发布。

## 表现形式
##### 一、超文本（Hyper text）
&nbsp;&nbsp;超文本是一种用户接口方式，用以显示文本及与文本相关的内容。现时超文本普遍以电子文档的方式存在，其中的文字包含有可以链接到其他字段或者文档的超文本链接，允许从当前阅读位置直接切换到超文本链接所指向的文字。
&nbsp;&nbsp;超文本的格式有很多，目前最常使用的是超文本标记语言(Hyper Text Markup Language，HTML)及富文本格式 (Rich Text Format，RTF)。我们日常浏览的网页上的链结都属于超文本。
&nbsp;&nbsp;超文本链接一种全局性的信息结构，它将文档中的不同部分通过关键字建立链接，使信息得以用交互方式搜索。
##### 二、超媒体（hypermedia）
&nbsp;&nbsp;超媒体是超级媒体的简称。是超文本（hypertext）和多媒体在信息浏览环境下的结合。用户不仅能从一个文本跳到另一个文本，而且可以激活一段声音，显示一个图形，甚至可以播放一段动画。
&nbsp;&nbsp;Internet采用超文本和超媒体的信息组织方式，将信息的链接扩展到整个Internet上。Web就是一种超文本信息系统，Web的一个主要的概念就是超文本链接。它使得文本不再像一本书一样是固定的线性的，而是可以从一个位置跳到另外的位置并从中获取更多的信息，还可以转到别的主题上。想要了解某一个主题的内容只要在这个主题上点一下，就可以跳转到包含这一主题的文档上。正是这种多连接性把它称为Web。
###### 三、超文本传输协议（HTTP，HyperText Transfer Protocol)是互联网上应用最为广泛的一种网络协议。


## 体系结构

##### 公共网关接口CGI
&nbsp;&nbsp;不知道是什么东西，这里先用php实现一下，看看是个什么东东，跟现在的Web网站有什么区别。


##### 扩展接口
&nbsp;&nbsp;为了克服CGI的局限性，出现的另一种中间件解决方案是基于服务器扩展API的结构。与CGI相比，API应用程序与Web服务器结合得更加紧密，占用的系统资源也少得多，而运行效率却大大提高，同时还提供更好的保护和安全性。

##### JDBC
&nbsp;&nbsp;Java的推出，使WWW页面有了活力和动感。Internet用户可以从WWW服务器上下载Java小程序到本地浏览器运行。这些下载的小程序就像本地程序一样，可独立地访问本地和其他服务器资源。而最初的Java语言并没有数据库访问的功能，随着应用的深入，要求Java提供数据库访问功能的呼声越来越高。为了防止出现对Java在数据库访问方面各不相同的扩展，JavaSoft公司指定了JDBC，作为Java语言的数据库访问API。



## CGI是一个协议
&nbsp;&nbsp;作为一个服务器，基本要求是能受理请求，提取信息并将消息分发给 CGI 解释器，再将解释器响应的消息包装后返回客户端。在这个过程中，除了和客户端 socket 之间的交互，还要牵扯到第三个实体 - 请求解释器。![示例图](https://images2015.cnblogs.com/blog/819496/201706/819496-20170608093512231-108012397.png)
如上图所示，客户端负责封装请求和解析响应，服务器的主要职责是管理连接、数据转换、传输和分发客户端请求，而真正进行数据文档处理与数据库操作的就是请求解释器，这个解释器，在 PHP 中一般是 PHP-FPM，JAVA 中是 Servlet。

我们之前进行的处理多在客户端和服务器之间的通信，以及服务器的内部调整，这次更新的内容主要是后面两个实体之间的进程间通信。

进程间通信牵涉到三个方面，即方式和形式和内容。

方式指的是进程间通信的传输媒介，如 Nginx 中实现的 TCP 方式和 Unix Domain Socket，它们分别有跨机器和高效率的优点，还有我实现的服务器用了很 low 的popen方式。

而形式就是数据格式了，我认为它并无定式，只要服务器容易组织数据，解释器能方便地接收并解析，最好也能节约传输资源，提高传输效率。目前的解决方案有经典的 xml，轻巧易理解的 json 和谷歌高效率的 protobuf。它们各有优点，我选择了 json，主要是因为有CJson库的存在，数据在 C 中方便组织，而在PHP中，一个json_decode()方法就完成了数据解析。

至于应该传输哪些内容呢？CGI 描述了一套协议：

CGI
> 通用网关接口（Common Gateway Interface/CGI）是一种重要的互联网技术，可以让一个客户端，从网页浏览器向执行在网络服务器上的程序请求数据。CGI描述了服务器和请求处理程序之间传输数据的一种标准。

CGI 是服务器与解释器交互的接口，服务器负责受理请求，并将请求信息解释为一条条基本的请求信息（在文档中被称为“元数据”），传送给解释器来解释执行，而解释器响应文档和数据库操作信息。

之前看了一下 CGI 的 RFC 文档，总结了几个重要点，有兴趣的可以看下底部参考文献。常见规范（信息太多，只考虑 MUST 的情况）如下：

###### CGI请求
* 服务器根据 以 / 分隔的路径选择解释器；
* 如果有 AUTH 字段，需要先执行 AUTH，再执行解释器; 
* 服务器确认 CONTENT-LENGTH 表示的是数据解析出来的长度，如果附带信息体，则必须将长度字段传送到解释器；
* 如果有 CONTENT-TYPE 字段，服务器必须将其传给解释器；若无此字段，但有信息体，则服务器判断此类型或抛弃信息体；
* 服务器必须设置 QUERY_STRING 字段，如果客户端没有设置，服务端要传一个空字符串“”
* 服务器必须设置 REMOTE_ADDR，即客户端请求IP；
* REQUEST_METHOD 字段必须设置， GET POST 等，大小写敏感；
* SCRIPT_NAME 表示执行的解释器脚本名，必须设置；
* SERVER_NAME 和 SERVER_PORT 代表着大小写敏感的服务器名和服务器受理时的TCP/IP端口；
* SERVER_PROTOCOL 字段指示着服务器与解释器协商的协议类型，不一定与客户端请求的SCHEMA 相同，如'https://'可能为HTTP；
* 在 CONTENT-LENGTH 不为 NULL 时，服务器要提供信息体，此信息体要严格与长度相符，即使有更多的可读信息也不能多传；
* 服务器必须将数据压缩等编码解析出来；
###### CGI响应
* CGI解释器必须响应 至少一行头 + 换行 + 响应内容；
* 解释器在响应文档时，必须要有 CONTENT-TYPE 头；
* 在客户端重定向时，解释器除了 client-redir-response=绝对url地址，不能再有其他返回，然后服务器返回一个 302 状态码；
* 解释器响应 三位数字状态码，具体配置可自行搜索；
* 服务器必须将所有解释器返回的数据响应给客户端，除非需要压缩等编码，服务器不能修改响应数据；


## 总结
&nbsp;&nbsp;可以看出最早基于HTTP协议，使浏览器和Web服务器可以通信，这时Web服务器只能满足对静态资源的访问。出于动态需要访问数据库，需要其他动态语言支持，就出现了CGI公共网关接口，CGI是一个协议，Web服务器可以通过进程间通信或者TCP套接字来和实现CGI的程序进行通信。


参考：[用C写一个web服务器（四） CGI协议](https://www.cnblogs.com/zhenbianshu/p/6958794.html)




