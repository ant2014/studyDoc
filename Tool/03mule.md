1、什么是ESB
enterprise Service Bus 企业服务总线
![](a)

以前A服务访问B，可能A直接去调用B的接口或者直接取数据库，应用多了后，调用很复杂，难以管理。所以，提出了服务总线的概念，我们把很多服务放到了ESB上来，其他应用要调用的时候，不用关心具体的应用，只要去找服务总线即可，由服务总线负责调具体的应用。因此，企业可以对内对外，发布统一的，标准的接口。实现各个应用系统间信息的共享。

企业总线要具有，路由、消息的转换，等基本的功能。

随着互联网的发展，很多第三方的服务通过开放平台来调用内部的服务，开发平台就是由ESB衍生出来的。

简单的来说，ESB上跑的就是很多服务，以及各个服务之间消息的转换。

所有的ESB都具备的功能

* 服务接入，不管是webService还是FTP服务都可以很容易的接入到ESB，也可以将数据库接入到ESB上，对外发布成一个另外一种统一的服务
* 消息转化，RESTFul接口到WebService接口，中间的消息是需要转换的
* 消息路由
* 消息过滤

那些地方会用到ESB
银行（用的最早和最多）、通信、支付宝、开放平台、系统集成、保险、政府、医院等

例如支付宝，当我们在付款的时候，假如要支付100元，可能80块钱是从余额支付的，10块钱使用代金券支付，还有10块钱是银行卡支付，这一个支付流程其实是分为三个子流程来执行的，我们可以在ESB平台上配置这三个服务，将其配置到一个请求下面，变成一个流程。当我们去调用时，会自动的走这三个流程。当业务逻辑变化时，只需要变化这个流程即可。比如再增加一种支付方式，只要简单的再配置一下即可，所以在不需要更改代码逻辑的时候，就可以满足业务变化的需求。并且各个节点，还可以加入事务的控制。

最多的应用场景在银行和金融，开放平台、系统集成是最适合ESB的场景。

开发IDE
AnypointStudio，旧版本叫MuleStudio

设计和发布接口的时候，有个接口统一设计的方式，推荐用RESTFul的方式，设计RESTFul接口有个规范，叫RAML（RESTFul API Modeling Language）。根据这个规范来设计RESTFul接口，设计完成后，可以通过API Designer生成文档，



需要下载：
1、runtime
2、AnypointStudio 开发IDE

runtime解压出来的目录

1. apps，部署应用的目录
2. bin，启动停止等命令脚本
3. conf，核心配置放在这
4. lib，Mule依赖的包



MEL和Message结构

MuleESB开发需要掌握的核心知识点

1. 熟悉MEL（Mule Expression Language）
2. 了解MuleMessage的结构和Payload对象（报文体）（重要）
3. 对常用的connector、scoper、component、transformer、filter、flowControl、errorHanding等控件要熟悉
4. 了解APIKit Router和APIKit console，熟悉RAML




快速入门

1. 创建helloWorld工程
2. 拖动一个Flow到Message Flow工作区中
3. 因为要对外提供一个web请求，所以我们再拖一个连接器，叫HTTP，到source区域中，如果控件上有红色标记，说明它的属性没有配置正确，我们给HTTP控件配置connector，采用默认的8081端口，Path设置为 /hello，则访问如下地址即可访问到http://localhost:8081/hello
4. 拖动一个Set Payload到Process区域中，设置其属性Value为"Hello,ESB"
5. 项目名右键，run Mule Application
6. 请求地址http://localhost:8081/hello，则页面输出“Hello,ESB”

两种方式部署应用

1. 手工上传目录部署，项目导出Mule Deployable Archive（zip格式），然后上传到mule/apps目录下，Mule会自动解压部署，热部署
2. 通过mmc控制台上传再部署，实际部署的地点也是在apps目录下
2. 通过Studio工具一键部署，项目名右键Anypoint Platform > Deploy to Mule Management Console 


##MEL语法
\#[表达式]

例如：

1. \#[server.dateTime]	//系统函数 
2. \#[2+2 == 4]  // 计算和逻辑
3. \#[message.inboundProperties['http.query.params']['uId']]  //获取uId参数
4. \#[message.inboundProperties.city]  //从properties键值对中取出city 
5. \#[message.inboundProperties['city']]  //同上
6. \#[xpath('/user/username').text ]	//查找xml文档内容

支持运算符(+-*/)、比较运算符(>、<、=)、逻辑运算符(与或)、三元运算符等
表达式使用分号（;）隔开，可以在[]中书写多条表达式
支持Xpath操作XML文件，支持使用正则表达式(Regex)

##MuleMessage的结构
目标是解决异构系统间的信息转换。MuleMessage Object这个对象封装了所有异构系统之间要传递的数据，包括你是什么协议，什么通讯协议的类型。可能会面对各种各样的网络通信协议，

##使用MEL的好处

1. MEL的语法很丰富和灵活
2. 可以非常方便的读写操作Message结构中的所有对象和属性，如payload、properties、variables等
3. 可以在Flow中的所有配置项使用MEL
4. MEL还可以用在Router和Filter中
5. 不会编程语言（Java、Groovy）的人也可以做ESB的开发


##MuleESB中的四大对象

1. server对象 #[server.dateTime]
2. mule对象 #[mule.version]
3. app对象	#[app.name]
4. message对象 #[message.payload]

这些对象中的属性，大多数都是只读的，只有message.replyTo、message.payload、message.outboundProperities、message.outboundAttachments和app.registry是可读可写的

##Message中的Properties
分为Inbound properties和Outbound properties，分别代表输入和输出，输出要设置

##Message中的Variables
message中可以通过Variables设置要传递的变量

Variables分为3种

1. flowVars，只在同一个Flow中使用
2. sessionVars，在同一个Application下的所有Flow中使用
3. 此外，Record variables只能在batch（批处理）中使用

演示Properties的操作
1、拖一个Property放在Set Plyload之前，设置Operation为set Property,name = city, value = beijing
2、在Set Payload 中设置value为#[message.outboundProperties.city]，注意response中输出的内容就是Payload种的内容，所以Set Payload会被输出到response

设置variable的方式和properties一样，但是取值时要注意，如果你是flow级别的，使用#[flowVars.city]取出来



