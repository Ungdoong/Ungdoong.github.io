# 190709(Tue)\_Day2\_Chapter1_Python_데이터처리

파일 입출력에 관한 내용



## 이론

### 파일 읽기

```python
file = open('data.txt')
content = file.read()
file.close()
```

### with

```python
with open('data.txt') as file
	content = file.read()
```

### for 반복문

```python
for num in range(10):
	print(num)
    
for i in range(len(fruits)):
    print("과일" + str(i+1) + ": fruits[i]")
```

### 인덱싱

```python
fruits = ["사과", "바나나", "키위", "배"] 
last_fruit = fruits[-1] 
tropical_fruits = fruits[1:3] 
no_apple = fruits[1:] 
no_pear = fruits[:3]
```

- 문자열 인덱싱

  ``` python
  word = "superman" 
  print(word[3])          # 'e' 
  print(word[-2])         # 'a' 
  print(word[5:])         # 'man' 
  print(word[:5])         # 'super'
  ```

### .startswith()

- ```python
  word = "superman" 
  print(word.startswith('s'))    # True 
  if word.startswith('a'):
  	print("a로 시작하는 단어입니다.")
  ```

### .split()

- ```python
  intro = "제 이름은 엘리스입니다." 
  print(intro.split()) 
  >>> ["제", "이름은", "엘리스입니다."] 
  
  fruits = "사과,귤,배,바나나" 
  print(fruits.split(',')) 
  >>> ["사과", "귤", "배", "바나나"]
  ```


### .append()

- 리스트에 요소를 뒤에 넣음

### 대소문자 변환

- .upper()
- .lower()

### append()와 lower()의 차이

- append()와 달리 lower()는 원래 문자열을 직접 수정하지 않음

  ```python
  intro = "My name is Elice" 
  intro.lower() 
  print(intro) 
  >>> "My name is Elice"
  
  #수정
  lower_intro = intro.lower()
  print(lower_intro)
  >> "my name is elice"
  ```

### .replace()

- .replace('교체할 문자열', '교체될 문자열')
- replace()도 lower()와 같이 원래 문자열을 직접 수정하진 않음



## 실습

### 1. 문장의 단어를 하나씩 가져오기

```python
# 트럼프 대통령의 1월 1~3일 트윗을 각각 리스트의 원소로 저장합니다.
trump_tweets = [
    'Will be leaving Florida for Washington (D.C.) today at 4:00 P.M. Much work to be done, but it will be a great New Year!',
    'Companies are giving big bonuses to their workers because of the Tax Cut Bill. Really great!',
    'MAKE AMERICA GREAT AGAIN!'
]

def date_tweet(tweet):
    '''
    트럼프 대통령의 트윗을 트윗 일자와 함께 출력합니다.
    '''
    
    # index에 0~2을 차례대로 저장하여 반복문을 실행합니다.
    for index in range(len(tweet)):
        print('2018년 1월 ' + str(index+1) + '일: ' + tweet[index])


# 실행 결과를 확인하기 위한 코드입니다.
date_tweet(trump_tweets)
```



### 2. 단어의 일부분 가져오기

```python
# 트럼프 대통령 트윗을 공백 기준으로 분리한 리스트입니다. 수정하지 마세요.
trump_tweets = ['thank', 'you', 'to', 'president', 'moon', 'of', 'south', 'korea', 'for', 'the', 'beautiful', 'welcoming', 'ceremony', 'it', 'will', 'always', 'be', 'remembered']

def print_korea(text):
    '''
    문자열로 구성된 리스트에서 k로 시작하는 문자열을 출력합니다.
    '''
    # 아래 코드를 작성하세요.
    for word in text:
        if word[0] == 'k':
            print(word)
    
    
# 아래 주석을 해제하고 결과를 확인해보세요.  
print_korea(trump_tweets)
```

### 3. 단어의 첫 글자 확인하기

- 문자열.startswith("찾을문자")

```python
# 트럼프 대통령 트윗을 공백 기준으로 분리한 리스트입니다. 수정하지 마세요.
trump_tweets = ['thank', 'you', 'to', 'president', 'moon', 'of', 'south', 'korea', 'for', 'the', 'beautiful', 'welcoming', 'ceremony', 'it', 'will', 'always', 'be', 'remembered']

def print_korea(text):
    '''
    문자열로 구성된 리스트에서 k로 시작하는 문자열을 출력합니다.
    '''
    
    # 아래 코드를 작성하세요.
    for word in text:
        if word.startswith('k'):
            print(word)
    
    
    
# 아래 주석을 해제하고 결과를 확인해보세요.  
print_korea(trump_tweets)
```



### 4. 문장을 단어 단위로 구분하기

- split() 사용

```python
# 트럼프 대통령의 트윗으로 구성된 문자열입니다. 수정하지 마세요. 
trump_tweets = "thank you to president moon of south korea for the beautiful welcoming ceremony it will always be remembered"

def break_into_words(text):
    '''
    공백 기준으로 분리된 문자열을 리스트형으로 반환합니다. 
    
    >>> break_into_words('merry christmas')
    ['merry', 'christmas']
    '''
    
    # 아래 코드를 작성하세요.
    words = []
    words = text.split(' ')
    
    
    return words


# 아래 주석을 해제하고 결과를 확인해보세요.  
print(break_into_words(trump_tweets))
```



### 5. 새로운 단어 추가하기

```python
# 트럼프 대통령 트윗을 공백 기준으로 분리한 리스트입니다. 수정하지 마세요.
trump_tweets = ['america', 'is', 'back', 'and', 'we', 'are', 'coming', 'back', 'bigger', 'and', 'better', 'and', 'stronger', 'than', 'ever', 'before']

def make_new_list(text):
    '''
    문자열로 구성된 리스트에서 b로 시작하는 모든 문자열을 새로운 리스트에 담습니다.
    '''
    
    # 아래 코드를 작성하세요.
    new_list = []
    for word in text:
        if word.startswith('b'):
            new_list.append(word)
    
    
    
    return new_list


# 아래 주석을 해제하고 결과를 확인해보세요.  
# new_list = make_new_list(trump_tweets)
# print(new_list)
```



### 6. 대소문자 변환하기

```python
# 트럼프 대통령의 트윗 세개로 구성된 리스트입니다. 수정하지 마세요.
trump_tweets = [
    "FAKE NEWS - A TOTAL POLITICAL WITCH HUNT!",
    "Any negative polls are fake news, just like the CNN, ABC, NBC polls in the election.",
    "The Fake News media is officially out of control.",
]
 
def lowercase_all_characters(text):
    '''
    리스트에 저장된 문자열을 모두 소문자로 변환합니다.
    
    >>> lowercase_all_characters(['FAKE NEWS', 'Fake News'])
    ['fake news', 'fake news']
    '''
    
    processed_text = []
    # 아래 코드를 작성하세요.
    for tweet in text:
        processed_text.append(tweet.lower())
    
    
    
    return processed_text


# 아래 주석을 해제하고 결과를 확인해보세요.  
print('\n'.join(lowercase_all_characters(trump_tweets)))
```



### 7. 특수기호 삭제하기

```python
# 트럼프 대통령의 트윗 세개로 구성된 리스트입니다. 수정하지 마세요.
trump_tweets = [
    "i hope everyone is having a great christmas, then tomorrow it’s back to work in order to make america great again.",
    "7 of 10 americans prefer 'merry christmas' over 'happy holidays'.",
    "merry christmas!!!",
]

def remove_special_characters(text):
    '''
    리스트에 저장된 문자열에서 쉼표, 작은따옴표, 느낌표를 제거합니다.
    
    >>> remove_special_characters(["wow!", "wall,", "liberals'"])
    ['wow', 'wall', 'liberals']
    '''
    
    processed_text = []
    # 아래 코드를 작성하세요.
    for word in text:
        word = word.replace('!', '')
        word = word.replace('\'', '')
        word = word.replace(',', '')
        processed_text.append(word)
    
    
    return processed_text


# 아래 주석을 해제하고 결과를 확인해보세요.  
print('\n'.join(remove_special_characters(trump_tweets)))
```

