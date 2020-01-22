# Climbing Stairs
- [문제 링크](https://leetcode.com/problems/climbing-stairs/)

## 문제 풀이
- 계단을 오르는 방법은 두 가지이므로 계단이 높아질수록 이전의 방법이 반복됨 -> 이전에 있던 값을 재활용 -> DP
    - 완전탐색으로 풀면 O(2^N)

```c++
class Solution {
public:
    int climbStairs(int n) {
        vector<int> dp;
        dp.reserve(n + 1);
        
        dp[0] = 1;
        dp[1] = 1;
        for (int i = 2; i <= n; ++i)
            dp[i] = dp[i-1] + dp[i-2];
        
        return dp[n];
    }
};
```