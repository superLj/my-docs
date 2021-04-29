> 优化

- 使用 DllPlugin [动态链接库]

  - 包含大量复用模块的动态链接库只需要编译一次，在之后的构建过程中被动态链接库包含的模块将不会在重新编译，而是直接使用动态链接库中的代码。 由于动态链接库中大多数包含的是常用的第三方模块，例如 react、react-dom，只要不升级这些模块的版本，动态链接库就不用重新编译

- 使用 HappyPack

  - 把任务分解给多个子进程去并发的执行，子进程处理完后再把结果发送给主进程

- 使用 ParallelUglifyPlugin

  - 压缩代码

- 区分环境

  - process.env.NODE_ENV

- 使用 CDN

- 使用 Tree Shaking

- 提取公共代码

```
  const CommonsChunkPlugin = require('webpack/lib/optimize/CommonsChunkPlugin');

  new CommonsChunkPlugin({
  // 从哪些 Chunk 中提取
  chunks: ['a', 'b'],
  // 提取出的公共部分形成一个新的 Chunk，这个新 Chunk 的名称
  name: 'common'
  })
```

- 分割代码按需加载

- Scope Hoisting

  ```
    [
      (function (module, __webpack_exports__, __webpack_require__) {
        var __WEBPACK_IMPORTED_MODULE_0__util_js__ = __webpack_require__(1);
        console.log(__WEBPACK_IMPORTED_MODULE_0__util_js__["a"]);
      }),
      (function (module, __webpack_exports__, __webpack_require__) {
        __webpack_exports__["a"] = ('Hello,Webpack');
      })
    ]
  ```

  开启 scope hosting 后

  ```
  [
    (function (module, __webpack_exports__, __webpack_require__) {
      var util = ('Hello,Webpack');
      console.log(util);
    })
  ]
  ```

- 输出分析

  - webpack-bundle-analyzer

    - 打包出的文件中都包含了什么
    - 每个文件的尺寸在总体中的占比，一眼看出哪些文件尺寸大
    - 模块之间的包含关系
    - 每个文件的 Gzip 后的大小
