#### [128. 最长连续序列](https://leetcode-cn.com/problems/longest-consecutive-sequence/)

难度中等1154收藏分享切换为英文接收动态反馈

给定一个未排序的整数数组 `nums` ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。

请你设计并实现时间复杂度为 `O(n)` 的算法解决此问题。

 

**示例 1：**

```
输入：nums = [100,4,200,1,3,2]
输出：4
解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。
```

**示例 2：**

```
输入：nums = [0,3,7,2,5,8,4,6,0,1]
输出：9
```

 

**提示：**

- `0 <= nums.length <= 105`
- `-109 <= nums[i] <= 109`

通过次数227,293

提交次数414,814

```java
//对每个num比较num+1, num+2...在不在数组里。
//用哈希表优化判断num在数组里的时间复杂度
class Solution {
    public int longestConsecutive(int[] nums) {
        //起点和终点
        HashMap<Integer, Integer> map = new HashMap<>();
        for(int num: nums){
            map.put(num, num);
        }
        int max = 0;
        for(int num: nums){
            if(map.containsKey(num-1)){
                continue;
            }
            int right = map.get(num);
            while(map.containsKey(right+1)){
                right = map.get(right+1);
            }
            map.put(num, right);
            max = Math.max(max, right-num+1);
        }
        return max;
    }
}


```

#### [599. 两个列表的最小索引总和](https://leetcode-cn.com/problems/minimum-index-sum-of-two-lists/)

难度简单206收藏分享切换为英文接收动态反馈

假设 Andy 和 Doris 想在晚餐时选择一家餐厅，并且他们都有一个表示最喜爱餐厅的列表，每个餐厅的名字用字符串表示。

你需要帮助他们用**最少的索引和**找出他们**共同喜爱的餐厅**。 如果答案不止一个，则输出所有答案并且不考虑顺序。 你可以假设答案总是存在。

 

**示例 1:**

```
输入: list1 = ["Shogun", "Tapioca Express", "Burger King", "KFC"]，list2 = ["Piatti", "The Grill at Torrey Pines", "Hungry Hunter Steakhouse", "Shogun"]
输出: ["Shogun"]
解释: 他们唯一共同喜爱的餐厅是“Shogun”。
```

**示例 2:**

```
输入:list1 = ["Shogun", "Tapioca Express", "Burger King", "KFC"]，list2 = ["KFC", "Shogun", "Burger King"]
输出: ["Shogun"]
解释: 他们共同喜爱且具有最小索引和的餐厅是“Shogun”，它有最小的索引和1(0+1)。
```

 

**提示:**

- `1 <= list1.length, list2.length <= 1000`
- `1 <= list1[i].length, list2[i].length <= 30` 
- `list1[i]` 和 `list2[i]` 由空格 `' '` 和英文字母组成。
- `list1` 的所有字符串都是 **唯一** 的。
- `list2` 中的所有字符串都是 **唯一** 的。

通过次数65,074

提交次数114,344

```java
class Solution {
    public String[] findRestaurant(String[] list1, String[] list2) {
        if(list1.length>list2.length){
            return findRestaurant(list2, list1);
        }
        HashMap<String, Integer> map = new HashMap<>();
        int min = 2000;
        for(int i = 0; i<list1.length; i++){
            map.put(list1[i], i);
        }
        List<String> res = new ArrayList<>();
        for(int i = 0; i<list2.length; i++){
            if(i>min){
                break;
            }
            if(map.containsKey(list2[i])){
                if(map.get(list2[i])+i<min){
                    res.clear();
                    min = map.get(list2[i])+i;
                    res.add(list2[i]);
                }
                else if(map.get(list2[i])+i == min){
                    res.add(list2[i]);
                }
            }
        }
        return res.toArray(new String[res.size()]);
    }
}
```



#### [447. 回旋镖的数量](https://leetcode-cn.com/problems/number-of-boomerangs/)

难度中等231收藏分享切换为英文接收动态反馈

给定平面上 `n` 对 **互不相同** 的点 `points` ，其中 `points[i] = [xi, yi]` 。**回旋镖** 是由点 `(i, j, k)` 表示的元组 ，其中 `i` 和 `j` 之间的距离和 `i` 和 `k` 之间的欧式距离相等（**需要考虑元组的顺序**）。

返回平面上所有回旋镖的数量。

**示例 1：**

```
输入：points = [[0,0],[1,0],[2,0]]
输出：2
解释：两个回旋镖为 [[1,0],[0,0],[2,0]] 和 [[1,0],[2,0],[0,0]]
```

**示例 2：**

```
输入：points = [[1,1],[2,2],[3,3]]
输出：2
```

**示例 3：**

```
输入：points = [[1,1]]
输出：0
```

 

**提示：**

- `n == points.length`
- `1 <= n <= 500`
- `points[i].length == 2`
- `-104 <= xi, yi <= 104`
- 所有点都 **互不相同**

通过次数50,728

提交次数76,334

```java
class Solution {
    public int numberOfBoomerangs(int[][] points) {
        //1. 其实就是找跟某个点距离相等的所有点。
        //2. 与其重复计算两点之间的距离，不如把距离存下来!!!
        //3. 将各个距离的个数统计出来。
        int ans = 0;
        for(int i = 0; i<points.length; i++){
            HashMap<Integer, Integer> map = new HashMap<>();
            for(int j = 0; j<points.length; j++){
                if(i == j){
                    continue;
                }
                int x = points[i][0] - points[j][0];
                int y = points[i][1] - points[j][1];
                int dist = x*x+y*y;
                map.put(dist, map.getOrDefault(dist, 0)+1);
            }
            for(int dist: map.values()){
                ans+= dist*(dist-1);
            }
        }
        return ans;
    }
}
```

#### [720. 词典中最长的单词](https://leetcode-cn.com/problems/longest-word-in-dictionary/)

难度简单214收藏分享切换为英文接收动态反馈

给出一个字符串数组 `words` 组成的一本英语词典。返回 `words` 中最长的一个单词，该单词是由 `words` 词典中其他单词逐步添加一个字母组成。

若其中有多个可行的答案，则返回答案中字典序最小的单词。若无答案，则返回空字符串。

 

**示例 1：**

```
输入：words = ["w","wo","wor","worl", "world"]
输出："world"
解释： 单词"world"可由"w", "wo", "wor", 和 "worl"逐步添加一个字母组成。

```

**示例 2：**

```
输入：words = ["a", "banana", "app", "appl", "ap", "apply", "apple"]
输出："apple"
解释："apply" 和 "apple" 都能由词典中的单词组成。但是 "apple" 的字典序小于 "apply" 

```

 

**提示：**

- `1 <= words.length <= 1000`
- `1 <= words[i].length <= 30`
- 所有输入的字符串 `words[i]` 都只包含小写字母。

通过次数

30,453

提交次数

61,182

```java
//所有算法题用暴力都可以解决。数据结构就是用来优化暴力算法的。
//哈希表就可以将判断某值是否存在的复杂度从O(n)降低为O(1)
class Solution {
    public String longestWord(String[] words) {
        Arrays.sort(words);
        String ans = "";
        HashSet<String> set = new HashSet<>();
        set.add("");
        for(String word: words){
            String pre = word.substring(0, word.length()-1);
            if(set.contains(pre)){
                if(word.length()>ans.length()){
                    ans = word;
                }
                set.add(word);
            }       
        }
        return ans;
    }
}
```

