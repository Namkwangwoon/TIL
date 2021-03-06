# 자물쇠와 열쇠 사본
## 문제 설명
고고학자인 "튜브"는 고대 유적지에서 보물과 유적이 가득할 것으로 추정되는 비밀의 문을 발견하였습니다. 그런데 문을 열려고 살펴보니 특이한 형태의 자물쇠로 잠겨 있었고 문 앞에는 특이한 형태의 열쇠와 함께 자물쇠를 푸는 방법에 대해 다음과 같이 설명해 주는 종이가 발견되었습니다.

잠겨있는 자물쇠는 격자 한 칸의 크기가 1 x 1인 N x N 크기의 정사각 격자 형태이고 특이한 모양의 열쇠는 M x M 크기인 정사각 격자 형태로 되어 있습니다.

자물쇠에는 홈이 파여 있고 열쇠 또한 홈과 돌기 부분이 있습니다. 열쇠는 회전과 이동이 가능하며 열쇠의 돌기 부분을 자물쇠의 홈 부분에 딱 맞게 채우면 자물쇠가 열리게 되는 구조입니다. 자물쇠 영역을 벗어난 부분에 있는 열쇠의 홈과 돌기는 자물쇠를 여는 데 영향을 주지 않지만, 자물쇠 영역 내에서는 열쇠의 돌기 부분과 자물쇠의 홈 부분이 정확히 일치해야 하며 열쇠의 돌기와 자물쇠의 돌기가 만나서는 안됩니다. 또한 자물쇠의 모든 홈을 채워 비어있는 곳이 없어야 자물쇠를 열 수 있습니다.

열쇠를 나타내는 2차원 배열 key와 자물쇠를 나타내는 2차원 배열 lock이 매개변수로 주어질 때, 열쇠로 자물쇠를 열수 있으면 true를, 열 수 없으면 false를 return 하도록 solution 함수를 완성해주세요.

## 제한사항
- key는 M x M(3 ≤ M ≤ 20, M은 자연수)크기 2차원 배열입니다.
- lock은 N x N(3 ≤ N ≤ 20, N은 자연수)크기 2차원 배열입니다.
- M은 항상 N 이하입니다.
- key와 lock의 원소는 0 또는 1로 이루어져 있습니다.
- 0은 홈 부분, 1은 돌기 부분을 나타냅니다.
## 입출력 예
|keylock|result|
|---|---|
|[[0, 0, 0], [1, 0, 0], [0, 1, 1]]|[[1, 1, 1], [1, 1, 0], [1, 0, 1]]|true|
## 입출력 예에 대한 설명

![image](https://user-images.githubusercontent.com/19163372/119627054-cc545880-be46-11eb-9895-bc1c83132bf7.png)

key를 시계 방향으로 90도 회전하고, 오른쪽으로 한 칸, 아래로 한 칸 이동하면 lock의 홈 부분을 정확히 모두 채울 수 있습니다.
# 해답
역시 카카오 기출답게 노가다(?)가 포함되어 있었다.

그러더가 너무 시간이 오래걸려서 결국 포기했고, 해답에서 더 효율적인 방법이나, 깔끔하고 직관적인 풀이를 사용했을 것 같아서 결국 참고하게 되었다..
```python
import copy

def expend_lock(lock, N, M, size):
    # 자물쇠를 확장시키는 함수
    # 순수 함수
    expended_lock = [[0 for i in range(size)] for _ in range(size)]
    for y in range(N):
        for x in range(N):
            expended_lock[y + M - 1][x + M - 1] = lock[y][x]
            
    return expended_lock

def rotate(key):
    # *key는 껍질(Unpacking)을 벗겨내는 역할을 합니다. [] () {}
    # [(0, 0, 0), (1, 0, 0), (0, 1, 1)] *key 
    # [(0, 1, 0), (0, 0, 1), (0, 0, 1)] zip(*key)
    # [(0, 1, 0), (1, 0, 0), (1, 0, 0)] reversed
    # [[0, 1, 0]] ... -> list
    # 순수 함수
    return [list(reversed(i)) for i in zip(*key)]

def is_unlock(y, x, key, lock, N, M):
    # 순수 함수
    # 순수 함수가 아니면 Side Effect가 있을 수 있는 함수라고 한다.
    _lock = copy.deepcopy(lock)
    for _y in range(M):
        for _x in range(M):
            _lock[_y + y][_x + x] += key[_y][_x]

    for _y in range(N):
        for _x in range(N):
            if _lock[_y + M - 1][_x + M - 1] != 1:
                return False
    
    return True

def solution(key, lock):
    # 선언
    # ---------------
    N = len(lock)
    M = len(key)
    size = (N - 1) * 2 + M
    expended_lock = expend_lock(lock, N, M, size)
    
    # 실행, 출력 ^^;
    # ---------------
    for _ in range(4):
        for y in range(size - M + 1):
            for x in range(size - M + 1):
                if is_unlock(y, x, key, expended_lock, N, M):
                    return True
        key = rotate(key)
    
    return False
```
- 해답은 전체적으로 푸는 방식은 비슷했지만(워낙 방식은 명확하고 노가다가 중요한 문제라..) 해답은 좀 더 직관적인 형태로 풀이했다. (효율성 테스트가 없는 문제라 가능했던 것 같다)
1. 해답에서는 열쇠가 들어갈 수 있도록 자물쇠의 공간을 확장하고 시작했다. (다른 사람의 풀이에서는 키와 자물쇠가 겹치지 않는 공간까지 확장했는데, 키가 없이도 자물쇠가 열리는 경우를 고려한다면, 그게 더 좋은 것 같다.)
2. 키를 한쪽 방향으로 돌려가며 자물쇠에 맞춰보는 모든 경우의 수를 검사했다.
3. 키를 맞춰보고 검사하는 방법은, 자물쇠와 키의 겹치는 좌표를 더해서, 모든 자물쇠의 좌표가 1일때, 자물쇠가 열리도록 했다.(2-3 방법은 나의 풀이와 같았다.)
정확성  테스트
```
테스트 1 〉	통과 (3.23ms, 10.3MB)
테스트 2 〉	통과 (0.89ms, 10.3MB)
테스트 3 〉	통과 (194.55ms, 10.4MB)
테스트 4 〉	통과 (0.15ms, 10.3MB)
테스트 5 〉	통과 (354.15ms, 10.4MB)
테스트 6 〉	통과 (876.28ms, 10.4MB)
테스트 7 〉	통과 (735.65ms, 10.3MB)
테스트 8 〉	통과 (1373.25ms, 10.4MB)
테스트 9 〉	통과 (1659.66ms, 10.4MB)
테스트 10 〉	통과 (4142.88ms, 10.4MB)
테스트 11 〉	통과 (5238.28ms, 10.2MB)
테스트 12 〉	통과 (0.14ms, 10.3MB)
테스트 13 〉	통과 (102.86ms, 10.2MB)
테스트 14 〉	통과 (28.72ms, 10.4MB)
테스트 15 〉	통과 (76.88ms, 10.3MB)
테스트 16 〉	통과 (457.47ms, 10.3MB)
테스트 17 〉	통과 (20.45ms, 10.4MB)
테스트 18 〉	통과 (1257.22ms, 10.4MB)
테스트 19 〉	통과 (18.52ms, 10.4MB)
테스트 20 〉	통과 (3394.73ms, 10.4MB)
테스트 21 〉	통과 (303.55ms, 10.4MB)
테스트 22 〉	통과 (399.62ms, 10.3MB)
테스트 23 〉	통과 (47.78ms, 10.5MB)
테스트 24 〉	통과 (45.06ms, 10.3MB)
테스트 25 〉	통과 (5350.34ms, 10.4MB)
테스트 26 〉	통과 (2950.95ms, 10.4MB)
테스트 27 〉	통과 (6155.74ms, 10.3MB)
테스트 28 〉	통과 (280.28ms, 10.3MB)
테스트 29 〉	통과 (93.38ms, 10.3MB)
테스트 30 〉	통과 (459.36ms, 10.3MB)
테스트 31 〉	통과 (3314.02ms, 10.3MB)
테스트 32 〉	통과 (4751.18ms, 10.4MB)
테스트 33 〉	통과 (456.73ms, 10.3MB)
테스트 34 〉	통과 (2.65ms, 10.3MB)
테스트 35 〉	통과 (22.60ms, 10.3MB)
테스트 36 〉	통과 (47.87ms, 10.3MB)
테스트 37 〉	통과 (33.40ms, 10.4MB)
테스트 38 〉	통과 (18.68ms, 10.3MB)
```
역시 쉽지 않다 카카오...
