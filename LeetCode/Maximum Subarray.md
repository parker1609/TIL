# 53. Maximum Subarray
- [문제 링크](https://leetcode.com/problems/maximum-subarray/)

## 문제 유형
- Array
- Divide and Conquer
- Dynamic Programming

## 문제 풀이
- 현재까지 더한 누적값보다 현재 위치의 nums 값이 더 작으면 누적값은 필요가 없어진다.
    - 즉, 현재 위치부터 새로 누적하는 것이 더 큰 값이 된다.
- 이전 값을 갱신하며 사용하므로 Dynamic Programming으로 예상함.

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int n = nums.size();
        int ans = nums[0];
        int cur = nums[0];
        for (int i=1; i<n; ++i) {
            if (nums[i] > cur + nums[i])
                cur = nums[i];
            else
                cur = cur + nums[i];
            
            ans = max(ans, cur);
        }
        
        return ans;
    }
};
```