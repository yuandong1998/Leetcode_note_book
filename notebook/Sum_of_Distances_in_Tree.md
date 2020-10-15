#### [Sum of Distances in Tree](https://leetcode-cn.com/problems/sum-of-distances-in-tree/)



Question ：An undirected, connected tree with `N` nodes labelled `0...N-1` and `N-1` edges are given.

The `i`th edge connects nodes `edges[i][0]` and `edges[i][1]` together.

Return a list `ans`, where `ans[i]` is the sum of the distances between node `i` and all other nodes.



## 方法一

**Idea**

​	首先定义

* $dp[u]$ 表示以 u 为根的子树，它的所有子节点到它的距离之和， 
* $sz[u]$  表示以 u 为根的子树的节点数量



​	可得转移方程：$dp[u]=\sum_{v\in son(u)} dp[v]+sz[v]$。

​	以u节点为根节点至下而上递推到根节点可以得到`dp[root]`，这时我们将v节点转为根节点，u节点转为v节点的孩子节点。`dp[u]`需减去$v$节点的贡献。
$$
dp[u]=dp[u]-(dp[v]+sz[v])
$$
​	`dp[v]`加上u节点的贡献。
$$
dp[v]=dp[v]+(dp[U]+sz[u])
$$
​	这样就完成了一次换根操作。



**Code**

```C++
class Solution {
public:
    vector<int> ans, sz, dp;
    vector<vector<int>> graph;

    void dfs(int u, int f) {
        sz[u] = 1;
        dp[u] = 0;
        for (auto& v: graph[u]) {
            if (v == f) {
                continue;
            }
            dfs(v, u);
            dp[u] += dp[v] + sz[v];
            sz[u] += sz[v];
        }
    }

    void dfs2(int u, int f) {
        ans[u] = dp[u];
        for (auto& v: graph[u]) {
            if (v == f) {
                continue;
            }
            //记录dp和sz
            int pu = dp[u], pv = dp[v];
            int su = sz[u], sv = sz[v];
			//换根
            dp[u] -= dp[v] + sz[v];
            sz[u] -= sz[v];
            dp[v] += dp[u] + sz[u];
            sz[v] += sz[u];
			//遍历
            dfs2(v, u);
			//遍历结束，变回原有的值
            dp[u] = pu, dp[v] = pv;
            sz[u] = su, sz[v] = sv;
        }
    }

    vector<int> sumOfDistancesInTree(int N, vector<vector<int>>& edges) {
        ans.resize(N, 0);
        sz.resize(N, 0);
        dp.resize(N, 0);
        graph.resize(N, {});
        //构建图
        for (auto& edge: edges) {
            int u = edge[0], v = edge[1];
            graph[u].emplace_back(v);
            graph[v].emplace_back(u);
        }
        dfs(0, -1);
        dfs2(0, -1);
        return ans;
    }
};
```



**Complexity analysis**

* Time Complexity：$O(N)$，遍历树两次。
* Space Complexity：$O(N)$



## Reference

[1] [Leetcode官方题解](https://leetcode-cn.com/problems/sum-of-distances-in-tree/solution/shu-zhong-ju-chi-zhi-he-by-leetcode-solution/)



