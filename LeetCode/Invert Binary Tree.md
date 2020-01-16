# Invert Binary Tree
- [문제 링크](https://leetcode.com/problems/invert-binary-tree/)

## 문제풀이
- 입력된 트리가 NULL인 경우를 처리해야한다.

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
    TreeNode* invertTree(TreeNode* root) {
        invert(root);
        
        return root;
    }
    
private:
    void invert(TreeNode* root) {
        if (root == NULL || (root->left == NULL && root->right == NULL)) {
            return;
        }
        
        TreeNode* temp = root->left;
        root->left = root->right;
        root->right = temp;
        
        invert(root->left);
        invert(root->right);
    }
};
```