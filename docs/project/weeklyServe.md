# 小报系统

[小报系统](https://juejin.cn/post/6882295776454639624)

```
|- controllers
          |- auth.ts
                 |- validate
                 |- login
                 |- register
          |- user.ts
                |- listUsers
                |- showUserDetail
                |- updateUser
                |- deleteUser
          |- article.ts
                   |- listArticles
                   |- addArticle
          |-
|
|- entity
      |- user: id, name, password, email
      |- article: id, title, desc, link, tag, user, weekId
      |- week: id, title, articleIds
|
|- units
|
|- types
|
|- index.ts
        |- 使用的中间件: typeorm, koa-jwt, koa-body
|
|- router.ts
         |- unprotectedRouter.post('/auth/validate', AuthController.validate)
```
