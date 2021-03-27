# 滑动窗口

```
int left = 0, right = 0;

while (right < s.size()) {`
    // 增大窗口
    window.add(s[right]);
    right++;

    while (window needs shrink) {
        // 缩小窗口
        window.remove(s[left]);
        left++;
    }
}
```

> [滑动窗口的最大值](https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/)

```
# 暴力解法
#
```

> [最小覆盖子串](https://leetcode-cn.com/problems/minimum-window-substring/)

> [字符串的排列](https://leetcode-cn.com/problems/permutation-in-string/)
