# 回溯算法(Backtracking)

[TOC]

## 1. 回溯算法思想

回溯算法是对所有可能的情况进行一次遍历，可以想象决策树。回溯问题可以看作一个决策树遍历的过程，只需要考虑：

* 路径：已经做出的选择
* 选择列表：当前可以做的选择
* 结束条件：也就是到达决策树底层，无法再做选择的条件



## 2. 回溯算法框架

​		在每一步先判断是否满足结束条件，如果满足则返回，否则遍历所有的可以做的选择，在递归调用之前做选择，在递归调用之后撤销选择。

```
result = []
def backtrack(路径, 选择列表):
    if 满足结束条件:
        result.add(路径)
        return

    for 选择 in 选择列表:
        做选择(路径添加选择)
        backtrack(路径, 选择列表)
        撤销选择(路径撤销选择)
```



## 3. 全排列问题

题目：给定一个 **没有重复** 数字的序列，返回其所有可能的全排列。



可以将全排列问题看作从空开始每一步进行决策加入的数字，用一个数组`visit[i]`来表示数字`i`是否已经被访问。



```C++
class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {
    
        vector<bool> visit(nums.size(),false);
        vector<vector<int>> res;
        vector<int>  path;//路径
        HDFS(res,path,nums,visit);
        return res;
    }
    
    void HDFS( vector<vector<int>>  & res,vector<int> & path,vector<int> & nums, vector<bool> &visit)
    {
        if(path.size()>=nums.size())//满足结束条件
        {
            res.push_back(path);
            return;
        }
        for(int i=0;i<nums.size();i++)//遍历选择列表
        {
            if(!visit[i])//判断是否是可选的选择列表
            {
                path.push_back(nums[i]);//做选择
                visit[i]=true;
                HDFS(res,path,nums,visit);//递归
                path.pop_back();//撤销选择             
                visit[i]=false;    
            }   
        }  
    }
};
```





## 4. N皇后问题

题目：*n* 皇后问题研究的是如何将 *n* 个皇后放置在 *n*×*n* 的棋盘上，并且使皇后彼此之间不能相互攻击。



```C++
class Solution {
public:
    void solve(vector<vector<string>>& res, vector<string>& tmp, vector<bool>& cols_, vector<bool>& diag1s_, vector<bool>& diag2s_, int n, int row){
        if(row == n){//结束条件
            res.push_back(tmp);
            return;
        }
        for(auto col = 0; col < n; col++){//遍历列
            int ll = row + col; 
            int rr = row - col + n - 1;
            if (cols_[col] && diag1s_[ll] && diag2s_[rr]){//判断是否可以选择
                tmp[row][col] = 'Q';//选择
                cols_[col] = false;
                diag1s_[ll] = false;
                diag2s_[rr] = false;

                solve(res, tmp, cols_, diag1s_, diag2s_, n, row+1); //递归

                tmp[row][col] = '.';//撤销
                cols_[col] = true;
                diag1s_[ll] = true;
                diag2s_[rr] = true;
            }
        }
    }
    
    vector<vector<string>> solveNQueens(int n) {
        vector<vector<string>> res;
        vector<string> tmp(n, string(n, '.'));
        vector<bool> cols_(n, true); //判断列是否可以
        vector<bool> diag1s_(2*n-1, true);//判断斜线方向是否可以
        vector<bool> diag2s_(2*n-1, true);//判断斜线方向是否可以
        solve(res, tmp, cols_, diag1s_, diag2s_, n, 0);
        return res;
    }
    
};
```



## Reference

[1] [回溯算法解题套路框架](https://labuladong.gitbook.io/algo/di-ling-zhang-bi-du-xi-lie/hui-su-suan-fa-xiang-jie-xiu-ding-ban)

