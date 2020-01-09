# 190709(Tue)\_Day2\_Chapter2_Python_데이터분석

## 이론

### Comprehension

#### └ List Comprehension(LC)

- 리트스를 쉽게 '생성'하기 위한 방법

- ```python
  evens = [x * 2 for x in range(11)]
  ```

#### └ Set Comprehension(SC)

- LC와 동일하며 단지 list가 아닌 set을 생성

- ```python
  # 다음의 LC는 중복된 값들을 포함한다
  no_primes = [j for i in range(2, 9) for j in range(i * 2, 50, i)]
  # [4, 6, 8, 10, 12, 14, 16, 18, 20, 22, 24, 26, 28, 30, 32, 34, 36, 38, 40, 42, 44, 46, 48, 6, 9, 12, 15, 18, 21, 24, 27, 30, 33, 36, 39, 42, 45, 48, 8, 12, 16, 20, 24, 28, 32, 36, 40, 44, 48, 10, 15, 20, 25, 30, 35, 40, 45, 12, 18, 24, 30, 36, 42, 48, 14, 21, 28, 35, 42, 49, 16, 24, 32, 40, 48]
  
  # SC를 사용하면 중복값이 없는 집합을 얻을 수 있다
  no_primes = {j for i in range(2, 9) for j in range(i * 2, 50, i)}
  # {4, 6, 8, 9, 10, 12, 14, 15, 16, 18, 20, 21, 22, 24, 25, 26, 27, 28, 30, 32, 33, 34, 35, 36, 38, 39, 40, 42, 44, 45, 46, 48, 49}
  ```

  

#### └ Dict Comprehension(DC)

- LC와 동일하며 dict를 생성

- ```python
  # 두 리스트를 하나의 dict로 합치는 DC. 하나는 key, 또 다른 하나는 value로 사용한다
  subjects = ['math', 'history', 'english', 'computer engineering']
  scores = [90, 80, 95, 100]
  score_dict = {key: value for key, value in zip(subjects, scores)}
  # {'math': 90, 'history': 80, 'english': 95, 'computer engineering': 100}
  
  # 튜플 리스트를 dict 형태로 변환하는 DC
  score_tuples = [('math', 90), ('history', 80), ('english', 95), ('computer engineering', 100)]
  score_dict = {t[0]: t[1] for t in score_tuples}
  # {'math': 90, 'history': 80, 'english': 95, 'computer engineering': 100}
  ```

  

#### └ Generator Expression(GE)

- 특별한 형태

- generator 생성 

  - 한 번에 모든 원소를 반환하지 않고 한 번에 하나의 원소만 반환

- ```python
  # 다음 Generator는 제곱수를 만들어낸다
  gen = (x**2 for x in range(10))
  print(gen)
  # <generator object <genexpr> at 0x105bde5c8>
  print(next(gen)) # call 1
  # 0
  print(next(gen)) # call 2
  # 1
  # 'next' 함수 호출을 10번 반복
  print(next(gen)) # call 10
  # 81
  print(next(gen)) # call 11
  """
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
  StopIteration
  """
  
  # Yes, it is an just generator. You can sum the yielding values.
  # GE로 생성한 Generator도 yield를 가진 함수로 생성한 것과 동일한 Generator이기 때문에, 똑같이 sum을 사용할 수 있다. (iterable 객체)
  gen = (x**2 for x in range(10))
  sum_of_squares = sum(gen)
  # 285
  ```

  



### Comprehension(리스트로 리스트 만들기)

- ```python
  new_list = [word for word in words]
  ```

### Comprehension에서 특정 요소 제거

- ```python
  new_list = [word for word in words if word.startswith(prefix)]
  ```

### 데이터 정렬

- sorted(변수, key= )
- key
  - abs : 절대값
  - reverse : 역순 (reverse=True 와 동일)

### 그래프 다루기

- Mathematical Plot Library - matplotlib - 파이썬 그래프 라이브러리

## 실습

### 1. 파일 열고 읽기

```python
# 텍스트 파일을 불러옵니다.
corpus = 'corpus.txt'

def print_lines(filename):
    '''
    파일의 내용을 줄 번호와 한 줄씩 출력합니다.
    
    >>> print_lines(corpus)
    1 zoo,768
    2 zones,1168
    3 zone,2553
    '''
    
    line_number = 1
    # 아래 코드를 작성하세요.
    with open('corpus.txt') as corpus:
        for line in corpus:
            print(str(line_number) + " " + line)
            # 1 This is Elice. 와 같이, "(줄번호) (내용)" 형식으로 출력합니다.
            
            line_number += 1
	
    #하위 내용은 enumerate를 사용한 동일한 출력을 나타내는 코드
    #with open('corpus.txt') as corpus:
    #    for index, line in enumerate(corpus):
    #        print(str(index+1) + " " + line)


# 아래 주석을 해제하고 결과를 확인해보세요.  
print_lines(corpus)
```



### 2. 데이터 형태 변환하기

```python
# 텍스트 파일을 불러옵니다.
corpus = 'corpus.txt'

def import_as_tuple(filename):
    '''
    (단어, 빈도수)` 튜플로 구성된 리스트를 리턴합니다.
    
    >>> import_as_tumple(corpus)
    [('zoo', '768'), ('zones', '1168'), ... ] 
    '''
    
    tuples = []
    with open(filename) as file:
        for line in file:
            # 아래 코드를 작성하세요.
            result = line.strip().split(',')
            word = result[0]
            count = result[1]
            t = (word, count)
            
            #t = (result[0], result[1])
            #t = tuple(result)
            
            tuples.append(t)
            
    return tuples


# 아래 주석을 해제하고 결과를 확인해보세요.  
print(import_as_tuple(corpus))
```



### 3. 한 줄 명령어로 데이터 다루기

```python
# 단어 모음을 선언합니다. 수정하지 마세요.
words = [
    'apple',
    'banana',
    'alpha',
    'bravo',
    'cherry',
    'charlie',
]

def filter_by_prefix(words, prefix):
    '''
    prefix로 시작하는 word를 리턴합니다. 

    >>> filter_by_prefix(words, 'a')
    'apple'
    '''
    
    # 아래 코드를 작성하세요.
    new_list = [word for word in words if word.startswith(prefix)]
    
    return new_list


# 아래 주석을 해제하고 결과를 확인해보세요.  
a_words = filter_by_prefix(words, 'a')
print(a_words)
```

### 

### 4. 데이터 정렬하기

```python
# 단어어 해당 단어의 빈도수를 담은 리스트를 선언합니다. 수정하지 마세요.
pairs = [
    ('time', 8),
    ('the', 15),
    ('turbo', 1),
]

def get_freq(pair):
    '''
    (단어, 빈도수) 쌍으로 이루어진 튜플을 받아, 빈도수를 리턴합니다.
    
    >>> get_freq(('time', 8))
    8
    '''
    
    return pair[1]


def sort_by_frequency(pairs):
    '''
    (단어, 빈도수) 꼴 튜플의 리스트를 받아, 빈도수가 낮은 순서대로 정렬하여 리턴합니다.
    
    >>> sort_by_frequency(pairs)
    [('turbo', 1), ('time', 8), ('the', 15)]
    '''
    
    return sorted(pairs, key=get_freq)


# 아래 주석을 해제하고 결과를 확인해보세요.  
print(sort_by_frequency(pairs))
```



### 5. 차트 그리기

```python
# matplotlib의 일부인 pyplot 라이브러리를 불러옵니다.
import matplotlib.pyplot as plt

# 엘리스에서 차트를 그릴 때 필요한 라이브러리를 불러옵니다.
from elice_utils import EliceUtils
elice_utils = EliceUtils()

# 월별 평균 기온을 선언합니다. 수정하지 마세요.
years = [2013, 2014, 2015, 2016, 2017]
temperatures = [5, 10, 15, 20, 17]

def draw_graph():
    '''
    막대 차트를 출력합니다.
    '''
    
    # 막대 그래프의 막대 위치를 결정하는 pos를 선언합니다.
    pos = range(len(years))  # [0, 1, 2, 3, 4]
    
    # 높이가 온도인 막대 그래프를 그립니다.
    # 각 막대를 가운데 정렬합니다.
    plt.bar(pos, temperatures, align='center')
    
    # 각 막대에 해당되는 연도를 표기합니다.
    plt.xticks(pos, years)
    
    # 그래프를 엘리스 플랫폼 상에 표시합니다.
    plt.savefig('graph.png')
    elice_utils.send_image('graph.png')

print('막대 차트를 출력합니다.')
draw_graph()
```

