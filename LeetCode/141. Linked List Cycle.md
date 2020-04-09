# 141. Linked List Cycle
- [문제 링크](https://leetcode.com/problems/linked-list-cycle/)

## 문제 풀이
- 연결 리스트의 순환을 찾는 유명한 알고리즘이 있다. [참고](https://en.wikipedia.org/wiki/Cycle_detection#Floyd's_Tortoise_and_Hare)

## 풀이 코드

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
    bool hasCycle(ListNode *head) {
        ListNode* fast = head;
        ListNode* slow = head;
        
        while (fast != NULL && fast->next != NULL) {
            slow = slow->next;
            fast = fast->next->next;
            
            if (slow == fast) {
                return true;
            }
        }
        
        return false;
    }
};
```