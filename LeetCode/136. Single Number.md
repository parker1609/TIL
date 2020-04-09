# Single Number
- [코드 링크](https://leetcode.com/problems/single-number/)

## 문제풀이
### 풀이 1
- Map 자료구조를 활용하여 `Map<숫자, 숫자의 개수>` 로 선언한다.
  - 숫자의 개수가 1인 key값이 답이다.

```c++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        map<int, int> nm;

        for(int i = 0; i < nums.size(); ++i) {
            nm[nums[i]]++;
        }

        int ans = 0;
        for(auto it = nm.begin(); it != nm.end(); ++it) {
            if (it->second == 1) {
                ans = it->first;
                break;
            }
        }

        return ans;
    }
};
```

#### 결과
```
Runtime: 28 ms, faster than 13.67% of C++ online submissions for Single Number.
Memory Usage: 11.8 MB, less than 12.34% of C++ online submissions for Single Number.
```

### 풀이 2
- 입력 vector를 오름차순으로 정렬 후 인접한 수와 같은지 확인한다.
  - 정답을 찾지 못했다는 것은 가장 마지막 인자의 숫자가 single number이다.

```c++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        sort(nums.begin(), nums.end());

        int ans = 0;
        bool find = false;
        for (int i = 0; i < nums.size() - 1; ++i) {
            if (nums[i] == nums[i + 1]) {
                ++i;
            }
            else {
                ans = nums[i];
                find = true;
                break;
            }
        }

        if (!find) {
            ans = nums[nums.size() - 1];
        }

        return ans;
    }
};
```

#### 결과
```
Runtime: 20 ms, faster than 37.82% of C++ online submissions for Single Number.
Memory Usage: 9.8 MB, less than 71.60% of C++ online submissions for Single Number.
```

### 정답 풀이
- XOR 연산을 활용한다.

예를 들어, 정수 a가 있을 때 `a ^ a = 0`, `a ^ 0 = a`를 이용한다.

```c++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        auto ans = 0;
        for (int i = 0; i < nums.size(); ++i) {
            ans ^= nums[i];
        }

        return ans;
    }
};
```

#### 결과
```
Runtime: 16 ms, faster than 69.19% of C++ online submissions for Single Number.
Memory Usage: 9.7 MB, less than 88.89% of C++ online submissions for Single Number.
```
