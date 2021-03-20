# tree[二叉树的问题难点在于，如何把题目的要求细化成每个节点需要做的事情]

```
/* 二叉树遍历框架 */
void traverse(TreeNode root) {
    // 前序遍历
    traverse(root.left)
    // 中序遍历
    traverse(root.right)
    // 后序遍历
}
```

> [二叉树的深度](https://leetcode-cn.com/problems/er-cha-shu-de-shen-du-lcof/)

```
var maxDepth = function(root) {
      if (root === null) return 0

      let leftNum = maxDepth(root.left)
      let rightNum = maxDepth(root.right)

      return (leftNum > rightNum)? leftNum + 1 : rightNum + 1
}
```

> [平衡二叉树](https://leetcode-cn.com/problems/ping-heng-er-cha-shu-lcof/)

```
var isBalanced = function (root) {
    if (root === null) { return true }
    let leftHeight = getNodeHeight(root.left);
    let rightHeight = getNodeHeight(root.right);
    if (Math.abs(leftHeight - rightHeight) > 1) {
        return false
    }
    return isBalanced(root.left) && isBalanced(root.right);
};
function getNodeHeight(node) {
    if (node === null) { return 0 }
    return Math.max(getNodeHeight(node.left), getNodeHeight(node.right)) + 1;
}
```

> [填充每个节点的下一个右侧节点指针](https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node/)

```
var connect = function (root) {
  if (!root) return
  connectHelp(root.left, root.right)
  return root
}

function connectHelp(left, right) {
  if (!left || !right) return null
  left.next = right

  // 相同父节点
  connectHelp(left.left, left.right)
  connectHelp(right.left, right.right)
  // 不同父节点
  connectHelp(left.right, right.left)
}
```

> [二叉树展开为链表](https://leetcode-cn.com/problems/flatten-binary-tree-to-linked-list/)

```
var flatten = function(root) {
  if (root === null) return null

}
```

> [通过前序和中序/后序和中序遍历结果构造二叉树]()

> [寻找重复的子树](https://leetcode-cn.com/problems/find-duplicate-subtrees/)
