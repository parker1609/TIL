# 160. Intersection of Two Linked Lists
- [문제 링크](https://leetcode.com/problems/intersection-of-two-linked-lists/)

## 문제 풀이

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
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        if (headA == NULL || headB == NULL) {
            return NULL;
        }

        /* 마지막 노드와 길이를 구한다 */     
        pair<ListNode*, int> result1 = getTailAndSize(headA);
        pair<ListNode*, int> result2 = getTailAndSize(headB);
        
        /* 마지막 노드가 다르면 교집합이 없다는 뜻이다. */
        if (result1.first != result2.first) {
            return NULL;
        }
        
        /* 각 연결리스트 시작점에 포인터를 세팅한다. */
        ListNode* longer = result1.second > result2.second ? headA : headB;
        ListNode* shorter = result1.second > result2.second ? headB : headA;
        
        /* 길이가 더 긴 리스트를 길이가 짧은 리스트와 길이가 같도록 앞으로 당긴다. */
        // Call By Reference
        passKThNode(&longer, abs(result1.second - result2.second));
        
         /* 두 포인터가 만날 때까지 포인터를 움직인다. */
        while (shorter != longer) {
            shorter = shorter->next;
            longer = longer->next;
        }
        
        return longer;
    }
    
private:
    pair<ListNode*, int> getTailAndSize(ListNode* node) {
        int size = 1;
        while (node->next != NULL) {
            size++;
            node = node->next;
        }
        
        return make_pair(node, size);
    }
    
    int abs(int num) {
        return num > 0 ? num : -num;
    }
    
    void passKThNode(ListNode** node, int k) {
        while (k > 0 && (*node) != NULL) {
            k--;
            (*node) = (*node)->next;
        }
    }
};
```