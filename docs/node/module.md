# 模块

```
function require(...) {
  var module = { exports: {} };
  ((module, exports) => {
    // Your module code here. In this example, define a function.
    function some_func() {};
    exports = some_func;
    // At this point, exports is no longer a shortcut to module.exports, and
    // this module will still export an empty default object.
    module.exports = some_func;
    // At this point, the module will now export some_func, instead of the
    // default object.
  })(module, module.exports);
  return module.exports;
}
```

> a.js 和 b.js 两个文件互相 require 是否会死循环?

启动 a.js 的时候，会加载 b.js，那么在 b.js 中又加载了 a.js，但是此时 a.js 模块还没有执行完，返回的是一个 a.js 模块的 exports 对象 未完成的副本 给到 b.js 模块（因此是不会陷入死循环的）。然后 b.js 完成加载之后将 exports 对象提供给了 a.js 模块

> const exports = modules.exports

> 上下文

vm.runInThisContext: 指定的 global 对象的上下文中执行 vm.Script 对象里被编译的代码并返回其结果。被执行的代码虽然无法获取本地作用域，但是能获取 global 对象

> 包管理
