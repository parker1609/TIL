# 155. Min Stack
- [문제 링크](https://leetcode.com/problems/min-stack/)

## 문제 유형
- Stack, Design

## 문제 풀이
- 최소값을 필드 변수를 활용해 유지하도록 했다.
    - `pop()`할 때 최소값이 나갈 수 있으므로 이를 처리해준다.
    - 최소값을 새로 갱신하려면 저장되어 있는 데이터를 순회해야하므로 vector를 사용하였다.

```c++
class MinStack {
public:
    /** initialize your data structure here. */
    MinStack() 
    : minValue(INT_MAX) {
    }
    
    void push(int x) {
        minValue = min(minValue, x);
        sv.push_back(x);
    }
    
    void pop() {
        int currentTop = top();
        if (currentTop == minValue) {
            minValue = INT_MAX;
            for (int i=0; i<sv.size() - 1; ++i)
                minValue = min(minValue, sv[i]);
        }
        sv.pop_back();
    }
    
    int top() {
        return sv[sv.size() - 1];
    }
    
    int getMin() {
        return minValue;
    }
    
private:
    int minValue;
    vector<int> sv;
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack* obj = new MinStack();
 * obj->push(x);
 * obj->pop();
 * int param_3 = obj->top();
 * int param_4 = obj->getMin();
 */
 ```