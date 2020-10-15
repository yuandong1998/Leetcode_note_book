# [不同的二叉搜索树 II](https://leetcode-cn.com/problems/unique-binary-search-trees-ii/)

题目：给定一个整数 *n*，生成所有由 1 ... *n* 为节点所组成的 **二叉搜索树** 。



## 方法一

**思路**

​		采用递归的方法，从[1,...,n]生成二叉搜索树，假设取i为根节点，则[1,...,i-1]为基础生成左子树，[i+1,...,n]为基础生成右子树。



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
    vector<TreeNode*> helper(int start,int end){
        vector<TreeNode*> tmp;
        if(start>end){
            tmp.push_back(NULL);
            return tmp;
        }
        for(int i=start;i<=end;i++){
            vector<TreeNode*> left_trees=helper(start,i-1);
            vector<TreeNode*> right_trees=helper(i+1,end);
            for(auto l:left_trees){
                for(auto r:right_trees){
                    TreeNode* cur=new TreeNode(i);
                    cur->left=l;
                    cur->right=r;
                    tmp.push_back(cur);
                }
            }
        }
        return tmp;
    }
    vector<TreeNode*> generateTrees(int n) {
        vector<TreeNode*> res;
        if(n==0){
            return res;
        }
        res=helper(1,n);
        return res;
    }
};
```



**复杂度分析**

* Time Complexity：$G_n$表示卡特兰数，也表示n个点生成二叉搜索树的数量，所以时间复杂为$O(nG_n)$，卡特兰数以$\frac{4^n}{n^{3/2}}$增长，所以时间复杂度为$O(\frac{4^n}{n^{1/2}})$

* Space Complexity：存储树需要$O(\frac{4^n}{n^{1/2}})$空间，递归需要$O(n)$的空间，所以总的空间复杂度为：$O(\frac{4^n}{n^{1/2}})$