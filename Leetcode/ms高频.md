#### [169. 多数元素](https://leetcode-cn.com/problems/majority-element/)

难度简单1318收藏分享切换为英文接收动态反馈

给定一个大小为 *n *的数组，找到其中的多数元素。多数元素是指在数组中出现次数 **大于** `⌊ n/2 ⌋` 的元素。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

 

**示例 1：**

```
输入：[3,2,3]
输出：3
```

**示例 2：**

```
输入：[2,2,1,1,1,2,2]
输出：2

```

 

**进阶：**

- 尝试设计时间复杂度为 O(n)、空间复杂度为 O(1) 的算法解决此问题。

```java
class Solution {
    public int majorityElement(int[] nums) {
        int cand = nums[0];
        int count =1;
        for(int i = 1; i<nums.length; i++){
            if(nums[i] == cand){
                count++;
            }
            else{
                count--;
            }
            if(count == 0){
                cand = nums[i];
                count = 1;
            }
        }
        return cand;
    }
}
```



#### [229. 求众数 II](https://leetcode-cn.com/problems/majority-element-ii/)

难度中等564收藏分享切换为英文接收动态反馈

给定一个大小为 *n *的整数数组，找出其中所有出现超过 `⌊ n/3 ⌋` 次的元素。

 

 

**示例 1：**

```
输入：[3,2,3]
输出：[3]
```

**示例 2：**

```
输入：nums = [1]
输出：[1]

```

**示例 3：**

```
输入：[1,1,1,3,3,2,2,2]
输出：[1,2]
```

 

**提示：**

- `1 <= nums.length <= 5 * 104`
- `-109 <= nums[i] <= 109`



**进阶：**尝试设计时间复杂度为 O(n)、空间复杂度为 O(1)的算法解决此问题。

```java
class Solution {
    public List<Integer> majorityElement(int[] nums) {
        List<Integer> res = new ArrayList<>();
        if(nums.length == 1){
            res.add(nums[0]);
            return res;
        }
        int num1 = nums[0];
        int count1 = 0;
        int num2 = nums[0];
        int count2 = 0;
        for(int i = 0; i<nums.length; i++){
            if(nums[i] == num1){
                count1++;
                
            }
            else if(nums[i] == num2){
                count2++;
            }
            else{
                if(count1 == 0){
                    num1 = nums[i];
                    count1 = 1;
                }
                else if(count2 == 0){
                    num2 = nums[i];
                    count2 = 1;
                }
                else{
                    count1--;
                    count2--;
                }
            }

        }
        count1 = 0;
        count2 = 0;
        for(int num: nums){
            if(num == num1){
                count1++;
            }
            else if(num == num2){
                count2++;
            }
        }
        if(count1>nums.length/3){
            res.add(num1);
        }
        if(count2>nums.length/3){
            res.add(num2);
        }
        return res;
    }
}
```

#### [31. 下一个排列](https://leetcode-cn.com/problems/next-permutation/)

难度中等1583

整数数组的一个 **排列**  就是将其所有成员以序列或线性顺序排列。

- 例如，`arr = [1,2,3]` ，以下这些都可以视作 `arr` 的排列：`[1,2,3]`、`[1,3,2]`、`[3,1,2]`、`[2,3,1]` 。

整数数组的 **下一个排列** 是指其整数的下一个字典序更大的排列。更正式地，如果数组的所有排列根据其字典顺序从小到大排列在一个容器中，那么数组的 **下一个排列** 就是在这个有序容器中排在它后面的那个排列。如果不存在下一个更大的排列，那么这个数组必须重排为字典序最小的排列（即，其元素按升序排列）。

- 例如，`arr = [1,2,3]` 的下一个排列是 `[1,3,2]` 。
- 类似地，`arr = [2,3,1]` 的下一个排列是 `[3,1,2]` 。
- 而 `arr = [3,2,1]` 的下一个排列是 `[1,2,3]` ，因为 `[3,2,1]` 不存在一个字典序更大的排列。

给你一个整数数组 `nums` ，找出 `nums` 的下一个排列。

必须** 原地 **修改，只允许使用额外常数空间。

 

**示例 1：**

```
输入：nums = [1,2,3]
输出：[1,3,2]

```

**示例 2：**

```
输入：nums = [3,2,1]
输出：[1,2,3]

```

**示例 3：**

```
输入：nums = [1,1,5]
输出：[1,5,1]

```

 

**提示：**

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 100`

通过次数

271,818

提交次数

726,892

```java
class Solution {
    public void nextPermutation(int[] nums) {
        if(nums.length == 1){
            return;
        }
        int idx = nums.length-2;
        //从右往左找到第一个波谷
        while(idx>=0&& nums[idx]>=nums[idx+1]){
            idx--;
        }
        if(idx == -1){
            int left = 0;
            int right = nums.length-1;
            while(left<right){
                swap(nums, left, right);
                left++;
                right--;
            }
            return;
        }
        for(int i = nums.length-1; i>idx; i--){
            if(nums[idx]<nums[i]){
                swap(nums, idx, i);
                //idx之后的数必定是递减的
                //扭转成递增的能减少增大的量
                Arrays.sort(nums, idx+1, nums.length);
                return;
            }
        }
    }
    private void swap(int[] nums, int left, int right){
        int tmp = nums[left];
        nums[left] = nums[right];
        nums[right] = tmp;
    }
}
```





#### [32. 最长有效括号](https://leetcode-cn.com/problems/longest-valid-parentheses/)

难度困难1695

给你一个只包含 `'('` 和 `')'` 的字符串，找出最长有效（格式正确且连续）括号子串的长度。

 

**示例 1：**

```
输入：s = "(()"
输出：2
解释：最长有效括号子串是 "()"

```

**示例 2：**

```
输入：s = ")()())"
输出：4
解释：最长有效括号子串是 "()()"

```

**示例 3：**

```
输入：s = ""
输出：0

```

 

**提示：**

- `0 <= s.length <= 3 * 104`
- `s[i]` 为 `'('` 或 `')'`

通过次数

237,189

提交次数

657,118

```java
//注意子串是连续的 动态规划解法
class Solution {
    public int longestValidParentheses(String s) {
        int[] dp = new int[s.length()];
        int max = 0;
        for(int i = 1; i<dp.length; i++){
            if(s.charAt(i) == '('){
                continue;
            }
            if(s.charAt(i-1) == '('){
                if(i-2>=0){
                    dp[i] = dp[i-2]+2;
                }
                else{
                    dp[i] = 2;
                }
            }
            else{
                if(i-1-dp[i-1]>=0){
                    if(s.charAt(i-1-dp[i-1]) == '('){
                        if(i-2-dp[i-1]>=0){
                            dp[i] = dp[i-2-dp[i-1]]+2+dp[i-1];
                        }
                        else{
                            dp[i] = dp[i-1]+2;
                        }
                    }
                }
                
            }
            max = Math.max(max, dp[i]);
        }
        return max;
    }
}

//滑动窗口解法
//用栈解决。本质是滑动窗口。
//每个左括号都是一个可能的解的left指针，所以都塞进栈里。
//每个右括号都预示着目前可能已经有解了，要从栈中pop出值来抵消。
//要是栈中没有左括号与之对应，说明右括号是不合法的，下一个有效括号子数组从这个右括号右边开始。
class Solution {
    public int longestValidParentheses(String s) {
        ArrayDeque<Integer> stack = new ArrayDeque<>();
        stack.push(-1);
        int max = 0;
        for(int i = 0; i<s.length(); i++){
            if(s.charAt(i) == '('){
                stack.push(i);
            }
            else{
                stack.pop();
                if(stack.isEmpty()){
                    stack.push(i);
                }
                else{
                    max = Math.max(max, i-stack.peek());
                }
            }
        }
        return max;
    }
}
```

