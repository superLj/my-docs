> Vue dep 1

```
let uid = 0
class Dep {
  constructor() {
    this.is = uid++
    this.subs = []
  }

  addSub(sub) {
    this.subs.push(sub)
  }

  depend() {
    if (Dep.target) {
      Dep.target.addDep(this)
    }
  }

  notify() {
    const subs = this.subs.slice()
    subs.forEach(sub => sub.update())
  }
}
```
