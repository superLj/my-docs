# 从输入 url 到页面展示

[从输入 URL 到页面加载的过程](https://zhuanlan.zhihu.com/p/34453198)

[js 运行机制](https://segmentfault.com/a/1190000012925872)

[普通图层和复合图层](https://link.zhihu.com/?target=https%3A//segmentfault.com/a/1190000012925872%23articleHeader16)

[V8 内存浅析](https://zhuanlan.zhihu.com/p/33816534)

- 从浏览器接收 url 到开启网络请求线程

  - 多进程的浏览器
  - 多线程的浏览器内核
  - 解析 URL
  - 网络请求都是单独的线程

- 开启网络线程到发出一个完整的 http 请求

  - DNS 查询得到 IP
  - tcp/ip 请求
  - 五层因特网协议栈

- 从服务器接收到请求到对应后台接收到请求

  - 负载均衡
  - 后台的处理

- 后台和前台的 http 交互

  - http 报文结构
  - cookie 以及优化
  - gzip 压缩
  - 长连接与短连接
  - http 2.0
  - https

- 单独拎出来的缓存问题，http 的缓存

  - 强缓存与弱缓存
  - 缓存头部简述
  - 头部的区别

- 解析页面流程

  - 流程简述
  - HTML 解析，构建 DOM
  - 生成 CSS 规则
  - 构建渲染树
  - 渲染
  - 简单层与复合层
  - Chrome 中的调试
  - 资源外链的下载[async & deffer]
  - loaded 和 DomContentloaded

- CSS 的可视化格式模型

  - 包含块（Containing Block）
  - 控制框（Controlling Box）
  - BFC（Block Formatting Context）
  - IFC（Inline Formatting Context）

- JS 引擎解析过程
  - JS 的解释阶段
  - S 的预处理阶段
  - JS 的执行阶段
  - 回收机制

### 主干流程

```
1. 从浏览器接收url到开启网络请求线程（这一部分可以展开浏览器的机制以及进程与线程之间的关系）

2. 开启网络线程到发出一个完整的http请求（这一部分涉及到dns查询，tcp/ip请求，五层因特网协议栈等知识）

3. 从服务器接收到请求到对应后台接收到请求（这一部分可能涉及到负载均衡，安全拦截以及后台内部的处理等等）

4. 后台和前台的http交互（这一部分包括http头部、响应码、报文结构、cookie等知识，可以提下静态资源的cookie优化，以及编码解码，如gzip压缩等）

5. 单独拎出来的缓存问题，http的缓存（这部分包括http缓存头部，etag，catch-control等）

6. 浏览器接收到http数据包后的解析流程（解析html-词法分析然后解析成dom树、解析css生成css规则树、合并成render树，然后layout、painting渲染、复合图层的合成、GPU绘制、外链资源的处理、loaded和domcontentloaded等）

7. CSS的可视化格式模型（元素的渲染规则，如包含块，控制框，BFC，IFC等概念）

8. JS引擎解析过程（JS的解释阶段，预处理阶段，执行阶段生成执行上下文，VO，作用域链、回收机制等等）

9. 其它（可以拓展不同的知识模块，如跨域，web安全，hybrid模式等等内容）
```

> 多线程的浏览器内核

- GUI 线程
- JS 引擎线程
- 事件触发线程
- 定时器线程
- 网络请求线程

> 五层因特网协议栈

- 应用层(dns,http) DNS 解析成 IP 并发送 http 请求
- 传输层(tcp,udp) 建立 tcp 连接（三次握手）
- 网络层(IP,ARP) IP 寻址
- 数据链路层(PPP) 封装成帧
- 物理层(利用物理介质传输比特流) 物理传输（然后传输的时候通过双绞线，电磁波等各种介质）

> http 报文结构

- 通用头部
  ```
  Request Url: 请求的web服务器地址
  Request Method: 请求方式Get、POST、OPTIONS、PUT、HEAD、DELETE、CONNECT、TRACE）
  Status Code: 请求的返回状态码，如200代表成功
  Remote Address: 请求的远程服务器地址（会转为IP）
  ```
- 请求/响应头部
  ```
  #请求头部
  Accept: 接收类型，表示浏览器支持的MIME类型对标服务端返回的Content-Type）
  Accept-Encoding：浏览器支持的压缩类型,如gzip等,超出类型不能接收
  Content-Type：客户端发送出去实体内容的类型
  Cache-Control: 指定请求和响应遵循的缓存机制，如no-cache
  If-Modified-Since：对应服务端的Last-Modified，用来匹配看文件是否变动，只能精确到1s之内，http1.0中
  Expires：缓存控制，在这个时间内不会请求，直接使用缓存，http1.0，而且是服务端时间
  Max-age：代表资源在本地缓存多少秒，有效时间内不会请求，而是使用缓存，http1.1中
  If-None-Match：对应服务端的ETag，用来匹配文件内容是否改变（非常精确），http1.1中
  Cookie: 有cookie并且同域访问时会自动带上
  Connection: 当浏览器与服务器通信时对于长连接如何进行处理,如keep-alive
  Host：请求的服务器URL
  Origin：最初的请求是从哪里发起的（只会精确到端口）,Origin比Referer更尊重隐私
  Referer：该页面的来源URL(适用于所有类型的请求，会精确到详细页面地址，csrf拦截常用到这个字段)
  User-Agent：用户客户端的一些必要信息，如UA头部等
  ```
  ```
  #响应头部
  Access-Control-Allow-Headers: 服务器端允许的请求Headers
  Access-Control-Allow-Methods: 服务器端允许的请求方法
  Access-Control-Allow-Origin: 服务器端允许的请求Origin头部（譬如为*）
  Content-Type：服务端返回的实体内容的类型
  Date：数据从服务器发送的时间
  Cache-Control：告诉浏览器或其他客户，什么环境可以安全的缓存文档
  Last-Modified：请求资源的最后修改时间
  Expires：应该在什么时候认为文档已经过期,从而不再缓存它
  Max-age：客户端的本地资源应该缓存多少秒，开启了Cache-Control后有效
  ETag：请求变量的实体标签的当前值
  Set-Cookie：设置和页面关联的cookie，服务器通过这个头部把cookie传给客户端
  Keep-Alive：如果客户端有keep-alive，服务端也会有响应（如timeout=38）
  Server：服务器的一些相关信息
  ```
- 请求/响应体
  - 请求实体中会将一些需要的参数都放入进入（用于 post 请求）, 序列化参数 || 表单对象 Form Data

### 解析页面流程

```
1. 解析HTML，构建DOM树

2. 解析CSS，生成CSS规则树

3. 合并DOM树和CSS规则，生成render树

4. 布局render树（Layout/reflow），负责各元素尺寸、位置的计算

5. 绘制render树（paint），绘制页面像素信息

6. 浏览器会将各层的信息发送给GPU，GPU会将各层合成（composite），显示在屏幕上
```

> HTML 解析, 构建 DOM

```
Bytes → characters → tokens → nodes → DOM
```

> 生成 CSS 规则

```
Bytes → characters → tokens → nodes → CSSOM
```

> 构建渲染树 -> 渲染

```
1. 计算CSS样式
2. 构建渲染树
3. 布局，主要定位坐标和大小，是否换行，各种position overflow z-index属性
4. 绘制，将图像绘制出来
```

> 回流

```
# 引起回流
1.页面渲染初始化
2.DOM结构改变，比如删除了某个节点
3.render树变化，比如减少了padding
4.窗口resize
5.最复杂的一种：获取某些属性，引发回流，
很多浏览器会对回流做优化，会等到数量足够时做一次批处理回流，
但是除了render树的直接变化，当获取一些属性时，浏览器为了获得正确的值也会触发回流，这样使得浏览器优化无效，包括
    （1）offset(Top/Left/Width/Height)
     (2) scroll(Top/Left/Width/Height)
     (3) cilent(Top/Left/Width/Height)
     (4) width,height
     (5) 调用了getComputedStyle()或者IE的currentStyle
```

```
# 优化方案

减少逐项更改样式，最好一次性更改style，或者将样式定义为class并一次性更新
避免循环操作dom，创建一个documentFragment或div，在它上面应用所有DOM操作，最后再把它添加到window.document
避免多次读取offset等属性。无法避免则将它们缓存到变量
将复杂的元素绝对定位或固定定位，使得它脱离文档流，否则回流代价会很高
```

> 简单层与复合层

- 可以认为默认只有一个复合图层，所有的 DOM 节点都是在这个复合图层下的
- 如果开启了硬件加速功能，可以将某个节点变成复合图层
- 复合图层之间的绘制互不干扰，由 GPU 直接控制
- 而简单图层中，就算是 absolute 等布局，变化时不影响整体的回流，但是由于在同一个图层中，仍然是会影响绘制的，因此做动画时性能仍然很低。而复合层是独立的，所以一般做动画推荐使用硬件加速

### CSS 的可视化格式模型

- 包含块（Containing Block）
- 控制框（Controlling Box）
- BFC（Block Formatting Context）
  ```
    # 触发条件
    根元素
    float属性不为none
    position为absolute或fixed
    display为inline-block, flex, inline-flex，table，table-cell，table-caption
    overflow不为visible
  ```

### JS 解析 & 预处理 & 执行

> 执行

- 执行上下文，执行堆栈概念
- VO（变量对象）和 AO（活动对象）
- 作用域链
- this

> 回收机制

- GC 时，停止响应其他操作
- 垃圾回收规则
  - 标记清除
    - 遍历所有可访问的对象
    - 回收已不可访问的对象
  - 引用计数
- GC 优化策略
  - 新生代
    - scavenge 算法 [对象区域 || 空闲区域]
  - 旧生代
    - mark-sweep, 对象占用空间大, 对象存活事件长
