# Greedy

## programmers 코딩 테스트 연습 "체육복"

[문제 페이지](https://programmers.co.kr/learn/courses/30/lessons/42862?language=python3)

전체 학생 수 `n`, 체육복을 잃은 학생 `lost`, 여벌 체육복을 가진 학생 `reserve`가 있습니다. 체육복을 잃은 학생의 번호에서 , -1 또는 +1번을 가진 `reserve`의 학생만이 체육복을 빌려줄 수 있습니다.

체육복을 입고 수업을 받을 수 있는 학생의 최댓값을 구해야 합니다.

## 작성한 코드

```python
def solution(n, lost, reserve):
    answer = n - len(lost)
    reserve_dict = dict()
    for reserve_num in reserve:
        reserve_dict[reserve_num] = True
    for lost_num in lost:
        if lost_num - 1 in reserve and reserve_dict[lost_num - 1] == True:
            answer += 1
            reserve_dict[lost_num - 1] = False
            continue
        elif lost_num + 1 in reserve and reserve_dict[lost_num + 1] == True:
            answer += 1
            reserve_dict[lost_num + 1] = False
            continue
        elif lost_num in reserve and reserve_dict[lost_num] == True:
            answer -= 1
            reserve_dict[lost_num] = False
            continue
        else:
            continue
    return answer
```

- `reserve`에서 번호를 검색하는 시간을 줄이기 O(1)으로 줄이기 위해 딕셔너리를 활용했습니다.
- `lost`의 학생 번호의 -1 또는 +1인 경우를 조건문으로 명시해 체육복을 입을 수 있는 학생으로 전환합니다.
  - 빌려준 후 `reserve` 학생의 체육복은 1개가 되어 더 이상 빌려줄 수 없음을 boolean 으로 구분했습니다.

## 수정한 코드

- 위의 문제점은 처음에 `answer`를 구할 때 체육복이 없는 학생을 빼고 시작하면서부터 발생합니다. `lost`와 `reserve`에서 서로 중복인 번호가 존재하기 때문입니다.
  - 차라리 학생 전체를 기준으로 하나씩 빼는 방식으로 가는 것이 더 안전했습니다.

다음은 인터넷의 풀이를 보고 수정한 코드입니다. [출처](https://velog.io/@ckr3453/프로그래머스-체육복)

```python
def solution(n, lost, reserve):
    answer = n
    lost = set(lost)
    reserve = set(reserve)

    filtered_lost = lost - reserve
    filtered_reserve = reserve - lost

    for lost_num in filtered_lost:
        if lost_num - 1 in filtered_reserve:
            filtered_reserve.remove(lost_num - 1)
        elif lost_num + 1 in filtered_reserve:
            filtered_reserve.remove(lost_num + 1)
        else:
            answer -= 1
    return answer
```

집합 자료형인 [set](https://wikidocs.net/1015)을 사용했습니다. `set`은 다음과 같은 특징이 있습니다.

- 중복을 허용하지 않습니다.
- 리스트나 튜플과 달리 정해진 순서가 없습니다. 따라서 인덱스 번호로 특정 요소를 조회할 수 없습니다.

`set`은 합집합, 교집합, 차집합을 지원하는데 여기서는 두 리스트의 중복을 제거하기 위해 차집합을 사용해 중복을 제거했습니다.

`lost` 리스트의 요소와 `filtered_reserve`를 비교하여 체육복을 빌려줄 수 있으면 `remove()`를 이용해 그 값을 `filtered_reserve`에서 제거합니다. (참고로, `a - b` 차집합의 시간은 **O(len(a))**만큼 걸립니다. [출처](https://wiki.python.org/moin/TimeComplexity))

## 느낀점

1단계의 문제임에도 오랜 시간 동안 헤매던 문제입니다. `여벌의 체육복이 있는 학생이 체육복을 잃었을 경우`의 의미를 더 집중해서 확인했다면, 그리고 `set` 자료형을 처음에 인지했다면 시간을 단축할 수 있었을 것입니다.
