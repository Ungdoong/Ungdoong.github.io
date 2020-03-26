# [Python]all,any의 short-circuit



SSAFY 빅데이터 특화프로젝트 진행중에 학습한 내용을 정리

## all, any, short-circuit 란?

__________

### all(iterable)

`all()`은 인자로 전달된 모든 값이 True인지 아닌지 판별

```python
def all(iterable):
	for element in iterable:
        if not element:
            return False
	return True
```

### any(iterable)

`any()`는 인자로 전달된 모든 값 중 하나라도 True 가 있는지 확인

```python
def any(iterable):
    for element in iterable:
        if element:
            return True
    return False
```

### short-circuit evaluation

and/or 연산에 있어 첫 번째 인수로 값을 평가하기에 충분하다면 이후 인수는 평가지 않음

```python
# example
False and True
True or False
```



## all, any의 실제 동작

_____________

Container Object가 생성될 때는 내부요소가 생성시에 전부 값으로 평가됨

```python
>>> import dis
>>> dis.dis('[1, 2, 3]')
  1           0 LOAD_CONST               0 (1)
              2 LOAD_CONST               1 (2)
              4 LOAD_CONST               2 (3)
              6 BUILD_LIST               3
              8 RETURN_VALUE
```

만약, `all()`을 이용해 변수로 처리하고 이를 if문에서 평가한다면?

```python
user = User.objects.get(pk=1)

is_authenticated = all([
    user.is_authenticated(),
    not user.is_banned,
    not user.session.expired,
])

if is_authenticated:
    print('Hello World!')
```

Django를 예로, Django 모델의 인스턴스에서 어떤 동작을 수행하면 대개 Database Query가 발생함

만약, `user.session.expired`라는 동작이 오랜 시간을 필요로 한다면 앞에 나오는 인수에서 short-circuit evaluation이 일어나기를 바래야함

하지만 `list` 생성 시의 동작을 보면 `LOAD_CONST`를 통해 값으로 평가되어 버림

(`all()`을 이용한 예제에서는 함수를 호출하므로 `CALL_FUNCTION`)

이렇게 사용하는 것은 좋지 못한 패턴



## 더 나은 방법

____________________

위의 코드를 short-circuit evaluate 되도록 하려면 if 조건식 안으로 옮기면 됨

```python
user = User.objects.get(pk=1)

if (user.is_authenticated()
    and not user.is_banned
    and not user.session.expired):
    print('Hello World!')
```

### 증명내용

```python
>>> counter = 0
>>> def true():
...     global counter
...     print(True)
...     counter += 1
...     return True
... 
>>> def false():
...     global counter
...     print(False)
...     counter += 1
...     return False
... 
>>> if (true() and false() and true()):
...     pass
... else:
...     print(counter)
... 
True
False
2  
```

`true()`, `false()` 함수를 만들고, 각각이 `True`, `False`를 반환하게 함

그리고 전역 변수 `counter`의 값이 호출될 때 마다 1씩 증가시킴

if조건문에서 2번 `true()`, 1번 `false()`함수가 호출되어, counter 값이 3이어야 하지만 `false()`에서 short-circuit evaluation 되어 `counter` 의 값이 2임을 확인할 수 있음

길어지는게 싫다면,

```python
>>> def conditions():
...     yield true()
...     yield false()
...     yield true()
... 
>>> is_true = all(conditions())
True
False
>>> if is_true:
...     pass
... else:
...     print(counter)
...
2
```

조건을 제너레이터 함수로 만든 후, `all()`에서 short-circuit 시키면 됨