# 올바른 괄호
## 문제 설명
올바른 괄호란 두 개의 괄호 '(' 와 ')' 만으로 구성되어 있고, 괄호가 올바르게 짝지어진 문자열입니다. 괄호가 올바르게 짝지어졌다는 것은 '(' 문자로 열렸으면 반드시 짝지어서 ')' 문자로 닫혀야 합니다.
예를들어

- "()()" 또는 "(())()" 는 올바른 괄호입니다.
- ")()(" 또는 "(()(" 는 올바르지 않은 괄호입니다.

'(' 또는 ')' 로만 이루어진 문자열 s가 주어졌을 때, 문자열 s가 올바른 괄호이면 true를 return 하고, 올바르지 않은 괄호이면 false를 return하는 solution 함수를 완성해 주세요.

## 제한사항
- 문자열 s의 길이 : 100,000 이하의 자연수
- 문자열 s는 '(' 또는 ')' 로만 이루어져 있습니다.
## 입출력 예
|s|answer|
|---|---|
|"()()"|true|
|"(())()"|true|
|")()("|false|
|"(()("|false|
## 입출력 예 설명
### 입출력 예 #1,2,3,4
문제의 예시와 같습니다.
# 풀이
처음에 문제를 봤을때 쉬운 문제라고 생각하고 금방 풀 수 있을꺼라고 생각했지만, 몇 번의 수정을 거쳤다. 다음과 같은 디테일한 상황을 고려하지 못했다.
> 앞에서부터 쭉 살펴보다가 ')'나 '())'처럼 ')'의 갯수가 '('의 갯수를 넘어가는 경우는 안된다.

위 제한사항을 코드에 추가해주었다.
```python
def solution(s):
    count = 0
    for c in s:
        if c=='(':
            count+=1
        else:
            count-=1
        if count<0:
            return False
    if count==0:
        return True
    else:
        return False
```
정확성  테스트
```
테스트 1 〉	통과 (0.00ms, 9.97MB)
테스트 2 〉	통과 (0.00ms, 10.2MB)
테스트 3 〉	통과 (0.00ms, 10.2MB)
테스트 4 〉	통과 (0.00ms, 10.2MB)
테스트 5 〉	통과 (0.00ms, 10.1MB)
테스트 6 〉	통과 (0.00ms, 10.2MB)
테스트 7 〉	통과 (0.00ms, 10.1MB)
테스트 8 〉	통과 (0.00ms, 10.1MB)
테스트 9 〉	통과 (0.00ms, 10.2MB)
테스트 10 〉	통과 (0.00ms, 9.97MB)
테스트 11 〉	통과 (0.00ms, 10.1MB)
테스트 12 〉	통과 (0.01ms, 10.2MB)
테스트 13 〉	통과 (0.01ms, 10.3MB)
테스트 14 〉	통과 (0.01ms, 10.2MB)
테스트 15 〉	통과 (0.01ms, 10.1MB)
테스트 16 〉	통과 (0.01ms, 10.2MB)
테스트 17 〉	통과 (0.01ms, 10.2MB)
테스트 18 〉	통과 (0.01ms, 10.2MB)
```
효율성  테스트
```
테스트 1 〉	통과 (6.88ms, 10.3MB)
테스트 2 〉	통과 (6.79ms, 10.2MB)
```
