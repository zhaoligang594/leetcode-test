### 2021.02.05 [1208. 尽可能使字符串相等](https://leetcode-cn.com/problems/get-equal-substrings-within-budget/) middle

给你两个长度相同的字符串，s 和 t。

将 s 中的第 i 个字符变到 t 中的x第 i 个字符需要 |s[i] - t[i]| 的开销（开销可能为 0），也就是两个字符的 ASCII 码值的差的绝对值。

用于变更字符串的最大预算是 maxCost。在转化字符串时，总开销应当小于等于该预算，这也意味着字符串的转化可能是不完全的。

如果你可以将 s 的子字符串转化为它在 t 中对应的子字符串，则返回可以转化的最大长度。

如果 s 中没有子字符串可以转化成 t 中对应的子字符串，则返回 0

```code
示例 1：

输入：s = "abcd", t = "bcdf", cost = 3
输出：3
解释：s 中的 "abc" 可以变为 "bcd"。开销为 3，所以最大长度为 3。
示例 2：

输入：s = "abcd", t = "cdef", cost = 3
输出：1
解释：s 中的任一字符要想变成 t 中对应的字符，其开销都是 2。因此，最大长度为 1。
示例 3：

输入：s = "abcd", t = "acde", cost = 0
输出：1
解释：你无法作出任何改动，所以最大长度为 1。
 

提示：

1 <= s.length, t.length <= 10^5
0 <= maxCost <= 10^6
s 和 t 都只含小写英文字母。
```

> 题目分析：采用双指针进行求解

时间复杂度：O(N)

空间复杂度：O(1)

```java
class Solution {
  /**
     * 采用双指针来进行实现
     *
     * @param s       第一个字符串
     * @param t       第二个字符串
     * @param maxCost 最高的花费
     * @return 返回最长的字串
     */
    public int equalSubstring(String s, String t, int maxCost) {
        // 设置双指针 左位置 右位置 返回值 以及 总体的花费
        int left = 0, right = 0, res = 0, totalCost = 0;
        // 计算字符串的长度
        int n = s.length();
        // 循环求解
        for (; right < n; right++) {
            // 计算当前的花费
            int cost = Math.abs(s.charAt(right) - t.charAt(right));
            // 计算总花费
            totalCost += cost;
            // 判断是否超过预期
            if (totalCost > maxCost) {
                // 超过预期最大值的处理
                while (totalCost > maxCost) {
                    int costA = Math.abs(s.charAt(left) - t.charAt(left));
                    totalCost -= costA;
                    left++;
                }
                // 计算最大的字串
                res = Math.max(res, right - left + 1);
            } else {
                // 计算
                res = Math.max(res, right - left + 1);
            }
        }
        // because right is the next location
        res = Math.max(res, right - left);
        return res;
    }
}
```



### 2021.02.04 [643. 子数组最大平均数 I](https://leetcode-cn.com/problems/maximum-average-subarray-i/) simple

```code
给定 n 个整数，找出平均数最大且长度为 k 的连续子数组，并输出该最大平均数。


示例：

输入：[1,12,-5,-6,50,3], k = 4
输出：12.75
解释：最大平均数 (12-5-6+50)/4 = 51/4 = 12.75
 
提示：

1 <= k <= n <= 30,000。
所给数据范围 [-10,000，10,000]。
```

> 题目分析：明显是滑动窗口的问题，涉及到左右指针的问题，在滑动的窗口里面求解最大和数。

```java
class Solution {
   public double findMaxAverage(int[] nums, int k) {
        int start = 0, end = 0, sum = 0, max = Integer.MIN_VALUE;
        while (end < nums.length) {
            sum += nums[end];
            if (end - start + 1 == k) {
                max = Math.max(max, sum);
                sum -= nums[start];
                start++;
            }
            end++;
        }
        return max * 1.0 / k;
    }
}
```



### 2021.02.02 [424. 替换后的最长重复字符](https://leetcode-cn.com/problems/longest-repeating-character-replacement/)

给你一个仅由大写英文字母组成的字符串，你可以将任意位置上的字符替换成另外的字符，总共可最多替换 k 次。在执行上述操作后，找到包含重复字母的最长子串的长度。

注意：字符串长度 和 k 不会超过 104。

示例 1：

```code
输入：s = "ABAB", k = 2
输出：4
解释：用两个'A'替换为两个'B',反之亦然。
```


示例 2：

```code
输入：s = "AABABBA", k = 1
输出：4
解释：
将中间的一个'A'替换为'B',字符串变为 "AABBBBA"。
子串 "BBBB" 有最长重复字母, 答案为 4。
```

题解代码：

```java
 public int characterReplacement(String s, int k) {
        int[] num = new int[26];
        int n = s.length();
        int maxn = 0;
        int left = 0, right = 0;
        while (right < n) {
            num[s.charAt(right) - 'A']++;
            maxn = Math.max(maxn, num[s.charAt(right) - 'A']);
            if (right - left + 1 - maxn > k) {
                num[s.charAt(left) - 'A']--;
                left++;
            }
            right++;
        }
        return right - left;
}
```

