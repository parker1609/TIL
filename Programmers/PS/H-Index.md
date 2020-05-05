# H-Index
- [문제 링크](https://programmers.co.kr/learn/courses/30/lessons/42747)

## 문제
### 문제 조건
- H-Index인 것은 N편 중, H번 이상 인용된 논문의 개수가 H편 이상이고 나머지 논문이 H번 이하인 경우 H의 최댓값이다.
- 문제에서 입력으로 주어진 인용된 횟수에 관계없이 0..10,000 횟수 사이에는 모두 H가 될 수 있다.

### 풀이
완전 탐색으로 풀면 가능한 인용된 횟수 범위 내로 반복하면서 해당 인용된 횟수만큼 논문이 있으면 H-Index라고 판단한다.
- 시간 복잡도: O(NM), N은 인용된 횟수, M은 논문 갯수

```cpp
#include <string>
#include <vector>

using namespace std;

int solution(vector<int> citations) {
    for (int i=10000; i>=0; --i) {
        int count = 0;
        for (int j=0; j<citations.size(); ++j) {
            if (citations[j] >= i) {
                ++count;
            }
        }
        
        if (count >= i) {
            return i;
        }
    }
    
    return 0;
}
```
