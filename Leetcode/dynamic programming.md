#### [97. 交错字符串](https://leetcode-cn.com/problems/interleaving-string/)

难度中等639收藏分享切换为英文接收动态反馈

给定三个字符串 `s1`、`s2`、`s3`，请你帮忙验证 `s3` 是否是由 `s1` 和 `s2` **交错** 组成的。

两个字符串 `s` 和 `t` **交错** 的定义与过程如下，其中每个字符串都会被分割成若干 **非空** 子字符串：

- `s = s1 + s2 + ... + sn`
- `t = t1 + t2 + ... + tm`
- `|n - m| <= 1`
- **交错** 是 `s1 + t1 + s2 + t2 + s3 + t3 + ...` 或者 `t1 + s1 + t2 + s2 + t3 + s3 + ...`

**注意：**`a + b` 意味着字符串 `a` 和 `b` 连接。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/09/02/interleave.jpg)

```
输入：s1 = "aabcc", s2 = "dbbca", s3 = "aadbbcbcac"
输出：true
```

**示例 2：**

```
输入：s1 = "aabcc", s2 = "dbbca", s3 = "aadbbbaccc"
输出：false
```

**示例 3：**

```
输入：s1 = "", s2 = "", s3 = ""
输出：true
```

 

**提示：**

- `0 <= s1.length, s2.length <= 100`
- `0 <= s3.length <= 200`
- `s1`、`s2`、和 `s3` 都由小写英文字母组成

 

**进阶：**您能否仅使用 `O(s2.length)` 额外的内存空间来解决它?



```java
class Solution {
    public boolean isInterleave(String s1, String s2, String s3) {
        if(s1.length() + s2.length() != s3.length()){
            return false;
        }
        boolean[][] dp = new boolean[s1.length()+1][s2.length()+1];
        dp[0][0] = true;
        for(int j = 1; j<=s2.length(); j++){
            dp[0][j] = dp[0][j-1]&& s2.charAt(j-1) == s3.charAt(j-1);
        }
        for(int i = 1; i<=s1.length(); i++){
            dp[i][0] = dp[i-1][0]&& s1.charAt(i-1) == s3.charAt(i-1);
        }
        for(int i = 1; i<=s1.length(); i++){
            for(int j = 1; j<=s2.length(); j++){
                dp[i][j] = (dp[i-1][j] && s1.charAt(i-1) == s3.charAt(i+j-1)) || 
                            (dp[i][j-1] && s2.charAt(j-1) == s3.charAt(i+j-1));
                
            }
        }
        return dp[s1.length()][s2.length()];
    }
}
```

