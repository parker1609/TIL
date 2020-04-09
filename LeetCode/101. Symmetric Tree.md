# 101. Symmetric Tree
- [문제 링크](https://leetcode.com/problems/symmetric-tree/)

## 문제 유형
- Tree, Depth-first Search, Breadth-first Search

## 문제 풀이
### 재귀
- 이진 트리를 InOrder로 탐색한 결과에서 root를 제외한 양쪽 자식노드들이 서로 대칭적으로 값이 같고, 방향이 반대인지 확인한다.

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
    bool isSymmetric(TreeNode* root) {        
        inorder(root, 0);
        int n = order.size();
        if (n == 0) return true;
        else if (n % 2 == 0) return false;
        
        stack<pair<int, int>> s;
        for (int i=0; i<n/2; ++i)
            s.push(order[i]);
        for (int i=n/2+1; i<n; ++i) {
            if (s.empty()) return false;
            pair<int, int> top = s.top();
            s.pop();
            if (top.first != order[i].first || 
               top.second == order[i].second) 
                return false;
        }
        
        return true;
    }
    
private:
    vector<pair<int, int>> order;
    
    void inorder(TreeNode* root, int dir) {
        if (root == NULL) return;
        
        inorder(root->left, -1);
        order.push_back({root->val, dir});
        inorder(root->right, 1);
    }
};
```

### 반복문
- BFS를 이용하여 이진 트리에서 대칭되어 같아야 하는 값을 queue에 순서대로 삽입하여 같은지 확인한다.

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
    bool isSymmetric(TreeNode* root) {        
        if (root == NULL) return true;
        
        queue<TreeNode*> q;
        q.push(root->left);
        q.push(root->right);
        while(!q.empty()) {
            TreeNode* curLeft = q.front();
            q.pop();
            TreeNode* curRight = q.front();
            q.pop();
            
            if (curLeft && curRight) {
                if (curLeft->val != curRight->val) return false;
                q.push(curLeft->left);
                q.push(curRight->right);
                q.push(curLeft->right);
                q.push(curRight->left);
            }
            else {
                if (curLeft != curRight) return false;
            }
        }
        
        return true;
    }
};
```