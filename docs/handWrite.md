### HandWrite Topic

![HandWrite](/imgs/handWrite.png)

> 浅拷贝

```
const shallClone = (target) => {
  if (typeof target === 'object' && target !== null) {
    const cloneTarget = Array.isArray(target)?[]: {}

    for(let prop in target) {
      if (target.hasOwnProperty(prop)) {
        cloneTarget[prop] = target[prop]
      }
    }
    return cloneTarget
  } else {
    return target
  }
}
```

> 深拷贝
