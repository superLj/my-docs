# Vue vs React

> 数据绑定

- Vue 采用双向绑定策略, 双向绑定和单向数据流并没有直接关联，双向绑定是指数据和视图之间的绑定关系，而单向数据流是指组件之间数据的传递
- React 当数据发生变化时，直接重新渲染组件，以得到最新的视图

> 组件化和数据流

- Vue 组件本质是一个 Vue 实例。每个 Vue 实例在创建时都需要经过：设置数据监听、编译模版、应用模版到 DOM，在更新时根据数据变化更新 DOM 的过程
- React 组件, 是完全以组件功能和 UI 为维度划分的

- Vue props 实现父组件向下传递数据，events 实现子组件向上发送消息给父组件
- React 中是基于 props 的回调实现子组件向父组件传递数据

- 分别通过 context 和 provide/inject 实现了跨层级数据通信

> 数据状态管理

> 渲染更新

- React 所有组件的渲染都依靠灵活而强大的 JSX
- Vue templates 是典型的模版

- 在 React 应用中，当某个组件的状态发生变化时，它会以该组件为根，重新渲染整个组件子树。当然我们可以使用 PureComponent，或是手动实现 shouldComponentUpdate 方法，来规避不必要的渲染
- 在 Vue 应用中，组件的依赖是在渲染过程中自动追踪的，因此系统能精确知晓哪个组件需要被重渲染

# Vue2.0 &vs Vue3.0

> 源码优化

- 更好的代码管理方式：monorepo, 根据功能将不同的模块拆分到 packages 目录下面不同的子目录
- TypeScript

> 性能优化

- 源码体积优化
- 数据劫持优化
  - object.defineProperty & Proxy
  - Proxy API 并不能监听到内部深层次的对象变化，因此 Vue.js 3.0 的处理方式是在 getter 中去递归响应式，这样的好处是真正访问到的内部对象才会变成响应式，而不是无脑递归
  - 编译优化

> 语法 API 优化: composition Api

- 优化逻辑组织
- 优化逻辑复用
