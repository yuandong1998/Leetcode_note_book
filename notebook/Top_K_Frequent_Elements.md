# [347. Top K Frequent Elements](https://leetcode-cn.com/problems/top-k-frequent-elements/)



Question: Given a non-empty array of integers, return the **k** most frequent elements.

Note: 

- You may assume k is always valid, 1 ≤ k ≤ number of unique elements.
- Your algorithm's time complexity must be better than $O(n log n)$, where n is the array's size.
- It's guaranteed that the answer is unique, in other words the set of the top k frequent elements is unique.
- You can return the answer in any order.



## 方法一

**Idea:** 首先遍历数组，记录每个数字出现的次数，生成一个【出现次数数组】，即需要找出【出现次数数组】的前k大的数。利用堆的思想，建立一个小根堆，遍历出现次数数组：

* 如果堆的元素个数小于 k，就可以直接插入堆中。
* 如果堆的元素个数等于 k，则检查堆顶与当前出现次数的大小。如果堆顶更大，说明至少有 k 个数字的出现次数比当前值大，故舍弃当前值；否则，就弹出堆顶，并将当前值插入堆中。


遍历完成后，堆中的元素就代表了「出现次数数组」中前 k 大的值。



**Code:**

```C++
class Solution {
public:
    struct cmp{
        bool operator()(pair<int, int>& m, pair<int, int>& n){
            return m.second > n.second;
        }
    };

    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> occurrences;
        for (auto& v : nums) {
            occurrences[v]++;
        }

        // pair 的第一个元素代表数组的值，第二个元素代表了该值出现的次数
        priority_queue<pair<int, int>, vector<pair<int, int>>, cmp> q;
        for (auto& [num, count] : occurrences) {
            if (q.size() == k) {
                if (q.top().second < count) {
                    q.pop();
                    q.emplace(num, count);
                }
            } else {
                q.emplace(num, count);
            }
        }
        vector<int> ret;
        while (!q.empty()) {
            ret.emplace_back(q.top().first);
            q.pop();
        }
        return ret;
    }
};
```



**Complexity Analysis:**

* Time Complexity: $O(Nlogk)$，堆的大小至多为 k，所以每次堆操作需要$log(k)$的时间，共需要$O(Nlogk)$

* Space Complexity: $O(N)$，哈希表大小$O(N)$，堆大小$O(k)$

  

## Reference

[1] [Leetcode官方题解](https://leetcode-cn.com/problems/top-k-frequent-elements/solution/qian-k-ge-gao-pin-yuan-su-by-leetcode-solution/)

[2] [priority_queue](http://www.cplusplus.com/reference/queue/priority_queue/)