# Counting Bits
- [문제 링크](https://leetcode.com/problems/counting-bits/)


## 문제풀이
- 시간복잡도의 제약사항은 `O(N)`이다.
- 현재 숫자를 나눌 수 있는 최대 2의 제곱수로 나눈 후 숫자의 bit 1의 개수를 더한 것과 같다.(동적 계획법)

예를 들어, 숫자 12의 bit 1의 개수를 구한다고 하자. 숫자 12 이전 숫자들의 bit 1 개수는 이미 배열에 있다고 가정한다. 숫자 12를 2진수로 나타내면 `1100`이다. 숫자 12를 나눌 수 있는 최대 2의 제곱수는 8이다. 이를 8로 나누면 `100`이 되며, 이는 숫자 4의 bit 1의 개수와 같다. 결과적으로 숫자 12의 bit 1 개수는 `1 + 숫자 4의 bit 1 개수`이다.


## 풀이코드
- 시간 복잡도: O(N)

```c++
class Solution {
public:
    vector<int> countBits(int num) {
        vector<int> answer;
        int n = 1;
        for (int i = 0; i <= num; ++i) {
            if (i == 0) {
                answer.push_back(0);
            }
            else if (i == 1) {
                answer.push_back(1);
            }
            else {
                int currentPowerOfTwo = (int)pow(2.0, (double)n);
                if (i == currentPowerOfTwo) {
                    answer.push_back(1);
                    ++n;
                }
                else {
                    int size = 1 + answer[i - currentPowerOfTwo / 2];
                    answer.push_back(size);
                }
            }
        }

        return answer;
    }
};
```
