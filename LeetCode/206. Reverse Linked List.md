# 206. Reverse Linked List
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

- 새로운 노드 생성하지 않고 구현하기 (반복문)

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
    ListNode* reverseList(ListNode* head) {
        ListNode* reversed = NULL;
        
        while (head != NULL) {
            ListNode* next = head->next;
            head->next = NULL;
            
            head->next = reversed;
            reversed = head;
            
            head = next;
        }
        
        return reversed;
    }
};
```

-- 새로운 노드 생성하지 않고 구현하기 (재귀)

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
    ListNode* reverseList(ListNode* head) {
        ListNode* reversedStart = NULL;
        ListNode* reversedEnd = NULL;
        reverseRecursively(head, &reversedStart, &reversedEnd);
        
        return reversedStart;
    }
    
private:
    void reverseRecursively(ListNode* head, ListNode** start, ListNode** end) {
        if (head == NULL) {
            return;
        }
        
        reverseRecursively(head->next, start, end);
        
        if ((*start) == NULL) {
            *start = head;
            *end = head;
        }
        else {
            head->next = NULL;
            (*end)->next = head;
            (*end) = (*end)->next;
        }
    }
};
```

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
    ListNode* reverseList(ListNode* head) {
        if (head == NULL || (head->next) == NULL) {
            return head;
        }
        
        ListNode* node = reverseList(head->next);
        head->next->next = head;
        head->next = NULL;
        
        return node;
    }
};
```