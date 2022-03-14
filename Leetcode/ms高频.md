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
