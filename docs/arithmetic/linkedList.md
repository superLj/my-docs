# linkedList

> [两个链表的第一个公共节点](https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/)

```
a + b + c, a + c + b
var getIntersectionNode = function(headA, headB) {
  let h1 = headA, h2 = headB

  while(h1 !== h2) {
      h1 = h1 === null?headB: h1.next
      h2 = h2 === null?headA: h2.next
  }

  return h1
}
```

> [如何判断回文链表]()

> [翻转链表]()

> [k 个一组翻转链表]()
