# Reverse Linked List
- [문제 링크](https://leetcode.com/problems/reverse-linked-list/)

## 문제풀이
- 아래 코드의 문제는 계속 `new`로 힙 할당으로 시간이 느려진다.
    - `new`를 매번 하지 않는 방법 생각해보기
    - 재귀로 구현해보기

```java
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
    ListNode* reverseList(ListNode* head) {
        ListNode* tail = NULL;
        ListNode* ans = NULL;
        
        while(head != NULL) {
            tail = new ListNode(head->val);
            tail->next = ans;
            
            head = head->next;
            ans = tail;
        }

        return ans;
    }
};
```