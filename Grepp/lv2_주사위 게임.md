# 주사위 게임
## 문제 설명
XX 모바일 보드게임은 같은 크기의 칸으로 구분된 직선 모양의 게임 보드와 특별한 주사위 3개를 사용해서 진행합니다. 주사위는 각각 1부터 S1, S2, S3까지의 숫자 중 하나가 나오며, 3개의 주사위를 동시에 굴려 나온 숫자의 합만큼 캐릭터를 이동시킵니다. 게임 보드의 몇몇 칸에는 몬스터가 있으므로, 캐릭터는 도착한 칸에서 몬스터를 만나게 될 수도 있습니다.
다음은 S1 = 2, S2 = 3, S3 = 4인 경우의 예시입니다.

<img width="425" alt="스크린샷 2021-05-05 오후 9 53 07" src="https://user-images.githubusercontent.com/19163372/117143897-49625400-adec-11eb-8d47-d7baab24633f.png">

위 그림에서 별은 캐릭터이며, 붉은 사각형은 몬스터입니다. 캐릭터는 1번 칸에 있습니다. 주사위를 던져 나온 숫자가 1, 1, 2라면 캐릭터는 총 4칸을 이동하여 5번째 칸에 도착해 몬스터를 만납니다. 반면에 주사위를 던져 나온 숫자가 2, 2, 1이라면 총 5칸을 이동한 캐릭터는 6번째 칸에 도착해 몬스터를 만나지 않습니다. 위 예시에서 주사위를 한 번만 굴렸을 때, 주사위 눈의 합만큼 이동해 도착한 칸에서 몬스터를 만나지 않을 확률은 0.5입니다.

몬스터의 위치를 담고 있는 배열 monster와 각 주사위에서 나올 수 있는 최대 숫자 S1, S2, S3가 매개변수로 주어질 때, 1번 칸에 위치한 캐릭터가 주사위를 한 번만 굴려 나온 눈금의 합만큼 이동해서 도착한 칸에 몬스터가 없을 확률을 return 하도록 solution 함수를 완성해 주세요. 단, return 값은 몬스터를 만나지 않을 확률에 1000을 곱한 후 소수부는 버리고 정수 부분만 return 하세요.

## 제한사항
* monster는 몬스터의 위치를 담은 배열이며 길이(몬스터의 개수)는 1 이상 99 이하입니다.
* monster의 각 원소는 현재 몬스터의 위치를 나타내며, 모든 몬스터의 위치는 2 이상 100 이하의 자연수입니다.
* 같은 위치의 몬스터가 여러 번 중복해서 주어지지 않으며, 한 칸에는 한 마리의 몬스터만 있습니다.
* 각 주사위를 던져 나올 수 있는 수의 최댓값 S1, S2, S3는 다음과 같습니다.
* 2 ≤ S1 ≤ 30, 2 ≤ S2 ≤ 30, 2 ≤ S3 ≤ 40, S1, S2, S3는 자연수
* 몬스터를 만나지 않을 확률에 1000을 곱한 후 소수부는 버리고 정수 부분만 int형으로 return 해주세요.
## 입출력 예
|monster|S1|S2|S3|result|
|---|---|---|---|---|
|[4,9,5,8]|2|3|4|500|
|[4,9,5,8]|2|3|3|555|
## 입출력 예 설명
### 입출력 예 #1
몬스터의 위치는 문제에 주어진 예시와 같습니다.
몬스터가 없는 칸에 도착할 확률은 1/2로 0.5이며 1000을 곱한 후 소수부를 버리면 500이 됩니다.

### 입출력 예 #2
몬스터의 위치는 문제에 주어진 예시와 같습니다.
몬스터가 없는 칸에 도착할 확률은 5/9로 0.55555... 이며 1000을 곱한 후 소수부를 버리면 555가 됩니다.
# 풀이1
주사위가 나오는 경우들을 다 저장하려다 보니 3중 반복문이 떠올랐다..
그래도 일단 해보았다.
```python
def solution(monster, S1, S2, S3):
    n = S1*S2*S3
    case = dict()
    meet = 0
    for i in range(1, S1+1):
        for j in range(1, S2+1):
            for k in range(1, S3+1):
                num = 1+i+j+k
                if num in case.keys():
                    case[num] += 1
                else:
                    case[num] = 1
    for m in monster:
        if m in case.keys():
            meet += case[m]
    return int(((n-meet) / n) * 1000)
```
정확성 테스트
```
테스트 1 〉	통과 (1.17ms, 10.2MB)
테스트 2 〉	통과 (0.02ms, 10.3MB)
테스트 3 〉	통과 (0.76ms, 10.2MB)
테스트 4 〉	통과 (0.04ms, 10.3MB)
테스트 5 〉	통과 (0.82ms, 10.2MB)
테스트 6 〉	통과 (1.34ms, 10.2MB)
테스트 7 〉	통과 (0.13ms, 10.1MB)
테스트 8 〉	통과 (0.02ms, 10.2MB)
테스트 9 〉	통과 (0.23ms, 10.2MB)
테스트 10 〉	통과 (6.48ms, 10.2MB)
테스트 11 〉	통과 (3.03ms, 10.3MB)
테스트 12 〉	통과 (0.55ms, 10.2MB)
테스트 13 〉	통과 (1.87ms, 10.2MB)
테스트 14 〉	통과 (0.63ms, 10.2MB)
테스트 15 〉	통과 (0.91ms, 10.1MB)
테스트 16 〉	통과 (5.68ms, 10.3MB)
테스트 17 〉	통과 (0.26ms, 10.3MB)
테스트 18 〉	통과 (0.31ms, 10.2MB)
테스트 19 〉	통과 (3.59ms, 10.2MB)
테스트 20 〉	통과 (2.43ms, 10.3MB)
테스트 21 〉	통과 (2.46ms, 10.3MB)
테스트 22 〉	통과 (2.03ms, 10.2MB)
테스트 23 〉	통과 (2.38ms, 10.2MB)
테스트 24 〉	통과 (0.65ms, 10.2MB)
테스트 25 〉	통과 (0.48ms, 10.2MB)
테스트 26 〉	통과 (2.72ms, 10.2MB)
테스트 27 〉	통과 (5.72ms, 10.2MB)
테스트 28 〉	통과 (5.66ms, 10.2MB)
테스트 29 〉	통과 (0.02ms, 10.3MB)
테스트 30 〉	통과 (0.01ms, 10.4MB)
```
통과가 되었지만 3중 반복문을 사용하지 않고도 풀 수 있는 방법을 생각해야겠다.
# 풀이2
itertools의 product를 통해 모든 경우의 수를 따져보았다.
```python
from itertools import product

def solution(monster, S1, S2, S3):
    meet = 0
    n = S1*S2*S3
    
    p = product(range(S1), range(S2), range(S3))
    for p1,p2,p3 in p:
        # p의 원소들은 0부터
        if 1+(p1+1)+(p2+1)+(p3+1) in monster:
            meet+=1
    return int((n-meet)/n*1000)
```
정확성 테스트
```
테스트 1 〉	통과 (1.86ms, 10.2MB)
테스트 2 〉	통과 (0.01ms, 10.1MB)
테스트 3 〉	통과 (1.16ms, 10.2MB)
테스트 4 〉	통과 (0.04ms, 10.2MB)
테스트 5 〉	통과 (0.67ms, 10.2MB)
테스트 6 〉	통과 (1.72ms, 10.3MB)
테스트 7 〉	통과 (0.17ms, 10.4MB)
테스트 8 〉	통과 (0.01ms, 10.4MB)
테스트 9 〉	통과 (0.25ms, 10.1MB)
테스트 10 〉	통과 (4.27ms, 10.2MB)
테스트 11 〉	통과 (7.89ms, 10.2MB)
테스트 12 〉	통과 (1.12ms, 10.2MB)
테스트 13 〉	통과 (4.14ms, 10.3MB)
테스트 14 〉	통과 (1.22ms, 10.2MB)
테스트 15 〉	통과 (1.65ms, 10.1MB)
테스트 16 〉	통과 (12.07ms, 10.3MB)
테스트 17 〉	통과 (0.85ms, 10.1MB)
테스트 18 〉	통과 (0.41ms, 10.1MB)
테스트 19 〉	통과 (10.02ms, 10.2MB)
테스트 20 〉	통과 (5.32ms, 10.2MB)
테스트 21 〉	통과 (7.78ms, 10.2MB)
테스트 22 〉	통과 (7.75ms, 10.2MB)
테스트 23 〉	통과 (5.84ms, 10.2MB)
테스트 24 〉	통과 (1.40ms, 10.3MB)
테스트 25 〉	통과 (1.59ms, 10.2MB)
테스트 26 〉	통과 (5.59ms, 10.3MB)
테스트 27 〉	통과 (16.82ms, 10.2MB)
테스트 28 〉	통과 (17.17ms, 10.2MB)
테스트 29 〉	통과 (0.01ms, 10.3MB)
테스트 30 〉	통과 (0.01ms, 10.3MB)
```
# 해답
```python
from itertools import product

def solution(monster, S1, S2, S3):
    p = product(range(S1), range(S2), range(S3))
    case = len([x for x in p if sum(x) + 4 not in monster])
    return int(case / (S1 * S2 * S3) * 1000)
```
정확성 테스트
```
테스트 1 〉	통과 (2.16ms, 10.2MB)
테스트 2 〉	통과 (0.02ms, 10.3MB)
테스트 3 〉	통과 (1.28ms, 10.2MB)
테스트 4 〉	통과 (0.05ms, 10.2MB)
테스트 5 〉	통과 (1.01ms, 10.3MB)
테스트 6 〉	통과 (2.38ms, 10.4MB)
테스트 7 〉	통과 (0.21ms, 10.2MB)
테스트 8 〉	통과 (0.02ms, 10.2MB)
테스트 9 〉	통과 (0.39ms, 10.2MB)
테스트 10 〉	통과 (9.34ms, 12.4MB)
테스트 11 〉	통과 (8.99ms, 10.4MB)
테스트 12 〉	통과 (1.43ms, 10.2MB)
테스트 13 〉	통과 (5.26ms, 10.4MB)
테스트 14 〉	통과 (1.36ms, 10.3MB)
테스트 15 〉	통과 (2.05ms, 10.2MB)
테스트 16 〉	통과 (12.33ms, 10.4MB)
테스트 17 〉	통과 (0.89ms, 10.2MB)
테스트 18 〉	통과 (0.57ms, 10.2MB)
테스트 19 〉	통과 (9.60ms, 10.2MB)
테스트 20 〉	통과 (6.23ms, 10.5MB)
테스트 21 〉	통과 (7.41ms, 10.1MB)
테스트 22 〉	통과 (5.97ms, 10.3MB)
테스트 23 〉	통과 (6.54ms, 10.3MB)
테스트 24 〉	통과 (1.62ms, 10.2MB)
테스트 25 〉	통과 (1.43ms, 10.2MB)
테스트 26 〉	통과 (7.03ms, 10.4MB)
테스트 27 〉	통과 (16.69ms, 10.3MB)
테스트 28 〉	통과 (16.69ms, 10.4MB)
테스트 29 〉	통과 (0.02ms, 10.2MB)
테스트 30 〉	통과 (0.01ms, 10.2MB)
```
