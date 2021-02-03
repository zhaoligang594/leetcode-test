### 回溯算法

> 回溯算法的实质是 深度优先遍历+剪支的操作，用于处理我们树结构的问题。

#### 1. [剑指 Offer 38. 字符串的排列](https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/)

```shell
输入一个字符串，打印出该字符串中字符的所有排列。

你可以以任意顺序返回这个字符串数组，但里面不能有重复元素。

示例:

输入：s = "abc"
输出：["abc","acb","bac","bca","cab","cba"]

限制：

1 <= s 的长度 <= 8
```

代码实例：

```java
// 求解问题的方法
public String[] permutation(String s) {
    if (null == s || s.length() == 0) return new String[]{""};
    tag = new boolean[s.length()];
    char[] chars = s.toCharArray();
    Arrays.sort(chars);
    dfs(chars, s.length());
    return list.toArray(new String[0]);
}

// 返回对象
List<String> list = new ArrayList<>();
// 存储临时的数据
StringBuilder sb = new StringBuilder();
// 标记为 标记为 是否访问过
boolean[] tag;

public void dfs(char[] chars, int cur) {
    for (int i = 0; i < chars.length; i++) {
        // 剪枝的操作 可以进行有效的剪枝 将同一层的数据进行剪枝了
        if (i > 0 && chars[i - 1] == chars[i] && !tag[i - 1]) continue;
        if (!tag[i]) {
            tag[i] = true;
            int len = sb.length();
            sb.append(chars[i]);
            if (cur == 1) {
                list.add(sb.toString());
            } else {
                dfs(chars, cur - 1);
            }
            sb.delete(len, sb.length());
            tag[i] = false;
        }
    }
}
```



