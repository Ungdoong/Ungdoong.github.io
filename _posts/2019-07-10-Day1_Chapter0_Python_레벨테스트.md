# 190710(Wed)\_Day1\_Chapter0\_Python_레벨테스트

# 커리큘럼

### 1. 파이썬 기초, 데이터 크롤링

### 2. 데이터 분석

### 3. 가장 간단한 챗봇의 구현

### 4. 나만의 챗봇 만들기

### 5. Git/Git Hub로 프로젝트 배포하기



## 참고사항

### 슬랙

- #general - 일반 공지
- #random - 다양한 랜덤 채팅
- #day(#) - 각 일별 수업 채팅
- #project-slackbot - 챗봇 프로젝트
- #issues-technical - 기술/문제
- #issues-operation - 교육/운영 문제



## 실습

### 1. 안녕 토끼!

```python
print('Hello Rabbit!')
```

### 2. 카드병정의 장미 세기

```python
# ?를 채워주세요
x = 6
y = 3

# 빈 곳도 채워주세요!
# 더하기 연산
my_sum = x + y
print('my_sum :', my_sum)

# 빼기 연산
my_sub = x - y
print('my_sub :', my_sub)

# 곱하기 연산
my_mul = x * y
print('my_mul :', my_mul)

# 나누기 연산
my_div = x / y
print('my_div :', my_div)
```

### 3. 거꾸로 숫자세기!

```python
count = 10

for i in range(10):
    print('현재 숫자: ' + str(10-i))
```

### 4. 개굴개굴 개구리

- 임의의 길이의 문자열을 입력시 그 길이만큼 "개굴"을 출력하도록 하세요. 단, 공백은 그대로 유지되어야 합니다.

```python
x = input()

string = ''
for ch in x:
    if ch == ' ':
        string += ' '
    else:
        string += '개굴'
        
print(string)
```

### 5. 구름다리를 건너는 토끼

```python
def crossBridge(steps):
    cnt = 0
    current = 0
    n = len(steps)

    while current < n:
        current += steps[current]
        cnt += 1
    return cnt
    
    
print(crossBridge([1,1,2,3,5]))
```

### 6. 개구리 왕자 이름 찾기

```python
def isPrince(frogList):
    names = []
    for elem in frogList:

        if elem[0] == "F":
            
            
            return elem
        
print(isPrince(['Alice', 'Bob', 'Frog']))
```

### 7. 토끼와 거북이의 달리기 경주

```python
string = input()

def gcd(a, b):
    while b != 0 :
        r = a % b
        a = b
        b = r
    return a

string = string.split(' ')

rabbit = int(string[0])
turtle = int(string[1])
print( int((rabbit * turtle) / gcd(rabbit, turtle)) )
```

### 8. 엘리스와 별 헤는 밤

