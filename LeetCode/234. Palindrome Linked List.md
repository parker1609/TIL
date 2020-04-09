# 234. Palindrome Linked List
- [문제 링크](https://leetcode.com/problems/palindrome-linked-list/)

## 문제 풀이
### 뒤집어서 비교하기

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        ListNode* reversed = reverseAndClone(head);
        return isEqual(head, reversed);
    }
private:
    ListNode* reverseAndClone(ListNode* head) {
        ListNode* reversed = NULL;
        
        while (head != NULL) {
            ListNode* node = new ListNode(head->val);
            node->next = reversed;
            reversed = node;
            head = head->next;
        }
        
        return reversed;
    }
    
    bool isEqual(ListNode* normal, ListNode* reversed) {
        while (normal != NULL && reversed != NULL) {
            if (normal->val != reversed->val) {
                return false;
            }
            normal = normal->next;
            reversed = reversed->next;
        }
        
        return normal == NULL && reversed == NULL;
    }
};
```

### 스택 이용하기

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        ListNode* slow = head;
        ListNode* fast = head;
        
        stack<int> s;
        while (fast != NULL && fast->next != NULL) {
            s.push(slow->val);
            slow = slow->next;
            fast = fast->next->next;
        }
        
        if (fast != NULL) {
            slow = slow->next;
        }
        
        while (slow != NULL) {
            int top = s.top();
            s.pop();
            if (top != slow->val) {
                return false;
            }
            slow = slow->next;
        }
        
        return true;
    }
};
```