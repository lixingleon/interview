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

#### [451. 根据字符出现频率排序](https://leetcode-cn.com/problems/sort-characters-by-frequency/)

难度中等374收藏分享切换为英文接收动态反馈

给定一个字符串，请将字符串里的字符按照出现的频率降序排列。

**示例 1:**

```
输入:
"tree"

输出:
"eert"

解释:
'e'出现两次，'r'和't'都只出现一次。
因此'e'必须出现在'r'和't'之前。此外，"eetr"也是一个有效的答案。
```

**示例 2:**

```
输入:
"cccaaa"

输出:
"cccaaa"

解释:
'c'和'a'都出现三次。此外，"aaaccc"也是有效的答案。
注意"cacaca"是不正确的，因为相同的字母必须放在一起。
```

**示例 3:**

```
输入:
"Aabb"

输出:
"bbAa"

解释:
此外，"bbaA"也是一个有效的答案，但"Aabb"是不正确的。
注意'A'和'a'被认为是两种不同的字符。
```

通过次数92,865

提交次数129,999

```java
class Solution {
    public String frequencySort(String s) {
        Map<Character, Integer> map = new HashMap<Character, Integer>();
        int maxFreq = 0;
        int length = s.length();
        for (int i = 0; i < length; i++) {
            char c = s.charAt(i);
            int frequency = map.getOrDefault(c, 0) + 1;
            map.put(c, frequency);
            maxFreq = Math.max(maxFreq, frequency);
        }
        StringBuilder[] sbs = new StringBuilder[maxFreq+1];
        for(int i = 0; i<sbs.length; i++){
            sbs[i] = new StringBuilder();
        }
        for(Map.Entry<Character, Integer> entry: map.entrySet()){
            char c = entry.getKey();
            int freq = entry.getValue();
            sbs[freq].append(c);
        }
        StringBuilder ans = new StringBuilder();
        for(int i = maxFreq; i>=0; i--){
            StringBuilder sb = sbs[i];
            for(int j = 0; j<sb.length(); j++){
                for(int k = 0; k<i; k++){
                ans.append(sb.charAt(j));
                }
            }
            
        }
        return ans.toString();

        // StringBuffer[] buckets = new StringBuffer[maxFreq + 1];
        // for (int i = 0; i <= maxFreq; i++) {
        //     buckets[i] = new StringBuffer();
        // }
        // for (Map.Entry<Character, Integer> entry : map.entrySet()) {
        //     char c = entry.getKey();
        //     int frequency = entry.getValue();
        //     buckets[frequency].append(c);
        // }
        // StringBuffer sb = new StringBuffer();
        // for (int i = maxFreq; i > 0; i--) {
        //     StringBuffer bucket = buckets[i];
        //     int size = bucket.length();
        //     for (int j = 0; j < size; j++) {
        //         for (int k = 0; k < i; k++) {
        //             sb.append(bucket.charAt(j));
        //         }
        //     }
        // }
        // return sb.toString();
    }
}
```



#### [168. Excel表列名称](https://leetcode-cn.com/problems/excel-sheet-column-title/)

难度简单504收藏分享切换为英文接收动态反馈

给你一个整数 `columnNumber` ，返回它在 Excel 表中相对应的列名称。

例如：

```
A -> 1
B -> 2
C -> 3
...
Z -> 26
AA -> 27
AB -> 28 
...
```

 

**示例 1：**

```
输入：columnNumber = 1
输出："A"
```

**示例 2：**

```
输入：columnNumber = 28
输出："AB"
```

**示例 3：**

```
输入：columnNumber = 701
输出："ZY"
```

**示例 4：**

```
输入：columnNumber = 2147483647
输出："FXSHRXW"
```

 

**提示：**

- `1 <= columnNumber <= 231 - 1`

```java
//大佬解答:
//这个题有点意思，切勿眼高手低。
//1对应A，26对应Z，看起来像是27进位，似乎应该每次余27，每次除以27。
//但是，因为1对应A，而27对应的也是A，1%27=1, 27%27=0，同一个A余数不同，构成矛盾。
//那么除以26行不行？
//1%26=1,27%26=1，看起来这样似乎可以保持一致。
//但是当26%26的时候，为0，可是实际的值却为Z，又构成了新的矛盾。

// 所以，我们调整对应关系，让0对应A，25对应Z，26对应AA，这样就构成了一个正常的26进位。
//这样对于A：0%26=0， 对于AA：26%26=0，在余数这里可以保持一致。
//新的对应关系是原先对应关系-1得到，所以在每次操作的时候，都要让columnNumber-1，得到新的对应关系。
class Solution {
    public String convertToTitle(int columnNumber) {
        StringBuilder sb = new StringBuilder();
        while(columnNumber!= 0){
            columnNumber --;
            char c = (char)((columnNumber%26)+'A');
            sb.insert(0, c);
            columnNumber/=26;
        }
        return sb.toString();
    }
}
```

#### [124. 二叉树中的最大路径和](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)

难度困难1448收藏分享切换为英文接收动态反馈

**路径** 被定义为一条从树中任意节点出发，沿父节点-子节点连接，达到任意节点的序列。同一个节点在一条路径序列中 **至多出现一次** 。该路径 **至少包含一个** 节点，且不一定经过根节点。

**路径和** 是路径中各节点值的总和。

给你一个二叉树的根节点 `root` ，返回其 **最大路径和** 。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/13/exx1.jpg)

```
输入：root = [1,2,3]
输出：6
解释：最优路径是 2 -> 1 -> 3 ，路径和为 2 + 1 + 3 = 6
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2020/10/13/exx2.jpg)

```
输入：root = [-10,9,20,null,null,15,7]
输出：42
解释：最优路径是 15 -> 20 -> 7 ，路径和为 15 + 20 + 7 = 42
```

 

**提示：**

- 树中节点数目范围是 `[1, 3 * 104]`
- `-1000 <= Node.val <= 1000`

通过次数197,615

提交次数441,414



```java
class Solution {
    int max = Integer.MIN_VALUE;
    public int maxPathSum(TreeNode root) {
        traverse(root);
        return max;

    }
    private int traverse(TreeNode root){
        if(root == null){
            return 0;
        }
        int left = traverse(root.left);
        int right = traverse(root.right);
        //一共四个值要比较，root，左+root， 右+root， 左+右+root
        int larger = Math.max(left, right);
        larger = Math.max(larger+ root.val, root.val);
        int largest = Math.max(larger, root.val+left+right);
        max = Math.max(max, largest);
        return larger;
    }
}
```

#### [386. 字典序排数](https://leetcode-cn.com/problems/lexicographical-numbers/)

难度中等237收藏分享切换为英文接收动态反馈

给你一个整数 `n` ，按字典序返回范围 `[1, n]` 内所有整数。

你必须设计一个时间复杂度为 `O(n)` 且使用 `O(1)` 额外空间的算法。

 

**示例 1：**

```
输入：n = 13
输出：[1,10,11,12,13,2,3,4,5,6,7,8,9]
```

**示例 2：**

```
输入：n = 2
输出：[1,2]
```

 

**提示：**

- `1 <= n <= 5 * 104`

通过次数26,804

提交次数35,654

```java
class Solution {
    public List<Integer> lexicalOrder(int n) {
        List<Integer> res = new ArrayList<>();
        for(int i = 1; i<=9; i++){
            if(i>n){
                break;
            }
            dfs(res, i, n);
        }
        return res;
    }
    private void dfs(List<Integer> res, int i, int n){
        res.add(i);
        for(int j = 0; j<=9; j++){
            int next = i*10+j;
            if(next>n){
                break;
            }
            dfs(res, next, n);
        }
    }
}
```
