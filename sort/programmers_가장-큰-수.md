# 정렬

## programmers 코딩 테스트 연습 "가장 큰 수"

[문제 페이지](https://programmers.co.kr/learn/courses/30/lessons/42746?language=python3)

숫자 요소를 가진 리스트 `numbers`가 주어집니다. 요소를 이어 붙여 만들 수 있는 가장 큰 수를 구하는 문제입니다.

- numbers의 길이는 1 이상 100,000 이하입니다.
- numbers의 원소는 0 이상 1,000 이하입니다.
- 정답이 너무 클 수 있으니 문자열로 바꾸어 return 합니다.

## 작성한 코드

이번 문제는 풀이까지 접근할 수 없었습니다.

```python
def solution(numbers):
    numbers_str_list = [str(num) for num in numbers]

    answer = ''
    max_num = 0

    for val in numbers_str_list:
        first_num_list = []
        # num_str_changed_list =
    return answer
```

- 요소를 이어 붙이기 위해, 숫자 값을 문자열로 변환합니다.
- 원래 의도는 반복문을 이용해 `val`을 앞 자리로 하는 문자열 리스트를 만들고, 거기서 가장 큰 값만을 뽑아 `max_num`에 넣어 반복하는 과정에서 비교하는 것이었습니다.
  - 하지만, 앞 자리를 반복하더라도, 뒷 자리를 반복하는 방법이 떠오르질 않았습니다. 예를 들어 `[6, 10, 2]`가 주어졌을 때, 처음 요소 6으로 만들 수 있는 값은 "6 10 2"와 "6 2 10"가 있습니다. 지금은 3개의 요소만 있기에 단순하게 끝낼 수 있었지만, numbers 의 길이는 10만 이하이기 때문에 첫 자리 이외의 요소들을 조합하기 위해 어떻게 해야 할지를 찾지 못했습니다.

## 수정한 코드

다음은 풀이를 보고 작성한 코드입니다.

```python
def solution(numbers):
    numbers_str_list = [str(num) for num in numbers]
    numbers_str_list.sort(key=lambda x: x * 3, reverse=True)

    answer = str(int(''.join(numbers_str_list)))
    return answer
```

여기서의 핵심은 `sort()`에서 람다 함수를 이용해 정렬을 수행한 것입니다.

- 람다 함수의 `x`는 문자열으로, 3을 곱하여 1자리 숫자는 3자리로, 2자리 숫자는 6자리로, 3자리 숫자는 9자리 문자열으로 만들어줍니다.
  - 여기서 3은 `number`의 자릿수를 의미합니다. 모든 문자열을 세 자릿수의 값으로 통일시켜 크기를 비교하는 것이 목적입니다.
  - `['3', '30', '34', '5', '9']`을 `['333', '303030', '343434', '555', '999']`로 만들고, 이것을 정렬하면 `['999', '555', '343434', '303030', '333']`가 됩니다. 의도한대로 가장 큰 숫자를 구할 수 있게 되는 것입니다.
- 그리고 이 문자열의 값을 내림차순으로 정렬합니다. 문자열의 정렬은 숫자 자체의 크기보다는, 각 숫자의 크기 (1 ~ 9) 가 큰 순서로 정렬하는 것을 이용한 것입니다.
