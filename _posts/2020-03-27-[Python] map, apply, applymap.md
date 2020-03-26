# [Python] map, apply, applymap



Pandas에서 제공하지 않는, 내가 만든 함수를 DataFrame에 적용하려면 `map`, `apply`, `applymap`함수를 사용

## 

## map함수

_______

- map함수는 DataFrame X, Series O
- Series는 값(value)의 1차원 배열
- `map`함수는 Series의 값 하나하나에 접근하여 해당 함수를 수행

```python
df['winning_rate']  = df['team'].map(lambda x : total_record(x)[3])
```



## apply함수

___________

- 커스텀 함수를 사용하기 위해 DataFrame에서 복수 개의 컬럼이 필요하다면, `apply`함수 사용

```python
df['winning_rate'] = df.apply(lambda x:relative_record(x['team'], x['against'])[3], axis=1)
```

커스텀 함수로 넘길 인자가 2개이므로 DataFrame의 `apply`함수를 사용

함수를 적용할 대상이 각각의 로우에 해당하므로 `axis`는 1이나 columns으로 지정



## applymap함수

__________

- DataFrame클래스의 함수이긴 하지만, 위의 `apply`함수처럼 각 row(axis=1)나 각 column(axis=0)별로 작동하지 않고 각 요소(element)별로 작동
- `applymap`에 인자로 전달하는 커스텀함수는 Single value로부터 Single value를 반환