# *号表示为题目较长，需要理解题意的题，先标记，之后再回来看。

#### [11. 盛最多水的容器](https://leetcode-cn.com/problems/container-with-most-water/)

难度中等3081收藏分享切换为英文接收动态反馈

给你 `n` 个非负整数 `a1，a2，...，a``n`，每个数代表坐标中的一个点 `(i, ai)` 。在坐标内画 `n` 条垂直线，垂直线 `i` 的两个端点分别为 `(i, ai)` 和 `(i, 0)` 。找出其中的两条线，使得它们与 `x` 轴共同构成的容器可以容纳最多的水。

**说明：**你不能倾斜容器。

 

**示例 1：**

![img](https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/07/25/question_11.jpg)

```
输入：[1,8,6,2,5,4,8,3,7]
输出：49 
解释：图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。
```

**示例 2：**

```
输入：height = [1,1]
输出：1
```

**示例 3：**

```
输入：height = [4,3,2,1,4]
输出：16
```

**示例 4：**

```
输入：height = [1,2,1]
输出：2
```

 

**提示：**

- `n == height.length`
- `2 <= n <= 105`
- `0 <= height[i] <= 104`

通过次数601,585

提交次数967,350



```java
class Solution {
    public int maxArea(int[] height) {
        int left = 0, right = height.length-1;
        int max = 0;
        while(left<right){
            int capacity = (right-left)*Math.min(height[left], height[right]);
            max = Math.max(max, capacity);
            if(height[left]<=height[right]){
                left++;
            }
            else{
                right--;
            }
        }
        return max;
    }
}
```

#### [42. 接雨水](https://leetcode-cn.com/problems/trapping-rain-water/)

难度困难2985收藏分享切换为英文接收动态反馈

给定 `n` 个非负整数表示每个宽度为 `1` 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

 

示例 1：

**每个位置能存的水和它左边与右边的最大高度有关。**

**提前把这两个值确定好，就很方便了。**

```
输入：height = [0,1,0,2,1,0,1,3,2,1,2,1]
输出：6
解释：上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。 
```

**示例 2：**

```
输入：height = [4,2,0,3,2,5]
输出：9
```

 

**提示：**

- `n == height.length`
- `1 <= n <= 2 * 104`
- `0 <= height[i] <= 105`

通过次数375,007

提交次数633,871

```java
class Solution {
    public int trap(int[] height) {
        int[] leftMax = new int[height.length];
        int[] rightMax = new int[height.length];
        int num = 0;
        leftMax[0] = height[0];
        for(int i = 1; i<height.length; i++){
            leftMax[i] = Math.max(leftMax[i-1], height[i-1]);
        }
        rightMax[height.length-1] = height[height.length-1];
        for(int i = height.length-2; i>=0; i--){
            rightMax[i] = Math.max(rightMax[i+1], height[i+1]);
        }

        for(int i = 1; i<height.length-1; i++){
            int shorter = Math.min(leftMax[i], rightMax[i]);
            if(shorter>height[i]){
                num+= shorter-height[i];
            }
        }
        return num;
    }
}
//y减少空间的使用。从O(n)降到O(1)

class Solution {
    public int trap(int[] height) {
        int leftMax = 0;
        int rightMax = 0;
        int left = 0;
        int right = height.length-1;
        int num = 0;
        while(left<=right){
            if(leftMax< rightMax){
                if(leftMax>height[left]){
                    num+= leftMax-height[left];
                }
                leftMax = Math.max(leftMax, height[left]);
                left++;
            }
            else{
                if(rightMax>height[right]){
                    num+= rightMax-height[right];
                }
                rightMax = Math.max(rightMax, height[right]);
                right--;
            }
        }
        return num;
    }
}
```



#### [125. 验证回文串](https://leetcode-cn.com/problems/valid-palindrome/)

难度简单454收藏分享切换为英文接收动态反馈

给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。

**说明：**本题中，我们将空字符串定义为有效的回文串。

 

**示例 1:**

```
输入: "A man, a plan, a canal: Panama"
输出: true
解释："amanaplanacanalpanama" 是回文串
```

**示例 2:**

```
输入: "race a car"
输出: false
解释："raceacar" 不是回文串
```

 

**提示：**

- `1 <= s.length <= 2 * 105`
- 字符串 `s` 由 ASCII 字符组成

通过次数308,771

提交次数654,886

**判断char是否是数字或字母**

```java
class Solution {
    public boolean isPalindrome(String s) {
        if(s.length()<2){
            return true;
        }
        int left = 0;
        int right = s.length()-1;
        while(left<right){
            while(left<right && !isValid(s.charAt(left))){
                left++;
            }
            while(left<right && !isValid(s.charAt(right))){
                right--;
            }
            if(Character.toLowerCase(s.charAt(left)) != Character.toLowerCase(s.charAt(right))){
                return false;
            }
            left++;
            right--;
        }
        return true;

    }
    private boolean isValid(char c){
        if((c>='0' && c<='9')|| (c>='A' && c<='Z') || (c>='a' && c<='z')){
            return true;
        }
        return false;
    }
}
```

#### [455. 分发饼干](https://leetcode-cn.com/problems/assign-cookies/)

难度简单424收藏分享切换为英文接收动态反馈

假设你是一位很棒的家长，想要给你的孩子们一些小饼干。但是，每个孩子最多只能给一块饼干。

对每个孩子 `i`，都有一个胃口值 `g[i]`，这是能让孩子们满足胃口的饼干的最小尺寸；并且每块饼干 `j`，都有一个尺寸 `s[j]` 。如果 `s[j] >= g[i]`，我们可以将这个饼干 `j` 分配给孩子 `i` ，这个孩子会得到满足。你的目标是尽可能满足越多数量的孩子，并输出这个最大数值。

**示例 1:**

```
输入: g = [1,2,3], s = [1,1]
输出: 1
解释: 
你有三个孩子和两块小饼干，3个孩子的胃口值分别是：1,2,3。
虽然你有两块小饼干，由于他们的尺寸都是1，你只能让胃口值是1的孩子满足。
所以你应该输出1。
```

**示例 2:**

```
输入: g = [1,2], s = [1,2,3]
输出: 2
解释: 
你有两个孩子和三块小饼干，2个孩子的胃口值分别是1,2。
你拥有的饼干数量和尺寸都足以让所有孩子满足。
所以你应该输出2.
```

 

**提示：**

- `1 <= g.length <= 3 * 104`
- `0 <= s.length <= 3 * 104`
- `1 <= g[i], s[j] <= 231 - 1`

通过次数172,340

提交次数299,468

**两个指针遍历两个数组。可以用两个while，也可以用一个for加一个指针。**

```java
class Solution {
    public int findContentChildren(int[] g, int[] s) {
        Arrays.sort(g);
        Arrays.sort(s);
        int cnt = 0;
        // int g_pointer = 0;
        // int s_pointer = 0;
        // while(g_pointer<g.length && s_pointer<s.length){
        //     while(s_pointer<s.length && g[g_pointer]>s[s_pointer]){
        //         s_pointer++;
        //     }
        //     if(s_pointer!= s.length){
        //         cnt++;
        //         s_pointer++;
        //         g_pointer++;
        //     }
        // }
        int g_pointer = 0;
        for(int i = 0; i<s.length; i++){
            if(g[g_pointer]<=s[i]){
                g_pointer++;
                cnt++;
                if(g_pointer>=g.length){
                    break;
                }
            }
        }
        return cnt;
        
    }
}
```

#### [917. 仅仅反转字母](https://leetcode-cn.com/problems/reverse-only-letters/)

难度简单98收藏分享切换为英文接收动态反馈

给定一个字符串 `S`，返回 “反转后的” 字符串，其中不是字母的字符都保留在原地，而所有字母的位置发生反转。

 



**示例 1：**

```
输入："ab-cd"
输出："dc-ba"
```

**示例 2：**

```
输入："a-bC-dEf-ghIj"
输出："j-Ih-gfE-dCba"
```

**示例 3：**

```
输入："Test1ng-Leet=code-Q!"
输出："Qedo1ct-eeLg=ntse-T!"
```

 

**提示：**

1. `S.length <= 100`
2. `33 <= S[i].ASCIIcode <= 122` 
3. `S` 中不包含 `\` or `"`

通过次数31,553

提交次数55,387

```java
class Solution {

    public String reverseOnlyLetters(String s) {
        if(s.length()<2){
            return s;
        }
        int left = 0;
        int right = s.length()-1;
        char[] chars = s.toCharArray();
        while(left<right){
            while(left<right && !isValid(chars[left])){
                left++;
            }
            while(left<right && !isValid(chars[right])){
                right--;
            }
            swap(chars, left, right);
            left++;
            right--;

        }
        StringBuilder sb = new StringBuilder();
        for(char c:chars){
            sb.append(c);
        }
        return sb.toString();
    }
    private void swap(char[] chars, int left, int right){
        char tmp = chars[left];
        chars[left] = chars[right];
        chars[right] = tmp;
    }
    private boolean isValid(char c){
        if((c>='a' && c<='z')|| (c>='A' && c<= 'Z')){
            return true;
        }
        return false;
    }
}
```





#### [925. 长按键入](https://leetcode-cn.com/problems/long-pressed-name/)

难度简单220收藏分享切换为英文接收动态反馈

你的朋友正在使用键盘输入他的名字 `name`。偶尔，在键入字符 `c` 时，按键可能会被*长按*，而字符可能被输入 1 次或多次。

你将会检查键盘输入的字符 `typed`。如果它对应的可能是你的朋友的名字（其中一些字符可能被长按），那么就返回 `True`。

 

**示例 1：**

```
输入：name = "alex", typed = "aaleex"
输出：true
解释：'alex' 中的 'a' 和 'e' 被长按。
```

**示例 2：**

```
输入：name = "saeed", typed = "ssaaedd"
输出：false
解释：'e' 一定需要被键入两次，但在 typed 的输出中不是这样。
```

**示例 3：**

```
输入：name = "leelee", typed = "lleeelee"
输出：true
```

**示例 4：**

```
输入：name = "laiden", typed = "laiden"
输出：true
解释：长按名字中的字符并不是必要的。
```

 

**提示：**

- `name.length <= 1000`
- `typed.length <= 1000`
- `name` 和 `typed` 的字符都是小写字母。





通过次数52,028

提交次数134,561



```java
class Solution {
    //两个指针i,j分别指向name和typed。
    //如果name[i] == typed[j] ij同时加1
    //如果name[i] != typed[j] 那么去掉typed 的重复字符后，再比较。
    public boolean isLongPressedName(String name, String typed) {
        int i = 0;
        int j = 0;
        while(i<name.length() && j< typed.length()){
            if(name.charAt(i) == typed.charAt(j)){
                i++;
                j++;
            }
            else{
                if(j == 0){
                    return false;
                }
                while(j<typed.length()&& typed.charAt(j) == typed.charAt(j-1)){
                    j++;
                }
                if(j<typed.length()){
                    if(name.charAt(i)!= typed.charAt(j)){
                        return false;
                    }
                    i++;
                    j++;
                }
                
            }
        }
        if(i<name.length()){
            return false;
        }
        while(j<typed.length()){
            if(typed.charAt(j)!= typed.charAt(j-1)){
                return false;
            }
            j++;
        }
        return true;
    }
}
```



#### [986. 区间列表的交集](https://leetcode-cn.com/problems/interval-list-intersections/)

难度中等229收藏分享切换为英文接收动态反馈

给定两个由一些 **闭区间** 组成的列表，`firstList` 和 `secondList` ，其中 `firstList[i] = [starti, endi]` 而 `secondList[j] = [startj, endj]` 。每个区间列表都是成对 **不相交** 的，并且 **已经排序** 。

返回这 **两个区间列表的交集** 。

形式上，**闭区间** `[a, b]`（其中 `a <= b`）表示实数 `x` 的集合，而 `a <= x <= b` 。

两个闭区间的 **交集** 是一组实数，要么为空集，要么为闭区间。例如，`[1, 3]` 和 `[2, 4]` 的交集为 `[2, 3]` 。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2019/01/30/interval1.png)

```
输入：firstList = [[0,2],[5,10],[13,23],[24,25]], secondList = [[1,5],[8,12],[15,24],[25,26]]
输出：[[1,2],[5,5],[8,10],[15,23],[24,24],[25,25]]
```

**示例 2：**

```
输入：firstList = [[1,3],[5,9]], secondList = []
输出：[]
```

**示例 3：**

```
输入：firstList = [], secondList = [[4,8],[10,12]]
输出：[]
```

**示例 4：**

```
输入：firstList = [[1,7]], secondList = [[3,10]]
输出：[[3,7]]
```

 

**提示：**

- `0 <= firstList.length, secondList.length <= 1000`
- `firstList.length + secondList.length >= 1`
- `0 <= starti < endi <= 109`
- `endi < starti+1`
- `0 <= startj < endj <= 109`
- `endj < startj+1`

通过次数31,795

提交次数46,567

**TreeSet是有序集合**

```java
class Solution {
    public int[][] intervalIntersection(int[][] firstList, int[][] secondList) {
        //两个指针
        int i = 0;
        int j = 0;
        List<int[]> res = new ArrayList<>();
        while(i<firstList.length && j<secondList.length){
            //排除没有重合的两个情况：1. first在前，second在后。2. first在后，second在前。
            if(secondList[j][0]>firstList[i][1]){
                i++;
            }
            else if(firstList[i][0]>secondList[j][1]){
                j++;
            }
            //当两个interval有重合时
            else{
                res.add(new int[]{Math.max(firstList[i][0], secondList[j][0]), Math.min(firstList[i][1], secondList[j][1])});
                //谁结束得早就移动谁的指针.
                if(firstList[i][1]< secondList[j][1]){
                    i++;
                }
                else{
                    j++;
                }
            } 
        }
        int[][] real_res = new int[res.size()][2];
        for(int k = 0; k<res.size(); k++){
            real_res[k] = res.get(k);
        }
        return real_res;

    }
}
```



#### [*855. 考场就座](https://leetcode-cn.com/problems/exam-room/)

难度中等106收藏分享切换为英文接收动态反馈

在考场里，一排有 `N` 个座位，分别编号为 `0, 1, 2, ..., N-1` 。

当学生进入考场后，他必须坐在能够使他与离他最近的人之间的距离达到最大化的座位上。如果有多个这样的座位，他会坐在编号最小的座位上。(另外，如果考场里没有人，那么学生就坐在 0 号座位上。)

返回 `ExamRoom(int N)` 类，它有两个公开的函数：其中，函数 `ExamRoom.seat()` 会返回一个 `int` （整型数据），代表学生坐的位置；函数 `ExamRoom.leave(int p)` 代表坐在座位 `p` 上的学生现在离开了考场。每次调用 `ExamRoom.leave(p)` 时都保证有学生坐在座位 `p` 上。

 

**示例：**

```
输入：["ExamRoom","seat","seat","seat","seat","leave","seat"], [[10],[],[],[],[],[4],[]]
输出：[null,0,9,4,2,null,5]
解释：
ExamRoom(10) -> null
seat() -> 0，没有人在考场里，那么学生坐在 0 号座位上。
seat() -> 9，学生最后坐在 9 号座位上。
seat() -> 4，学生最后坐在 4 号座位上。
seat() -> 2，学生最后坐在 2 号座位上。
leave(4) -> null
seat() -> 5，学生最后坐在 5 号座位上。
```

 

**提示：**

1. `1 <= N <= 10^9`
2. 在所有的测试样例中 `ExamRoom.seat()` 和 `ExamRoom.leave()` 最多被调用 `10^4` 次。
3. 保证在调用 `ExamRoom.leave(p)` 时有学生正坐在座位 `p` 上。

通过次数5,695

提交次数13,777

```java
class ExamRoom {
    int N;
    TreeSet<Integer> students;
    public ExamRoom(int n) {
        this.N = n;
        students = new TreeSet<>();
    }
    
    public int seat() {
        //默认将新学生放在0位置
        int student = 0;
        if(students.size()>0){
            //distance是0位置到第一个学生的距离。
            int distance = students.first();
            Integer prev = null;
            //当遍历数组且需要和前一个元素比较时，声明一个prev，并在for循环里加一个if(prev!= null) 
            for(Integer s: students){
                if(prev!= null){
                    int tmpdist = (s- prev)/2;
                    if(tmpdist> distance){
                        distance = tmpdist;
                        student = (s+ prev)/2;
                    }
                }
                prev = s;
            }
            //最后一个学生到最后一个位置的距离
            if(N-1-students.last()>distance){
                student = N-1;
            }
        }
        students.add(student);
        return student;

    }
    
    public void leave(int p) {
        students.remove(p);
    }
}

/**
 * Your ExamRoom object will be instantiated and called as such:
 * ExamRoom obj = new ExamRoom(n);
 * int param_1 = obj.seat();
 * obj.leave(p);
 */
```

#### [167. 两数之和 II - 输入有序数组](https://leetcode-cn.com/problems/two-sum-ii-input-array-is-sorted/)

难度简单654收藏分享切换为英文接收动态反馈

给定一个已按照 **非递减顺序排列** 的整数数组 `numbers` ，请你从数组中找出两个数满足相加之和等于目标数 `target` 。

函数应该以长度为 `2` 的整数数组的形式返回这两个数的下标值*。*`numbers` 的下标 **从 1 开始计数** ，所以答案数组应当满足 `1 <= answer[0] < answer[1] <= numbers.length` 。

你可以假设每个输入 **只对应唯一的答案** ，而且你 **不可以** 重复使用相同的元素。

**示例 1：**

```
输入：numbers = [2,7,11,15], target = 9
输出：[1,2]
解释：2 与 7 之和等于目标数 9 。因此 index1 = 1, index2 = 2 。
```

**示例 2：**

```
输入：numbers = [2,3,4], target = 6
输出：[1,3]
```

**示例 3：**

```
输入：numbers = [-1,0], target = -1
输出：[1,2]
```

 

**提示：**

- `2 <= numbers.length <= 3 * 104`
- `-1000 <= numbers[i] <= 1000`
- `numbers` 按 **非递减顺序** 排列
- `-1000 <= target <= 1000`
- **仅存在一个有效答案**

通过次数343,627

提交次数585,034



```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int left = 0;
        int right = numbers.length-1;
        while(left<right){
            if(numbers[left]+numbers[right]>target){
                right--;
            }
            else if(numbers[left]+numbers[right]<target){
                left++;
            }
            else{
                break;
            }
        }
        return new int[]{left+1, right+1};
    }
}
```



#### [15. 三数之和](https://leetcode-cn.com/problems/3sum/)

难度中等4172收藏分享切换为英文接收动态反馈

给你一个包含 `n` 个整数的数组 `nums`，判断 `nums` 中是否存在三个元素 *a，b，c ，*使得 *a + b + c =* 0 ？请你找出所有和为 `0` 且不重复的三元组。

**注意：**答案中不可以包含重复的三元组。

 

**示例 1：**

```
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
```

**示例 2：**

```
输入：nums = []
输出：[]
```

**示例 3：**

```
输入：nums = [0]
输出：[]
```

 

**提示：**

- `0 <= nums.length <= 3000`
- `-105 <= nums[i] <= 105`

通过次数756,954

提交次数2,211,799



```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        Arrays.sort(nums);
        for(int i = 0; i<nums.length-2; i++){
            //剪枝
            if(nums[i]>0){
                break;
            }
            //避免重复
            if(i>0 && nums[i] == nums[i-1]){
                continue;
            }
            int target = -nums[i];
            int left = i+1; 
            int right = nums.length-1;
            while(left<right){
                //嵌套while会拖慢速度。
                if(nums[left]+nums[right]<target){
                    left++;  
                }
                else if(nums[left]+nums[right]>target){
                    right--;       
                }
                else{
                    List<Integer> list = new ArrayList<>();
                    list.add(nums[i]);
                    list.add(nums[left]);
                    list.add(nums[right]);
                    res.add(list);
                    left++;
                    while(left<right && nums[left] == nums[left-1]){
                        left++;
                    }
                    right--;
                    while(left<right && nums[right] == nums[right+1]){
                        right--;
                    }
                }
            }
        }
        return res;
    }
}
```

#### [16. 最接近的三数之和](https://leetcode-cn.com/problems/3sum-closest/)

难度中等1003收藏分享切换为英文接收动态反馈

给你一个长度为 `n` 的整数数组 `nums` 和 一个目标值 `target`。请你从 `nums` 中选出三个整数，使它们的和与 `target` 最接近。

返回这三个数的和。

假定每组输入只存在恰好一个解。

 

**示例 1：**

```
输入：nums = [-1,2,1,-4], target = 1
输出：2
解释：与 target 最接近的和是 2 (-1 + 2 + 1 = 2) 。
```

**示例 2：**

```
输入：nums = [0,0,0], target = 1
输出：0
```

 

**提示：**

- `3 <= nums.length <= 1000`
- `-1000 <= nums[i] <= 1000`
- `-104 <= target <= 104`

通过次数292,186

提交次数637,351



```java
class Solution {
    public int threeSumClosest(int[] nums, int target) {

        Arrays.sort(nums);
        int closestSum = 0;
        int difference = Integer.MAX_VALUE;
        for(int i = 0; i<nums.length-2; i++){
            if(i>0 && nums[i] == nums[i-1]){
                continue;
            }
            int left = i+1; 
            int right = nums.length-1;
            while(left<right){
                int sum = nums[i]+nums[left]+nums[right];
                if(difference> Math.abs(target-sum)){
                    difference = Math.abs(target-sum);
                    closestSum = sum;
                }
                if(sum<target){
                    left++;
                }
                else if(sum>target){
                    right--;       
                }
                else{
                    return target;
                }
            }
        }
        return closestSum;
    }
}
```

#### [977. 有序数组的平方](https://leetcode-cn.com/problems/squares-of-a-sorted-array/)

难度简单396收藏分享切换为英文接收动态反馈

**这道题其实是合并两个有序数组**

给你一个按 **非递减顺序** 排序的整数数组 `nums`，返回 **每个数字的平方** 组成的新数组，要求也按 **非递减顺序** 排序。



 

**示例 1：**

```
输入：nums = [-4,-1,0,3,10]
输出：[0,1,9,16,100]
解释：平方后，数组变为 [16,1,0,9,100]
排序后，数组变为 [0,1,9,16,100]
```

**示例 2：**

```
输入：nums = [-7,-3,2,3,11]
输出：[4,9,9,49,121]
```

 

**提示：**

- `1 <= nums.length <= 104`
- `-104 <= nums[i] <= 104`
- `nums` 已按 **非递减顺序** 排序



**进阶：**

- 请你设计时间复杂度为 `O(n)` 的算法解决本问题



负数平方后需要重新排序。

但原数组已经是排好序的了。平方后，最大的平方数只可能出现在数组的两端。用两个指针指向数组的两端即可找到最大的值。

```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        int[] res = new int[nums.length];
        int k = nums.length-1;
        int left = 0;
        int right = nums.length-1;
        while(left<=right){
            if(nums[left]*nums[left]< nums[right]*nums[right]){
                res[k] = nums[right]*nums[right];
                right--;
            }
            else{
                res[k] = nums[left]*nums[left];
                left++;
            }
            k--;
        }
        return res;
    }
}
```

#### [159. 至多包含两个不同字符的最长子串](https://leetcode-cn.com/problems/longest-substring-with-at-most-two-distinct-characters/)

难度中等144收藏分享切换为英文接收动态反馈

给定一个字符串 ***s\*** ，找出 **至多** 包含两个不同字符的最长子串 ***t\*** ，并返回该子串的长度。

**示例 1:**

```
输入: "eceba"
输出: 3
解释: t 是 "ece"，长度为3。
```

**示例 2:**

```
输入: "ccaabbb"
输出: 5
解释: t 是 "aabbb"，长度为5。
```

通过次数18,345

提交次数33,332

**~~最长子串和最小子串的处理方式差不多。都是先移右边，移到不能移了再移左边。~~**

其实这个理解是不对的，正确理解应该是：滑动窗口内的元素要有明确的目的性。当我们能找到所有满足该目的的窗口后，无论是找最长，最短，还是取个数，就都很简单了。

```java
class Solution {
    public int lengthOfLongestSubstringTwoDistinct(String s) {
        int left = 0;
        int right = 0;
        int length = 0;
        HashMap<Character, Integer> map = new HashMap<>();
        for(; right<s.length(); right++){
            char c = s.charAt(right);
            map.put(c, map.getOrDefault(c, 0)+1);
            while(left<right && map.size() >2){
                c = s.charAt(left);
                map.put(c, map.get(c)-1);
                if(map.get(c) == 0){
                    map.remove(c);
                }
                left++;
            }

            length = Math.max(length, right-left+1);
        }
        return length;
    }
}
```

#### [992. K 个不同整数的子数组](https://leetcode-cn.com/problems/subarrays-with-k-different-integers/)

难度困难350收藏分享切换为英文接收动态反馈

给定一个正整数数组 `A`，如果 `A` 的某个子数组中不同整数的个数恰好为 `K`，则称 `A` 的这个连续、不一定不同的子数组为*好子数组*。

（例如，`[1,2,3,1,2]` 中有 `3` 个不同的整数：`1`，`2`，以及 `3`。）

返回 `A` 中*好子数组*的数目。

 

**示例 1：**

```
输入：A = [1,2,1,2,3], K = 2
输出：7
解释：恰好由 2 个不同整数组成的子数组：[1,2], [2,1], [1,2], [2,3], [1,2,1], [2,1,2], [1,2,1,2].
```

**示例 2：**

```
输入：A = [1,2,1,3,4], K = 3
输出：3
解释：恰好由 3 个不同整数组成的子数组：[1,2,1,3], [2,1,3], [1,3,4].
```

 

**提示：**

1. `1 <= A.length <= 20000`
2. `1 <= A[i] <= A.length`
3. `1 <= K <= A.length`

通过次数24,737

提交次数54,186

```java
class Solution {
    //这道题求的是最多包含K个不同整数的子数组的个数（众多子数组的个数）
    //区别于其他的题目：最多包含K个不同整数的最长子数组的长度（单个子数组的长度）
    public int subarraysWithKDistinct(int[] nums, int k) {
        return subarraysWithAtMostKDistinct(nums, k)-subarraysWithAtMostKDistinct(nums, k-1);
    }
    private int subarraysWithAtMostKDistinct(int[] nums, int k){
        if(k == 0){
            return 0;
        }
        int left = 0; 
        int right = 0;
        int count = 0;
        int ans = 0;
        //number and freq
        HashMap<Integer, Integer> map = new HashMap<>();
        for(; right<nums.length; right++){
            int freq = map.getOrDefault(nums[right], 0);
            if(freq == 0){
                count++;
            }
            map.put(nums[right], freq+1);
            while(left<right && count>k){
                if(map.get(nums[left]) == 1){
                    count--;
                }
                map.put(nums[left], map.get(nums[left])-1);
                left++;

            }
            //这步是最关键的。
            //新子数组长度就是增加的子数组个数。
            //举个例子就好理解了：
//当满足条件的子数组从 [A,B,C] 增加到 [A,B,C,D] 时，新子数组的长度为 4，同时增加的子数组为 [D], [C,D], [B,C,D], [A,B,C,D] 也为4。
            ans+= right - left+1;
        }
        return ans;
    }

}
```

#### [27. 移除元素](https://leetcode-cn.com/problems/remove-element/)

难度简单1223收藏分享切换为英文接收动态反馈

给你一个数组 `nums` 和一个值 `val`，你需要 **[原地](https://baike.baidu.com/item/原地算法)** 移除所有数值等于 `val` 的元素，并返回移除后数组的新长度。

不要使用额外的数组空间，你必须仅使用 `O(1)` 额外空间并 **[原地 ](https://baike.baidu.com/item/原地算法)修改输入数组**。

元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。

 

**说明:**

为什么返回数值是整数，但输出的答案是数组呢?

请注意，输入数组是以**「引用」**方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。

你可以想象内部操作如下:

```
// nums 是以“引用”方式传递的。也就是说，不对实参作任何拷贝
int len = removeElement(nums, val);

// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中 该长度范围内 的所有元素。
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

 

**示例 1：**

```
输入：nums = [3,2,2,3], val = 3
输出：2, nums = [2,2]
解释：函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。你不需要考虑数组中超出新长度后面的元素。例如，函数返回的新长度为 2 ，而 nums = [2,2,3,3] 或 nums = [2,2,0,0]，也会被视作正确答案。
```

**示例 2：**

```
输入：nums = [0,1,2,2,3,0,4,2], val = 2
输出：5, nums = [0,1,4,0,3]
解释：函数应该返回新的长度 5, 并且 nums 中的前五个元素为 0, 1, 3, 0, 4。注意这五个元素可为任意顺序。你不需要考虑数组中超出新长度后面的元素。
```

 

**提示：**

- `0 <= nums.length <= 100`
- `0 <= nums[i] <= 50`
- `0 <= val <= 100`

通过次数622,007

提交次数1,045,140

```java
//left指向的值一定不能等于val
class Solution {
    public int removeElement(int[] nums, int val) {
        int left = 0;
        int right = 0;
        while(right<nums.length){
            if(nums[right]!= val){
                nums[left] = nums[right];
                left++;
            }
            right++;
        }
        return left;
    }
}
```

#### [26. 删除有序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)

难度简单2513收藏分享切换为英文接收动态反馈

给你一个 **升序排列** 的数组 `nums` ，请你**[ 原地](http://baike.baidu.com/item/原地算法)** 删除重复出现的元素，使每个元素 **只出现一次** ，返回删除后数组的新长度。元素的 **相对顺序** 应该保持 **一致** 。

由于在某些语言中不能改变数组的长度，所以必须将结果放在数组nums的第一部分。更规范地说，如果在删除重复项之后有 `k` 个元素，那么 `nums` 的前 `k` 个元素应该保存最终结果。

将最终结果插入 `nums` 的前 `k` 个位置后返回 `k` 。

不要使用额外的空间，你必须在 **[原地 ](https://baike.baidu.com/item/原地算法)修改输入数组** 并在使用 O(1) 额外空间的条件下完成。

**判题标准:**

系统会用下面的代码来测试你的题解:

```
int[] nums = [...]; // 输入数组
int[] expectedNums = [...]; // 长度正确的期望答案

int k = removeDuplicates(nums); // 调用

assert k == expectedNums.length;
for (int i = 0; i < k; i++) {
    assert nums[i] == expectedNums[i];
}
```

如果所有断言都通过，那么您的题解将被 **通过**。

 

**示例 1：**

```
输入：nums = [1,1,2]
输出：2, nums = [1,2,_]
解释：函数应该返回新的长度 2 ，并且原数组 nums 的前两个元素被修改为 1, 2 。不需要考虑数组中超出新长度后面的元素。
```

**示例 2：**

```
输入：nums = [0,0,1,1,1,2,2,3,3,4]
输出：5, nums = [0,1,2,3,4]
解释：函数应该返回新的长度 5 ， 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4 。不需要考虑数组中超出新长度后面的元素。
```

 

**提示：**

- `0 <= nums.length <= 3 * 104`
- `-104 <= nums[i] <= 104`
- `nums` 已按 **升序** 排列

通过次数1,011,046

提交次数1,882,247

```java
//插入位置是left，left指向的值一定要跟left-1指向的值不一样。
//right负责找到第一个符合条件的值，插到left位置，然后left++更新,重复上述操作
class Solution {
    public int removeDuplicates(int[] nums) {
        if(nums.length<=1){
            return nums.length;
        }
        int left = 1;
        int right = 1;
        while(right<nums.length){
            if(nums[right] == nums[left-1]){
                right++;
            }
            else{
                nums[left] = nums[right];
                left++;
                right++;
            }
        }
        return left;
    }
}
```

#### [80. 删除有序数组中的重复项 II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array-ii/)

难度中等655收藏分享切换为英文接收动态反馈

给你一个有序数组 `nums` ，请你**[ 原地](http://baike.baidu.com/item/原地算法)** 删除重复出现的元素，使每个元素 **最多出现两次** ，返回删除后数组的新长度。

不要使用额外的数组空间，你必须在 **[原地 ](https://baike.baidu.com/item/原地算法)修改输入数组** 并在使用 O(1) 额外空间的条件下完成。

 

**说明：**

为什么返回数值是整数，但输出的答案是数组呢？

请注意，输入数组是以**「引用」**方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。

你可以想象内部操作如下:

```
// nums 是以“引用”方式传递的。也就是说，不对实参做任何拷贝
int len = removeDuplicates(nums);

// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中 该长度范围内 的所有元素。
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

 

**示例 1：**

```
输入：nums = [1,1,1,2,2,3]
输出：5, nums = [1,1,2,2,3]
解释：函数应返回新长度 length = 5, 并且原数组的前五个元素被修改为 1, 1, 2, 2, 3 。 不需要考虑数组中超出新长度后面的元素。
```

**示例 2：**

```
输入：nums = [0,0,1,1,1,1,2,3,3]
输出：7, nums = [0,0,1,1,2,3,3]
解释：函数应返回新长度 length = 7, 并且原数组的前五个元素被修改为 0, 0, 1, 1, 2, 3, 3 。 不需要考虑数组中超出新长度后面的元素。
```

 

**提示：**

- `1 <= nums.length <= 3 * 104`
- `-104 <= nums[i] <= 104`
- `nums` 已按升序排列

通过次数169,237

提交次数273,269

```java
//left指向的值一定要跟left-2指向的值不一样。right负责找到这样的值，赋值给left。
//left++且right++，然后重复上述操作
class Solution {
    public int removeDuplicates(int[] nums) {
        if(nums.length<=2){
            return nums.length;
        }
        int left = 2;
        int right = 2;
        while(right<nums.length){
            if(nums[right] == nums[left-2]){
                right++;
            }
            else{
                nums[left] = nums[right];
                left++;
                right++;
            }
        }
        return left;
    }
}
```

