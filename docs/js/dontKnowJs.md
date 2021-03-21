# 你不知道的 Javascript

### 作用域和闭包

> 作用域是什么

- 确定当前执行代码对这些标识的访问权限

> 词法作用域

- 定义在词法阶段的作用
- 欺骗词法
  - eval, 接受字符串为参数, 将其中内容视为书写时的代码
  - with, 重复引用同一个对象的多个属性快捷方式

> 函数作用域&块级作用域

> 提升

> 作用域闭包

- 函数在定义是的词法作用域以为的地方调用, 闭包使得函数可以继续访问定义时的词法作用域
- 自由变量 + 函数
- 模块

```
let myModules = (function () {
  let modules = {}

  function define(name, deps, impl) {
    for(let i = 0, i <deps.length;i++) {
      dep[i] = modules[deps[i]]
    }
    modules[name] = impl.apply(impl, deps)
  }

  function get(name) {
    return mosules[name]
  }
})()
```

### this 和对象原型

> this 解析

> 对象

> 混合对象"类"

> 原型

> 行为委托
