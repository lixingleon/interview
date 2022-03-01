# [剑指 Offer 03. 数组中重复的数字](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)

```java
//1. sort and compare ith element with i+1th, return if equal.
//time:O(nlogn),space: O(1).
//2. use hashset to detect duplicate element
//time:O(n), space: O(n)
//3. switch elements inside array
//time:O(n), space:O(1)

//3. 
class Solution {
    public int findRepeatNumber(int[] nums) {
        int i = 0;
        while(i<nums.length){
            //if nums[i] already equals to i,this index is fine, go check the next index
            if(nums[i] == i){
                i++;
                continue;
            }
            //if the correct location for nums[i] is already been taken by the correct value, it means there are duplicate values for one index.simply return the value
            if(nums[i] == nums[nums[i]]) return nums[i];
            //switch nums[i] and nums[nums[i]]
            int tmp = nums[i];
            nums[i] = nums[tmp];
            nums[tmp] = tmp;
        }
        return -1;
    }
}

```

# [剑指 Offer 22. 链表中倒数第k个节点](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)

考虑edge case

1. k为0
2. head为null
3. k>list.size()

```java
class Solution {
    public ListNode getKthFromEnd(ListNode head, int k) {
        if(head == null || k == 0){
            return null;
        }
        ListNode runner = head, walker = head;
        for(int i = 0; i<k; i++){
            if(runner == null && i<k){
                throw new Error("k is larger than list size! ");
            }
            runner = runner.next;
        }
        while(runner!= null){
            runner = runner.next;
            walker = walker.next;
        }
        return walker;
    }
}
```

# [剑指 Offer 13. 机器人的运动范围](https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/)

```java
//dfs配合visited数组
class Solution {
    public int movingCount(int m, int n, int k) {
        boolean[][] visited = new boolean[m][n];
        return dfs(0,0,m,n,k,visited);
    }
    private int dfs(int row, int col, int m, int n, int k, boolean[][] visited){
        if(row<0|| row>=m || col<0 || col>=n || visited[row][col]
        || (row/10+row%10+col/10+col%10)>k){
            return 0;
        }
        visited[row][col] = true;
        return 1+dfs(row-1, col, m, n, k, visited)+dfs(row+1, col, m, n, k, visited)+dfs(row, col-1, m, n, k, visited)+dfs(row, col+1, m, n, k, visited);
    }
}
```

# [剑指 Offer 59 - I. 滑动窗口的最大值](https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/)

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        //维护一个非严格单调递减队列,存的是index
        int[] ans = new int[nums.length-k+1];
        Deque<Integer> deque = new ArrayDeque<>();
        for(int i = 0; i<k; i++){
            while(!deque.isEmpty()&& nums[i]>nums[deque.peekLast()]){
                deque.removeLast();
            }
            deque.addLast(i);
        }
        ans[0] = nums[deque.peekFirst()];
        int index = 1;
        for(int i = k; i<nums.length; i++){
            while(!deque.isEmpty()&& nums[i]>nums[deque.peekLast()]){
                deque.removeLast();
            }
            deque.addLast(i);
            while(deque.peekFirst()<i-k+1){
                deque.removeFirst();
            }
            ans[i-k+1] = nums[deque.peekFirst()];
        }
        return ans;
    }
}
```

# [剑指 Offer 10- II. 青蛙跳台阶问题](https://leetcode-cn.com/problems/qing-wa-tiao-tai-jie-wen-ti-lcof/)

取余要用素数并且这个素数要取得非常大的原因：

1. 为什么要素数：提高余数空间的利用率
2. 为什么要用大素数：减少余数之间的碰撞

```java
class Solution {
    public int numWays(int n) {
        //f(n) = f(n-1)+f(n-2)
        if(n<=1) return 1;
        int former = 1, latter = 1;
        for(int i = 1; i<n; i++){
            latter = former+ latter;
            former = latter - former;
            latter %= 1000000007;
        }
        return latter;
    }
}
```

# [剑指 Offer 07. 重建二叉树](https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof/)

```java
//preorder 左子树: [preStart+1, preStart+leftTreeLength]
//preorder 右子树: [preStart+leftTreeLength+1, preEnd]
//inorder 左子树: [inStart, indexOfRoot-1]
//inorder 右子树: [indexOfRoot+1, inEnd]
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        if(preorder== null || inorder == null) throw new Error("input invalid");
        if(preorder.length == 0 || inorder.length == 0) return null;
        return helper(preorder, 0, preorder.length-1, inorder, 0, inorder.length-1);
    }
    private TreeNode helper(int[] preorder, int preStart, int preEnd, int[] inorder, int inStart, int inEnd){
        if(inStart>inEnd) return null;
        int rootVal = preorder[preStart];
        TreeNode root = new TreeNode(rootVal);
        //find root index in inorder
        int rootIndex = inStart;
        for(int i = inStart; i<=inEnd; i++){
            if(inorder[i] == rootVal){
                rootIndex = i;
            }
        }
        int leftTreeLength = rootIndex-inStart;//1  1
        int rightTreeLength = inEnd - rootIndex;//3  0
        root.left = helper(preorder, preStart+1, preStart+leftTreeLength, inorder, inStart, rootIndex-1);
        root.right = helper(preorder, preStart+leftTreeLength+1, preEnd, inorder, rootIndex+1, inEnd);
        return root;
    }
}
```



# [剑指 Offer 40. 最小的k个数](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/)

四种方法

1. 大顶堆

   - 创建size为k的大顶堆。当num比堆顶元素小时，poll堆顶元素并把num加进堆内
   - 时间：O(nlogk)
   - 空间: O(k)

   ```java
   class Solution {
       public int[] getLeastNumbers(int[] arr, int k) {
           //一个数组可能有三种取值：
           //1.null 代表arr = null
           //2.[] 代表arr.length = 0, 就像这里创建一个长度为0的数组
           //3.[1,2...] 正常数组
           if(k == 0|| arr.length == 0) return new int[0];
           //priorityqueue 声明时指定一个size?
           //默认是小顶堆,我们需要一个大顶堆
           PriorityQueue<Integer> pq = new PriorityQueue<>(k,(n1, n2) ->{
               return n2-n1;
           });
           for(int num: arr){
               if(pq.size()<k){
                   pq.offer(num);
               }
               else if(num<pq.peek()){
                   pq.poll();
                   pq.offer(num);
               }
           }
           int[] ans = new int[k];
           int idx = 0;
           for(int num:pq){
               ans[idx++] = num;
           }
           return ans;
       }
   }
   ```


2. 快排

   1. 每次将第一个数k当作待归位的数，两指针一个从左到右，一个从右到左，目的是确保当k归位后，k左边的数都小于等于k，k右边的数都大于等于k
   2. 注意右指针先动，左指针后动
   3. 时间：每次归位第一个数的时间复杂度是O(n)，每次归位结束后n都会减半。于是总体时间复杂度为O(n)+O(n/2)+O(n/4)+...+O(1),等比数列，计算后得到O(n)
   4. 空间：O(1)

   ```java
   class Solution {
       public int[] getLeastNumbers(int[] arr, int k) {
           if(k == 0 || arr.length == 0) return new int[0];
           return quicksort(arr, 0, arr.length-1, k-1);
       }
       private int[] quicksort(int[] arr, int left, int right, int k){
           int i = left, j = right;
           while(i<j){
               while(i<j && arr[j]>=arr[left]) j--;
               while(i<j && arr[i]<=arr[left]) i++;
               swap(arr, i, j);
           }
           swap(arr, i, left);
           if(k<i) return quicksort(arr, left, i-1, k);
           if(k>i) return quicksort(arr, i+1, right, k);
           return Arrays.copyOf(arr, k+1);
       }
       private void swap(int[] arr, int i, int j){
           int tmp = arr[i];
           arr[i] = arr[j];
           arr[j] = tmp;
       }
   }
   ```


# [169. 多数元素](https://leetcode-cn.com/problems/majority-element/)

摩尔投票法：

初始化num = nums[0], 票数count=1

遍历列表余下的元素，若与num相等，则count++

若与num不等，则count--

若count = 0

更换num，并把count重设为1

```java
原理解释：
因为列表中存在多数元素a， a的个数> n/2。
多数元素个数 - 其余元素个数总和 >=1
多数元素a与其他元素两两互相抵消。最后肯定还剩余至少1个多数元素。
就是我们的解。
    class Solution {
    public int majorityElement(int[] nums) {
        int num = nums[0], count = 1;
        for(int i = 1; i<nums.length; i++){
            if(num == nums[i]) count++;
            else{
                count--;
                if(count == 0){
                    num = nums[i];
                    count = 1;
                }
            }
        }
        return num;
    }
}
```



# [3. 无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

用一个HashMap<Character, Integer>记录每个字符和它出现的index

遍历所有Character, 如果没遇见过，则在map中记录index，如果遇见过，则将left直接放置到上次出现的index+1的位置，意图是将出现过的character排除在窗口之外。

这里注意如果index+1<left，说明重复元素已经在窗口之外了，这时就不用管它了。

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        //store character and its index
        HashMap<Character, Integer> map = new HashMap<>();
        int maxLength = 0;
        for(int left = 0, right = 0; right<s.length(); right++){
            if(map.getOrDefault(s.charAt(right), -1)!= -1){
                left = Math.max(left, map.get(s.charAt(right))+1);
            }
            map.put(s.charAt(right), right);
            maxLength = Math.max(maxLength, right-left+1);
        }
        return maxLength;
    }
}
```

# [剑指 Offer 26. 树的子结构](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/)

```java
class Solution {
    //对A进行先序遍历，在遍历的同时判断A的每个node是否是B的root
    public boolean isSubStructure(TreeNode A, TreeNode B) {
        if(A == null|| B == null) return false;
        boolean AequalsB = isEqual(A, B);
        boolean left = isSubStructure(A.left, B);
        boolean right = isSubStructure(A.right, B);
        return AequalsB|| left|| right;
    }
    private boolean isEqual(TreeNode A, TreeNode B){
        if(B == null) return true;
        if(A == null || A.val != B.val) return false;
        return isEqual(A.left, B.left)&& isEqual(A.right, B.right);
    }
}
```


#### [56. 合并区间](https://leetcode-cn.com/problems/merge-intervals/)

难度中等1339收藏分享切换为英文接收动态反馈

以数组 `intervals` 表示若干个区间的集合，其中单个区间为 `intervals[i] = [starti, endi]` 。请你合并所有重叠的区间，并返回 *一个不重叠的区间数组，该数组需恰好覆盖输入中的所有区间* 。

 

**示例 1：**

```
输入：intervals = [[1,3],[2,6],[8,10],[15,18]]
输出：[[1,6],[8,10],[15,18]]
解释：区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].

```

**示例 2：**

```
输入：intervals = [[1,4],[4,5]]
输出：[[1,5]]
解释：区间 [1,4] 和 [4,5] 可被视为重叠区间。
```

 

**提示：**

- `1 <= intervals.length <= 104`
- `intervals[i].length == 2`
- `0 <= starti <= endi <= 104`

```java
//思路是对的，但实现方式还有更简洁的。
class Solution {
    public int[][] merge(int[][] intervals) {
        if(intervals.length == 1){
            return intervals;
        }
        Arrays.sort(intervals, (interval1, interval2) ->interval1[0]-interval2[0]);
        List<List<Integer>> res = new ArrayList<>();
        LinkedList<Integer> cur = buildList(intervals[0]);
        for(int i = 1; i<intervals.length; i++){
            LinkedList<Integer> toMerge = buildList(intervals[i]);
            if(cur.peekLast()>= toMerge.peekFirst()){
                int curLast = cur.removeLast();
                int toMergeLast = toMerge.removeLast();
                cur.add(Math.max(curLast, toMergeLast));
            }
            else{
                res.add(new ArrayList<>(cur));
                cur = toMerge;
            }
            if(i == intervals.length-1){
                    res.add(new ArrayList<>(cur));
                }
        }
        int[][] result = new int[res.size()][2];
        for(int i = 0; i<res.size(); i++){
            for(int j = 0; j<2; j++){
                result[i][j] = res.get(i).get(j);
            }
        }
        return result;
    }
    private LinkedList<Integer> buildList(int[] nums){
        LinkedList<Integer> res = new LinkedList<>();
        for(int num: nums){
            res.add(num);
        }
        return res;
    }
}
//更简洁版本
//这个方案将intervals数组直接赋值到res数组，省去了中间的步骤。
//并且用idx == -1 来跳过一开始当res数组为空的情况
class Solution {
    public int[][] merge(int[][] intervals) {
        if(intervals.length == 1){
            return intervals;
        }
        Arrays.sort(intervals, (interval1, interval2) ->interval1[0]-interval2[0]);
        int[][] res = new int[intervals.length][2];
        int idx = -1;
        for(int i = 0; i<intervals.length; i++){
            if(idx == -1 || intervals[i][0]>res[idx][1]){
                idx++;
                res[idx] = intervals[i];
            }
            else{
                res[idx][1] = Math.max(res[idx][1], intervals[i][1]);
            }
        }
       
        return Arrays.copyOf(res, idx+1);
    }
}
```





#### [剑指 Offer II 087. 复原 IP ](https://leetcode-cn.com/problems/0on3uN/)

难度中等16收藏分享切换为英文接收动态反馈

给定一个只包含数字的字符串 `s` ，用以表示一个 IP 地址，返回所有可能从 `s` 获得的 **有效 IP 地址 **。你可以按任何顺序返回答案。

**有效 IP 地址** 正好由四个整数（每个整数位于 0 到 255 之间组成，且不能含有前导 `0`），整数之间用 `'.'` 分隔。

例如："0.1.2.201" 和 "192.168.1.1" 是 **有效** IP 地址，但是 "0.011.255.245"、"192.168.1.312" 和 "192.168@1.1" 是 **无效** IP 地址。

 

**示例 1：**

```
输入：s = "25525511135"
输出：["255.255.11.135","255.255.111.35"]

```

**示例 2：**

```
输入：s = "0000"
输出：["0.0.0.0"]

```

**示例 3：**

```
输入：s = "1111"
输出：["1.1.1.1"]

```

**示例 4：**

```
输入：s = "010010"
输出：["0.10.0.10","0.100.1.0"]

```

**示例 5：**

```
输入：s = "10203040"
输出：["10.20.30.40","102.0.30.40","10.203.0.40"]

```

 

**提示：**

- `0 <= s.length <= 3000`
- `s` 仅由数字组成

```java
//尽早剪枝的方式没想出来
//每个for循环有三次循环
//四个segment都得到以后用String.join('.' segments)
class Solution {
    public List<String> restoreIpAddresses(String s) {
        List<String> res = new ArrayList<>();
        LinkedList<String> path = new LinkedList<>(); 
        findIPs(res, path, 0, 0, s);
        return res;
    }
    private void findIPs(List<String> res, LinkedList<String> path, int segmentNum, int begin, String s){
        //尽早剪枝
        if(begin+3*(4-segmentNum)<s.length() || begin+4-segmentNum>s.length()){
            return;
        }

        if(segmentNum == 4){
            res.add(String.join(".", path));
            return;
        }

        for(int i = 0; i<3; i++){
            if(i+begin>=s.length()){
                break;
            }
            if(isValidSegment(s, begin, begin+i+1)){
                path.add(s.substring(begin, begin+i+1));
                findIPs(res, path, segmentNum+1, begin+i+1, s);
                path.removeLast();
            }

        }

    }
    private boolean isValidSegment(String s, int begin, int end){
        //注意这里 == ‘0’ 而不是 == 0 是char跟char比较，不是char跟int比较。
        if(end-begin>1 && s.charAt(begin) == '0'){
            return false;
        }
        int ret = Integer.parseInt(s.substring(begin, end));
        return ret>255? false: true;
    }
}
```



#### [剑指 Offer II 077. 链表排序](https://leetcode-cn.com/problems/7WHec2/)

难度中等31收藏分享切换为英文接收动态反馈

给定链表的头结点 `head` ，请将其按 **升序** 排列并返回 **排序后的链表** 。




**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/09/14/sort_list_1.jpg)

```
输入：head = [4,2,1,3]
输出：[1,2,3,4]

```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2020/09/14/sort_list_2.jpg)

```
输入：head = [-1,5,3,4,0]
输出：[-1,0,3,4,5]

```

**示例 3：**

```
输入：head = []
输出：[]

```

 

**提示：**

- 链表中节点的数目在范围 `[0, 5 * 104]` 内
- `-105 <= Node.val <= 105`

 

**进阶：**你可以在 `O(n log n)` 时间复杂度和常数级空间复杂度下，对链表进行排序吗？

 

注意：本题与主站 148 题相同：<https://leetcode-cn.com/problems/sort-list/>

```java
class Solution {
    public ListNode sortList(ListNode head) {
        return mergeSort(head);
    }
    private ListNode mergeSort(ListNode head){
        if(head == null || head.next == null){
            return head;
        }
        //fast指针先走一格，这样能让slow指向中点的前一个节点，就可以分割slow了
        ListNode fast = head.next;
        ListNode slow = head;
        while(fast!= null && fast.next!= null){
            fast = fast.next.next;
            slow = slow.next;
        }
        ListNode node2 = mergeSort(slow.next);
        slow.next = null;
        ListNode node1 = mergeSort(head);
        return merge(node1, node2);

    }
    private ListNode merge(ListNode n1, ListNode n2){
        if(n1 == null){
            return n2;
        }
        if(n2 == null){
            return n1;
        }
        if(n1.val<=n2.val){
            n1.next = merge(n1.next, n2);
            return n1;
        }
        else{
            n2.next = merge(n2.next, n1);
            return n2;
        }
    }
}
```

#### [ 713. 乘积小于K的子数组](https://leetcode-cn.com/problems/subarray-product-less-than-k/)

难度中等348收藏分享切换为英文接收动态反馈

给定一个正整数数组 `nums`和整数 `k` 。

请找出该数组内乘积小于 `k` 的连续的子数组的个数。

 

**示例 1:**

```
输入: nums = [10,5,2,6], k = 100
输出: 8
解释: 8个乘积小于100的子数组分别为: [10], [5], [2], [6], [10,5], [5,2], [2,6], [5,2,6]。
需要注意的是 [10,5,2] 并不是乘积小于100的子数组。
```

**示例 2:**

```
输入: nums = [1,2,3], k = 0
输出: 0
```

 

**提示:** 

- `1 <= nums.length <= 3 * 104`
- `1 <= nums[i] <= 1000`
- `0 <= k <= 106`

通过次数34,456

提交次数79,451

```
class Solution {
    public int numSubarrayProductLessThanK(int[] nums, int k) {
        if(k == 0 || k == 1){
            return 0;
        }
        int multiple = 1;
        int left = 0;
        int count = 0;
        //每加进一个数，就把以这个数结尾的子数组乘积都看一遍，小于k的就加到count中去。
        //因为小于k的数组的子数组的乘积都小于k，所以每次循环只需要O（1）复杂度
        //所以总体复杂度只有O（n）
        for(int right = 0; right<nums.length; right++){
            multiple *= nums[right];
            while(multiple>=k){
                multiple/= nums[left];
                left++;
            }
            count+= right-left+1;
        }
        return count;
    }
}
```

#### [84. 柱状图中最大的矩形](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)

难度困难1771收藏分享切换为英文接收动态反馈

给定 *n* 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。

求在该柱状图中，能够勾勒出来的矩形的最大面积。

 

**示例 1:**

![img](https://assets.leetcode.com/uploads/2021/01/04/histogram.jpg)

```
输入：heights = [2,1,5,6,2,3]
输出：10
解释：最大的矩形为图中红色区域，面积为 10
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/01/04/histogram-1.jpg)

```
输入： heights = [2,4]
输出： 4
```

 

**提示：**

- `1 <= heights.length <=105`
- `0 <= heights[i] <= 104`

```
//核心思想就是找到一个柱形左边第一个小于它的，右边第一个小于它的，取长度乘以高度即可。
class Solution {
    public int largestRectangleArea(int[] heights) {
        int[] newHeights = new int[heights.length+2];
        System.arraycopy(heights, 0, newHeights, 1, heights.length);
        ArrayDeque<Integer> deque = new ArrayDeque<>();
        int max = 0;
        for(int i = 0; i<newHeights.length; i++){
            while(!deque.isEmpty()&& newHeights[i]<newHeights[deque.peekLast()]){
                int idx = deque.removeLast();
                int height = newHeights[idx];
                int len = i-deque.peekLast()-1;
                max = Math.max(max, height* len);
            }
            deque.addLast(i);
        }
        return  max;

    }
}
```



#### [85. 最大矩形](https://leetcode-cn.com/problems/maximal-rectangle/)

难度困难1182收藏分享切换为英文接收动态反馈

给定一个仅包含 `0` 和 `1` 、大小为 `rows x cols` 的二维二进制矩阵，找出只包含 `1` 的最大矩形，并返回其面积。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/09/14/maximal.jpg)

```
输入：matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]
输出：6
解释：最大矩形如上图所示。
```

**示例 2：**

```
输入：matrix = []
输出：0
```

**示例 3：**

```
输入：matrix = [["0"]]
输出：0
```

**示例 4：**

```
输入：matrix = [["1"]]
输出：1
```

**示例 5：**

```
输入：matrix = [["0","0"]]
输出：0
```

 

**提示：**

- `rows == matrix.length`
- `cols == matrix[0].length`
- `1 <= row, cols <= 200`
- `matrix[i][j]` 为 `'0'` 或 `'1'`

```
class Solution {
    public int maximalRectangle(char[][] matrix) {
        int[] heights = new int[matrix[0].length];
        int max = 0;
        for(int i = 0; i<matrix.length; i++){
            for(int j = 0; j<matrix[0].length; j++){
                if(matrix[i][j] == '1'){
                    heights[j] += 1;
                }
                else{
                    heights[j] = 0;
                }
            }
            int val = findArea(heights);
            max = Math.max(max, val);
        }
        return max;
    }
    private int findArea(int[] heights){
        int[] newHeights = new int[heights.length+2];
        System.arraycopy(heights, 0, newHeights, 1, heights.length);
        LinkedList<Integer> indexes = new LinkedList<>();
        int max = 0;
        for(int i = 0; i<newHeights.length; i++){
            while(!indexes.isEmpty() && newHeights[i]<newHeights[indexes.peekLast()]){
                int idx = indexes.removeLast();
                int h = newHeights[idx];
                int len = i- indexes.peekLast()-1;
                max = Math.max(max, len* h);
            }
            indexes.add(i);
        }
        return max;
    }
}
```

#### [120. 三角形最小路径和](https://leetcode-cn.com/problems/triangle/)

难度中等968收藏分享切换为英文接收动态反馈

给定一个三角形 `triangle` ，找出自顶向下的最小路径和。

每一步只能移动到下一行中相邻的结点上。**相邻的结点** 在这里指的是 **下标** 与 **上一层结点下标** 相同或者等于 **上一层结点下标 + 1** 的两个结点。也就是说，如果正位于当前行的下标 `i` ，那么下一步可以移动到下一行的下标 `i` 或 `i + 1` 。

 

**示例 1：**

```
输入：triangle = [[2],[3,4],[6,5,7],[4,1,8,3]]
输出：11
解释：如下面简图所示：
   2
  3 4
 6 5 7
4 1 8 3
自顶向下的最小路径和为 11（即，2 + 3 + 5 + 1 = 11）。
```

**示例 2：**

```
输入：triangle = [[-10]]
输出：-10
```

 

**提示：**

- `1 <= triangle.length <= 200`
- `triangle[0].length == 1`
- `triangle[i].length == triangle[i - 1].length + 1`
- `-104 <= triangle[i][j] <= 104`

 

**进阶：**

- 你可以只使用 `O(n)` 的额外空间（`n` 为三角形的总行数）来解决这个问题吗？



```java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        int len = triangle.size();
        int[] dp = new int[len+1];
        for(int i = len-1; i>=0; i--){
            for(int j = 0; j<=i; j++){
                dp[j] = Math.min(dp[j], dp[j+1])+triangle.get(i).get(j);
            }
        }
        return dp[0];
    }
}
```

