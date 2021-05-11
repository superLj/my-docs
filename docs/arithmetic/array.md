# array

> [两数之和]()

```
1. 暴力循环
2. hash, 空间换时间
3. 排序 + 双指针
```

> [和为 s 的连续正数序列](https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/)

```
# 双指针
var findContinuousSequence = function(target) {
    if (target < 3) return
    let left = 1, right = 2, middle = (target + 1)/2
    let sum = left + right, res = []
    while(left < middle) {
        if (sum === target) {
          res.push([left, right])
          sum -= left
          left ++
        }
        if (sum < target) {
            right ++
            sum += right
        } else if (sum > target) {
            sum -= left
            left ++
        }
    }

    return res.map(item => {
      return sequenceNums(item)
    })
}

function sequenceNums(s2e) {
  let start = s2e[0], end = s2e[1], res = []
  for(let i = start;i <= end;i++) {
    res.push(i)
  }
  return res
}
```
