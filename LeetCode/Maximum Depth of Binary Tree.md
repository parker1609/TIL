# Maximum Depth of Binary Tree
- [문제 링크](https://leetcode.com/problems/maximum-depth-of-binary-tree/)


## 문제풀이
- 가장 큰 높이를 저장하는 전역 변수를 둔다.
- 재귀함수로 위 전역 변수를 갱신한다.
- NULL까지 가므로 1을 빼주어야 한다.


## 풀이코드

```c++
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
    int maxDepth(TreeNode* root) {
        calMaxDepth(root, 1);

        return maxHeight - 1;
    }
private:
    int maxHeight = 0;
    void calMaxDepth(TreeNode* root, int currentDepth) {
        if (root == NULL) {
            maxHeight = maxHeight > currentDepth ? maxHeight : currentDepth;
            return;
        }

        calMaxDepth(root->left, currentDepth + 1);
        calMaxDepth(root->right, currentDepth + 1);
    }
};
```
