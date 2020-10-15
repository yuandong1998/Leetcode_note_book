# [142. Linked List Cycle II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)



**Q**:Given a linked list, return the node where the cycle begins. If there is no cycle, return `null`.There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to. Note that `pos` is not passed as a parameter.

**Notice** that you **should not modify** the linked list.

**Follow up**: Can you solve it using O(1)  (i.e. constant) memory?

**Constraints:**

* The number of the nodes in the list is in the range [0, 104].
* `-105 <= Node.val <= 105`
* `pos` is -1 or a valid index in the linked-list.



## 方法一

**Idea:**使用两个指针都起始于头部，`fast`一次两步，`slow`

一次一步，如果存在环，则最终会在环中相遇。任意时刻，`fast` 指针走过的距离都为 `slow` 指针的 2 倍。因此，我们有：
$$
a+(n+1)b+nc=2(a+b);a=c+(n-1)(b+c)
$$
这样从相遇点到入环点的距离加上 n-1 圈的环长，恰好等于从链表头部到入环点的距离。因此，当发现 `slow` 与 `fast`相遇时，我们再额外使用一个指针 `ptr`。起始，它指向链表头部；随后，它和 `slow` 每次向后移动一个位置。最终，它们会在入环点相遇。

![](https://assets.leetcode-cn.com/solution-static/142/142_fig1.png)

**Code:**

```C++
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode *slow = head, *fast = head;
        while (fast != nullptr) {
            slow = slow->next;
            if (fast->next == nullptr) {
                return nullptr;
            }
            fast = fast->next->next;
            if (fast == slow) {
                ListNode *ptr = head;
                while (ptr != slow) {
                    ptr = ptr->next;
                    slow = slow->next;
                }
                return ptr;
            }
        }
        return nullptr;
    }
};
```



**Complexity analysis:**

* Time Complexity: $O(N)$，在最初判断快慢指针是否相遇时，`slow` 指针走过的距离不会超过链表的总长度；随后寻找入环点时，走过的距离也不会超过链表的总长度。
* Space Complexity: $O(1)$

## Reference

[1] [Leetcode官方题解](https://leetcode-cn.com/problems/linked-list-cycle-ii/solution/huan-xing-lian-biao-ii-by-leetcode-solution/)