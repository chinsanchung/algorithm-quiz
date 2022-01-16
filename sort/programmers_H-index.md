# 정렬

## programmers 코딩 테스트 연습 "H-index"

[문제 페이지](https://programmers.co.kr/learn/courses/30/lessons/42747?language=python3)

어떤 과학자가 발표한 논문의 인용 횟수를 담은 배열 citations가 매개변수로 주어집니다. 논문 `n`편 중, `h`번 이상 인용된 논문이 `h`편 이상이고 나머지 논문이 `h`번 이하 인용되었을 때, `h`의 최대값을 구하는 문제입니다.

- 과학자가 발표한 논문의 수는 1편 이상 1,000편 이하입니다.
- 논문별 인용 횟수는 0회 이상 10,000회 이하입니다.

## 작성한 코드

```python
def solution(citations):
    answer = 0
    n = len(citations)

    for i in range(1, n + 1):
        over_h_cnt = len([val for val in citations if val >= i])
        if i == over_h_cnt and i > answer:
            answer = i
    return answer
```

이 코드는 11번 테스트를 통과하지 못했습니다.

- `citations` 리스트의 개수만큼 반복문을 돌리고,
  - 범위를 $1 $ ~ $ {n} + 1$ 으로 한 이유는 "과학자가 발표한 논문의 수가 1편 이상"이라는 조건 때문입니다.
- `i` 이상인 요소의 개수가 `i`와 일치하는지를 확인합니다. 동시에 그 값이 최대값인지를 확인하여 `answer`를 갱신합니다.
- 실패한 이유
  - `[0, 1, 1]`인 리스트에서 1 이상 인용된 논문이 1편 이상이므로 정답은 1입니다. 하지만 위의 조건문으로는 `i`가 1일 때 `over_h_cnt`는 2이므로 조건을 통과하지 못하고, 결국 0을 리턴하게 됩니다.

## 수정한 코드

```python
def solution(citations):
    answer = 0
    n = len(citations)

    for i in range(1, n + 1):
        over_h_cnt = len([val for val in citations if val >= i])
        if over_h_cnt >= i and i > answer:
            answer = i
    return answer
```

- 조건문을 `over_h_cnt >= i`으로 바꿔 위에 언급한 실패 사례를 통과할 수 있도록 했습니다.
