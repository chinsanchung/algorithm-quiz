# stack & queue

## programmers 코딩 테스트 연습 "프린터"

[문제 페이지](https://programmers.co.kr/learn/courses/30/lessons/42587?language=python3)

`priorities`에는 출력의 중요도를 담은 문서의 대기 목록을, `location`은 요청한 문서의 위치를 나타냅니다.

문제에서 제시한 로직은 다음과 같습니다.

1. 인쇄 대기목록의 가장 앞에 있는 문서(J)를 대기목록에서 꺼냅니다.
2. 나머지 인쇄 대기목록에서 J보다 중요도가 높은 문서가 한 개라도 존재하면 J를 대기목록의 가장 마지막에 넣습니다.
3. 그렇지 않으면 J를 인쇄합니다.

제한사항

- 대기목록 `priorities`은 1 ~ 100 이하의 문서가 있습니다.
- 중요도는 1 ~ 9 로 표현합니다. 숫자가 클수록 중요합니다.
- `location`은 0부터 시작합니다.

## 작성한 코드

```python
from collections import deque

def solution(priorities, location):
    priorities_deque = deque(priorities)
    priorities_length = len(priorities)

    while True:
        first_value = priorities_deque.popleft()
        over_first_list_from_rest = [i for i in priorities_deque if i > first_value]

        if len(over_first_list_from_rest) == 0:
            return location + 1
        else:
            # 대기목록의 이동 시작
            priorities_deque.append(first_value)
            if location == 0: location = priorities_length - 1
            else: location -= 1
```

- 앞과 뒤에서 데이터를 처리할 수 있는 `deque`를 사용했습니다. 첫 요소를 제거할 때의 시간복잡도가 O(1)이기 때문입니다.
- `popleft()`를 이용해서 첫 번째 요소를 가져옵니다.
- 나머지 deque 에서 첫 요소보다 큰 값이 있는지를 확인합니다.
  - 큰 값이 없는 경우 대기목록을 이동합니다. 이동하는 것에 맞춰 `location`의 값을 수정합니다.
  - 만약 큰 값이 존재하지 않는다면 `location`을 리턴합니다. 이 값은 인덱스 값이므로 1을 더합니다.

## 수정한 코드

위의 코드에서는 전체 테스트 케이스를 통과하지 못했습니다. 다른 사람의 풀이를 참고하여 작성했습니다.

```python
from collections import deque

def solution(priorities, location):
    answer = 0
    priorities_deque = deque([(val, idx) for idx, val in enumerate(priorities)])

    while True:
        first_value = priorities_deque.popleft()

        if priorities_deque and max(priorities_deque)[0] > first_value[0]:
            priorities_deque.append(first_value)
        else:
            answer += 1
            if first_value[1] == location:
                break

    return answer
```

- `enumerate`를 이용해 인덱스와 값 모두를 deque 에 저장했습니다.
- if 조건문에서 `popleft()`으로 값을 제거해도 deque 가 존재하는지 여부를 확인하고, 첫 요소가 최대값인지를 확인합니다.
  - 존재 여부를 확인하지 않을 경우 일부 테스트 케이스에서 에러가 발생했기 때문에 추가한 것으로 보입니다.
