#### 1. [剑指 Offer 03. 数组中重复的数字](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof)  (simple)

```shell
找出数组中重复的数字。

在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

示例 1：

输入：
[2, 3, 1, 0, 2, 5, 3]
输出：2 或 3

限制：

2 <= n <= 100000
```

代码：

```java
/**
 * 找到重复数
 *
 * @param nums 输入的参数
 * @return 返回的对象
 */
public int findRepeatNumber(int[] nums) {
    // 设定一个标记的数组 来表示 当前是否已经查看过了 实际上也是一个 hash
    int[] tags = new int[nums.length];
    for (int i = 0; i < nums.length; i++) {
        if (tags[nums[i]] < 0) { // 说明已经访问过了
            return nums[i];
        } else {
            tags[nums[i]] = -1;// 没有访问过 那么就标记为 -1
        }
    }
    return -1;
}
```

> 题目的思考：实际上，我们也可以在原有的基础上进行操作。我们可以进行深度的遍历我们的数组，如果在遍历的过程中，发现当前的位置是-1，那么这个值就是我们需要的值

