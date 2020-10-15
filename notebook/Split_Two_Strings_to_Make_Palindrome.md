# [1616. Split Two Strings to Make Palindrome](https://leetcode-cn.com/problems/split-two-strings-to-make-palindrome/)



**Q:** You are given two strings `a` and `b` of the same length. Choose an index and split both strings **at the same index**, splitting `a` into two strings: `aprefix` and `asuffix` where `a = aprefix + asuffix`, and splitting `b` into two strings: `bprefix `and `bsuffix` where `b = bprefix + bsuffix`. Check if `aprefix + bsuffix` or `bprefix + asuffix` forms a palindrome.

When you split a string `s` into `sprefix` and `ssuffix`, either `ssuffix` or `sprefix` is allowed to be empty. For example, if `s = "abc"`, then `"" + "abc"`, `"a" + "bc"`, `"ab" + "c"` , and `"abc" + ""` are valid splits.

Return `true` if it is possible to form a palindrome string, otherwise return `false`.

**Notice** that `x + y` denotes the concatenation of strings `x` and `y`.

**Constraints:**

* `1 <= a.length, b.length <= 10^5`
* `a.length == b.length`
* `a` and `b` consist of lowercase English letters



## 方法一

**Idea:**

​		见代码

**Code:**

```C++
class Solution {
public:
    bool checkPalindromeFormation(string a, string b) {
        int len = a.length();
        // a或b任意一个是回文串，直接true。
        if(isPalindrome(a, 0, len - 1) || isPalindrome(b, 0, len - 1)) return true; 
        if(a[0] == b[len - 1])
        {
            if(check(a, b)) return true; 
            // 注意不能直接写成：return check(a, b)。因为有可能a和b满足a[0] == b[len - 1]，但是不能得到回文串；
            // 但是a和b也满足b[0] == a[len - 1]，同时可以得到回文串。
        }
        if(b[0] == a[len - 1])
        {
            if(check(b, a)) return true;
        }
        return false;
    }
    bool isPalindrome(string& s, int left, int right)
    {
        // 判断某个字符串是否是回文串
        if(s.empty() || s.length() == 1) return true;
        while(left < right)
        {
            if(s[left] != s[right]) return false;
            ++ left; -- right;
        }
        return true;
    }
    bool check(string& s1, string& s2)
    {
        int left = 0, right = s2.length() - 1;
        while(left < right)
        {
            if(s1[left] == s2[right])
            {
                left ++;
                right --;
                // 如果我们最终能得到 left>right，则退出while循环，返回true。此时只需要在left处分割，就能得到回文串。
                // 参考"ulacfd"和"jizalu"。
            }
            // 如果还没得到 left>right，s1[left]就不等于s2[right]了，也是有可能能得到回文串的。
            // 只需要满足下面条件之一也行，即s1的[left,right]部分或s2的[left,right]部分是回文串，也行。
            // 若满足下面s1的这个条件，我们在right+1处分割即可。参考"uaabaef"和"kjimhau"。
            // 若满足下面s2的这个条件，我们在left处分割即可。参考"uaacdef"和"kjhmhau"。
            else return isPalindrome(s1, left, right) || isPalindrome(s2, left, right);
        }
        return true;
    }
};
```



**Complexity Analysis:**

* Time Complexity: $O(N)$

* Space Complexity: $O(1)$

  

## Reference

[1] [Leetcode题解：第210场周赛 - #1616 - 中等 - 分割两个字符串得到回文串 - 1刷](https://leetcode-cn.com/problems/split-two-strings-to-make-palindrome/solution/di-210chang-zhou-sai-5537-zhong-deng-fen-ge-liang-/)