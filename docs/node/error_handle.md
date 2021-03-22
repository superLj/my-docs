# 错误处理/调试

> Error 类型

| 错误                       |         名称         |               触发 |
| -------------------------- | :------------------: | -----------------: |
| Standard JavaScript errors | 标准 JavaScript 错误 |     由错误代码触发 |
| System errors              |       系统错误       |     由操作系统触发 |
| User-specified errors      |    用户自定义错误    |  通过 throw 抛出容 |
| Assertion errors           |       断言错误       | 由 assert 模块触发 |

> 常见的错误

- SyntaxError
- ReferenceError
- TypeError

> 处理错误的方法

- callback(err, data)
- throw / try / catch
- EventEmir error 事件

> 错误栈丢失

[错误处理的最佳实践](https://www.joyent.com/node-js/production/design/errors)
[Koa Error Handle](https://github.com/koajs/koa/wiki/Error-Handling)

> unhandledRejection

- Promise 被 reject 且没有绑定监听处理时, 就会触发该事件. 该事件对排查和追踪没有处理 reject 行为的 Promise 很有用

```
process.on('unhandledRejection', (reason, p) => {
  console.log('Unhandled Rejection at: Promise', p, 'reason:', reason);
  // application specific logging, throwing an error, or other logic here
});

somePromise.then((res) => {
  return reportToUser(JSON.pasre(res)); // note the typo (`pasre`)
}); // no `.catch` or `.then`
```

> 内存快照

- [如何分析 Node.js 中的内存泄漏](https://zhuanlan.zhihu.com/p/25736931?group_id=825001468703674368)

> CPU profiling

```
node --prof app.js
node --prof-process isolate-0xnnnnnnnnnnnn-v8.log
```

[v8 profile](https://v8.dev/docs/profile)
