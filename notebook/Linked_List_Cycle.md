# [Linked List Cycle](https://leetcode-cn.com/problems/linked-list-cycle/)



Question: Given `head`, the head of a linked list, determine if the linked list has a cycle in it .There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally , `pos` is used to denote the index of the node that tail's next pointer is connected to. Note that `pos` is not passed as a parameter. Return `true` if there is a cycle in the linked list. Otherwise, return `false`.

Follow up: Can you solve it using `O(1)`  (i.e. constant) memory?



## 方法一

**Idea:** 采用 Floyed 判圈算法的思想，生成一个快指针一个慢指针，快指针在慢指针的前面，快指针一次前进两个单位，慢指针一次一个单位，如果有环快慢指针一定会在某个时刻相遇。



**Code:**

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
    bool hasCycle(ListNode* head) {
        if (head == nullptr || head->next == nullptr) {
            return false;
        }
        ListNode* slow = head;
        ListNode* fast = head->next;
        while (slow != fast) {
            if (fast == nullptr || fast->next == nullptr) {
                return false;
            }
            slow = slow->next;
            fast = fast->next->next;
        }
        return true;
    }
};
```



**Complexity Analysis:**

* Time Complexity: $O(N)$，如果无环快指针先到达尾部，每个节点至多访问两次；如果有环，每一次移动距离减一，所以至多N轮。
* Space Complexity: $O(1)$



## Reference

[1] [Floyd判圈算法](https://zh.wikipedia.org/wiki/Floyd%E5%88%A4%E5%9C%88%E7%AE%97%E6%B3%95)

[2] [力扣官方题解](https://leetcode-cn.com/problems/linked-list-cycle/solution/huan-xing-lian-biao-by-leetcode-solution/)

