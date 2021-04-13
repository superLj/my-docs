> 策略模式

```
const strategies = {
  isNonEmpty: function(value, errorMsg) {
      if (value === '') {
          return errorMsg
      }
  },
  minLength: function(value,length, errorMsg) {
      if (value.length<length) {
          return errorMsg
      }
  },
  isMobile: function(value, errorMsg) {
      if (!/^1[3|5|8][0-9]$/.test(value)) {
          return errorMsg
      }
  }
}

class Validator {
  constructor() {
    this.cache = []
  }

  add(value, rule, errorMsg) {
    this.cache.push(() => {
      const strategie = strategies[rule]
      let res = strategie(value, errorMsg)
      return res
    })
  }

  run() {}
}

const validataFunc = () => {
  let validator = new Validator()
  validator.add(username,'isNoEmpty', '用户名不能为空')

  let errorMsg = validator.run()
  return errorMsg
}

```
