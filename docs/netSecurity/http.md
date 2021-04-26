> http

- 组成

  - 头部
    - 请求行／请求头
    - 状态行/响应头
  - 空行
  - 实体
    - 请求体/响应体

- 请求方法

  - GET: 通常用来获取资源
  - HEAD: 获取资源的元信息
  - POST: 提交数据，即上传数据
  - PUT: 修改数据
  - DELETE: 删除资源(几乎用不到)
  - CONNECT: 建立连接隧道，用于代理服务器
  - OPTIONS: 列出可对资源实行的请求方法，用来跨域请求
  - TRACE: 追踪请求-响应的传输路径

> get&post 的区别

- 参数, GET 一般放在 URL 中, 因此不安全, POST 放在请求体中, 更适合传输敏感信息
- TCP, GET 请求会把请求报文一次性发出去, 而 POST 会分为两个 TCP 数据包, 首先发 header 部分, 如果服务器响应 100(continue), 然后发 body 部分
- 缓存

> 缺点

- 无状态
- 明文传输
- 队头阻塞

> accept 系列字段

- 数据格式
  - text： text/html, text/plain, text/css
  - image: image/gif, image/jpeg, image/png
  - audio/video: audio/mpeg, video/mp4
  - application: application/json
- 压缩方式
  - Accept-Encoding: gzip
- 支持语言
  - Accept-Language: zh-CN, zh, en
- 字符集
  - Accept-Charset: charset=utf-8

> HTTP 处理大文件传输

采取了范围请求的解决方案, 允许客户端仅仅请求一个资源的一部分.

- Range 字段拆解

```
// 单段数据
Range: bytes=0-9
// 多段数据
Range: bytes=0-9, 30-39
```

> http 处理表单数据的提交

- application/x-www-form-urlencoded
  - 其中的数据会被编码成以&分隔的键值对
  - 字符以 URL 编码方式编码

```
// 转换过程: {a: 1, b: 2} -> a=1&b=2 -> 如下(最终形式)
"a%3D1%26b%3D2"
```

- multipart/form-data

- 请求头中的 Content-Type 字段会包含 boundary，且 boundary 的值有浏览器默认指定。例: Content-Type: multipart/form-data;boundary=----WebkitFormBoundaryRRJKeWfHPGrS4LKe。
- 数据会分为多个部分，每两个部分之间通过分隔符来分隔，每部分表述均有 HTTP 头部描述子包体，如 Content-Type，在最后的分隔符会加上--表示结束

```
Content-Disposition: form-data;name="data1";
Content-Type: text/plain
data1
----WebkitFormBoundaryRRJKeWfHPGrS4LKe
Content-Disposition: form-data;name="data2";
Content-Type: text/plain
data2
----WebkitFormBoundaryRRJKeWfHPGrS4LKe--
```

> 服务器代理

- 负载均衡
- 保障安全
- 缓存代理

> 跨域

同源政策: scheme(协议), host(主机), port(端口)

> CORS 跨域资源共享

- 简单请求

  - 请求方法为 GET、POST 或者 HEAD
  - 请求头的取值范围: Accept、Accept-Language、Content-Language、Content-Type(只限于三个值 application/x-www-form-urlencoded、multipart/form-data、text/plain)

  在请求头当中, 添加一个 Origin 字段, 用来说明请求来自哪个源. 服务器拿到请求之后, 在回应时对应地添加 Access-Control-Allow-Origin 字段, 如果 Origin 不在这个字段的范围中, 那么浏览器就会将响应拦截.

- 非简单请求

  预检请求和响应字段

> Nginx 处理跨域问题

```
server {
  listen  80;
  server_name  client.com;
  location /api {
    proxy_pass server.com;
  }
}
```

Nginx 相当于起了一个跳板机，这个跳板机的域名也是 client.com，让客户端首先访问 client.com/api，这当然没有跨域，然后 Nginx 服务器作为反向代理，将请求转发给 server.com，当响应返回时又将响应给到客户端，这就完成整个跨域请求的过程

> http2 的改进

- 头部压缩
- 多路复用
  - 二进制分帧
- 设置请求优先级
- 服务器推送
