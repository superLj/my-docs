> Vue watcher

```
let uid = 0
class Watcher {
  constructor(expOrFn, cb, options) {
    if (options) {
      const { deep, computed, sync } = options
      this.deep = deep
      this.computed = computed
      this.sync = sync
    }
    this.id = uid++
    this.cb = cb

    if (typeof expOrFn === 'function') {
      this.getter = expOrFn
    }

    if (this.computed) {
      this.value = undefined
      this.dep = new Dep()
    } else {
      this.value = this.get()
    }
  }

  get() {
    let value = this.getter()
    return value
  }

  // 这时候的 Dep.target 是渲染 watcher，所以 this.dep.depend() 相当于渲染 watcher 订阅了这个 computed watcher 的变化
  depend() {
    if (this.dep && Dep.target) {
      this.dep.depend()
    }
  }

  addDep(dep) {
    const id = dep.id
    dep.addSub(this)
  }

  update() {
    if (this.computed) {
      this.getAndInvoke(() => {
        this.dep.notify()
      })
    }  else if(this.sync) {
      this.run()
    } else {
      queueWatcher(this)
    }
  }

  run() {
    this.getAndInvoke(this.cb)
  }

  getAndInvoke(cb) {
    let value = thsi.get()
    if (value !== this.value) {
      let oldValue = this.value
      this.value = value

      cb.call(null, value, oldValue)
    }
  }

}

let queue = []
let has = {}
let waiting = false

function queueWatcher(watcher) {
  let id = watcher.id
  if(has[id]) return
  if (!has[id]) has[id] = true

  queue.push(watcher)

  if (!waiting) {
    waiting = true
    nextTick(flushSchedulerQueue)
  }
}

function flushSchedulerQueue() {
  let watcher, id
  /**
    * 组件的更新由父到子；因为父组件的创建过程是先于子的，所以 watcher 的创建也是先父后子，执行顺序也应该保持先父后子
    * 用户的自定义 watcher 要优先于渲染 watcher 执行；因为用户自定义 watcher 是在渲染 watcher 之前创建的
  */
  queue.sort((a, b) => a.id - b.id)
  for (var i=0;i<queue.length;i++) {
    watcher = queue[index]
    id = watcher.id

    watcher.run()
  }
}

 // demo 渲染watcher
 updateComponent = () => {
  vm._update(vm._render(), hydrating)
}
new Watcher(vm, updateComponent, noop, {
  before() {
      if (vm._isMounted) {
          callHook(vm, 'beforeUpdate')
      }
  }
}, true /* isRenderWatcher */ )
```
