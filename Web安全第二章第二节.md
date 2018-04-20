# Web安全第二章第二节 暴力破解
> 2018年4月20日 杭州

## 一、爆破原理
&nbsp;&nbsp;顾名思义，暴力破解的原理就是使用攻击者自己的用户名和密码字典，一个一个去枚举，尝试是否能够登录。因为理论上来说，只要字典足够庞大，枚举总是能够成功的！

&nbsp;&nbsp;但实际发送的数据并不像想象中的那样简单——“ 每次只向服务器发送用户名和密码字段即可！”，实际情况是每次发送的数据都必须要封装成完整的 HTTP 数据包才能被服务器接收。但是你不可能一个一个去手动构造数据包，所以在实施暴力破解之前，我们只需要先去获取构造HTTP包所需要的参数，然后扔给暴力破解软件构造工具数据包，然后实施攻击就可以了。
Web暴力破解通常用在，已知部分信息，尝试爆破网站后台，为下一步的渗透测试做准备。

&nbsp;&nbsp;Http 中的 response 和 request 是相对浏览器来说的。浏览器发送request，服务器返回response。

> Get和Post：get放在url中，而post放在http的body中。
> http_referer:是http中header的一部分，向浏览器发送请求时，一般会带上referer，告诉服务器我是从哪个页面链接而来，为服务器处理提供一些信息。

## 二、爆破实例
#### 2.1 必要的信息收集
这里我们使用dvwa渗透测试平台中的暴力破解模块来进行演示。

先使用任意账号密码尝试登录，并同时使用```firefox F12```进行抓包分析。
![暴力破解原理与过程详解](https://upload-images.jianshu.io/upload_images/6230889-e6a6c717931c95c1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![暴力破解原理与过程详解](https://upload-images.jianshu.io/upload_images/6230889-9d5bf6dbfa832406.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



这一步的作用是，收集构造HTTP数据包所需要的参数，比如cookie、get/post、referer、提交得字段名等。

> cookie：用在暴力破解过程中保持和服务器的连接
> referer：一些网站需要验证referer信息，告诉服务器，我是从哪个页面转过来的
> post/get： 决定数据包的提交方式
> 字段名：通常存放在cookie当中，知道正确的字段名，才能将数据正确的提交给服务器
可以看到cookie里面除了 username 和 password 字段之外还有一个 token，这个通常是用来防止CSRF攻击的。

收集到以上信息之后，我们就可以构造用于攻击的数据包。

#### 2.2 实施攻击
需要用到的参数收集完毕之后，接下来就需要使用到爆破软件，这里我们先讲一个专用与爆破的软件——Bruter，之后会再介绍一款综合的Web类安全软件 ：
如下图所示，这款软件支持包括FTP、SSH在内的十多种不同应用场景的暴力破解。我们这里是Web登录的爆破，所以选择Web Form：

![Paste_Image.png](https://upload-images.jianshu.io/upload_images/6230889-230281656465c003.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击协议右侧的选项，将我们之前获得的信息输入进去。
其实我们也可以直接在网址一栏中输入我们要攻击的URL，点击载入，它会自动将构造攻击数据包所需要的信息识别出来并填好，如果我们发现有问题或者有遗漏，也可以手动修改。

有些朋友可能要说，既然可以自动获取相关参数，那为什么我们还要花时间精力去手动收集信息呢？其实之前的手动收集主要是帮助我们理解暴力破解的原理，正所谓知其然不够，还要知其所以然。并且软件自动获取的参数也可能会出错，我们可以再验证一次。
![Paste_Image.png](https://upload-images.jianshu.io/upload_images/6230889-e6f5a979c36efef8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

接下来设置用户名和密码。用户名可以使用字典，如果你知道用户名是什么，你也可以直接输入字符串，比如：admin。
密码则有多种选择，如果选择字典选项，则需要加载我们自己事先准备好的字典（比如自己收集的弱口令字典），右侧还可以设置大小写、字符长度等：

![Paste_Image.png](https://upload-images.jianshu.io/upload_images/6230889-1f2f93aee76d70c8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

ps:注意bruter字典路径似乎不支持中文路径，笔者这里使用中文路径会报错。

如果选择暴力破解选项，就是软件使用自动生成字符串进行攻击，我们可以自定义使用到的字符种类、长度等：

![Paste_Image.png](https://upload-images.jianshu.io/upload_images/6230889-d27ecaef7965dc0a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

至于右侧的选项，大家可以根据自己的需要进行选择，设置完毕之后点击开始，就可以开始暴力破解：

[Uploading github.pages_3_brutard7_653421.png . . .]

![Paste_Image.png](https://upload-images.jianshu.io/upload_images/6230889-6f4508288f730459.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

接下来我们介绍另一款软件WebCruiser Web Vulnerability Scanner ，这是一款相对综合的软件，包括常见的Sql注入、XSS检测等功能，其中的暴力破解模块也非常强大！
这款软件自带web界面，我们可以直接在url一栏中输入攻击网址，并做一次任意用户名密码的登录提交，之手点击Resend按钮，可以看到已经自动对之前操作进行抓包：

![Paste_Image.png](https://upload-images.jianshu.io/upload_images/6230889-eef31801601950c4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![Paste_Image.png](https://upload-images.jianshu.io/upload_images/6230889-05ae09f8d3fbafb0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


之后在点击右侧的Bruter按钮，会直接跳转到Bruter界面，同样需要的参数都已经自动填好，设置好字典就可以开始破解了：
![Paste_Image.png](https://upload-images.jianshu.io/upload_images/6230889-00576134448146cf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 工具
[bruter](http://7xsf0k.com1.z0.glb.clouddn.com/Bruter_1.1.zip)
[WebCruiser Web Vulnerability Scanner](http://7xsf0k.com1.z0.glb.clouddn.com/WebCruiserWVS%28jb51.net%29.rar)


参考资料
[暴力破解原理与过程详解](https://www.cnblogs.com/Jewel591/p/7484516.html)























