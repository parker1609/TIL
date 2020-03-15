# Maximum Depth of Binary Tree
- [문제 링크](https://leetcode.com/problems/maximum-depth-of-binary-tree/)


## 문제풀이
- 재귀함수를 이용하여 가장 깊은 높이를 계산한다.


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
        if (root == NULL) return 0;
        
        int maxOfDepth = 1;
        calMaxOfDepth(root, maxOfDepth, &maxOfDepth);
        
        return maxOfDepth;
    }

private:
    void calMaxOfDepth(TreeNode* root, int currentDepth, int* maxOfDepth) {
        if (root == NULL) {
            return;
        }
        
        (*maxOfDepth) = currentDepth > (*maxOfDepth) ? currentDepth : (*maxOfDepth);
        
        calMaxOfDepth(root->left, currentDepth + 1, maxOfDepth);
        calMaxOfDepth(root->right, currentDepth + 1, maxOfDepth);
    }
};
```

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
        if (root == NULL) return 0;
        
        return calMaxOfDepth(root, 1);
    }

private:
    int calMaxOfDepth(TreeNode* root, int currentDepth) {
        if (root == NULL) {
            return currentDepth - 1;
        }
        
        int leftDepth = calMaxOfDepth(root->left, currentDepth + 1);
        int rightDepth = calMaxOfDepth(root->right, currentDepth + 1);
        
        return leftDepth > rightDepth ? leftDepth : rightDepth;
    }
};
```

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
        if (root == NULL) return 0;
        
        return max(maxDepth(root->left) + 1, maxDepth(root->right) + 1);
    }
};
```