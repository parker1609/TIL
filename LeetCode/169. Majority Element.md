# Majority Element
- [문제 링크](https://leetcode.com/problems/majority-element/)


## 문제풀이
- Map 자료구조를 활용하여 숫자의 빈도를 계산한다.
- 입력 벡터의 크기 / 2 보다 빈도수가 높은 수가 정답이다.

```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int numberOfMajority = nums.size() / 2;
        
        unordered_map<int, int> freqs;
        
        for (int i = 0; i < nums.size(); ++i) {
            freqs[nums[i]]++;
        }
        
        int ans = 0;
        for (auto it = freqs.begin(); it != freqs.end(); ++it) {
            if (it->second > numberOfMajority) {
                ans = it->first;
                break;
            }
        }
        
        return ans;
    }
};
```