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

