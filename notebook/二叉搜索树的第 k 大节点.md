# [二叉搜索树的第k大节点](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)

Title：给定一棵二叉搜索树，请找出其中第k大的节点。



## 方法一

**思路**

​		二叉搜索树的中序遍历为从小到大的顺序遍历，所以问题转换为顺序遍历的倒数第k个值。



**代码**

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int kthLargest(TreeNode* root, int k) {
        stack<TreeNode*> S;
        vector<int> v;
        TreeNode* rt=root;
        while(rt||S.size()){
            while(rt){
                S.push(rt);
                rt=rt->left;
            }
            rt=S.top();S.pop();
            v.push_back(rt->val);
            rt=rt->right;
        }
        reverse(v.begin(),v.end());
        return v[k-1];
    }
};
```



**复杂度分析**

* 时间复杂度：$O(N)$，N个节点每个遍历一遍。
* 空间复杂度：$O(N)$，需要的vector大小为N。



## 方法二

**思路**

​		**提前返回**，按照“右-中-左”遍历树，没遍历一个节点k--，进行计数，直到k==0。



**代码**

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int kthLargest(TreeNode* root, int k) {
        stack<TreeNode*> S;
        TreeNode* rt=root;
        int res;
        while(rt||S.size()){
            while(rt){
                S.push(rt);
                rt=rt->right;
            }
            rt=S.top();S.pop();
            res=rt->val;
            rt=rt->left;
            k--;
            if(k==0) break;
        }
        return res;
    }
};
```



**复杂度分析**

* 时间复杂度：$O(N)$
* 空间复杂度：$O(N)$

