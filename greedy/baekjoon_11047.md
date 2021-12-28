# Greedy

## 백준 11047번 동전 0

[문제 페이지](https://www.acmicpc.net/problem/11047)

n 개만큼 동전을 입력하고, 그 동전을 이용해 k 원을 내기 위해 최소 몇 개의 동전이 필요한지를 계산하는 문제입니다. 입력하는 동전은 반드시 오름차순 순으로 정렬하여 입력합니다.

## 작성한 코드

```python
n, k = map(int, input().split())

coin_list = list()
for i in range(n):
  value = int(input())
  coin_list.append(value)

result = 0

for j in range(-1, -(n + 1), -1):
  result += k // coin_list[j]
  remain = k % coin_list[j]
  if remain == 0:
    break
  else:
    k = k % coin_list[j]

print(result)
```

간단한 탐욕 알고리즘 문제입니다. 탐욕 알고리즘은 가장 좋은 것을 우선시하여 계산하므로, 여기서는 가장 큰 동전을 먼저 계산해야 합니다. 입력한 동전은 오름차순으로 정렬했기에, `range(-1, -(n + 1), -1)`으로 마지막 동전(가장 큰 동전)부터 반복문을 돌려 몫과 나머지를 계산합니다. 나머지가 0이 될 경우 반복문을 중지하고 결과를 리턴합니다.
