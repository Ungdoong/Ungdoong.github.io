# 190709(Tue)\_Day2\_Chapter3_Python_데이터형변환

## 이론

### 딕셔너리

- { key: value }

  - key : 값을 찾기 위해 넣어 주는 데이터
  - value : 찾고자 하는 데이터

- ```python
  my_dict = {
      'apple' : '사과',
      'pear' : '배',
      'orange' : '오렌지'
  }
  
  print(my_dict['pear'])
  ```

### 딕셔너리 vs. 리스트

- 리스트로 딕셔너리 형태 구현

  ```python
  #dictionary - 순서대로 입력되긴하나 무의미
  accounts = {
  	"kdhong.elice" : "Kildong Hong",
  	...
  }
  #list - 순서대로 인덱스가 매겨짐
  accounts = [
  	("kdhong.elice", "Kildong Hong"),
  	...
  ]
  ```

- 리스트로 구현시

  ```python
  #[(아이디, 이름)]
  for id_, name in accounts:
  	if id_ == "kdhong.elice":
  		print(name)
  ```

  모든 아이디를 확인 → 데이터가 많을 시 성능 차이 > 수십 배

### 딕셔너리의 키

- 변할 수 없는 값만이 key가 될 수 있다.

  ```python
  kdhong = ("kdhong", "cantcalldad")
  accounts = {
  	kdhong: ('Kildong Hong', ...),
  	...
  }
  kdhong[0] = "kdhong.elice" #에러 발생
  ```

  

- 키 값을 슈퍼키로 둘 수 있음

  ```python
  new_dic["taekjin", "29"] = student
  ```

### 딕셔너리의 키 확인하기

- ```python
  accounts = {
  	"kdhong" : "kildong Hong",
  }
  
  print("kdhong" in accounts)	#True
  print("elice" in accounts)	#False
  ```

### 딕셔너리 순회하기

- items()

- ```python
  accounts = {
  	"kdhong" : "kildong Hong",
  }
  
  for username, name in accounts.items():
  	print(username + " - " + name)
  ```

### JSON(JavaScript Object Notation)

- { key: value }
- 웹 환경에서 데이터를 주고 받는 가장 표준적인 방식
- 키를 이용하여 원하는 데이터만 빠르게 추출
- 데이터가 쉽게 오염되지 않음
- 다른 포맷에 비해 용량이 조금 큰 편
- JSON → 딕셔너리 : loads()
- JSON ← 딕셔너리 : dumps()

### 집합

- 중복이 없다.
- 순서가 없다.

- 집합 생성

  ```python
  # 셋 다 같은 값
  set1 = {1, 2, 3}
  set2 = set([1, 2, 3])
  set3 = {3, 2, 3, 1}
  ```

- 원소 추가/삭제

  ```python
  num_set = {1, 3, 5, 7}
  num_set.add(9) 				#원소 추가
  num_set.update([3, 15, 4]) 	#원소 리스트 추가(중복 제외)
  num_set.remove(7) 			#제거(주로 이용)
  num_set.discard(13)			#제거
  ```

- ```python
  num_set = {1, 3, 5, 7} 
  print(6 in num_set)        # False 
  print(len(num_set))        # 4
  ```

- 집합연산

  ```python
  union = set1 | set2         # 합집합 
  intersection = set1 & set2  # 교집합 
  diff = set1 - set2          # 차집합 
  xor = set1 ^ set2           # XOR
  ```





## 실습

### 1. 데이터 빠르게 탐색하기

```python
# 텍스트 파일을 불러옵니다.
source_file = "netflix.txt"

def make_dictionary(filename):
    '''
    텍스트 파일을 읽고 {'사용자 번호': '작품 번호'}로 구성된 딕셔너리를 리턴합니다.
    
    >>> make_dictionary(source_file)
    {'1': '1012', '2': '3781', ... }
    '''
    
    user_to_titles = {}
    with open(filename) as file:
        for line in file:
            # 아래 코드를 작성하세요.
            user, title = line.strip().split(':')
            user_to_titles[user] = title
            
            
        return user_to_titles


# 아래 주석을 해제하고 결과를 확인해보세요.  
print(make_dictionary(source_file))
```

### 2.데이터 순회하기

```python
# 사용자가 시청한 작품의 리스트를 저장합니다. 수정하지 마세요. 
user_to_titles = {
    1: [271, 318, 491],
    2: [318, 19, 2980, 475],
    3: [475],
    4: [271, 318, 491, 2980, 19, 318, 475],
    5: [882, 91, 2980, 557, 35],
}

def get_user_to_num_titles(user_to_titles):
    '''
    사용자가 시청한 작품의 수를 리턴합니다.
    
    >>> get_user_to_num_titles({1: [271, 318, 491]})
    {1: 3}
    '''
    # 아래 함수를 완성하세요.
    user_to_num_titles = {}
    for user, title_list in user_to_titles.items():
        user_to_num_titles[user] = len(title_list)
    
    
    return user_to_num_titles
    

# 아래 주석을 해제하고 결과를 확인해보세요.  
print(get_user_to_num_titles(user_to_titles))
```



### 3. JSON 데이터 읽고 생성하기

```python
# json 패키지를 임포트합니다.
import json

def create_dict(filename):
    '''
    JSON 파일을 읽고 문자열을 딕셔너리로 변환합니다.
    '''
    
    with open(filename) as file:
        contents = file.read()
        data = json.loads(contents)
        # 함수를 완성하세요.
        
    return data


def create_json(dictionary, filename):
    '''
    딕셔너리를 JSON 형태의 문자열로 저장합니다.
    '''

    with open(filename, 'w') as file:
        
        json_string = json.dumps(dictionary)
        file.write(json_string)
        
        
# 아래 주석을 해제하고 결과를 확인해보세요.  
src = 'netflix.json'
dst = 'new_netflix.json'

netflix_dict = create_dict(src)
print('원래 데이터: ' + str(netflix_dict))

netflix_dict['Dark Knight'] = 39217
create_json(netflix_dict, dst)
updated_dict = create_dict(dst)
print('수정된 데이터: ' + str(updated_dict))
```

### 4. 데이터의 집합 나타내기

```python
# 정수 3과 5를 원소로 갖는 새로운 집합을 생성합니다.
my_set = {3, 5, 3}

# 채점을 위한 코드입니다. 수정하지 마세요. 
submit1 = my_set.copy()

# 정수 7을 my_set에 추가합니다.
my_set.add(7)

# 채점을 위한 코드입니다. 수정하지 마세요. 
submit2 = my_set.copy()

# new_numbers 리스트의 원소를 my_set에 추가합니다.
new_numbers = [1, 2, 3, 4, 5]
my_set.update(new_numbers)


# 채점을 위한 코드입니다. 수정하지 마세요. 
submit3 = my_set.copy()

# my_set에서 짝수를 모두 제거합니다.
my_set = {num for num in my_set if num%2 == 1}


# 채점을 위한 코드입니다. 수정하지 마세요. 
submit4 = my_set.copy()
```

### 5. 교집합과 합집합 구하기 - 집합 연산자

```python
# 각 영화 별 시청자 리스트를 임포트합니다.
from viewers import dark_knight, iron_man

dark_knight_set = set(dark_knight)
iron_man_set = set(iron_man)

# 두 작품을 모두 시청한 사람의 수
both = len(dark_knight_set & iron_man_set)

# 두 작품 중 최소 하나를 시청한 사람의 수
either = len(dark_knight_set | iron_man_set)

# 다크나이트만 시청한 사람의 수
dark_knight_only = len(dark_knight_set - iron_man_set)

# 아이언맨만 시청한 사람의 수
iron_man_only = len(iron_man_set - dark_knight_set)


# 아래 주석을 해제하고 실행 결과를 확인해보세요.
print("두 작품 모두 시청: {}명".format(both))
print("하나 이상 시청: {}명".format(either))
print("다크나이트만 시청: {}명".format(dark_knight_only))
print("아이언맨만 시청: {}명".format(iron_man_only))
```

### 6. matplotlib으로 차트 설정하기

```python
import matplotlib.pyplot as plt
import matplotlib.font_manager as fm

from elice_utils import EliceUtils
elice_utils = EliceUtils()

# 날짜 별 온도 데이터를 세팅합니다.
dates = ["1월 {}일".format(day) for day in range(1, 32)]
temperatures = list(range(1, 32))

# 막대 그래프의 막대 위치를 결정하는 pos를 선언합니다.
pos = range(len(dates))

# 한국어를 보기 좋게 표시할 수 있도록 폰트를 설정합니다.
font = fm.FontProperties(fname='./NanumBarunGothic.ttf')

# 막대의 높이가 빈도의 값이 되도록 설정합니다.
plt.bar(pos, temperatures, align='center')

# 각 막대에 해당되는 단어를 입력합니다.
plt.xticks(pos, dates, rotation='vertical', fontproperties=font)

# 그래프의 제목을 설정합니다.
plt.title('1월 중 기온 변화', fontproperties=font)

# Y축에 설명을 추가합니다.
plt.ylabel('온도', fontproperties=font)

# 단어가 잘리지 않도록 여백을 조정합니다.
plt.tight_layout()

# 그래프를 표시합니다.
plt.savefig('graph.png')
elice_utils.send_image('graph.png')
```

