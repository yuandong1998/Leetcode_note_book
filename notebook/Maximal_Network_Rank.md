# [5536. Maximal Network Rank](https://leetcode-cn.com/problems/maximal-network-rank/)



**Q**：There is an infrastructure of n cities with some number of roads connecting these cities. Each `roads[i] = [ai, bi]` indicates that there is a bidirectional road between cities `ai` and `bi`.

The **network rank** of **two different cities** is defined as the total number of directly connected roads to either city. If a road is directly connected to both cities, it is only counted once.

The **maximal network rank** of the infrastructure is the **maximum network rank** of all pairs of different cities.

Given the integer `n` and the array roads, return the **maximal network rank** of the entire infrastructure.

**Constraints:**

* `2 <= n <= 100`
* `0 <= roads.length <= n * (n - 1) / 2`
* `roads[i].length == 2`
* `0 <= ai, bi <= n-1`
* `ai != bi`
* Each pair of cities has at most one road connecting them.



## 方法一

**Idea :** 题目可以理解为，找出图中任意两个点，以其中至少一个为端点的不重复边的数目的最大值。先把边集转为邻接表，然后枚举，先确定一个点，然后枚举与其相邻的点，再枚举剩余的点。



**Code :**

```C++
class Solution {
public:
    int maximalNetworkRank(int n, vector<vector<int>>& roads) {
        int ans = 0;
        vector<vector<int>> adj(n);
        for (auto road : roads)
            adj[road[0]].emplace_back(road[1]), adj[road[1]].emplace_back(road[0]);
        for (int i = 0; i < n; ++i) {
            vector<bool> v(n);
            for (int j : adj[i]) {
                ans = max(ans, (int)adj[i].size() + (int)adj[j].size() - 1);
                v[j] = true;
            }
            for (int j = i + 1; j < n; ++j)
                if (!v[j])
                    ans = max(ans, (int)adj[i].size() + (int)adj[j].size());
        }
        return ans;
    }
};
```



**Complexity Analysis :**

* Time complexity :$O(N^2)$
* Space complexity : $O(N)$



## Reference

[1] [Leetcode题解：简单的枚举](https://leetcode-cn.com/problems/maximal-network-rank/solution/jian-dan-de-mei-ju-by-lucifer1004/)