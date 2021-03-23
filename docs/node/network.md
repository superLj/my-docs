# Network

> TCP 可靠传输

| 表头                     |                                                      简述                                                      |
| ------------------------ | :------------------------------------------------------------------------------------------------------------: |
| CLOSED                   |                                          连接关闭, 所有连接的初始状态                                          |
| LISTEN                   |                                          监听状态, 等待客户端发送 SYN                                          |
| SYN-SENT                 |                                        客户端发送了 SYN, 等待服务端回复                                        |
| SYN-RECEIVED             |                                           双方都收到了 SYN, 等待 ACK                                           |
| ESTABLISHED SYN-RECEIVED |                                      收到 ACK 之后, 状态切换为连接已建立.                                      |
| CLOSE-WAIT               | 被动方收到了关闭请求(FIN)后, 发送 ACK, 如果有数据要发送, 则发送数据, 无数据发送则回复 FIN. 状态切换到 LAST-ACK |
| LAST-ACK                 |                       等待对方 ACK 当前设备的 CLOSE-WAIT 时发送的 FIN, 等到则切换 CLOSED                       |
| FIN-WAIT-1               |                                            主动方发送 FIN, 等待 ACK                                            |
| FIN-WAIT-2               |                                        主动方收到被动方的 ACK, 等待 FIN                                        |
| CLOSING                  |                       主动方收到了 FIN, 却没收到 FIN-WAIT-1 时发的 ACK, 此时等待那个 ACK                       |
| TIME-WAIT                |                                                                                                                |

> UDP

| 协议 |  连接性  |      双工性 | 可靠性         |       有序性        |         有界性 | 拥塞控制 | 传输速度 | 量级 |   头部大小 |
| ---- | :------: | ----------: | -------------- | :-----------------: | -------------: | -------- | :------: | ---: | ---------: |
| TCP  | 面向连接 | 全双工(1:1) | 可靠(重传机制) | 有序(通过 SYN 排序) | 无(有粘包情况) | 有       |    慢    |   低 | 20~60 字节 |
| UDP  |  无连接  |         n:m | 不可靠         |        无序         |     有消息边界 | 无       |    快    |   高 |     8 字节 |

> 常用的应用场景

- TCP

  - 电子邮件 SMTP
  - 终端连接 TELNET
  - 终端连接 SSH
  - 万维网 HTTP
  - 文件传输 FTP
  - UDP 域名解析 DNS
  - 简单文件传输 TFTP
  - 网络时间校对 NTP
  - 网络文件系统 NFS
  - 路由选择 RIP
  - IP 电话 -
  - 流式多媒体通信 -

- TCP
  - 电子邮件 SMTP
  - 终端连接 TELNET
  - 终端连接 SSH
  - 万维网 HTTP
  - 文件传输 FTP
  - UDP 域名解析 DNS
  - 简单文件传输 TFTP
  - 网络时间校对 NTP
  - 网络文件系统 NFS
  - 路由选择 RIP
  - IP 电话 -
  - 流式多媒体通信 -

### HTTP

[图解 HTTP]()
[HTTP 协议](http://www.ruanyifeng.com/blog/2016/08/http.html)
[理解 RESTFUL 架构](http://www.ruanyifeng.com/blog/2011/09/restful.html)

> POST 和 PUT 有什么区别?

POST 是新建 (create) 资源, 非幂等, 同一个请求如果重复 POST 会新建多个资源. PUT 是 Update/Replace, 幂等, 同一个 PUT 请求重复操作会得到同样的结果.

> headers

> DNS
