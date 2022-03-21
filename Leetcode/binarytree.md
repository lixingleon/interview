## [606. 根据二叉树创建字符串](https://leetcode-cn.com/problems/construct-string-from-binary-tree/)

##### 简单前序遍历，if判断是否开启下层递归。

难度简单282

你需要采用前序遍历的方式，将一个二叉树转换成一个由括号和整数组成的字符串。

空节点则用一对空括号 "()" 表示。而且你需要省略所有不影响字符串与原始二叉树之间的一对一映射关系的空括号对。

**示例 1:**

```
输入: 二叉树: [1,2,3,4]
       1
     /   \
    2     3
   /    
  4     

输出: "1(2(4))(3)"

解释: 原本将是“1(2(4)())(3())”，
在你省略所有不必要的空括号对之后，
它将是“1(2(4))(3)”。

```

**示例 2:**

```
输入: 二叉树: [1,2,3,null,4]
       1
     /   \
    2     3
     \  
      4 

输出: "1(2()(4))(3)"

解释: 和第一个示例相似，
除了我们不能省略第一个对括号来中断输入和输出之间的一对一映射关系。

```

通过次数

46,386

提交次数

75,853

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
 //二叉树开启下层递归前其实可以if判断，不满足条件就不开启下层递归了。
class Solution {
    StringBuilder sb;
    public String tree2str(TreeNode root) {
        sb = new StringBuilder();
        if(root == null){
            return sb.toString();
        } 
        traverse(root);
        return sb.toString().substring(1,sb.length()-1);
    }
    private void traverse(TreeNode root){
        if(root == null){
            return;
        }
        sb.append("(");
        sb.append(root.val);
        if(root.left!= null){
            traverse(root.left);
        }
        else if(root.right!= null){
            sb.append("()");
        }
        if(root.right != null){
            traverse(root.right);
        }
        sb.append(")");
    }
}
```

