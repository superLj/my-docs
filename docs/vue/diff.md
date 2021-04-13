> vNode diff

```
const nodeOps = {
  setTextContent(text) { node.textContent = text },
  parentNode() {},
  removeChild() {},
  nextSibling() {},
  insertBefore() {},
  appendChild(parentNode, childNode) { parentNode.appendChild(childNode) },
  createElement(tag) { return document.createElement(tag) }
}
function insert(parent, elm) {
  nodeOps.appendChild(parent, elm)
}
function createElm (vnode, parentElm) {
  insert(parentElm, nodeOps.createElement(vnode.tag))
}
function addVnodes (parentElm, vnodes, startIdx, endIdx) {
  for (; startIdx <= endIdx; ++startIdx) {
    createElm(vnodes[startIdx], parentElm)
  }
}
function removeNode (el) {
  const parent = nodeOps.parentNode(el);
  if (parent) {
      nodeOps.removeChild(parent, el);
  }
}
function removeVnodes(parentElm, oldVnode, startIdx, endIdx) {}

// 判断是否是相同的节点
function sameVnode(a, b) {
  return (
    a.key === b.key &&
    a.tag === b.tag &&
    a.isComment === b.isComment
    // sameInputType(a, b): 满足当标签类型为 input 的时候 type 相同
  )
}

function patch (oldVnode, vnode, parentElm) {
  if (!vnode) {
    removeVnodes(parentElm, oldVnode, 0,  oldVnode.length - 1)
  } else if (!oldVnode) {
    addVnodes(parentElm, vnodes, 0, vnodes.length - 1)
  } else {
    if (sameVnode(oldVnode, vnode)) {
      patchVnode(oldVnode, vnode)
    } else {
      removeVnodes(parentElm, oldVnode, 0,  oldVnode.length - 1)
      addVnodes(parentElm, vnodes, 0, vnodes.length - 1)
    }
  }
}

function patchVnode(oldVnode, vnode) {
  if (oldVnode === vnode) return

  const elm = vnode.elm = oldVnode.elm
  const oldCh = oldVnode.children
  const ch = vnode.children

  if (vnode.text) {
    nodeOps.setTextContent(elm, vnode.text)
  } else {
    if (oldCh && ch && (oldCh !== ch)) {
      updateChildren(elm, oldCh, ch)
    } else if (ch) {
      addVnodes(elm, null, ch, 0, ch.length - 1)
    } else if (oldCh) {
      removeVnodes(elm, oldCh, 0, oldCh.length - 1)
    }
  }
}

function createKeyToOldIdx(children, beginIdx, endIdx) {
  let i, key
  const map = {}
  for (i = beginIdx; i <= endIdx; ++i) {
    key = children[i].key
    map[key] = i
  }
  return map // key与index索引对应的一个map表
}

function updateChildren(parentElm, oldCh, newCh) {
  let oldStartIdx = 0
  let newStartIdx = 0
  let oldEndIdx = oldCh.length - 1
  let oldStartVnode = oldCh[0]
  let oldEndVnode = oldCh[oldEndIdx]
  let newEndIdx = newCh.length - 1
  let newStartVnode = newCh[0]
  let newEndVnode = newCh[newEndIdx]
  let oldKeyToIdx, idxInOld, elmToMove, refElm

  while (oldStartIdx <= oldEndIdx && newStartIdx <= newEndIdx) {
    if (sameVnode(oldStartVnode, newStartVnode)) {
      patchVnode(oldStartVnode, newStartVnode)
      oldStartVnode = oldCh[++oldStartIdx]
      newStartVnode = newCh[++newStartIdx]
    } else if (sameVnode(oldEndVnode, newEndVnode)) {
      patchVnode(oldEndVnode, newEndVnode)
      oldEndVnode = oldCh[--oldEndIdx]
      newEndVnode = newCh[--newEndIdx]
    } else if (sameVnode(oldStartVnode, newEndVnode)) {
      patchVnode(newStartVnode, oldEndVnode)
      oldStartVnode = oldCh[++oldStartIdx]
      newEndVnode = newCh[--newEndIdx]
    } else if (sameVnode(newStartVnode, oldEndVnode)) {
      patchVnode(newStartVnode, oldEndVnode)
      newStartVnode = newCh[++newStartIdx]
      oldEndVnode = oldCh[--oldEndIdx]
    } else {
      let elmToMove = oldCh[idxInOld]
      if (!oldKeyToIdx) oldKeyToIdx = createKeyToOldIdx(oldCh, oldStartIdx, oldEndIdx)
      idxInOld = newStartVnode.key ? oldKeyToIdx[newStartVnode.key] : null

      if (!idxInOld) {

      } else {
        let elmToMove = oldCh[idxInOld]
        if(sameVnode(elmToMove, newStartVnode)) {
          oldCh[idxInOld] = undefined
          nodeOps.insertBefore(parentElm, newStartVnode.elm, oldStartVnode.elm)
          newStartVnode = newCh[++newStartIdx]
        } else {
          createElm(newStartVnode, parentElm)
          newStartVnode = newCh[++newStartIdx]
        }
      }
    }

    if (oldStartIdx > oldEndIdx) {
      refElm = (newCh[newEndIdx + 1]) ? newCh[newEndIdx + 1].elm : null
      addVnodes(parentElm, refElm, newCh, newStartIdx, newEndIdx)
    } else if (newStartIdx > newEndIdx) {
      removeVnodes(parentElm, oldCh, oldStartIdx, oldEndIdx)
    }
  }
}
```
