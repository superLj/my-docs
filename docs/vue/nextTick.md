> vue nextTick

```
const callbacks = []
let pending = false

function flushCallbacks() {
  pending = false

  const copies = callbacks.slice()
  for (var i = 0;i<copies.length;i++) {
    callbacks[i]()
  }
}

let microTimerFunc, macroTimerFunc, useMacroTask = false

macroTimerFunc = () => {
  setTimeout(flushCallbacks, 0)
}

// 能力检测
if (typeof Promise !== undefined) {
  microTimerFunc = () => {
    Promise.resolve().then(flushCallbacks)
  }
}

function nextTick(cb, ctx) {
  let _resolve
  callbacks.push(() => {
    if (cb) {
      cb.call(ctx)
    } else if(_resolve) {
      _resolve(ctx)
    }
  })

  if (!pending) {
    pending = true
    if (useMacroTask) {
        macroTimerFunc()
    } else {
        microTimerFunc()
    }
  }
}
```
