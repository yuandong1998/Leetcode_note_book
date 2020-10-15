# [416. Partition Equal Subset Sum](https://leetcode-cn.com/problems/partition-equal-subset-sum/)



**Q:** Given a **non-empty** array `nums` containing **only positive integers**, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.



**Constraints:**

- `1 <= nums.length <= 200`
- `1 <= nums[i] <= 100`



## Solution1

**Idea:**	参考[1]中的思想，将问题可以转换为**选取数字和恰好等于整个数组和的一半**，和传统的0-1背包问题比较相似，可以采用动态规划求解。

首先：

* 如果数组长度小于2不能划分，直接false
* 计算数组和`sum`与最大元素`maxnum`，如果`sum`是奇数直接false，如果`sum`是偶数，`target=sum/2`，如果`maxnum>target`，直接返回false

然后创建二维数组`dp`，有`n`行`target+1`列，**`dp[i][j]`表示从数据`nums[0,i]`范围内选取若干个正整数是否存在一种选取方案使得选取的正整数的和等于`j`。**

边界情况：

* 如果不选取数，则对所有的`0<=i<=n` 都有`dp[i][0]=true`
* 当`i==0`，只有一个数可以被选取，则`dp[0][nums[0]]=true`

状态转移方程：

* 如果`j>=nums[i]`，则当前数字`nums[i]`可以选取也可以不选取
  * 如果不选取：`dp[i][j]=dp[i-1][j]`
  * 如果选取：`dp[i][j]=dp[i-1][j-nums[i]]`
* 如果`j<nums[i]`，则当前数字无法选取，所以有`dp[i][j]=dp[i-1][j]`

最终`dp[n-1][target]`即为答案。

可以从代码一看出每一行的`dp`值都与上一行的`dp`值有关，并且对于每个值都和上一行自己位置左边的值相关，所以只需要一个一维数组，并且从**右向左更新**。



**Code:**

```C++
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int n = nums.size();
        if (n < 2) {
            return false;
        }
        int sum = accumulate(nums.begin(), nums.end(), 0);
        int maxNum = *max_element(nums.begin(), nums.end());
        if (sum & 1) {
            return false;
        }
        int target = sum / 2;
        if (maxNum > target) {
            return false;
        }
        vector<vector<int>> dp(n, vector<int>(target + 1, 0));
        for (int i = 0; i < n; i++) {
            dp[i][0] = true;
        }
        dp[0][nums[0]] = true;
        for (int i = 1; i < n; i++) {
            int num = nums[i];
            for (int j = 1; j <= target; j++) {
                if (j >= num) {
                    dp[i][j] = dp[i - 1][j] | dp[i - 1][j - num];
                } else {
                    dp[i][j] = dp[i - 1][j];
                }
            }
        }
        return dp[n - 1][target];
    }
};
```



```C++
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int n = nums.size();
        if (n < 2) {
            return false;
        }
        int sum = 0, maxNum = 0;
        for (auto& num : nums) {
            sum += num;
            maxNum = max(maxNum, num);
        }
        if (sum & 1) {
            return false;
        }
        int target = sum / 2;
        if (maxNum > target) {
            return false;
        }
        vector<int> dp(target + 1, 0);
        dp[0] = true;
        for (int i = 0; i < n; i++) {
            int num = nums[i];
            for (int j = target; j >= num; --j) {
                dp[j] |= dp[j - num];
            }
        }
        return dp[target];
    }
};
```



**Complexity Analysis:**

* Time Complexity :$O(n*target)$
* Space Complexity : $O(target)$

## Reference

[1] [Leetcode官方题解](https://leetcode-cn.com/problems/partition-equal-subset-sum/solution/fen-ge-deng-he-zi-ji-by-leetcode-solution/)