# Move Zeroes
- [문제 링크](https://leetcode.com/problems/move-zeroes/)

## 문제풀이
- 입력 벡터와 크기가 같은 벡터를 생성하여, 0이 아닌 경우에만 생성한 벡터에 담는다.
- 입력 벡터가 생성한 벡터를 가르키게 한다.

```c++
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        vector<int> results(nums.size());  // 자동으로 0으로 초기화됨
        
        int rIdx = 0;
        for (int i = 0; i < nums.size(); ++i) {
            if (nums[i] != 0)
                results[rIdx++] = nums[i];
        }
        
        nums = results;
    }
};
```