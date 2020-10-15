# [链表中倒数第k个节点](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)

题目：输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。例如，一个链表有6个节点，从头节点开始，它们的值依次是1、2、3、4、5、6。这个链表的倒数第3个节点是值为4的节点。



## 方法一

**思路**

​		双指针法。



**代码**

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* getKthFromEnd(ListNode* head, int k) {
        ListNode* lnode=head;
        ListNode* rnode=head;
        for(int i=1;i<k;i++){
            rnode=rnode->next;
        }
        while(rnode->next!=NULL){
            rnode=rnode->next;
            lnode=lnode->next;
        }
        return lnode;
    }
};
```



**复杂度分析**

* 时间复杂度：$O(N)$
* 空间复杂度：$O(1)$