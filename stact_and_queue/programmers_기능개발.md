# stack & queue

## programmers 코딩 테스트 연습 "기능 개발"

[문제 페이지](https://programmers.co.kr/learn/courses/30/lessons/42586?language=python3#)

## 작성한 코드

```python
from collections import deque
import math

def solution(progresses, speeds):
    queue = deque(progresses)
    last_end_duration = 0
    count = 0
    result = []

    for speed in speeds:
        value = queue.popleft()
        end_duration = math.ceil((100 - value) / speed)
        if last_end_duration == 0:
            count += 1
            last_end_duration = end_duration
        elif last_end_duration <= end_duration:
            result.append(count)
            count = 1
            last_end_duration = end_duration
        else:
            count += 1
    result.append(count)
    return result
```

선입선출 구조를 가진 큐를 이용했습니다. 첫 과정을 완료하는데 걸리는 시간을 `end_duration`으로 구한 후, 그것을 이전 과정의 완료 시간과 비교합니다. 두 번째 완료 시간이 작을 경우, 첫 과정을 완료한 후에 `count`에 포함시켜 계산합니다.

- 예) 완료일이 7일, 3일, 9일이 걸린다고 가정할 때 첫 배포일은 7일 째 2개, 9일 째 1개로 결과는 `[2, 1]`이 됩니다.

처음 두 개의 테스트 케이스는 통과했지만, 전체 테스트에서의 성공률은 63.6%였습니다.

## 수정한 코드

다음은 풀이를 참고하여 수정한 코드입니다. 차이점은 완료할 때까지 걸리는 시간을 포함하여 계산한다는 것입니다. `date_count`를 이용해 지나간 날짜를 저장하고, 그것과 개발 속도를 함께 계산하여 완료한 과정을 배포일에 포함시킬지 아니면 독립시킬지를 결정합니다.

```python
from collections import deque

def solution(progresses, speeds):
    progress_queue = deque(progresses)
    speed_queue = deque(speeds)
    count = 0
    date_count = 0
    result = []

    while progress_queue:
        progress = progress_queue[0]
        speed = speed_queue[0]

        if (progress + date_count * speed) >= 100:
            progress_queue.popleft()
            speed_queue.popleft()
            count += 1
        else:
            if count > 0:
                result.append(count)
                count = 0
            date_count += 1
    result.append(count)
    return result
```

이전처럼 완료하는 데 7일, 3일, 9일이 걸린다고 가정하고 계산하겠습니다.

- `date_count`를 7이 될 때까지 더합니다.
- 7이 되어 `(progress + date_count * speed) >= 100`이 True 가 됩니다. `count`를 1 더합니다.
- 다음은 완료일이 3일인데, 이미 7일이 되었으므로 `count`를 1 더하고 바로 통과합니다.
- 마지막은 완성하는데 9일이 걸립니다. `date_count`가 9가 될 때까지 더합니다. 더하고 나면, if 조건문을 통과하고 `count`를 1 더합니다.
- 모든 큐를 제거했기에 while 조건문이 끝납니다. 결과값으로 `[2, 1]`을 리턴합니다.
