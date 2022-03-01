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
