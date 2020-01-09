# 190709(Tue)\_Day2\_Chapter4_Python_복잡한형태의데이터

## 이론

### CSV(Comma Separated Value)

- ,(comma)로 구분
- 콤마 대신 |(delimiter) 도 사용 가능

- 데이터 내에 콤마(,)가 포함된 경우 → 큰따옴표("")를 이용하여 데이터를 감싼다.
  
- "Eat, Pray, Love", 2010, "헬로우, 뉴욕"
  
- 장점

  - 같은 데이터를 저장하는 데 용량을 적게 소모

- 단점

  - 데이터 오염에 취약

- 사용

  ```python
  import csv with open('movies.csv') as file:     
      reader = csv.reader(file, delimiter=',')     
      for row in reader:         
          print(row[0])
  ```

### lambda

- 간이 함수 작성

- ```python
  def square(x):
  	return x * x
  
  #lambda
  square = lambda x: x * x
  ```

- ```python
  def get_eng_title(row):     
  split = row.split(',')     
  return split[1] 
  sorted(movies, key=get_eng_title)
  
  #lambda
  get_eng_title = lambda row: row.split(',')[1] 
  sorted(movies, key=get_eng_title)
  ```

### assert()

- 두 함수의 값이 같으면 통과 아니면 에러

  ```python
  def square1(x):     
  	return x * x 
  
  square2 = lambda x: x * x 
  
  # 두 값이 같으면 통과, 아니면 에러 
  assert(square1(3) == square2(3))
  ```

### map()

- for문을 쓰지 않고도 리스트에서 원하는 요소를 뽑을수 있음

- 맵으로 생성한 결과 값은 리스트가 아닌 map이라는 타입을 가짐
- for문은 사용 가능
- 리스트처럼 사용하기 위해서는 형변환 해주어야함

- ```python
  movies = [     
  	"다크나이트,The Dark Knight,2008",     
  	"겨울왕국,Frozen,2013",     
  	"슈렉,Shrek,2001",     
  	"슈퍼맨,Superman,1978" 
  ]
  
  def helper(movie_str):
      movie_str.split(',')[1]
  
  eng_movies = map(helper, movies)
  
  print(eng_movies[0])			#error 에러가 나지 않게 하려면
  eng_movies = list(eng_movies)	#리스트 형태로 변형해 주어야 한다.
  
  for eng_movie in eng_movies:	#가능
  
  eng_titles = [     
  	"The Dark Knight",     
  	"Frozen",     
  	"Shrek",     
  	"Superman" 
  ]
  
  def get_eng_title(row):     
  	split = row.split(',')     
  	return split[1] 
  
  eng_titles = \     
  	[get_eng_title(row) for row in movies]
  
  #[get_eng_title(row) for row in movies]
  eng_titles = map(get_eng_title, movies)
  
  #[row.split(',')[1] for row in movies]
  eng_titles = map(     
      lambda row: row.split(',')[1],     
      movies 
  )
  ```



### filter()

- 리스트에서 원하는 결과값만을 추출

- 리턴 값은 리스트가 아닌  filter 타입을 가짐

- ```python
  words = ['real', 'man', 'rhythm', ...] 
  #r_words = [word for word in words if word.startswith('r')]
  r_words = ['real', 'rhythm', 'right', ...]
  
  #1
  def starts_with_r(word):    
      return word.startswith('r')
  
  r_words = filter(starts_with_r, words)
  
  #2
  starts_with_r = lambda w: w.startswith('r')
  r_words = filter(starts_with_r, words)
  
  print(r_words)	#<filter object at 0x~ 
  				#리스트가 아닌 filter 타입을 가짐
  ```

  



## 실습

### 1.  CSV 데이터 읽고 처리하기

```python
# csv 모듈을 임포트합니다. 
import csv

def print_book_info(filename):
    '''
    CSV에서 제목, 저자, 페이지를 추출합니다.
    '''
    
    # 아래 주석을 해제하고 실행 결과를 확인해보세요.
    with open(filename) as file:
        # ',' 기호로 분리된 CSV 파일을 처리하세요..
        reader = csv.reader(file)
        
        # 처리된 파일의 각 줄을 불러옵니다.
        for row in reader:
            
            # 함수를 완성하세요.
            title = row[0]
            author = row[1]
            pages = row[3]
            print("{} ({}): {}p".format(title, author, pages))


# 아래 주석을 해제하고 실행 결과를 확인해보세요.
filename = 'books.csv'
print_book_info(filename)
```

### 2. CSV 데이터 변환하기

```python
# CSV, JSON 모듈을 임포트합니다.
import csv
import json
from elice_utils import EliceUtils

elice_utils = EliceUtils()

def books_to_json(src_file, dst_file):
    '''
    CSV 데이터를 JSON 형태로 변환합니다.
    '''
    
    # 아래 함수를 완성하세요.
    books = []
    with open(src_file) as src:
        reader = csv.reader(src)
        
        # 각 줄 별로 대응되는 book 딕셔너리를 만듭니다.
        for row in reader:
            # 책 정보를 저장하는 딕셔너리를 생성합니다.
            book = {
                "title" : row[0],
                "author" : row[1],
                "genre" : row[2],
                "pages" : int(row[3]),
                "publisher" : row[4]
            }
            books.append(book)
    
    with open(dst_file, 'w') as dst:
        # JSON 형식으로 dst_file에 저장합니다.
        dst.write(json.dumps(books))


# 아래 주석을 해제하고 결과를 확인해보세요.  
src_file = 'books.csv'
dst_file = 'books.json'
books_to_json(src_file, dst_file)
elice_utils.send_file(dst_file)
```

### 3. 한 줄 함수 작성하기

```python
# num을 제곱한 값을 리턴합니다.
def _square(num):
    return num * num

# _square()와 동일한 기능을 하는 lambda 함수 square를 만들어 보세요.
square = lambda num: num * num


# string이 빈 문자열일 경우 빈 문자열을, 아니면 첫 번째 글자를 리턴합니다.
def _first_letter(string):
    return string[0] if string else ''

first_letter = lambda string: string[0] if string else ''


# assert를 이용하여 두 함수의 기능이 동일한 지 테스트합니다. 아래 주석을 해제하고 결과 값을 확인해보세요.
testcases1 = [3, 10, 7, 1, -5]
for num in testcases1:
    assert(_square(num) == square(num))

testcases2 = ['', 'hello', 'elice', 'abracadabra', '  abcd  ']
for string in testcases2:
    assert(_first_letter(string) == first_letter(string))

# 위의 assert 테스트를 모두 통과해야만 아래의 print문이 실행됩니다.
print("성공했습니다!")
```

### 4. 함수를 리턴하는 함수

```python
def min_validator(minimum):
    '''
    주어진 값이 정수가 아니거나 최솟값 minimum보다 작으면 False를 리턴하는 함수를 리턴합니다.
    '''

    def helper(n):
        # n의 타입이 정수가 아니면 False를 리턴합니다.
        if type(n) is not int:
            return False
        
        # 아래 함수를 완성하세요.
        return n >= minimum 
    
    return helper
    

def max_validator(maximum):
    '''
    주어진 값이 정수가 아니거나 최댓값 maximum보다 크면 False를 리턴하는 함수를 리턴합니다.
    '''
    
    def helper(n):
        # n의 타입이 정수가 아니면 False를 리턴합니다.
        if type(n) is not int:
            return False
        
        # 아래 함수를 완성하세요.
        return n <= maximum
    
    return helper


def validate(n, validators):
    # validator 중 하나라도 통과하지 못하면 False를 리턴합니다.
    for validator in validators:
        if not validator(n):
            return False
    
    return True


# 작성한 함수를 테스트합니다. # 아래 주석을 해제하고 결과 값을 확인해보세요.
# # 나이 데이터를 검증하는 validator를 선언합니다. 
age_validators = [min_validator(0), max_validator(120)]
ages = [9, -3, 7, 33, 18, 1999, 287, 0, 13]

# # 주어진 나이 데이터들에 대한 검증 결과를 출력합니다.
print("검증 결과")
for age in ages:
    result = "유효함" if validate(age, age_validators) else "유효하지 않음"
    print("{}세 : {}".format(age, result))	
```

### 5. 리스트에 함수 적용하기

```python
# CSV 모듈을 임포트합니다.
import csv

def get_titles(books_csv):
    '''
    CSV 파일을 읽고 제목의 리스트를 리턴합니다. 
    '''
    
    with open(books_csv) as books:
        reader = csv.reader(books, delimiter=',')
        # 함수를 완성하세요.
        get_title = lambda row: row[0]
        titles = map(get_title, reader)
        
        return list(titles)


# 작성한 코드를 테스트합니다. 주석을 해제하고 실행하세요.
books = 'books.csv'
titles = get_titles(books)
for title in titles:
     print(title)
```

### 6. 리스트에 함수 적용하기

```python
# CSV 모듈을 임포트합니다.
import csv

def get_titles_of_long_books(books_csv):
    '''
    페이지 수가 250이 넘는 책들의 제목을 리스트로 리턴합니다.
    '''
    
    with open(books_csv) as books:
        reader = csv.reader(books, delimiter=',')
        # 함수를 완성하세요.
        is_long = lambda row: int(row[3]) >= 250
        get_title = lambda row: row[0]
        
        long_books = filter(is_long, reader)
        long_book_titles = map(get_title, long_books)
        
        return list(long_book_titles)


# 작성한 함수를 테스트합니다. 주석을 해제하고 실행하세요.
books  = 'books.csv'
titles = get_titles_of_long_books(books)
for title in titles:
    print(title)
```

