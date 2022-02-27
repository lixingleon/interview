#### [17. 电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)

难度中等1699收藏分享切换为英文接收动态反馈

给定一个仅包含数字 `2-9` 的字符串，返回所有它能表示的字母组合。答案可以按 **任意顺序** 返回。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/11/09/200px-telephone-keypad2svg.png)

 

**示例 1：**

```
输入：digits = "23"
输出：["ad","ae","af","bd","be","bf","cd","ce","cf"]
```

**示例 2：**

```
输入：digits = ""
输出：[]
```

**示例 3：**

```
输入：digits = "2"
输出：["a","b","c"]
```

 

**提示：**

- `0 <= digits.length <= 4`
- `digits[i]` 是范围 `['2', '9']` 的一个数字。

通过次数399,251

提交次数693,194

```java
回溯的关键步骤：定义好一个递归函数，设置好递归跳出条件。
class Solution {
    HashMap<Character, String> map;
    public List<String> letterCombinations(String digits) {
        List<String> res = new ArrayList<>();
        if(digits.length() == 0){
            return res;
        }
        map = new HashMap<>();
        map.put('2', "abc");
        map.put('3', "def");
        map.put('4', "ghi");
        map.put('5', "jkl");
        map.put('6', "mno");
        map.put('7', "pqrs");
        map.put('8', "tuv");
        map.put('9', "wxyz");
        StringBuilder sb = new StringBuilder();
        getCombination(digits, 0, sb, res);
        return res;
    }
    private void getCombination(String digits, int idx, StringBuilder sb, List<String> res){
        if(digits.length() == sb.length()){
            res.add(sb.toString());
            return;
        }
        // if(idx>=digits.length()){
        //     return;
        // }
        char num = digits.charAt(idx);
        String letters = map.get(num);
        for(char n: letters.toCharArray()){
            sb.append(n);
            getCombination(digits, idx+1, sb, res);
            sb.deleteCharAt(sb.length()-1);
        }


    }
}
```



#### [39. 组合总和](https://leetcode-cn.com/problems/combination-sum/)

难度中等1737收藏分享切换为英文接收动态反馈

给你一个 **无重复元素** 的整数数组 `candidates` 和一个目标整数 `target` ，找出 `candidates` 中可以使数字和为目标数 `target` 的 *所有* **不同组合** ，并以列表形式返回。你可以按 **任意顺序** 返回这些组合。

`candidates` 中的 **同一个** 数字可以 **无限制重复被选取** 。如果至少一个数字的被选数量不同，则两种组合是不同的。 

对于给定的输入，保证和为 `target` 的不同组合数少于 `150` 个。

 

**示例 1：**

```
输入：candidates = [2,3,6,7], target = 7
输出：[[2,2,3],[7]]
解释：
2 和 3 可以形成一组候选，2 + 2 + 3 = 7 。注意 2 可以使用多次。
7 也是一个候选， 7 = 7 。
仅有这两种组合。
```

**示例 2：**

```
输入: candidates = [2,3,5], target = 8
输出: [[2,2,2,2],[2,3,3],[3,5]]
```

**示例 3：**

```
输入: candidates = [2], target = 1
输出: []
```

 

**提示：**

- `1 <= candidates.length <= 30`
- `1 <= candidates[i] <= 200`
- `candidate` 中的每个元素都 **互不相同**
- `1 <= target <= 500`

通过次数400,995

提交次数550,991

```java
/*
1. 排序后可以在组合和大于target时及时剪枝。
2. begin的作用是不再回头去取已经用过的值。
3. target用作新增组合的标志。
4. 最外层for循环结束后，整个程序就结束了。
*/
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        Arrays.sort(candidates);
        List<List<Integer>> res = new ArrayList<>();
        LinkedList<Integer> path = new LinkedList<>();
        findCombination(candidates, target, 0, res, path);
        return res;
    }
    private void findCombination(int[] candidates, int target, int begin, List<List<Integer>> res, LinkedList<Integer> path){
        if(target == 0){
            res.add(new ArrayList<>(path));
            return;
        }
        for(int i = begin; i<candidates.length; i++){
            if(target-candidates[i]<0){
                break;
            }
            path.addLast(candidates[i]);
            findCombination(candidates, target-candidates[i], i, res, path);
            path.removeLast();
        }
    }
}
```



#### [40. 组合总和 II](https://leetcode-cn.com/problems/combination-sum-ii/)

难度中等816收藏分享切换为英文接收动态反馈

给定一个候选人编号的集合 `candidates` 和一个目标数 `target` ，找出 `candidates` 中所有可以使数字和为 `target` 的组合。

`candidates` 中的每个数字在每个组合中只能使用 **一次** 。

**注意：**解集不能包含重复的组合。 

 

**示例 1:**

```
输入: candidates = [10,1,2,7,6,1,5], target = 8,
输出:
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]
```

**示例 2:**

```
输入: candidates = [2,5,2,1,2], target = 5,
输出:
[
[1,2,2],
[5]
]
```

 

**提示:**

- `1 <= candidates.length <= 100`
- `1 <= candidates[i] <= 50`
- `1 <= target <= 30`

通过次数235,964

提交次数384,464

```java
/*
1. 排序后可以在组合和大于target时及时剪枝。
2. begin的作用是只取还没用过的值。
3. target用作新增组合的标志。
4. 最外层for循环结束后，整个程序就结束了。
5. 
*/
class Solution {
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        Arrays.sort(candidates);
        List<List<Integer>> res = new ArrayList<>();
        LinkedList<Integer> path = new LinkedList<>();
        findCombination(candidates, target, 0, res, path);
        return res;
    }
    private void findCombination(int[] candidates, int target, int begin, List<List<Integer>> res, LinkedList<Integer> path){
        if(target == 0){
            res.add(new ArrayList<>(path));
            return;
        }
        for(int i = begin; i<candidates.length; i++){
            if(target- candidates[i]<0){
                break;
            }
            //如果重复的值在当前循环已经被用过，就跳过。避免重复
            if(i>begin && candidates[i] == candidates[i-1]){
                continue;
            }
            path.add(candidates[i]);
            //每个数只能用一次，所以begin每次赋值为i+1
            findCombination(candidates, target-candidates[i], i+1, res, path);
            path.removeLast();
        }
    }
}
```



#### [77. 组合](https://leetcode-cn.com/problems/combinations/)

难度中等850收藏分享切换为英文接收动态反馈

给定两个整数 `n` 和 `k`，返回范围 `[1, n]` 中所有可能的 `k` 个数的组合。

你可以按 **任何顺序** 返回答案。

 

**示例 1：**

```
输入：n = 4, k = 2
输出：
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```

**示例 2：**

```
输入：n = 1, k = 1
输出：[[1]]
```

 

**提示：**

- `1 <= n <= 20`
- `1 <= k <= n`

通过次数271,800

提交次数353,153

```java
//所有组合类的回溯题，都需要一个begin变量来规定每次循环开启的位置
class Solution {
    public List<List<Integer>> combine(int n, int k) {
        List<List<Integer>> res = new ArrayList<>();
        LinkedList<Integer> path = new LinkedList<>();
        getCombination(n, k, 1, res, path);
        return res;
    }
    private void getCombination(int n, int k, int begin, List<List<Integer>> res, LinkedList<Integer> path){
        if(k == 0){
            res.add(new ArrayList<>(path));
            return;
        }
        //i最大就是n-k+1,因为要得到长度为k的组合
        for(int i = begin; i<=n-k+1; i++){
            path.add(i);
            getCombination(n, k-1, i+1, res, path);
            path.removeLast();
        }
    }
}
```

#### [78. 子集](https://leetcode-cn.com/problems/subsets/)

难度中等1465收藏分享切换为英文接收动态反馈

给你一个整数数组 `nums` ，数组中的元素 **互不相同** 。返回该数组所有可能的子集（幂集）。

解集 **不能** 包含重复的子集。你可以按 **任意顺序** 返回解集。

 

**示例 1：**

```
输入：nums = [1,2,3]
输出：[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```

**示例 2：**

```
输入：nums = [0]
输出：[[],[0]]
```

 

**提示：**

- `1 <= nums.length <= 10`
- `-10 <= nums[i] <= 10`
- `nums` 中的所有元素 **互不相同**

通过次数363,959

提交次数453,245

```java
//思路：每遍历一个新的num，就将这个num加入每一个现有的子集生成一个新子集。

//遍历list时如果要动态地添加元素，用for int i遍历
            //如果要动态地删除元素，用iterator has next遍历
            /*
            //准备数据
        List<Student> list = new ArrayList<>();
        list.add(new Student("male"));
        list.add(new Student("female"));
        list.add(new Student("female"));
        list.add(new Student("male"));
 
        //遍历删除,除去男生
        Iterator<Student> iterator = list.iterator();
        while (iterator.hasNext()) {
            Student student = iterator.next();
            if ("male".equals(student.getGender())) {
                iterator.remove();//使用迭代器的删除方法删除
            }
        }
            */
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        res.add(new ArrayList<>());
        for(int num: nums){
            int size = res.size();
            for(int i = 0; i<size; i++){
                List<Integer> new_l = new ArrayList<>(res.get(i));
                new_l.add(num);
                res.add(new_l);
            }
        }
        return res;
    }
}
//思路2：正常遍历每个num即可得到所有子集
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        LinkedList<Integer> path = new LinkedList<>();
        getSUbCombination(nums, res, path, 0);
        return res;
    }
    private void getSUbCombination(int[] nums, List<List<Integer>> res, LinkedList<Integer> path, int begin){
        res.add(new ArrayList<>(path));
        for(int i = begin; i<nums.length; i++){
            path.add(nums[i]);
            getSUbCombination(nums, res, path, i+1);
            path.removeLast();
        }
    }
}
```



#### [90. 子集 II](https://leetcode-cn.com/problems/subsets-ii/)

难度中等731收藏分享切换为英文接收动态反馈

给你一个整数数组 `nums` ，其中可能包含重复元素，请你返回该数组所有可能的子集（幂集）。

解集 **不能** 包含重复的子集。返回的解集中，子集可以按 **任意顺序** 排列。

 

**示例 1：**

```
输入：nums = [1,2,2]
输出：[[],[1],[1,2],[1,2,2],[2],[2,2]]
```

**示例 2：**

```
输入：nums = [0]
输出：[[],[0]]
```

 

**提示：**

- `1 <= nums.length <= 10`
- `-10 <= nums[i] <= 10`

通过次数161,923

提交次数255,620

```java
class Solution {
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> res = new ArrayList<>();
        LinkedList<Integer> path = new LinkedList<>();
        getSUbCombination(nums, res, path, 0);
        return res;
    }
    private void getSUbCombination(int[] nums, List<List<Integer>> res, LinkedList<Integer> path, int begin){
        res.add(new ArrayList<>(path));
        for(int i = begin; i<nums.length; i++){
            if(i>begin && nums[i] == nums[i-1]){
                continue;
            }
            path.add(nums[i]);
            getSUbCombination(nums, res, path, i+1);
            path.removeLast();
        }
    }
}
```

#### [216. 组合总和 III](https://leetcode-cn.com/problems/combination-sum-iii/)

难度中等419收藏分享切换为英文接收动态反馈

找出所有相加之和为 ***n*** 的 ***k\*** 个数的组合***。\***组合中只允许含有 1 - 9 的正整数，并且每种组合中不存在重复的数字。

**说明：**

- 所有数字都是正整数。
- 解集不能包含重复的组合。 

**示例 1:**

```
输入: k = 3, n = 7
输出: [[1,2,4]]
```

**示例 2:**

```
输入: k = 3, n = 9
输出: [[1,2,6], [1,3,5], [2,3,4]]
```

```java
class Solution {
    public List<List<Integer>> combinationSum3(int k, int n) {
        List<List<Integer>> res = new ArrayList<>();
        LinkedList<Integer> path = new LinkedList<>();
        getCombination(k, n, 1, res, path);
        return res;
    }
    private void getCombination(int k, int n, int begin, List<List<Integer>> res, LinkedList<Integer> path){
        if(k == 0){
            if(n == 0){
                res.add(new ArrayList<>(path));
            }
            return;
        }
        for(int i = begin; i<=9; i++){
            if(n-i<0){
                break;
            }
            path.add(i);
            getCombination(k-1, n-i, i+1, res, path);
            path.removeLast();
        }
        
        
    }
}
```



#### [377. 组合总和 Ⅳ](https://leetcode-cn.com/problems/combination-sum-iv/)

难度中等576收藏分享切换为英文接收动态反馈

给你一个由 **不同** 整数组成的数组 `nums` ，和一个目标整数 `target` 。请你从 `nums` 中找出并返回总和为 `target` 的元素组合的个数。

题目数据保证答案符合 32 位整数范围。

 

**示例 1：**

```
输入：nums = [1,2,3], target = 4
输出：7
解释：
所有可能的组合为：
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)
请注意，顺序不同的序列被视作不同的组合。
```

**示例 2：**

```
输入：nums = [9], target = 3
输出：0
```

 

**提示：**

- `1 <= nums.length <= 200`
- `1 <= nums[i] <= 1000`
- `nums` 中的所有元素 **互不相同**
- `1 <= target <= 1000`



**进阶：**如果给定的数组中含有负数会发生什么？问题会产生何种变化？如果允许负数出现，需要向题目中添加哪些限制条件？

```java
//dp[i]表示和为id
class Solution {
    public int combinationSum4(int[] nums, int target) {
        int[] dp = new int[target+1];
        dp[0] = 1;
        for(int i = 1; i<=target; i++){
            for(int num: nums){
                if(i>=num){
                    dp[i]+= dp[i-num];
                }
            }
        }
        return dp[target];

    }
}
```



#### [46. 全排列](https://leetcode-cn.com/problems/permutations/)

难度中等1759收藏分享切换为英文接收动态反馈

给定一个不含重复数字的数组 `nums` ，返回其 *所有可能的全排列* 。你可以 **按任意顺序** 返回答案。

 

**示例 1：**

```
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

**示例 2：**

```
输入：nums = [0,1]
输出：[[0,1],[1,0]]
```

**示例 3：**

```
输入：nums = [1]
输出：[[1]]
```

 

**提示：**

- `1 <= nums.length <= 6`
- `-10 <= nums[i] <= 10`
- `nums` 中的所有整数 **互不相同**

通过次数504,771

提交次数643,123

```java
//permutation不同于combination, 使用的是used数组
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        LinkedList<Integer> path = new LinkedList<>();
        boolean[] used = new boolean[nums.length];
        getPermutation(nums, used, path, res);
        return res;
    }
    private void getPermutation(int[] nums, boolean[] used, LinkedList<Integer> path, List<List<Integer>> res){
        if(path.size() == nums.length){
            res.add(new ArrayList<>(path));
            return;
        }
        for(int i = 0; i<nums.length; i++){
            if(used[i]){
                continue;
            }
            used[i] = true;
            path.add(nums[i]);
            getPermutation(nums, used, path, res);
            used[i] = false;
            path.removeLast();
        }
    }
}
```

#### [47. 全排列 II](https://leetcode-cn.com/problems/permutations-ii/)

难度中等935收藏分享切换为英文接收动态反馈

给定一个可包含重复数字的序列 `nums` ，***按任意顺序*** 返回所有不重复的全排列。

 

**示例 1：**

```
输入：nums = [1,1,2]
输出：
[[1,1,2],
 [1,2,1],
 [2,1,1]]
```

**示例 2：**

```
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

 

**提示：**

- `1 <= nums.length <= 8`
- `-10 <= nums[i] <= 10`

通过次数258,714

提交次数402,427

```java
class Solution {
    public List<List<Integer>> permuteUnique(int[] nums) {
        //先排序有助于剪枝
        Arrays.sort(nums);
        List<List<Integer>> res = new ArrayList<>();
        LinkedList<Integer> path = new LinkedList<>();
        boolean[] used = new boolean[nums.length];
        getPermutation(nums, used, path, res);
        return res;
    }
    private void getPermutation(int[] nums, boolean[] used, LinkedList<Integer> path, List<List<Integer>> res){
        if(path.size() == nums.length){
            res.add(new ArrayList<>(path));
            return;
        }
        for(int i = 0; i<nums.length; i++){
            if(used[i]){
                continue;
            }
            if(i>0 && nums[i] == nums[i-1] && !used[i-1]){
                continue;
            }
            used[i] = true;
            path.add(nums[i]);
            getPermutation(nums, used, path, res);
            used[i] = false;
            path.removeLast();
        }
    }
}
```



#### [752. 打开转盘锁](https://leetcode-cn.com/problems/open-the-lock/)

难度中等451收藏分享切换为英文接收动态反馈

你有一个带有四个圆形拨轮的转盘锁。每个拨轮都有10个数字： `'0', '1', '2', '3', '4', '5', '6', '7', '8', '9'` 。每个拨轮可以自由旋转：例如把 `'9'` 变为 `'0'`，`'0'` 变为 `'9'` 。每次旋转都只能旋转一个拨轮的一位数字。

锁的初始数字为 `'0000'` ，一个代表四个拨轮的数字的字符串。

列表 `deadends` 包含了一组死亡数字，一旦拨轮的数字和列表里的任何一个元素相同，这个锁将会被永久锁定，无法再被旋转。

字符串 `target` 代表可以解锁的数字，你需要给出解锁需要的最小旋转次数，如果无论如何不能解锁，返回 `-1` 。

 

**示例 1:**

```
输入：deadends = ["0201","0101","0102","1212","2002"], target = "0202"
输出：6
解释：
可能的移动序列为 "0000" -> "1000" -> "1100" -> "1200" -> "1201" -> "1202" -> "0202"。
注意 "0000" -> "0001" -> "0002" -> "0102" -> "0202" 这样的序列是不能解锁的，
因为当拨动到 "0102" 时这个锁就会被锁定。

```

**示例 2:**

```
输入: deadends = ["8888"], target = "0009"
输出：1
解释：把最后一位反向旋转一次即可 "0000" -> "0009"。

```

**示例 3:**

```
输入: deadends = ["8887","8889","8878","8898","8788","8988","7888","9888"], target = "8888"
输出：-1
解释：无法旋转到目标数字且不被锁定。

```

 

**提示：**

- `1 <= deadends.length <= 500`
- `deadends[i].length == 4`
- `target.length == 4`
- `target` **不在** `deadends` 之中
- `target` 和 `deadends[i]` 仅由若干位数字组成

```java
//需要一个queue，一个visited集合。
//对bfs的每一层
    //对每一层的每个元素
        //对每个元素的每个char
        //判断一下是不是target，如果是，结束程序。
        //如果不是，将元素加到下层bfs中。
class Solution {
    public int openLock(String[] deadends, String target) {
        HashSet<String> visited = new HashSet<>();
        HashSet<String> deadendsSet = new HashSet<>(Arrays.asList(deadends));
        LinkedList<String> queue = new LinkedList<>();
        String start = "0000";
        if(deadendsSet.contains(start) || deadendsSet.contains(target)){
            return -1;
        }
        queue.add(start);
        int step = 0;
        while(!queue.isEmpty()){
            int size = queue.size();
            for(int i = 0; i<size; i++){
                String cur = queue.removeFirst();
                if(cur.equals(target)){
                    return step;
                }
                List<String> nexts = buildNexts(cur);
                for(String next: nexts){
                    if(!visited.contains(next)&& !deadendsSet.contains(next)){
                        queue.add(next);
                        visited.add(next);
                    }
                }
            }
            step++;
        }
        return -1;
    }
    private List<String> buildNexts(String key){
        List<String> res = new ArrayList<>();
        char[] chars = key.toCharArray();
        for(int i = 0; i<chars.length; i++){
            char origin= chars[i];
            chars[i] = chars[i]!='9'? (char)(chars[i]+1): '0';
            res.add(String.valueOf(chars));
            chars[i] = origin;
            chars[i] = chars[i]!='0'? (char)(chars[i]-1): '9';
            res.add(String.valueOf(chars));
            chars[i] = origin;
        }
        return res;
    }
}
```



#### [127. 单词接龙](https://leetcode-cn.com/problems/word-ladder/)

难度困难962收藏分享切换为英文接收动态反馈

字典 `wordList` 中从单词 `beginWord`* *和 `endWord` 的 **转换序列 **是一个按下述规格形成的序列 `beginWord -> s1 -> s2 -> ... -> sk`：

- 每一对相邻的单词只差一个字母。
-  对于 `1 <= i <= k` 时，每个 `si` 都在 `wordList` 中。注意， `beginWord`* *不需要在 `wordList` 中。
- `sk == endWord`

给你两个单词* *`beginWord`* *和 `endWord` 和一个字典 `wordList` ，返回 *从 beginWord 到 endWord 的 \**最短转换序列** 中的 **单词数目*** 。如果不存在这样的转换序列，返回 `0` 。

**示例 1：**

```
输入：beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
输出：5
解释：一个最短转换序列是 "hit" -> "hot" -> "dot" -> "dog" -> "cog", 返回它的长度 5。

```

**示例 2：**

```
输入：beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]
输出：0
解释：endWord "cog" 不在字典中，所以无法进行转换。
```

 

**提示：**

- `1 <= beginWord.length <= 10`
- `endWord.length == beginWord.length`
- `1 <= wordList.length <= 5000`
- `wordList[i].length == beginWord.length`
- `beginWord`、`endWord` 和 `wordList[i]` 由小写英文字母组成
- `beginWord != endWord`
- `wordList` 中的所有字符串 **互不相同**



```java
//在图的两个端点之间找最短路径
//图的每条边的权重都是1，意味着两个单词之间只有一个字母有差别
//从beginWord开始，看最终到endWord的路径有多长,也就是要经过几次变换
class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        HashSet<String> wordSet = new HashSet<>(wordList);
        if(!wordSet.contains(endWord)){
            return 0;
        }
        wordSet.remove(beginWord);

        LinkedList<String> queue = new LinkedList<>();
        queue.add(beginWord);
        HashSet<String> visited = new HashSet<>();
        visited.add(beginWord);

        int step = 1;
        while(!queue.isEmpty()){
            int curSize = queue.size();
            for(int i = 0; i<curSize; i++){
                String curWord = queue.removeFirst();
                char[] curWordCharArray = curWord.toCharArray();
                for(int j = 0; j<curWord.length(); j++){
                    char origin = curWordCharArray[j];
                    for(char k = 'a'; k<='z'; k++){
                        if(k == origin){
                            continue;
                        }
                        curWordCharArray[j] = k;
                        String newWord = String.valueOf(curWordCharArray);
                        if(visited.contains(newWord)){
                            continue;
                        }
                        if(newWord.equals(endWord)){
                            return step+1;
                        }
                        if(wordSet.contains(newWord)){
                            queue.add(newWord);
                            visited.add(newWord);
                        }
                    }
                    curWordCharArray[j] = origin;
                }
            }
            step++;
        }
        return 0;
    }
}
```

