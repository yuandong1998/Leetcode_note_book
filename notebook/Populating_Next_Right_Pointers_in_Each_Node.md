#### [116. Populating Next Right Pointers in Each Node](https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node/)



**Q:**You are given a **perfect binary tree** where all leaves are on the same level, and every parent has two children. The binary tree has the following definition:

```
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```

Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to `NULL`.Initially, all next pointers are set to `NULL`.

**Follow up:**

* You may only use constant extra space.
* Recursive approach is fine, you may assume implicit stack space does not count as extra space for this problem.

**Constraints:**

* The number of nodes in the given tree is less than 4096.
* `-1000 <= node.val <= 1000`



## solution 1

**Idea:** 通过层序遍历来填充next指针，程序遍历需要一个辅助队列，算法如下：

```
首先根元素入队
while(队列不为空)
	求当前队列的长度 $s_i$
	依次从队列中取 $s_i$ 个元素进行拓展

```

**Code**

```C++
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;
    Node* next;

    Node() : val(0), left(NULL), right(NULL), next(NULL) {}

    Node(int _val) : val(_val), left(NULL), right(NULL), next(NULL) {}

    Node(int _val, Node* _left, Node* _right, Node* _next)
        : val(_val), left(_left), right(_right), next(_next) {}
};
*/

class Solution {
public:
    Node* connect(Node* root) {
        if(!root) return NULL;

        queue<Node*> q;
        q.push(root);
        while(!q.empty()){
            int currentLevelSize=q.size();
            auto preNode=q.front();q.pop();
            if(preNode->left) q.push(preNode->left);
            if(preNode->right) q.push(preNode->right);
            for(int i=1;i<currentLevelSize;i++){
                auto node=q.front();q.pop();
                preNode->next=node;
                preNode=node;
                if(node->left) q.push(node->left);
                if(node->right) q.push(node->right);
            }
            preNode->next=NULL;
        }
        return root;
    }
};
```



**Complexity Analysis**

* Time Complexity：遍历每个节点一次所以为$O(N)$。
* Space Complexity：需要一个队列，大小小于N，所以为$O(N)$。



## Reference

