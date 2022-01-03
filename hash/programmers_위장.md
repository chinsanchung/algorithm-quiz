# hash

## programmers 코딩 테스트 연습 "위장"

[문제 페이지](https://programmers.co.kr/learn/courses/30/lessons/42578?language=python3)

`cloth` 리스트에 의상의 목록이 담겨 있습니다. 각 요소는 의상의 이름과 종류로 이루어져 있으며, 이들을 조합하여 위장하는 것이 목적입니다. 서로 다른 옷의 조합의 수를 리턴하는 문제입니다.

- 조건: 최소 한 개의 의상을 입어야 합니다. 그리고 같은 이름의 의상은 존재하지 않습니다.

> 입출력 예시

- `[["crowmask", "face"], ["bluesunglasses", "face"], ["smoky_makeup", "face"]]`

모두 같은 종류의 의상이기에 각각 한 번씩만 입을 수 있고, 결과는 3이 됩니다.

- `[["yellowhat", "headgear"], ["bluesunglasses", "eyewear"], ["green_turban", "headgear"]] `

각 의상을 한 번씩 입는 경우 3개 + `headgear`와 `eyewear`의 조합 2개로 총 5개를 리턴합니다.

## 작성한 코드

```python
def solution(clothes):
    answer = 0
    answer += len(clothes)

    cloth_dict = dict()

    for cloth in clothes:
        cloth_name, cloth_type = cloth
        if cloth_type in cloth_dict:
            cloth_dict[cloth_type].append(cloth_name)
        else:
            cloth_dict[cloth_type] = [cloth_name]

    if len(cloth_dict.keys()) == 1:
        return answer

    combine_num = 1

    for value in cloth_dict.values():
        combine_num *= len(value)
    answer += combine_num

    return answer
```

- 처음에 각 의상을 한 번씩만 입는 것을 가정하여 의상의 개수만큼 더합니다.
- 딕셔너리 `cloth_dict`를 만들고, 종류별로 구분하여 리스트의 형식으로 담았습니다.
- 의상이 오직 한 가지 종류일 때는 더 이상의 연산 없이 의상의 개수만큼 리턴하도록 했습니다.
- `cloth_dict` 딕셔너리의 값으로 반복문을 수행하여 서로 다른 조합일 경우를 계산했습니다.

## 수정한 코드

```python
def solution(clothes):
    answer = 0

    cloth_dict = dict()

    for cloth_name, cloth_type in clothes:
        if cloth_type in cloth_dict:
            cloth_dict[cloth_type] += 1
        else:
            cloth_dict[cloth_type] = 1

    combine_num = 1

    for value in cloth_dict.values():
        combine_num *= (value  + 1)
    answer += combine_num

    return answer - 1

```

- 딕셔너리에는 의상의 이름을 담은 리스트 대신, 숫자 값으로 변경했습니다. "같은 이름을 가진 의상은 존재하지 않는다"는 조건으로 인해 이름으로 구분할 필요가 없기 떄문입니다.
- 처음 `answer`에 `clothes`의 개수만큼 더한 것을 삭제했습니다. 딕셔너리를 이용한 반복문에서 그것을 고려하여 계산하기 때문입니다.
- `combine_num *= (value + 1)`: 의상을 입지 않는 경우의 수만큼 1을 추가했습니다.
- `answer - 1`: 모든 옷을 입지 않는 경우의 수를 제거합니다.
