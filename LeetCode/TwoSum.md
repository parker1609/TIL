# Two Sum
- [문제 링크](https://leetcode.com/problems/two-sum/)


## 문제풀이
- 같은 인덱스를 두 번 사용하지 않는다.
- 입력 값은 오름차순이 아닐 수 있다.
- 이진 탐색으로 시간복잡도를 개선할 수 있다.


## 풀이코드
- 시간복잡도: O(N^2)

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> answer;
        bool check = false;
        for (int i = 0; i < nums.size() - 1; ++i) {
            for (int j = i + 1; j < nums.size(); ++j) {
                if (nums[i] + nums[j] == target) {
                    answer.push_back(i);
                    answer.push_back(j);
                    check = true;
                    break;
                }
            }
            if (check) break;
        }

        return answer;
    }
};
```

- 시간복잡도: O(NlogN)

```cpp
class Solution {
public:
    int binarySearch(vector<pair<int, int>>& nums, int key) {
        int begin = 0;
        int end = nums.size() - 1;

        int mid;
        while(begin <= end) {
            mid = (begin + end) / 2;
            if (nums[mid].first == key) return mid;
            else if (nums[mid].first > key) end = mid - 1;
            else begin = mid + 1;
        }

        return -1;
    }

    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> answer;
        vector<pair<int, int>> indexNums;
        for (int i = 0; i < nums.size(); ++i)
            indexNums.push_back({nums[i], i});

        sort(indexNums.begin(), indexNums.end());

        for (int i = 0; i < indexNums.size(); ++i) {
            int key = target - indexNums[i].first;
            int check = binarySearch(indexNums, key);
            if (check > -1 && check != i) {
                answer.push_back(indexNums[i].second);
                answer.push_back(indexNums[check].second);
                break;
            }
        }

        return answer;
    }
};
```
