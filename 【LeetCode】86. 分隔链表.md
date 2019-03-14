#### 【LeetCode】86. 分隔链表

##### 题目

给定一个链表和一个特定值 *x*，对链表进行分隔，使得所有小于 *x* 的节点都在大于或等于 *x* 的节点之前。

你应当保留两个分区中每个节点的初始相对位置。

**示例:**

```
输入: head = 1->4->3->2->5->2, x = 3
输出: 1->2->2->4->3->5
```

##### 思路

​        题目要求小于x的节点放在链表的前部分，其他的放在链表的后部分，同时保持两部分中的相对顺序。这样，我们可以想到用两个链表来存储。遍历原链表，当值小于x时放在第一个链表，大于或等于x时放在第二个链表，再把这两个链表首位连接即可。

​        因为追加节点时需知道两个链表的尾指针，又需要对两个链表的头指针进行记录。因此过程中创建4个记录指针。

#### 解答代码：

```
class Solution {
    public ListNode partition(ListNode head, int x) {
        if(head == null || head.next == null){
            return head;
        }
        //第一条链表，同时用它来记录第一条链表的尾指针
        ListNode preList = new ListNode(-1);
        //第二条链表，同时用它来记录第二条链表的头指针
        ListNode postList = new ListNode(-1);
        //记录第一条链表的头指针，也是结果
        ListNode result = preList;
        //记录第二条链表的尾指针
        ListNode tail = postList;
        while(head != null){
            ListNode current = head;
            if(current.val < x){
                //连到第一条链表
                preList.next = current;
                head = head.next;
                //第一条链表尾指针移动
                preList = preList.next;
            } else{
                //连到第二条链表
                tail.next = current;
                head = head.next;
                //第二条链表尾指针移动
                tail = tail.next;
                //尾指针的next需保证为null，因为它可能是最后一个元素
                tail.next = null;
            }
        }
        //第一条链表的尾指针和第二条链表的头指针连接
        preList.next = postList.next;
        return result.next;
    }
}
```

