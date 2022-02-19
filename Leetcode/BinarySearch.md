#### [35. 搜索插入位置](https://leetcode-cn.com/problems/search-insert-position/)

难度简单1273收藏分享切换为英文接收动态反馈

给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

请必须使用时间复杂度为 `O(log n)` 的算法。

 

**示例 1:**

```
输入: nums = [1,3,5,6], target = 5
输出: 2
```

**示例 2:**

```
输入: nums = [1,3,5,6], target = 2
输出: 1
```

**示例 3:**

```
输入: nums = [1,3,5,6], target = 7
输出: 4
```

**示例 4:**

```
输入: nums = [1,3,5,6], target = 0
输出: 0
```

**示例 5:**

```
输入: nums = [1], target = 0
输出: 0
```

 

**提示:**

- `1 <= nums.length <= 104`
- `-104 <= nums[i] <= 104`
- `nums` 为**无重复元素**的**升序**排列数组
- `-104 <= target <= 104`

通过次数616,634

提交次数1,345,214

```JAVA
class Solution {
    public int searchInsert(int[] nums, int target) {
        int left = 0;
        int right = nums.length-1;
        while(left<right){
            int mid = left+(right-left)/2;
            if(nums[mid] >= target){
                right= mid;
            }
            else{
                left = mid+1;
            }
        }
        if(left == nums.length-1 && nums[left]< target){
            return nums.length;
        }
        return left;
    }
}
```



#### [34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

难度中等1391收藏分享切换为英文接收动态反馈

给定一个按照升序排列的整数数组 `nums`，和一个目标值 `target`。找出给定目标值在数组中的开始位置和结束位置。

如果数组中不存在目标值 `target`，返回 `[-1, -1]`。

**进阶：**

- 你可以设计并实现时间复杂度为 `O(log n)` 的算法解决此问题吗？

 

**示例 1：**

```
输入：nums = [5,7,7,8,8,10], target = 8
输出：[3,4]
```

**示例 2：**

```
输入：nums = [5,7,7,8,8,10], target = 6
输出：[-1,-1]
```

**示例 3：**

```
输入：nums = [], target = 0
输出：[-1,-1]
```

 

**提示：**

- `0 <= nums.length <= 105`
- `-109 <= nums[i] <= 109`
- `nums` 是一个非递减数组
- `-109 <= target <= 109`

通过次数407,534

提交次数963,783

```java
//因为left<right的模板不能确定target在不在数组中。
//所以这里先用left<=right的模板判断target是否在数组中，再用left<right的模板找到target的左右边界。
//但这么做有点多此一举。
//我们可以声明一个{-1，-1}数组，在left<right模板最后比较nums[left]和target，如果不相等，说明数组中不存在target。那我们直接返回之前声明的数组即可。
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int left = 0;
        int right = nums.length-1;
        while(left<=right){
            int mid = left+(right-left)/2;
            if(nums[mid] == target){
                break;
            }
            else if (nums[mid]<target){
                left = mid+1;
            }
            else{
                right = mid-1;
            }
        }
        if(left>right){
           
            return new int[]{-1,-1};
        }
        left = 0;
        right = nums.length-1;
        while(left<right){
            int mid = left+(right-left)/2;
            if(nums[mid]>=target){
                right = mid;
            }
            else{
                left = mid+1;
            }
        }
        int start = left;
        left = 0;
        right = nums.length-1;
        while(left<right){
            int mid = left+(right-left+1)/2;
            if(nums[mid]<=target){
                left = mid;
            }
            else{
                right = mid-1;
            }
        }
        int end = left;
        return new int[]{start, end};

    }
}

//改进版
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int left = 0;
        int right = nums.length-1;
        int[] ans = new int[]{-1,-1};
        if(nums.length<1){
            return ans;
        }
        while(left<right){
            int mid = left+(right-left)/2;
            if(nums[mid]>=target){
                right = mid;
            }
            else{
                left = mid+1;
            }
        }
        if(nums[left]!= target){
            return ans;
        }
        int start = left;
        left = 0;
        right = nums.length-1;
        while(left<right){
            int mid = left+(right-left+1)/2;
            if(nums[mid]<=target){
                left = mid;
            }
            else{
                right = mid-1;
            }
        }
        int end = left;
        return new int[]{start, end};

    }
}
```

#### [704. 二分查找](https://leetcode-cn.com/problems/binary-search/)

难度简单572收藏分享切换为英文接收动态反馈

给定一个 `n` 个元素有序的（升序）整型数组 `nums` 和一个目标值 `target` ，写一个函数搜索 `nums` 中的 `target`，如果目标值存在返回下标，否则返回 `-1`。


**示例 1:**

```
输入: nums = [-1,0,3,5,9,12], target = 9
输出: 4
解释: 9 出现在 nums 中并且下标为 4
```

**示例 2:**

```
输入: nums = [-1,0,3,5,9,12], target = 2
输出: -1
解释: 2 不存在 nums 中因此返回 -1
```

 

**提示：**

1. 你可以假设 `nums` 中的所有元素是不重复的。
2. `n` 将在 `[1, 10000]`之间。
3. `nums` 的每个元素都将在 `[-9999, 9999]`之间。

通过次数392,703

提交次数717,432

```java
class Solution {
    public int search(int[] nums, int target) {
        return find(nums, 0, nums.length-1, target);
    }
    private int find(int[] nums, int left, int right, int target){
        if(left>right){
            return -1;
        }
        int mid = left+(right-left)/2;
        if(nums[mid] == target){
            return mid;
        }
        if(target>nums[mid]){
            return find(nums, mid+1, right, target);
        }
        else{
            return find(nums, left, mid-1, target);
        }
    }
}
```



#### [981. 基于时间的键值存储](https://leetcode-cn.com/problems/time-based-key-value-store/)

难度中等154收藏分享切换为英文接收动态反馈

设计一个基于时间的键值数据结构，该结构可以在不同时间戳存储对应同一个键的多个值，并针对特定时间戳检索键对应的值。

实现 `TimeMap` 类：

- `TimeMap()` 初始化数据结构对象

- `void set(String key, String value, int timestamp)` 存储键 `key`、值 `value`，以及给定的时间戳 `timestamp`。

- ```
  String get(String key, int timestamp)
  ```

  - 返回先前调用 `set(key, value, timestamp_prev)` 所存储的值，其中 `timestamp_prev <= timestamp` 。
  - 如果有多个这样的值，则返回对应最大的 `timestamp_prev` 的那个值。
  - 如果没有值，则返回空字符串（`""`）。

**示例：**

```
输入：
["TimeMap", "set", "get", "get", "set", "get", "get"]
[[], ["foo", "bar", 1], ["foo", 1], ["foo", 3], ["foo", "bar2", 4], ["foo", 4], ["foo", 5]]
输出：
[null, null, "bar", "bar", null, "bar2", "bar2"]

解释：
TimeMap timeMap = new TimeMap();
timeMap.set("foo", "bar", 1);  // 存储键 "foo" 和值 "bar" ，时间戳 timestamp = 1   
timeMap.get("foo", 1);         // 返回 "bar"
timeMap.get("foo", 3);         // 返回 "bar", 因为在时间戳 3 和时间戳 2 处没有对应 "foo" 的值，所以唯一的值位于时间戳 1 处（即 "bar"） 。
timeMap.set("foo", "bar2", 4); // 存储键 "foo" 和值 "bar2" ，时间戳 timestamp = 4  
timeMap.get("foo", 4);         // 返回 "bar2"
timeMap.get("foo", 5);         // 返回 "bar2"
```

 

**提示：**

- `1 <= key.length, value.length <= 100`
- `key` 和 `value` 由小写英文字母和数字组成
- `1 <= timestamp <= 107`
- `set` 操作中的时间戳 `timestamp` 都是严格递增的
- 最多调用 `set` 和 `get` 操作 `2 * 105` 次



```java
T
class TimeMap {
//对于一个key，按timestamp从大到小的顺序遍历value
//可以在外层用hashmap，内层嵌套TreeMap。相当于双主键。
//也可以建一个 string， List 的hashmap
    HashMap<String, TreeMap<Integer, String>> map;
    public TimeMap() {
        map = new HashMap<>();
    }
    
    public void set(String key, String value, int timestamp) {
        TreeMap<Integer, String> treemap = map.get(key);
        if(treemap == null){
            treemap = new TreeMap<>();
        }
        treemap.put(timestamp, value);
        map.put(key, treemap);
    }
    
    public String get(String key, int timestamp) {
        TreeMap<Integer, String> treemap = map.get(key);
        if(treemap == null){
            return "";
        }
        Map.Entry<Integer, String> res = treemap.floorEntry(timestamp);
        return res == null? "": res.getValue();
    }
}

/**
 * Your TimeMap object will be instantiated and called as such:
 * TimeMap obj = new TimeMap();
 * obj.set(key,value,timestamp);
 * String param_2 = obj.get(key,timestamp);
 */


二分法
class TimeMap {
//对于一个key，按timestamp从大到小的顺序遍历value
//可以在外层用hashmap，内层嵌套TreeMap。相当于双主键。
//也可以建一个 string， List<Node> 的hashmap
//因为timestamp是严格递增的，所以每个list都是单调排列的，所以可以用二分查找speed up
class Node {
    int timestamp;
    String value;
    public Node(int timestamp, String value){
        this.timestamp = timestamp;
        this.value = value;
    }
}

    HashMap<String, List<Node>> map;
    public TimeMap() {
        map = new HashMap<>();
    }
    
    public void set(String key, String value, int timestamp) {
        List<Node> list = map.get(key);
        if(list == null){
            list = new ArrayList<>();
        }
        list.add(new Node(timestamp, value));
        map.put(key, list);
    }
    
    public String get(String key, int timestamp) {
        List<Node> list = map.get(key);
        if(list == null){
            return "";
        }
        return binarySearch(timestamp, list);
    }
    private String binarySearch(int timestamp, List<Node> list){
        int size = list.size();
        int left = 0;
        int right = size-1;
        while(left<right){
            int mid = (right-left+1)/2+left;
            if(timestamp>=list.get(mid).timestamp){
                left = mid;
            }
            else{
                right = mid-1;
            }
        }
        //当二分结束后， 判断left指向的元素是否符合题意。
        //这一点很精妙。
        if(list.get(left).timestamp<= timestamp){
            return list.get(left).value;
        }
        return "";
    }
}

/**
 * Your TimeMap object will be instantiated and called as such:
 * TimeMap obj = new TimeMap();
 * obj.set(key,value,timestamp);
 * String param_2 = obj.get(key,timestamp);
 */
```



#### [33. 搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)

难度中等1786收藏分享切换为英文接收动态反馈

整数数组 `nums` 按升序排列，数组中的值 **互不相同** 。

在传递给函数之前，`nums` 在预先未知的某个下标 `k`（`0 <= k < nums.length`）上进行了 **旋转**，使数组变为 `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]`（下标 **从 0 开始** 计数）。例如， `[0,1,2,4,5,6,7]` 在下标 `3` 处经旋转后可能变为 `[4,5,6,7,0,1,2]` 。

给你 **旋转后** 的数组 `nums` 和一个整数 `target` ，如果 `nums` 中存在这个目标值 `target` ，则返回它的下标，否则返回 `-1` 。

 

**示例 1：**

```
输入：nums = [4,5,6,7,0,1,2], target = 0
输出：4
```

**示例 2：**

```
输入：nums = [4,5,6,7,0,1,2], target = 3
输出：-1
```

**示例 3：**

```
输入：nums = [1], target = 0
输出：-1
```

 

**提示：**

- `1 <= nums.length <= 5000`
- `-10^4 <= nums[i] <= 10^4`
- `nums` 中的每个值都 **独一无二**
- 题目数据保证 `nums` 在预先未知的某个下标上进行了旋转
- `-10^4 <= target <= 10^4`

 

**进阶：**你可以设计一个时间复杂度为 `O(log n)` 的解决方案吗？



```java
每次消除一半。通过比较nums[left]和nums[mid]，确定哪一半是有序的。
class Solution {
    public int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length-1;
        while(left<=right){
            int mid = (right-left)/2+left;
            if(target == nums[mid]){
                return mid;
            }
            if(nums[mid]>=nums[left]){
                //left到mid递增，旋转点在mid右边
                if(target<nums[mid] && target>= nums[left]){
                    right = mid-1;
                }
                else{
                    left = mid+1;
                }
            }
            else{
                if(target>nums[mid]&& target<= nums[right]){
                    left = mid+1;
                }
                else{
                    right = mid-1;
                }
            }
        }
        return -1;
    }
}
```



#### [81. 搜索旋转排序数组 II](https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii/)

难度中等535收藏分享切换为英文接收动态反馈

已知存在一个按非降序排列的整数数组 `nums` ，数组中的值不必互不相同。

在传递给函数之前，`nums` 在预先未知的某个下标 `k`（`0 <= k < nums.length`）上进行了 **旋转** ，使数组变为 `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]`（下标 **从 0 开始** 计数）。例如， `[0,1,2,4,4,4,5,6,6,7]` 在下标 `5` 处经旋转后可能变为 `[4,5,6,6,7,0,1,2,4,4]` 。

给你 **旋转后** 的数组 `nums` 和一个整数 `target` ，请你编写一个函数来判断给定的目标值是否存在于数组中。如果 `nums` 中存在这个目标值 `target` ，则返回 `true` ，否则返回 `false` 。

 

**示例 1：**

```
输入：nums = [2,5,6,0,0,1,2], target = 0
输出：true
```

**示例 2：**

```
输入：nums = [2,5,6,0,0,1,2], target = 3
输出：false
```

 

**提示：**

- `1 <= nums.length <= 5000`
- `-104 <= nums[i] <= 104`
- 题目数据保证 `nums` 在预先未知的某个下标上进行了旋转
- `-104 <= target <= 104`

 

**进阶：**

- 这是 [搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/description/) 的延伸题目，本题中的 `nums` 可能包含重复元素。
- 这会影响到程序的时间复杂度吗？会有怎样的影响，为什么？



```java
class Solution {
    public boolean search(int[] nums, int target) {
        /*
        如果nums[mid]>nums[left] 那left到mid是有序的
        如果nums[mid]<nums[left] 那mid到right是有序的
         */

        int left = 0;
        int right = nums.length-1;
        while(left<=right){
            int mid = (right-left)/2+left;
            if(nums[mid] == target){
                return true;
            }
            if(nums[mid]>nums[left]){
                if(target>=nums[left] && target< nums[mid]){
                    right = mid-1;
                }
                else{
                    left = mid+1;
                }
            }
            else if(nums[mid]<nums[left]){
                if(target>nums[mid]&& target<= nums[right]){
                    left = mid+1;
                }
                else{
                    right = mid-1;
                }
            }
            //当nums[mid] == nums[left]时，mid左边有可能是有序的，mid右边也有可能是有序的，无法确定舍弃哪一边。但知道的是nums[left]不是target，所以可以让left++，排除一个错误答案。
            else{
                left++;
            }
        }
        return false;
    }
}
```



#### [153. 寻找旋转排序数组中的最小值](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/)

难度中等650收藏分享切换为英文接收动态反馈

已知一个长度为 `n` 的数组，预先按照升序排列，经由 `1` 到 `n` 次 **旋转** 后，得到输入数组。例如，原数组 `nums = [0,1,2,4,5,6,7]` 在变化后可能得到：

- 若旋转 `4` 次，则可以得到 `[4,5,6,7,0,1,2]`
- 若旋转 `7` 次，则可以得到 `[0,1,2,4,5,6,7]`

注意，数组 `[a[0], a[1], a[2], ..., a[n-1]]` **旋转一次** 的结果为数组 `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]` 。

给你一个元素值 **互不相同** 的数组 `nums` ，它原来是一个升序排列的数组，并按上述情形进行了多次旋转。请你找出并返回数组中的 **最小元素** 。

 

**示例 1：**

```
输入：nums = [3,4,5,1,2]
输出：1
解释：原数组为 [1,2,3,4,5] ，旋转 3 次得到输入数组。
```

**示例 2：**

```
输入：nums = [4,5,6,7,0,1,2]
输出：0
解释：原数组为 [0,1,2,4,5,6,7] ，旋转 4 次得到输入数组。
```

**示例 3：**

```
输入：nums = [11,13,15,17]
输出：11
解释：原数组为 [11,13,15,17] ，旋转 4 次得到输入数组。
```

 

**提示：**

- `n == nums.length`
- `1 <= n <= 5000`
- `-5000 <= nums[i] <= 5000`
- `nums` 中的所有整数 **互不相同**
- `nums` 原来是一个升序排序的数组，并进行了 `1` 至 `n` 次旋转

通过次数240,015

提交次数422,095

```java
//比较nums[mid]和nums[right]
class Solution {
    public int findMin(int[] nums) {
        int left = 0;
        int right = nums.length-1;
        while(left<right){
            int mid = (right-left)/2+left;
            if(nums[mid]>nums[right]){
                //mid左边有序
                left = mid+1;
            }
            else{
                right = mid;
            }
        }
        return nums[left];
    }
}
```

#### [154. 寻找旋转排序数组中的最小值 II](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii/)

难度困难441收藏分享切换为英文接收动态反馈

已知一个长度为 `n` 的数组，预先按照升序排列，经由 `1` 到 `n` 次 **旋转** 后，得到输入数组。例如，原数组 `nums = [0,1,4,4,5,6,7]` 在变化后可能得到：

- 若旋转 `4` 次，则可以得到 `[4,5,6,7,0,1,4]`
- 若旋转 `7` 次，则可以得到 `[0,1,4,4,5,6,7]`

注意，数组 `[a[0], a[1], a[2], ..., a[n-1]]` **旋转一次** 的结果为数组 `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]` 。

给你一个可能存在 **重复** 元素值的数组 `nums` ，它原来是一个升序排列的数组，并按上述情形进行了多次旋转。请你找出并返回数组中的 **最小元素** 。

 

**示例 1：**

```
输入：nums = [1,3,5]
输出：1
```

**示例 2：**

```
输入：nums = [2,2,2,0,1]
输出：0
```

 

**提示：**

- `n == nums.length`
- `1 <= n <= 5000`
- `-5000 <= nums[i] <= 5000`
- `nums` 原来是一个升序排序的数组，并进行了 `1` 至 `n` 次旋转

 

**进阶：**

- 这道题是 [寻找旋转排序数组中的最小值](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/description/) 的延伸题目。
- 允许重复会影响算法的时间复杂度吗？会如何影响，为什么？

通过次数123,940

提交次数233,563



```java
class Solution {
    public int findMin(int[] nums) {
        int left = 0;
        int right = nums.length-1;
        while(left<right){
            int mid = (right-left)/2+left;
            if(nums[mid]>nums[right]){
                //mid左边有序
                left = mid+1;
            }
            else if(nums[mid]<nums[right]){
                right = mid;
            }
            else{
                right--;
            }
        }
        return nums[left];
    }
}
```

#### [162. 寻找峰值](https://leetcode-cn.com/problems/find-peak-element/)

难度中等687收藏分享切换为英文接收动态反馈

峰值元素是指其值严格大于左右相邻值的元素。

给你一个整数数组 `nums`，找到峰值元素并返回其索引。数组可能包含多个峰值，在这种情况下，返回 **任何一个峰值** 所在位置即可。

你可以假设 `nums[-1] = nums[n] = -∞` 。

你必须实现时间复杂度为 `O(log n)` 的算法来解决此问题。

 

**示例 1：**

```
输入：nums = [1,2,3,1]
输出：2
解释：3 是峰值元素，你的函数应该返回其索引 2。
```

**示例 2：**

```
输入：nums = [1,2,1,3,5,6,4]
输出：1 或 5 
解释：你的函数可以返回索引 1，其峰值元素为 2；
     或者返回索引 5， 其峰值元素为 6。
```

 

**提示：**

- `1 <= nums.length <= 1000`
- `-231 <= nums[i] <= 231 - 1`
- 对于所有有效的 `i` 都有 `nums[i] != nums[i + 1]`

通过次数184,926

提交次数372,707



```java
class Solution {
    public int findPeakElement(int[] nums) {
        int left = 0;
        int right = nums.length-1;
        while(left<right){
            int mid = (right-left)/2+left;
            if(nums[mid]<=nums[mid+1]){
                left = mid+1;
            }
            else{
                right = mid;
            }
        }
        return left;
    }
}
```



#### [852. 山脉数组的峰顶索引](https://leetcode-cn.com/problems/peak-index-in-a-mountain-array/)

难度简单225

符合下列属性的数组 `arr` 称为 **山脉数组** ：

- `arr.length >= 3`

- 存在

   

  ```
  i
  ```

  （

  ```
  0 < i < arr.length - 1
  ```

  ）使得：

  - `arr[0] < arr[1] < ... arr[i-1] < arr[i]`
  - `arr[i] > arr[i+1] > ... > arr[arr.length - 1]`

给你由整数组成的山脉数组 `arr` ，返回任何满足 `arr[0] < arr[1] < ... arr[i - 1] < arr[i] > arr[i + 1] > ... > arr[arr.length - 1]` 的下标 `i` 。

 

**示例 1：**

```
输入：arr = [0,1,0]
输出：1
```

**示例 2：**

```
输入：arr = [0,2,1,0]
输出：1
```

**示例 3：**

```
输入：arr = [0,10,5,2]
输出：1
```

**示例 4：**

```
输入：arr = [3,4,5,1]
输出：2
```

**示例 5：**

```
输入：arr = [24,69,100,99,79,78,67,36,26,19]
输出：2
```

 

**提示：**

- `3 <= arr.length <= 104`
- `0 <= arr[i] <= 106`
- 题目数据保证 `arr` 是一个山脉数组

 

**进阶：**很容易想到时间复杂度 `O(n)` 的解决方案，你可以设计一个 `O(log(n))` 的解决方案吗？



```java
class Solution {
    public int peakIndexInMountainArray(int[] arr) {
        int left = 0;
        int right = arr.length-1;
        while(left<right){
            int mid = (right-left)/2+left;
            if(arr[mid]<arr[mid+1]){
                left = mid+1;
            }
            else{
                right = mid;
            }
        }
        return left;
    }
}
```

#### [69. Sqrt(x)](https://leetcode-cn.com/problems/sqrtx/)

难度简单870收藏分享切换为英文接收动态反馈

给你一个非负整数 `x` ，计算并返回 `x` 的 **算术平方根** 。

由于返回类型是整数，结果只保留 **整数部分** ，小数部分将被 **舍去 。**

**注意：**不允许使用任何内置指数函数和算符，例如 `pow(x, 0.5)` 或者 `x ** 0.5` 。

 

**示例 1：**

```
输入：x = 4
输出：2
```

**示例 2：**

```
输入：x = 8
输出：2
解释：8 的算术平方根是 2.82842..., 由于返回类型是整数，小数部分将被舍去。
```

 

**提示：**

- `0 <= x <= 231 - 1`

```java
class Solution {
    public int mySqrt(int x) {
        if(x<=1){
            return x;
        }
        int left = 0;
        int right = x/2;
        while(left<right){
            int mid = (right-left+1)/2+left;
            if(x/mid<mid){
                right = mid-1;
            }
            else{
                left = mid;
            }
        }
        return left;
    }
}
```

#### [875. 爱吃香蕉的珂珂](https://leetcode-cn.com/problems/koko-eating-bananas/)

难度中等246收藏分享切换为英文接收动态反馈

珂珂喜欢吃香蕉。这里有 `N` 堆香蕉，第 `i` 堆中有 `piles[i]` 根香蕉。警卫已经离开了，将在 `H` 小时后回来。

珂珂可以决定她吃香蕉的速度 `K` （单位：根/小时）。每个小时，她将会选择一堆香蕉，从中吃掉 `K` 根。如果这堆香蕉少于 `K` 根，她将吃掉这堆的所有香蕉，然后这一小时内不会再吃更多的香蕉。 

珂珂喜欢慢慢吃，但仍然想在警卫回来前吃掉所有的香蕉。

返回她可以在 `H` 小时内吃掉所有香蕉的最小速度 `K`（`K` 为整数）。

 



**示例 1：**

```
输入: piles = [3,6,7,11], H = 8
输出: 4
```

**示例 2：**

```
输入: piles = [30,11,23,4,20], H = 5
输出: 30
```

**示例 3：**

```
输入: piles = [30,11,23,4,20], H = 6
输出: 23
```

 

**提示：**

- `1 <= piles.length <= 10^4`
- `piles.length <= H <= 10^9`
- `1 <= piles[i] <= 10^9`

通过次数55,139

提交次数113,867

**Java向上取整**

```java
Java里向上取整：
    a/b+(a%b == 0?0:1)
class Solution {
    public int minEatingSpeed(int[] piles, int h) {
        int max = Integer.MIN_VALUE;
        for(int num: piles){
            max = Math.max(max, num);
        }
        int left = 1, right = max;
        while(left<right){
            int mid = (right-left)/2+left;
            if(canFinish(piles, mid, h)){
                right = mid;
            }
            else{
                left = mid+1;
            }
        }
        return left;
    }
    private boolean canFinish(int[] piles, int speed, int h){
        for(int num: piles){
            h-= num/speed+ (num%speed == 0? 0:1);
            if(h<0){
               return false;
            }

        }
        return true;
    }
}
```

#### [1011. 在 D 天内送达包裹的能力](https://leetcode-cn.com/problems/capacity-to-ship-packages-within-d-days/)

难度中等425收藏分享切换为英文接收动态反馈

传送带上的包裹必须在 `days` 天内从一个港口运送到另一个港口。

传送带上的第 `i` 个包裹的重量为 `weights[i]`。每一天，我们都会按给出重量（`weights`）的顺序往传送带上装载包裹。我们装载的重量不会超过船的最大运载重量。

返回能在 `days` 天内将传送带上的所有包裹送达的船的最低运载能力。

 

**示例 1：**

```
输入：weights = [1,2,3,4,5,6,7,8,9,10], days = 5
输出：15
解释：
船舶最低载重 15 就能够在 5 天内送达所有包裹，如下所示：
第 1 天：1, 2, 3, 4, 5
第 2 天：6, 7
第 3 天：8
第 4 天：9
第 5 天：10

请注意，货物必须按照给定的顺序装运，因此使用载重能力为 14 的船舶并将包装分成 (2, 3, 4, 5), (1, 6, 7), (8), (9), (10) 是不允许的。 
```

**示例 2：**

```
输入：weights = [3,2,2,4,1,4], days = 3
输出：6
解释：
船舶最低载重 6 就能够在 3 天内送达所有包裹，如下所示：
第 1 天：3, 2
第 2 天：2, 4
第 3 天：1, 4
```

**示例 3：**

```
输入：weights = [1,2,3,1,1], D = 4
输出：3
解释：
第 1 天：1
第 2 天：2
第 3 天：3
第 4 天：1, 1
```

 

**提示：**

- `1 <= days <= weights.length <= 5 * 104`
- `1 <= weights[i] <= 500`

通过次数66,301

提交次数106,082

```java
class Solution {
    public int shipWithinDays(int[] weights, int days) {
        int sum = 0;
        int min = Integer.MAX_VALUE;
        for(int weight: weights){
            sum += weight;
            min = Math.min(min, weight);
        }
        int left = min;
        int right = sum;
        while(left< right){
            int mid = (right-left)/2+left;
            if(canDO(weights, days, mid)){
                right = mid;
            }
            else{
                left = mid+1;
            }
        }
        return left;
    }
    private boolean canDO(int[] weights, int days, int capacity){
        int idx= 0;
        for(int i = 0; i<days; i++){
            int tmp = capacity;
            while(idx<weights.length&& tmp- weights[idx]>=0){
                tmp-= weights[idx];
                idx++;

                if(idx>=weights.length){
                return true;
                }
            }
            
        }
        return false;
    }
}
```

#### [4. 寻找两个正序数组的中位数](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/)

难度困难4924收藏分享切换为英文接收动态反馈

给定两个大小分别为 `m` 和 `n` 的正序（从小到大）数组 `nums1` 和 `nums2`。请你找出并返回这两个正序数组的 **中位数** 。

算法的时间复杂度应该为 `O(log (m+n))` 。

 

**示例 1：**

```
输入：nums1 = [1,3], nums2 = [2]
输出：2.00000
解释：合并数组 = [1,2,3] ，中位数 2
```

**示例 2：**

```
输入：nums1 = [1,2], nums2 = [3,4]
输出：2.50000
解释：合并数组 = [1,2,3,4] ，中位数 (2 + 3) / 2 = 2.5
```

**示例 3：**

```
输入：nums1 = [0,0], nums2 = [0,0]
输出：0.00000
```

**示例 4：**

```
输入：nums1 = [], nums2 = [1]
输出：1.00000
```

**示例 5：**

```
输入：nums1 = [2], nums2 = []
输出：2.00000
```

 

**提示：**

- `nums1.length == m`
- `nums2.length == n`
- `0 <= m <= 1000`
- `0 <= n <= 1000`
- `1 <= m + n <= 2000`
- `-106 <= nums1[i], nums2[i] <= 106`



```java

class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int len1 = nums1.length;
        int len2 = nums2.length;
        return (findKth(nums1, 0, nums2, 0, (len1+len2+1)/2)+findKth(nums1, 0, nums2, 0, (len1+len2+2)/2))/2.0;
    }
    private int findKth(int[] nums1, int idx1, int[] nums2, int idx2, int k){
        if(idx1>=nums1.length){
            return nums2[idx2+k-1];
        }
        if(idx2>=nums2.length){
            return nums1[idx1+k-1];
        }
        if(k == 1){
            return Math.min(nums1[idx1], nums2[idx2]);
        }
        int half_kth_1 = idx1+k/2-1>=nums1.length? Integer.MAX_VALUE: nums1[idx1+k/2-1];
        int half_kth_2 = idx2+k/2-1>=nums2.length? Integer.MAX_VALUE: nums2[idx2+k/2-1];
        if(half_kth_1<half_kth_2){
            idx1 += k/2;
            return findKth(nums1, idx1, nums2, idx2, k-k/2);
        }
        else{
            idx2 += k/2;
            return findKth(nums1, idx1, nums2, idx2, k-k/2);
        }
    }
}
```

#### [378. 有序矩阵中第 K 小的元素](https://leetcode-cn.com/problems/kth-smallest-element-in-a-sorted-matrix/)

难度中等747收藏分享切换为英文接收动态反馈

给你一个 `n x n` 矩阵 `matrix` ，其中每行和每列元素均按升序排序，找到矩阵中第 `k` 小的元素。
请注意，它是 **排序后** 的第 `k` 小元素，而不是第 `k` 个 **不同** 的元素。

 

**示例 1：**

```
输入：matrix = [[1,5,9],[10,11,13],[12,13,15]], k = 8
输出：13
解释：矩阵中的元素为 [1,5,9,10,11,12,13,13,15]，第 8 小元素是 13
```

**示例 2：**

```
输入：matrix = [[-5]], k = 1
输出：-5
```

 

**提示：**

- `n == matrix.length`
- `n == matrix[i].length`
- `1 <= n <= 300`
- `-109 <= matrix[i][j] <= 109`
- 题目数据 **保证** `matrix` 中的所有行和列都按 **非递减顺序** 排列
- `1 <= k <= n2`

通过次数89,898

提交次数142,363



```java
//判断二分法怎么调整left和right指针？
//用另一个函数计算小于等于mid的数的个数
class Solution {
    int m;
    int n;
    int[][] matrix;
    public int kthSmallest(int[][] matrix, int k) {
        m = matrix.length;
        n = matrix[0].length;
        this.matrix = matrix;
        int left = matrix[0][0];
        int right = matrix[m-1][n-1];
        while(left<right){
            int mid = (right-left)/2+left;
            if(lessEqualNum(mid)< k){
                left = mid+1;
            }
            else{
                right = mid;
            }
        }
        return right;
    }
    //找到小于等于target的元素个数。
    private int lessEqualNum(int target){
        int i = m-1;
        int j = 0;
        int count = 0;
        while(i>=0 && j<n){
            while(i>=0 && matrix[i][j]>target){
                i--;
            }
            count+= i+1;
            j++;
        }
        return count;
    }

}
```



#### [668. 乘法表中第k小的数](https://leetcode-cn.com/problems/kth-smallest-number-in-multiplication-table/)

难度困难169收藏分享切换为英文接收动态反馈

几乎每一个人都用 [乘法表](https://baike.baidu.com/item/乘法表)。但是你能在乘法表中快速找到第`k`小的数字吗？

给定高度`m` 、宽度`n` 的一张 `m * n`的乘法表，以及正整数`k`，你需要返回表中第`k` 小的数字。

**例 1：**

```
输入: m = 3, n = 3, k = 5
输出: 3
解释: 
乘法表:
1	2	3
2	4	6
3	6	9

第5小的数字是 3 (1, 2, 2, 3, 3).
```

**例 2：**

```
输入: m = 2, n = 3, k = 6
输出: 6
解释: 
乘法表:
1	2	3
2	4	6

第6小的数字是 6 (1, 2, 2, 3, 4, 6).
```

**注意：**

1. `m` 和 `n` 的范围在 [1, 30000] 之间。
2. `k` 的范围在 [1, m * n] 之间。

```java
class Solution {
    int m;
    int n;
    public int findKthNumber(int m, int n, int k) {
        this.m = m;
        this.n = n;
        int left = 1;
        int right = m*n;
        while(left<right){
            int mid = (right-left)/2+left;
            if(lessEqualCount(mid)<k){
                left = mid+1;
            }
            else{
                right = mid;
            }
        }
        return right;
    }
    此处相对于378题，有一个小小的优化：
    i代表行数, target/i就是这一行小于等于target的元素数量。
    private int lessEqualCount(int target){
        int count = 0;
        for(int i = 1; i<=m; i++){
            count+= Math.min(target/i, n);
        }
        return count;
    }
}
```



#### [719. 找出第 k 小的距离对](https://leetcode-cn.com/problems/find-k-th-smallest-pair-distance/)

难度困难222收藏分享切换为英文接收动态反馈

给定一个整数数组，返回所有数对之间的第 k 个最小**距离**。一对 (A, B) 的距离被定义为 A 和 B 之间的绝对差值。

**示例 1:**

```
输入：
nums = [1,3,1]
k = 1
输出：0 
解释：
所有数对如下：
(1,3) -> 2
(1,1) -> 0
(3,1) -> 2
因此第 1 个最小距离的数对是 (1,1)，它们之间的距离为 0。
```

**提示:**

1. `2 <= len(nums) <= 10000`.
2. `0 <= nums[i] < 1000000`.
3. `1 <= k <= len(nums) * (len(nums) - 1) / 2`.



```java
class Solution {
    public int smallestDistancePair(int[] nums, int k) {
        Arrays.sort(nums);
        int n = nums.length;
        int right = nums[n-1]-nums[0];
        //这里left应该是0而不是nums[1]-nums[0]
        //因为nums[1]和nums[0]最小不代表它们的差最小。
        int left = 0;
        while(left<right){
            int mid = (right-left)/2+left;
            if(lessEqualCount(mid, nums)<k){
                left = mid+1;
            }
            else{
                right = mid;
            }
        }
        return right;
    }
    //双指针法：找到一个数组中有多少个距离对.
    private int lessEqualCount(int target, int[] nums){
        int i = 0;
        int j = 1;
        int count = 0;
        while(i<j && j<nums.length){
            while(i<j && nums[j]-nums[i]>target){
                i++;
            }
            count+= j-i;
            j++;
        }
        return count;
    }
}
```

#### [786. 第 K 个最小的素数分数](https://leetcode-cn.com/problems/k-th-smallest-prime-fraction/)

难度困难206收藏分享切换为英文接收动态反馈

给你一个按递增顺序排序的数组 `arr` 和一个整数 `k` 。数组 `arr` 由 `1` 和若干 **素数** 组成，且其中所有整数互不相同。

对于每对满足 `0 <= i < j < arr.length` 的 `i` 和 `j` ，可以得到分数 `arr[i] / arr[j]` 。

那么第 `k` 个最小的分数是多少呢? 以长度为 `2` 的整数数组返回你的答案, 这里 `answer[0] == arr[i]` 且 `answer[1] == arr[j]` 。

**示例 1：**

```
输入：arr = [1,2,3,5], k = 3
输出：[2,5]
解释：已构造好的分数,排序后如下所示: 
1/5, 1/3, 2/5, 1/2, 3/5, 2/3
很明显第三个最小的分数是 2/5
```

**示例 2：**

```
输入：arr = [1,7], k = 1
输出：[1,7]
```

 

**提示：**

- `2 <= arr.length <= 1000`
- `1 <= arr[i] <= 3 * 104`
- `arr[0] == 1`
- `arr[i]` 是一个 **素数** ，`i > 0`
- `arr` 中的所有数字 **互不相同** ，且按 **严格递增** 排序
- `1 <= k <= arr.length * (arr.length - 1) / 2`



```java
通过优先队列来做多路合并。
小顶堆，每次取的都是最小的值，第k次取的就是第k小的值了。
二分暂时
class Solution {
    public int[] kthSmallestPrimeFraction(int[] arr, int k) {
        PriorityQueue<int[]> queue = new PriorityQueue<>((e1, e2) ->{
            return Double.compare(arr[e1[0]]*1.0/arr[e1[1]], arr[e2[0]]*1.0/arr[e2[1]]);
        });
        for(int i = 1; i<arr.length; i++){
            queue.add(new int[]{0, i});
        }
        while(k>=2){
            int[] smallest = queue.poll();
            if(smallest[0]+1<smallest[1]){
                queue.add(new int[]{smallest[0]+1, smallest[1]});
            }
            k--;
        }
        int[] res = queue.peek();
        return new int[]{arr[res[0]], arr[res[1]]};
    }
}
```

