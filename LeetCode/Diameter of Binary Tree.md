# Diameter of Binary Tree
- [문제 링크](https://leetcode.com/problems/diameter-of-binary-tree/)

## 문제 풀이
- 트리를 순회하면서 해당 루트에서 왼쪽, 오른쪽 가지 길이 구하기

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
    int diameterOfBinaryTree(TreeNode* root) {
        preOrderTraversal(root);
        
        return maxDiameter;
    }

private:
    int maxDiameter = 0;
    int length = 0;
    
    void preOrderTraversal(TreeNode* root) {
        if (root == NULL) return;
        calculateMaxDiameter(root);
        preOrderTraversal(root->left);
        preOrderTraversal(root->right);
    }
    
    void calculateMaxDiameter(TreeNode* root) {
        int currentDiameter = 0;
        length = 0;
        calculateMaxLeafLength(root->left, 1);
        currentDiameter += length;
        length = 0;
        calculateMaxLeafLength(root->right, 1);
        currentDiameter += length;
        maxDiameter = maxDiameter > currentDiameter ? maxDiameter : currentDiameter;
    }
    
    void calculateMaxLeafLength(TreeNode* node, int currentLength) {
        if (node == NULL) return;
        length = length > currentLength ? length : currentLength;
        calculateMaxLeafLength(node->left, currentLength + 1);
        calculateMaxLeafLength(node->right, currentLength + 1);
    }
};
```

```
Runtime: 20 ms
Memory Usage: 19.4 MB
```

- 트리 순회하면서 중간 중간에 최대값 갱신하기

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
    int diameterOfBinaryTree(TreeNode* root) {
        calculateMaxDiameter(root);
        
        return maxDiameter;
    }

private:
    int maxDiameter = 0;
    int calculateMaxDiameter(TreeNode* root) {
        if (root == NULL) return 0;
        
        int leftLength = calculateMaxDiameter(root->left);
        int rightLength = calculateMaxDiameter(root->right);
        
        // 중간 루트마다 최대 diameter가 있는지 검사
        maxDiameter = max(maxDiameter, leftLength + rightLength);
        
        return max(leftLength, rightLength) + 1;
    }
};
```

```
Runtime: 16 ms
Memory Usage: 19.7 MB
```