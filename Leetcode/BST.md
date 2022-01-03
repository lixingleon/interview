#### [230. 二叉搜索树中第K小的元素](https://leetcode-cn.com/problems/kth-smallest-element-in-a-bst/)

给定一个二叉搜索树的根节点 root ，和一个整数 k ，请你设计一个算法查找其中第 k 个最小元素（从 1 开始计数）。

 

示例 1：


输入：root = [3,1,4,null,2], k = 1
输出：1
示例 2：


输入：root = [5,3,6,2,4,null,null,1], k = 3
输出：3




提示：

树中的节点数为 n 。
1 <= k <= n <= 104
0 <= Node.val <= 104


进阶：如果二叉搜索树经常被修改（插入/删除操作）并且你需要频繁地查找第 k 小的值，你将如何优化算法？



```java
class Solution {
    int count = 0;
    int num = 0;
    public int kthSmallest(TreeNode root, int k) {
        traverse(root, k);
        return num;
    }
    private void traverse(TreeNode root, int k){
        if(root == null){
            return;
        }
        //加一个if判断条件，实现BST递归过程中的强制返回。
        if(count == k){
            return;
        }
        traverse(root.left, k);
        count++;
        if(count == k){
            num = root.val;
        }
        traverse(root.right, k);
    }
}
```



#### [99. 恢复二叉搜索树](https://leetcode-cn.com/problems/recover-binary-search-tree/)

给你二叉搜索树的根节点 root ，该树中的两个节点的值被错误地交换。请在不改变其结构的情况下，恢复这棵树。

 

示例 1：


输入：root = [1,3,null,null,2]
输出：[3,1,null,null,2]
解释：3 不能是 1 左孩子，因为 3 > 1 。交换 1 和 3 使二叉搜索树有效。
示例 2：


输入：root = [3,1,4,null,null,2]
输出：[2,1,4,null,null,3]
解释：2 不能在 3 的右子树中，因为 2 < 3 。交换 2 和 3 使二叉搜索树有效。


提示：

树上节点的数目在范围 [2, 1000] 内
-231 <= Node.val <= 231 - 1


进阶：使用 O(n) 空间复杂度的解法很容易实现。你能想出一个只使用 O(1) 空间的解决方案吗？

**TreeNode引用也可以当成员变量。**

```java
class Solution {
    //TreeNode引用也可以当成员变量。
    TreeNode node1, node2, pre;
    public void recoverTree(TreeNode root) {
        traverse(root);
        int tmp = node1.val;
        node1.val = node2.val;
        node2.val = tmp;
    }
    private void traverse(TreeNode root){
        if(root == null){
            return;
        }
        traverse(root.left);
        //中序遍历。找到降序对。
        //如果只有一个降序对，就交换彼此位置。
        //如果有两个降序对，交换前一个降序对的第一个和后一个降序对的第二个。
        if(pre!=null && pre.val>root.val){
            if(node1 == null){
                node1 = pre;
            }
            node2 = root;
        }
        pre = root;
        traverse(root.right);    
    }
}
```



#### [108. 将有序数组转换为二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/)

**警惕求数组mid的错误解法！！！**

给你一个整数数组 nums ，其中元素已经按 升序 排列，请你将其转换为一棵 高度平衡 二叉搜索树。

高度平衡 二叉树是一棵满足「每个节点的左右两个子树的高度差的绝对值不超过 1 」的二叉树。

 

示例 1：


输入：nums = [-10,-3,0,5,9]
输出：[0,-3,9,-10,null,5]
解释：[0,-10,5,null,-3,null,9] 也将被视为正确答案：

示例 2：


输入：nums = [1,3]
输出：[3,1]
解释：[1,3] 和 [3,1] 都是高度平衡二叉搜索树。


提示：

1 <= nums.length <= 104
-104 <= nums[i] <= 104
nums 按 严格递增 顺序排列

```java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        return buildBST(nums, 0, nums.length-1);
    }
    private TreeNode buildBST(int[] nums, int start, int end){
        if(start>end){
            return null;
        }
        //这个做法是错误的
        //int mid_idx = (end-start+1)/2;
        int mid_idx = (end-start)
        TreeNode root = new TreeNode(nums[mid_idx]);
        System.out.println(root.val);
        root.left = buildBST(nums, start, mid_idx-1);
        root.right = buildBST(nums, mid_idx+1, end);
        return root;

    }
}
```



#### [*501. 二叉搜索树中的众数](https://leetcode-cn.com/problems/find-mode-in-binary-search-tree/)

给定一个有相同值的二叉搜索树（BST），找出 BST 中的所有众数（出现频率最高的元素）。

假定 BST 有如下定义：

结点左子树中所含结点的值小于等于当前结点的值
结点右子树中所含结点的值大于等于当前结点的值
左子树和右子树都是二叉搜索树
例如：
给定 BST [1,null,2,2],

   1
    \
     2
    /
   2
返回[2].

提示：如果众数超过1个，不需考虑输出顺序

进阶：你可以不使用额外的空间吗？（假设由递归产生的隐式调用栈的开销不被计算在内）



**只遍历一遍数组就可以找出所有众数（出现频率最高的数）的方法 ,需要重点掌握！设置三个变量：pre，curCount， maxCount** 



```java
class Solution {
    int pre, curCount, maxCount;
    List<Integer> ans_list = new ArrayList<>();
    public int[] findMode(TreeNode root) {
        traverse(root);
        int[] ans = new int[ans_list.size()];
        for(int i = 0; i<ans_list.size(); i++){
            ans[i] = ans_list.get(i);
        } 
        return ans;
    }
    private void traverse(TreeNode root){
        if(root == null){
            return;
        }
        traverse(root.left);
        if(root.val == pre){
            curCount++;
        }
        else{
            curCount = 1;
            pre = root.val;
        }
        if(curCount>maxCount){
            maxCount = curCount;
            ans_list.clear();
            ans_list.add(root.val);
        }
        else if(curCount == maxCount){
            ans_list.add(root.val);
        }
        traverse(root.right);
    }
}
```



#### *[450. 删除二叉搜索树中的节点](https://leetcode-cn.com/problems/delete-node-in-a-bst/)

给定一个二叉搜索树的根节点 root 和一个值 key，删除二叉搜索树中的 key 对应的节点，并保证二叉搜索树的性质不变。返回二叉搜索树（有可能被更新）的根节点的引用。

一般来说，删除节点可分为两个步骤：

首先找到需要删除的节点；
如果找到了，删除它。


示例 1:



输入：root = [5,3,6,2,4,null,7], key = 3
输出：[5,4,6,2,null,null,7]
解释：给定需要删除的节点值是 3，所以我们首先找到 3 这个节点，然后删除它。
一个正确的答案是 [5,4,6,2,null,null,7], 如下图所示。
另一个正确答案是 [5,2,6,null,4,null,7]。


示例 2:

输入: root = [5,3,6,2,4,null,7], key = 0
输出: [5,3,6,2,4,null,7]
解释: 二叉树不包含值为 0 的节点
示例 3:

输入: root = [], key = 0
输出: []


提示:

节点数的范围 [0, 104].
-105 <= Node.val <= 105
节点值唯一
root 是合法的二叉搜索树
-105 <= key <= 105


进阶： 要求算法时间复杂度为 O(h)，h 为树的高度。

**使用迭代可以找到左子树中的最大值！**

```java
class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        if(root == null){
            return root;
        }
        
        if(root.val == key){
            return delete(root);
        }
        if(root.val>key){
            root.left = deleteNode(root.left, key);
        }
        if(root.val<key){
            root.right = deleteNode(root.right, key);
        }
        return root;

    }
    //用左子树中的最大值来顶替。
    private TreeNode delete(TreeNode root){
        if(root.left == null){
            return root.right;
        } 
        if(root.right == null){
            return root.left;
        }
        //用迭代找到左子树中的最大值
        TreeNode p = root.left;
        while(p.right!= null){
            p = p.right;
        }
        //交换root跟左子树中的最大值
        int tmp = p.val;
        p.val = root.val;
        root.val = tmp;
        //重复利用主函数，把换下去的节点删掉
        root.left = deleteNode(root.left, p.val);
        return root;
    }
   
}
```

