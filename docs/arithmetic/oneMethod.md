### 合并区间问题

> [合并区间](https://leetcode-cn.com/problems/merge-intervals/)

```
var merge = function(intervals) {
    if (intervals.length === 0) return []
    if (intervals.length === 1) return intervals

    intervals.sort((a, b) => a[0] - b[0])
    let res = [], n = intervals.length
    res.push(intervals[0])

    for(let i = 1;i < n;i++) {
      let curr = intervals[i]
      let last = res[res.length - 1]
      if (curr[0] <= last[1]) {
        last[1] = Math.max(curr[1], last[1])
      } else {
        res.push(curr)
      }
    }
    return res
}
```

> [插入区间](https://leetcode-cn.com/problems/insert-interval/)
