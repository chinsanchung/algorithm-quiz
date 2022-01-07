# 정렬

## programmers 코딩 테스트 연습 "K번째 수"

[문제 페이지](https://programmers.co.kr/learn/courses/30/lessons/42748?language=python3)

배열 `array`에서 `i`번째 숫자부터 `j`번째 숫자까지 자르고 정렬했을 떄 `k`번째 수를 구하는 문제입니다.

`i`, `j`, `k`는 `commands`에 리스트 형태로 담겨있습니다.

## 작성한 코드

```python
def solution(array, commands):
  answer = []

  for command in commands:
    i, j, k = command

    target_arr = array[i - 1:j]
    answer.append(target_arr[k - 1])
  return answer
```

- `commands`를 반복하여 `i`, `j`, `k`를 destructuring 으로 불러냅니다.
- 파이썬의 슬라이싱 `array[i - 1:j]`으로 `i` ~ `j`번째 사이의 값을 구합니다. 슬라이싱의 첫 값은 0부터 시작하므로 i 에 1을 뺍니다.
- 슬라이싱한 리스트를 정렬합니다.
- 정렬한 리스트에서 `k`번째 값을 `answer` 리스트에 추가합니다.
