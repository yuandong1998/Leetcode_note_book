# [和为s的两个数字](https://leetcode-cn.com/problems/he-wei-sde-liang-ge-shu-zi-lcof/)

​		**Title:**输入一个递增排序的数组和一个数字s，在数组中查找两个数，使得它们的和正好是s。如果有多对数字的和等于s，则输出任意一对即可。



## 方法一

**思路**

​		双指针法。



**代码**

```C++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        int i=0;
        int j=nums.size()-1;
        while(i<j){
            if(nums[i]+nums[j]>target) j--;
            else if(nums[i]+nums[j]<target) i++;
            else break;
        }
        return {nums[i],nums[j]};
    }
};
```



**复杂度分析**

* 时间复杂度：$O(N)$，最差遍历一遍数组。
* 空间复杂度：$O(1)$。