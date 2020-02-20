# Path Sum 3
- [문제 링크](https://leetcode.com/problems/path-sum-iii/)

## 문제 풀이
- 트리 순회 + 각 노드마다 Path가 있는지 검사
- 예외 사항
    - 노드 값이 0 일 때
    - NULL 체크

```cpp
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
    int pathSum(TreeNode* root, int sum) {
        preOrder(root, sum);
        
        return answer;
    }
private:
    int answer = 0;
    void preOrder(TreeNode* root, int sum) {
        if (root == NULL) return;
        
        canMakePath(root, root->val, sum);
        preOrder(root->left, sum);
        preOrder(root->right, sum);
    }
    
    void canMakePath(TreeNode* root, int current, int sum) {
        if (root == NULL) return;
        if (current == sum) {
            answer++;
        }
        
        canMakePath(root->left, current + ((root->left == NULL) ? 0 : root->left->val), sum);
        canMakePath(root->right, current + ((root->right == NULL) ? 0 : root->right->val), sum);
    }
};
```