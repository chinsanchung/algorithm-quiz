# hash

## programmers 코딩 테스트 연습 "완주하지 못한 선수"

[문제 페이지](https://programmers.co.kr/learn/courses/30/lessons/42576?language=python3)

참가자 `participant`와 완주자 `completion` 두 리스트가 있습니다. 참가자 중에서 단 한 사람이 완주를 하지 못했는데 그 사람의 이름을 리턴하는 문제입니다.

## 작성한 코드

```python
from collections import Counter

def solution(participant, completion):
    participant_hash = Counter(participant)
    completion_hash = Counter(completion)

    not_completed = list(participant_hash - completion_hash)

    return not_completed[0]
```

파이썬의 collections 모듈 중 [Counter](https://docs.python.org/3/library/collections.html#collections.Counter) 딕셔너리 서브클래스를 이용했습니다. 리스트, 문자열을 인자로 입력하면 문자열은 문자의 개수, 리스트는 요소의 개수를 계산하여 해시테이블 객체를 생성합니다.

```python
# 예시
c = Counter(['a', 'b', 'a'])
print(c) # Counter({'a': 2, 'b': 1})
```

Counter 객체끼리 +, - 연산으로 합집합과 차집합을 구할 수 있습니다. 이번 문제에서 활용한 것은 `A - B`으로, 서로의 교집합을 제거한 ${A}\cap{B}^{c}$ 만을 구합니다.
