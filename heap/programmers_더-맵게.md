# heap

## programmers 코딩 테스트 연습 "더 맵게"

[문제 페이지](https://programmers.co.kr/learn/courses/30/lessons/42626?language=python3)

음식의 매움 지수를 담은 리스트 `scoville`, 그리고 매움 지수 `K`를 인자 값으로 합니다. 여기서 가장 매운 음식을 만드는 공식이 있는데, 그것을 사용해 음식들을 조합하여 리스트의 모든 음식이 K 이상일 때의 조합 횟수를 구하는 것입니다. 전부 K 미만이면 -1을 리턴합니다.

- 가장 매운 음식 = 가장 덜 매운 음식 + (두 번째로 덜 매운 음식 \* 2)

## 작성한 코드

```python
from heapq import heappop, heappush, heapify

def get_over_k_foods_length(scoville, K):
    result =  [val for val in scoville if val >= K]
    return len(result)

def solution(scoville, K):
    heapify(scoville)
    # 가장 매운 = 가장 덜 매운 + (두 번째로 덜 매운 * 2)
    answer = 0
    while len(scoville) > 1:
        first = heappop(scoville)
        second = heappop(scoville)
        new_scov = first + (second * 2)
        heappush(scoville, new_scov)
        answer += 1

        over_k_scov_length = get_over_k_foods_length(scoville, K)

        if over_k_scov_length == len(scoville):
            return answer
            # break
    return -1
```

문제는 풀었지만, 효율성 테스트에서 실패한 코드입니다.

- `heapify`로 리스트를 힙으로 변경합니다.
- `heappop`으로 힙에서 가장 작은 원소를 pop 합니다. 그것으로 첫 번째, 두 번째 요소를 구합니다.
- 그것으로 계산을 한 후, `heappush`로 힙에 추가합니다. 이것을 1회 섞은 것으로 보아 `answer`에 1을 더합니다.
- `scoville` 힙에서 지수가 K 이상인 요소의 개수를 구해 전부 K 를 넘었으면 `answer`를 리턴합니다. 아닐 경우에는 -1을 리턴합니다.

## 수정한 코드

```python
from heapq import heappop, heappush, heapify

def solution(scoville, K):
    heapify(scoville)
    answer = 0
    while scoville[0] < K:
        if len(scoville) > 1:
            first = heappop(scoville)
            second = heappop(scoville)
            new_scov = first + (second * 2)
            heappush(scoville, new_scov)
            answer += 1
        else:
            return -1

    return answer

```

- 반복문, 조건문의 내용이 바뀌었습니다. 힙의 첫 요소가 K 보다 작은 경우, 즉 K 미만의 요소가 있을 경우 계속 반복문을 수행하도록 합니다. 또한, `heappop`으로 첫째와 둘째 요소를 제거하기에 if 조건문으로 힙이 2 이상이어야 계산을 수행하도록 했습니다.
  - `heapify`로 변환했을 때 첫 요소(트리의 Root)는 언제나 가장 작은 요소가 되기에, while 의 조건문에 첫 번째 요소를 넣어 비교한 것입니다. 문제에서 제시한 조건이 "모든 음식이 K 이상일 떄"이기 때문입니다.
