# [Python]itertools



## itertools

_________

 반복되는 요소에 대한 데이터 조작을 편하게 하기 위한 모듈

### import

```python
import itertools
```

### chain()

리스트를 연결

```python
letters = ['a', 'b', 'c', 'd', 'e', 'f']
booleans = [1, 0, 1, 0, 0, 1]
decimals = [0.1, 0.7, 0.4, 0.4, 0.5]

print list(itertools.chain(letters, booleans, decimals))
```

```
['a', 'b', 'c', 'd', 'e', 'f', 1, 0, 1, 0, 0, 1, 0.1, 0.7, 0.4, 0.4, 0.5]
```

### izip()

요소를 튜플에 결합

```python
from itertools import izip

print list(izip([1, 2, 3], ['a', 'b', 'c']))
```

```
[(1, 'a'), (2, 'b'), (3, 'c')]
```

### count()

반복하고자 하는 최대수를 미리 알지 않아도 되는 경우 사용

```python
from itertools import count , izip
for number, letter in izip(count(0, 10), ['a', 'b', 'c', 'd', 'e']):
    print '{0}: {1}'.format(number, letter)
```

```
0: a  10: b  20: c  30: d  40: e
```

### imap()

요소를 튜플에 결합하는 map()과 같은 함수

```python
from itertools import imap

print list(imap(lambda x: x * x, xrange(10)))
```

```
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

### islice()

```python
from itertools import islice

for i in islice(range(10), 5):
    print i
```

```
0 1 2 3 4
```

[0~10]의 반복가능 객체에서 5번재 안으로 자름

```python
for i in islice(range(100), 0, 100, 10):
    print i
```

```
0 10 20 30 40 50 60 70 80 90
```

첫 번째 매개변수는 반복 가능한 객체, 두 번째 매개변수는 시작 색인, 세 번째 매개변수는 끝 색인, 마지막은 각 반복 후에 건너 뛸 수 있는 단계 또는 숫자

### tee()

두 개의 매개변수 사용

첫 번째는 반복 가능이며 두 번째 인자는 만들고자하는 복사본 갯수

한번 사용된 레퍼런스는 더이상 값을 참조하지 않음

원본 제네레이터를 실행시켜도 나머지 본제본들이 참조가 끊김

```python
from itertools import tee

i1, i2, i3 = tee(xrange(10), 3)
print i1
# <itertools.tee object at 0x2a1fc68>
print list(i1)
# [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
print list(i1)
# []
print list(i2)
# [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
print list(i2)
# []
print list(i3)
# [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
print list(i3)
# []
```

### cycle()

순환 가능한 객체에서 요소를 반복적으로 생성

```python
from itertools import cycle , izip
for number, letter in izip(cycle(range(2)), ['a', 'b', 'c', 'd', 'e']):
    print '{0}: {1}'.format(number, letter)
    # 0: a
    # 1: b
    # 0: c
    # 1: d
    # 0: e
```

### repeat()

요소를 반복. 반복되는 횟수 지정

```python
from itertools import repeat
print list(repeat('Hello, world!', 3))
# ['Hello, world!', 'Hello, world!', 'Hello, world!']
```

### dropwhile()

필터링 함수중 하나

첫 번째 인자의 조건이 만족할 때까지 drop시키고 이후 인자는 모두 리턴

```python
from itertools import dropwhile
print list(dropwhile(lambda x: x < 10, [1, 4, 6, 7, 11, 34, 66, 100, 1]))
# [11, 34, 66, 100, 1]
```

### takewhile()

필터링 함수중 하나

첫 번째 인자의 조건이 만족할 때까지 모든 요소를 리턴

```python
from itertools import takewhile
print list(takewhile(lambda x: x < 10, [1, 4, 6, 7, 11, 34, 66, 100, 1]))
# [1, 4, 6, 7]
```

### ifilter()

필터링 함수 중 하나

첫 번째 인자의 조건에 만족하는 모든 것을 리턴

```python
from itertools import ifilter
print list(ifilter(lambda x: x < 10, [1, 4, 6, 7, 11, 34, 66, 100, 1]))
# [1, 4, 6, 7, 1]
```

### groupby()

```python
from operator import itemgetter
from itertools import groupby

attempts = [
    ('dan', 87),
    ('erik', 95),
    ('jason', 79),
    ('erik', 97),
    ('dan', 100)
]

# Sort the list by name for groupby
attempts.sort(key=itemgetter(0))

# Create a dictionary such that name: scores_list
print {key: sorted(map(itemgetter(1), value)) for key, value in groupby(attempts, key=itemgetter(0))}
# {'dan': [87, 100], 'jason': [79], 'erik': [95, 97]}
```

key 함수로 itemgetter(0)를 이용하여 정렬 후, 첫 번째 인자로 groupby(정렬 필요)