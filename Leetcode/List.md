#### [2. 两数相加](https://leetcode-cn.com/problems/add-two-numbers/)

给你两个 非空 的链表，表示两个非负的整数。它们每位数字都是按照 逆序 的方式存储的，并且每个节点只能存储 一位 数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0 开头。

 

示例 1：


输入：l1 = [2,4,3], l2 = [5,6,4]
输出：[7,0,8]
解释：342 + 465 = 807.
示例 2：

输入：l1 = [0], l2 = [0]
输出：[0]
示例 3：

输入：l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
输出：[8,9,9,9,0,0,0,1]


提示：

每个链表中的节点数在范围 [1, 100] 内
0 <= Node.val <= 9
题目数据保证列表表示的数字不含前导零
通过次数1,118,951提交次数2,716,897

**递归和迭代两种方法**

```java
迭代
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(-1);
        ListNode p = dummy;
        int carry = 0;
        while(l1!= null || l2!= null){
            if(l1!= null){
                carry+= l1.val;
                l1 = l1.next;
            }
            if(l2!= null){
                carry+= l2.val;
                l2 = l2.next;
            }
            int curVal = carry%10;
            carry = carry/10;
            p.next = new ListNode(curVal);
            p = p.next;
        }
        if(carry == 1){
            p.next = new ListNode(1);
        }
        return dummy.next;
    }
    
}
递归
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        return add(l1, l2, 0);
    }
    private ListNode add(ListNode l1, ListNode l2, int carry){
        if(l1 == null && l2 == null && carry == 0){
            return null;
        }
        if(l1 != null){
            carry+= l1.val;
            //这一步是关键。将l1指针后移一位。在移动l1和l2指针后，再调用下层递归。
            l1 = l1.next;
        }
        if(l2 != null){
            carry+= l2.val;
            l2 = l2.next;
        }
        //每层递归做的事：创建一个node，并把node之后的所有节点都创建好，再返回node。
        ListNode node = new ListNode(carry%10);
        node.next = add(l1, l2, carry/10);
        return node;
    }
}
```



#### [24. 两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

给你一个链表，两两交换其中相邻的节点，并返回交换后链表的头节点。你必须在不修改节点内部的值的情况下完成本题（即，只能进行节点交换）。

 

示例 1：


输入：head = [1,2,3,4]
输出：[2,1,4,3]
示例 2：

输入：head = []
输出：[]
示例 3：

输入：head = [1]
输出：[1]


提示：

链表中节点的数目在范围 [0, 100] 内
0 <= Node.val <= 100
通过次数357,321提交次数507,111

```java
递归
class Solution {
    public ListNode swapPairs(ListNode head) {
        if(head == null || head.next == null){
            return head;
        }
        ListNode tail = head.next.next;
        ListNode newHead = head.next;
        newHead.next = head;
        head.next = swapPairs(tail);
        return newHead;
    }
}
迭代: 穿针引线法。
```



#### [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

给你单链表的头节点 head ，请你反转链表，并返回反转后的链表。


示例 1：


输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]
示例 2：


输入：head = [1,2]
输出：[2,1]
示例 3：

输入：head = []
输出：[]


提示：

链表中节点的数目范围是 [0, 5000]
-5000 <= Node.val <= 5000


进阶：链表可以选用迭代或递归方式完成反转。你能否用两种方法解决这道题？

**递归反转链表：先递归到最底层。再将最底层的值慢慢return到最外面。**

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if(head == null || head.next == null){
            return head;
        }
        ListNode newHead = reverseList(head.next);
        head.next.next =head;
        head.next = null;
        return newHead;
    }
}
```



#### [141. 环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)

给你一个链表的头节点 `head` ，判断链表中是否有环。

如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 `pos` 来表示链表尾连接到链表中的位置（索引从 0 开始）。如果 `pos` 是 `-1`，则在该链表中没有环。**注意：`pos` 不作为参数进行传递**，仅仅是为了标识链表的实际情况。

如果链表中存在环，则返回 `true` 。 否则，返回 `false` 。

 

**示例 1：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist.png)

```
输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。
```

**示例 2：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test2.png)

```
输入：head = [1,2], pos = 0
输出：true
解释：链表中有一个环，其尾部连接到第一个节点。
```

**示例 3：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test3.png)

```
输入：head = [1], pos = -1
输出：false
解释：链表中没有环。
```

 

**提示：**

- 链表中节点的数目范围是 `[0, 104]`
- `-105 <= Node.val <= 105`
- `pos` 为 `-1` 或者链表中的一个 **有效索引** 。

 

**进阶：**你能用 `O(1)`（即，常量）内存解决此问题吗？

1. HashSet去重法
2. 快慢指针

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        if(head == null || head.next == null){
            return false;
        }
        ListNode fast = head;
        ListNode slow = head;
        while(fast!= null&& fast.next!= null){
            fast = fast.next.next;
            slow = slow.next;
            if(fast == slow){
                return true;
            }
        }
        return false;
    }
}
```



#### [142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

难度中等1307收藏分享切换为英文接收动态反馈

给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 `null`。

如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 `pos` 来表示链表尾连接到链表中的位置（索引从 0 开始）。如果 `pos` 是 `-1`，则在该链表中没有环。**注意：`pos` 不作为参数进行传递**，仅仅是为了标识链表的实际情况。

**不允许修改** 链表。



 

**示例 1：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist.png)

```
输入：head = [3,2,0,-4], pos = 1
输出：返回索引为 1 的链表节点
解释：链表中有一个环，其尾部连接到第二个节点。
```

**示例 2：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test2.png)

```
输入：head = [1,2], pos = 0
输出：返回索引为 0 的链表节点
解释：链表中有一个环，其尾部连接到第一个节点。
```

**示例 3：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test3.png)

```
输入：head = [1], pos = -1
输出：返回 null
解释：链表中没有环。
```

 

**提示：**

- 链表中节点的数目范围在范围 `[0, 104]` 内
- `-105 <= Node.val <= 105`
- `pos` 的值为 `-1` 或者链表中的一个有效索引

 

**进阶：**你是否可以使用 `O(1)` 空间解决此题？



```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        if(head == null || head.next == null){
            return null;
        }
        ListNode fast = head;
        ListNode slow = head;
        while(fast!= null && fast.next != null){
            fast = fast.next.next;
            slow = slow.next;
            if(fast == slow){
                //fast = 2*slow
                //fast = slow+n*b
                //slow = n*b
                fast = head;
                while(fast!= slow){
                    fast = fast.next;
                    slow = slow.next;
                }
                return fast;
            }
        }
        return null;
    }
}
```



#### [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

难度简单2126收藏分享切换为英文接收动态反馈

将两个升序链表合并为一个新的 **升序** 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/03/merge_ex1.jpg)

```
输入：l1 = [1,2,4], l2 = [1,3,4]
输出：[1,1,2,3,4,4]
```

**示例 2：**

```
输入：l1 = [], l2 = []
输出：[]
```

**示例 3：**

```
输入：l1 = [], l2 = [0]
输出：[0]
```

 

**提示：**

- 两个链表的节点数目范围是 `[0, 50]`
- `-100 <= Node.val <= 100`
- `l1` 和 `l2` 均按 **非递减顺序** 排列

通过次数813,791

提交次数1,220,370



```java
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        if(list1 == null){
            return list2;
        }
        if(list2 == null){
            return list1;
        }
        if(list1.val<= list2.val){
            list1.next = mergeTwoLists(list1.next, list2);
            return list1;
        }
        else{
            list2.next = mergeTwoLists(list1, list2.next);
            return list2;
        }
    }
}
```

#### [23. 合并K个升序链表](https://leetcode-cn.com/problems/merge-k-sorted-lists/)

难度困难1676收藏分享切换为英文接收动态反馈

给你一个链表数组，每个链表都已经按升序排列。

请你将所有链表合并到一个升序链表中，返回合并后的链表。

 

**示例 1：**

```
输入：lists = [[1,4,5],[1,3,4],[2,6]]
输出：[1,1,2,3,4,4,5,6]
解释：链表数组如下：
[
  1->4->5,
  1->3->4,
  2->6
]
将它们合并到一个有序链表中得到。
1->1->2->3->4->4->5->6
```

**示例 2：**

```
输入：lists = []
输出：[]
```

**示例 3：**

```
输入：lists = [[]]
输出：[]
```

 

**提示：**

- `k == lists.length`
- `0 <= k <= 10^4`
- `0 <= lists[i].length <= 500`
- `-10^4 <= lists[i][j] <= 10^4`
- `lists[i]` 按 **升序** 排列
- `lists[i].length` 的总和不超过 `10^4`

通过次数368,432

提交次数655,233

**分治法**

```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if(lists.length == 0){
            return null;
        }
        return divide(lists, 0, lists.length-1);
    }
    private ListNode divide(ListNode[] lists, int start, int end){
        if(start>=end){
            return lists[start];
        }
        int mid = (end-start)/2+start;
        ListNode upper = divide(lists, start, mid);
        ListNode lower = divide(lists, mid+1, end);
        return merge(upper, lower);
    }
    private ListNode merge(ListNode list1, ListNode list2){
        if(list1 == null){
            return list2;
        }
        if(list2 == null){
            return list1;
        }
        if(list1.val<= list2.val){
            list1.next = merge(list1.next, list2);
            return list1;
        }
        else{
            list2.next = merge(list1, list2.next);
            return list2;
        }
    }
}
```



#### [147. 对链表进行插入排序](https://leetcode-cn.com/problems/insertion-sort-list/)

难度中等468收藏分享切换为英文接收动态反馈

对链表进行插入排序。
插入排序的动画演示如上。从第一个元素开始，该链表可以被认为已经部分排序（用黑色表示）。
每次迭代时，从输入数据中移除一个元素（用红色表示），并原地将其插入到已排好序的链表中。

 

**插入排序算法：**

1. 插入排序是迭代的，每次只移动一个元素，直到所有元素可以形成一个有序的输出列表。
2. 每次迭代中，插入排序只从输入数据中移除一个待排序的元素，找到它在序列中适当的位置，并将其插入。
3. 重复直到所有输入数据插入完为止。

 

**示例 1：**

```
输入: 4->2->1->3
输出: 1->2->3->4
```

**示例 2：**

```
输入: -1->5->3->4->0
输出: -1->0->3->4->5
```

通过次数107,861

提交次数157,673



```java
class Solution {
    public ListNode insertionSortList(ListNode head) {
        if(head == null || head.next == null){
            return head;
        }
        ListNode dummy = new ListNode(-1, head);
        //outerP.next是待插入的节点p。
        ListNode outerP = head;
        while(outerP.next!= null){
            //提前判断，如果p比前一个节点大，则直接跳过内层循环
            //这一步是为了剪枝,提升效率
            if(outerP.next.val>outerP.val){
                outerP = outerP.next;
                continue;
            }
            ListNode innerP = dummy;
            //innerP.next用来找第一个比p大的节点。
            while(innerP!= outerP){
                if(innerP.next.val>outerP.next.val){
                    innerP.next = new ListNode(outerP.next.val, innerP.next);
                    outerP.next = outerP.next.next;
                    break;
                }
                innerP = innerP.next;
            }
            if(innerP == outerP){
                outerP = outerP.next;
            }
        }
        return dummy.next;


    }
}
```



#### [148. 排序链表](https://leetcode-cn.com/problems/sort-list/)

难度中等1426收藏分享切换为英文接收动态反馈

给你链表的头结点 `head` ，请将其按 **升序** 排列并返回 **排序后的链表** 。



 

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

```java
归并排序

class Solution {
    public ListNode sortList(ListNode head) {
        if(head == null || head.next == null){
            return head;
        }
        return mergeSort(head);
    }
    private ListNode mergeSort(ListNode head){
        if(head.next == null){
            return head;
        }
        ListNode fast = head;
        ListNode slow = head;
        ListNode pre = null;
        while(fast!= null && fast.next!= null){
            pre = slow;
            fast = fast.next.next;
            slow = slow.next;
        }
        //把链表一分为二
        pre.next = null;
        ListNode left = mergeSort(head);
        ListNode right = mergeSort(slow);
        return merge(left, right);

    }
    private ListNode merge(ListNode left, ListNode right){
        if(left == null){
            return right;
        }
        if(right == null){
            return left;
        }
        if(left.val<= right.val){
            left.next = merge(left.next, right);
            return left;
        }
        else{
            right.next = merge(left, right.next);
            return right;
        }

    }
}
```



#### [707. 设计链表](https://leetcode-cn.com/problems/design-linked-list/)

难度中等336收藏分享切换为英文接收动态反馈

设计链表的实现。您可以选择使用单链表或双链表。单链表中的节点应该具有两个属性：`val` 和 `next`。`val` 是当前节点的值，`next` 是指向下一个节点的指针/引用。如果要使用双向链表，则还需要一个属性 `prev` 以指示链表中的上一个节点。假设链表中的所有节点都是 0-index 的。

在链表类中实现这些功能：

- get(index)：获取链表中第 `index` 个节点的值。如果索引无效，则返回`-1`。
- addAtHead(val)：在链表的第一个元素之前添加一个值为 `val` 的节点。插入后，新节点将成为链表的第一个节点。
- addAtTail(val)：将值为 `val` 的节点追加到链表的最后一个元素。
- addAtIndex(index,val)：在链表中的第 `index` 个节点之前添加值为 `val` 的节点。如果 `index` 等于链表的长度，则该节点将附加到链表的末尾。如果 `index` 大于链表长度，则不会插入节点。如果`index`小于0，则在头部插入节点。
- deleteAtIndex(index)：如果索引 `index` 有效，则删除链表中的第 `index` 个节点。

 

**示例：**

```
MyLinkedList linkedList = new MyLinkedList();
linkedList.addAtHead(1);
linkedList.addAtTail(3);
linkedList.addAtIndex(1,2);   //链表变为1-> 2-> 3
linkedList.get(1);            //返回2
linkedList.deleteAtIndex(1);  //现在链表是1-> 3
linkedList.get(1);            //返回3
```

 

**提示：**

- 所有`val`值都在 `[1, 1000]` 之内。
- 操作次数将在 `[1, 1000]` 之内。
- 请不要使用内置的 LinkedList 库。

通过次数78,453

提交次数240,407

```java
class MyLinkedList {
    class ListNode{
        int val;
        ListNode next;
        public ListNode(int val, ListNode next){
            this.val = val;
            this.next = next;
        }
        public ListNode(int val){
            this.val = val;
        }
        public ListNode(){

        }
    }
    ListNode dummy;
    ListNode tail;
    public MyLinkedList() {
        dummy = new ListNode(-1);
        tail = dummy;
    }
    
    public int get(int index) {
        if(dummy.next == null){
            return -1;
        }
        ListNode p = dummy;
        while(p!= null && index>=0){
            p = p.next;
            index--;
        }
        if(p == null){
            return -1;
        }

        return p.val;
    }
    
    public void addAtHead(int val) {
        dummy.next = new ListNode(val, dummy.next);
        if(tail == dummy){
            tail = tail.next;
        }
    }
    
    public void addAtTail(int val) {
        tail.next = new ListNode(val);
        tail = tail.next;
    }
    
    public void addAtIndex(int index, int val) {
        if(index<=0){
            addAtHead(val);
            return;
        }
        ListNode p = dummy;
        while(p.next!= null && index>0){
            p = p.next;
            index--;
        }
        if(p.next == null && index>0){
            return;
        }
        else if (p.next == null && index == 0){
            addAtTail(val);
        }
        else{
            p.next = new ListNode(val, p.next);
        }
    }
    
    public void deleteAtIndex(int index) {
        if(index<0){
            return;
        }
        ListNode p = dummy;
        while(p.next!= null && index>0){
            p = p.next;
            index--;
        }
        if(p.next == null){
            return;
        }
        if(p.next.next == null){
            tail = p;
            p.next = null;
        }
        else{
            p.next = p.next.next;
        }
    }
}
```

