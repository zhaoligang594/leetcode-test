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

