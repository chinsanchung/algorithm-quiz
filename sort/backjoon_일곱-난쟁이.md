# 정렬

## 백준 2309 "일곱 난쟁이"

[문제 페이지](https://www.acmicpc.net/problem/2309)

9명의 숫자 데이터를 입력받습니다. 거기서 7개의 숫자를 합하면 반드시 100이 나오게 됩니다. 합이 100이 나오게 하는 7개의 숫자를 오름차순으로 출력하는 문제입니다.

## 작성한 코드

```python
num_list = []
for i in range(9):
    num_list.append(int(input()))

num_list.sort()

for i in range(9):
    result = [num_list[i]]
    total = num_list[i]
    cnt = 1
    for j in range(i + 1, 9):
        total += num_list[j]
        result.append(num_list[j])
        cnt += 1
        if cnt == 7 and total == 100:
            for k in result: print(k)
            break
```

- for 반복문을 두 번 사용하여, 7번 합했을 때 100이 나오는 경우를 구하도록 했습니다.
  - 순차적으로 7개를 더하기 때문에, 일부 사례에서는 틀릴 가능성이 있습니다. 실제로 예시 데이터로는 통과했지만, 틀렸다는 결과가 나왔습니다.

## 수정한 코드

```python
num_list = []
for i in range(9):
    num_list.append(int(input()))

total = sum(num_list)
one = 0
two = 0

for i in range(9):
    for j in range(i + 1, 9):
        tmp_one = num_list[i]
        tmp_two = num_list[j]
        if total - (tmp_one + tmp_two) == 100:
            one, two = tmp_one, tmp_two
            break

num_list.remove(one)
num_list.remove(two)
num_list.sort()

for num in num_list:
    print(num)

```

- 9명의 인원 그리고 반드시 7명의 합이 100이 나온다는 것을 이용해서, 전체의 합에서 두 명을 빼서 100이 나오는 경우를 찾으면 해결할 수 있었습니다.
  - 인터넷에서 조회했을 떄, 이것이 결과가 나올 떄까지 가능한 모든 값을 대입하는 [무차별 대입 공격](https://ko.wikipedia.org/wiki/%EB%AC%B4%EC%B0%A8%EB%B3%84_%EB%8C%80%EC%9E%85_%EA%B3%B5%EA%B2%A9)이라는 방법임을 알게 되었습니다.
