# Greedy

## programmers 코딩 테스트 연습 "전화번호 목록"

[문제 페이지](https://programmers.co.kr/learn/courses/30/lessons/42577?language=python3)

숫자를 문자열로 변환하여 저장한 리스트 중, 특정 요소를 접두어를 가지는 요소가 있으면 False 를 리턴하는 문제입니다.

```python
# 예시
phone_book = ["119", "97674223", "1195524421"]
print("3번째 요소가 1번쨰 요소 119를 접두어를 가지고 있으니 정답은 False")

phone_book = ["123","456","789"]
print("아무도 겹치는 숫자가 없으므로 정답은 True")
```

## 작성한 코드

```python
def solution(phone_book):
    answer = True

    for i in phone_book:
        for j in phone_book:
            if i != j and i.find(j) > -1:
                answer = False
                break


    return answer
```

주제가 **해시**이고 파이썬에서는 딕셔너리를 이용해 해시를 구현한다는 것을 확인했지만, 이번 문제에서 그것을 어떻게 활용하면 좋을지를 이해하기 어려웠습니다.

가장 기초적인 방법으로 두 번의 반복문을 돌려서 같은 전화번호가 아니면서, 해당하는 문자열의 순서(인덱스)를 리턴하는 `find` 메소드를 이용했습니다.

하지만 대략 62%의 정답률을 보이고, 효율성 검사에서도 전부 성공하진 않았습니다.

## 수정한 코드

여기서 가장 중요한 점은 `temp_num`입니다. 숫자를 반복문을 이용해 **한 자릿수의 숫자로 구분한 후, 그것을 하나씩 더하며 딕셔너리의 키와 비교하는 것**입니다. 또한 `temp_num`이 자기 자신과 일치하지 않는 경우를 조건문으로 추가해 문제가 요구 사항을 충족했습니다.

```python
def solution(phone_book):
    answer = True
    hash_map = dict()

    for phone_num in phone_book:
        hash_map[phone_num] = 1

    for phone_num in phone_book:
        temp_num = ""

        for small_num in phone_num:
            temp_num += small_num
            if temp_num in hash_map and temp_num != phone_num:
                answer = False
                break
    return answer
```
