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

