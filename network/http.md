### http 的请求准备  
用访问 www.google.com 来举例，http 的准备过程如下：
1. 浏览器会将域名发送给 DNS 服务器，让它解析为 IP 地址；
2. HTTP 是基于 TCP 协议的，先建立三次握手连接，
目前使用的 HTTP 协议大部分都是 1.1。在 1.1 的协议里面，默认是开启了 Keep-Alive 的，这样建立的 TCP 连接，就可以在多次请求中复用；
3. 建立了连接以后，浏览器就要发送 HTTP 的请求。

### http 请求的构建 
HTTP 的报文分为三部分如下所示：  
* 请求行，
* 请求的首部，
* 请求的正文实体。  

![image](https://github.com/islongfei/Blog/blob/master/images/http%E8%AF%B7%E6%B1%82%E6%8A%A5%E6%96%87.jpg)  

1. **请求行**  

* URL :就是 http://www.google.com ;
* 版本:比如为 HTTP 1.1;
* 方法：GET（去服务器获取一些资源）、 POST(主动告诉服务端一些信息，而非获取)、PUT(向指定资源位置上传最新内容)、 DELETE(删除资源)

2. **首部字段**  
例如，Accept-Charset，表示客户端可以接受的字符集。
Content-Type 是指正文的格式，比如 JSON。  
在 HTTP 头里面，Cache-control、If-Modified-Since  是用来控制缓存的。  

### http 请求的发送  
HTTP 协议是基于 TCP 协议的，所以它使用面向连接的方式发送请求，通过 stream 二进制流的方式传给对方。
到了 TCP 层，它会把二进制流变成一个的报文段发送给服务器。

### http 2.0 的改进
1. **索引表**  
HTTP 1.1 在应用层以纯文本的形式进行通信。每次通信都要带完整的 HTTP 的头，这样在实时性、并发性上都存在问题。  
为了解决这些问题，HTTP 2.0 会对 HTTP 的头进行一定的压缩，将原来每次都要携带的大量 key value 在两端建立一个索引表，对相同的头只发送索引表中的索引。  

2. **切分流**  
HTTP 2.0 协议将一个 TCP 的连接中，切分成多个流，每个流都有自己的 ID，而且流可以是客户端发往服务端，也可以是服务端发往客户端。
它其实只是一个虚拟的通道，流是有优先级的。  

3. **分割消息和帧**  
HTTP 2.0 还将所有的传输信息分割为更小的消息和帧，并对它们采用二进制格式编码。常见的帧有 Header 帧，用于传输 Header 内容，
并且会开启一个新的流。再就是 Data 帧，用来传输正文实体。多个 Data 帧属于同一个流。