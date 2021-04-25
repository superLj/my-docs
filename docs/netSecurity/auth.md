# 单页应用鉴权实战

> Json Web Token

- header
- payload[消息体, 储存用户 id, 用户角色]
- signature

涉及客户端如何存储和维护 JWT 问题

> 采用 Authentication cookie 实现鉴权

Cookie 关键配置

- HttpOnly cookie, 浏览器端 JavaScript 没有读 cookie 权限
- Secure cookie,
- SameSite cooki, 在跨域情况下, 相关 cookie 无法被请求携带, 这里主要是为了防止 CSRF 攻击

# Web 攻击

> XSS

- 脚本注入攻击, 将前端输出数据都进行转义
- 保护好 cookie

> CSRF[cross-site request forgery]

- 条件

  - 用户已经登录了站点 A, 并在本地记录了 cookie
  - 在用户没有登出站点 A 的情况下(也就是 cookie 生效的情况下), 访问了恶意攻击者提供的引诱危险站点 B (B 站点要求访问站点 A)
  - 站点 A 没有做任何 CSRF 防御

- 预防
  - post 请求输入验证码
  - 提交表单时, 携带 csrfToken, 在后端做 csrfToken 验证
