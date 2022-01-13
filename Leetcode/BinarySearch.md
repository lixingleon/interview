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

