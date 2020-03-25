## Matplotlib 기초



## Matplotlib

_________

​	파이썬에서 자료를 차트(chart)나 플롯(plot)으로 시각화(visualization)하는 패키지

​	정형화된 차트나 플롯 이외에도 저수준 api를 사용한 다양한 시각화 기능을 제공

- 라인 플롯(line plot)
- 스캐터 플롯(scatter plot)
- 컨투어 플롯(contour plot)
- 서피스 플롯(surface plot)
- 바 차트(bar chart)
- 히스토그램(histogram)
- 박스 플롯(box plot)

- 예제 URL - http://matplotlib.org/gallery.html

### pylab 서브패키지

- `Matplotlib` 패키지에는 `pylab` 이라는 서브패키지가 존재

  - `pylab`

    ​	`matlab`이라는 수치해석 소프트웨어의 시각화 명령을 거의 그대로 사용할 수 있도록 `Matplotlib`의 하의 API를 포장한 명령어 집합을 제공

    ​	간단한 시각화 프로그램을 만드는 경우에는 `pylab` 서브패키지의 명령만으로도 충분

- `Matplotlib` 패키지를 사용할 때는 보통 주 패키지는 `mpl`이라는 별칭(alias)으로 임포트하고 `pylab` 서브패키지는 `plt` 라는 별칭으로 임포트하여 사용하는 것이 관례

  ```python
  import matplotlib as mpl
  import matplotlib.pylab as plt
  ```

- 주피터 노트북의 경우 매직(magic) 명령으로 노트북 내부에 그림을 표시하도록 지정해야 함

  ```python
  %matplotlib inline
  ```

### 라인 플롯

선을 그리는 가장 간단한 플롯

주로 데이터가 시간, 순서 등에 따라 어떻게 변화하는지 보여주기 위해 사용

pylab 서브패키지의 `plot` 명령을 사용

- [http://Matplotlib.org/api/pyplot_api.html#Matplotlib.pyplot.plot](http://matplotlib.org/api/pyplot_api.html#Matplotlib.pyplot.plot)

```python
plt.title("Plot")
plt.plot([1, 4, 9, 16])
plt.show()
```

- show()

  - 시각화 명령을 실제 차트로 렌더링하고 마우스 움직임 등의 이벤트를 기다리라는 지시

  - jupyter notebook 에서는 셀 단위로 플롯 명령을 자동 렌더링 하므로 `show` 명령이 필요X

### 한글 폰트 사용

설치된 폰트를 사용하는 방법은 크게 두 가지

- rc parameter 설정으로 이후의 그림 전체에 적용
- 인수를 사용하여 개별 텍스트 관련 명령에만 적용

한글 문자열은 항상 유니코드를 사용해야 함

##### rc parameter

```python
# 폰트 설정
mpl.rc('font', family="NanumGothic")
# 유니코드에서 음수 부호설정
mpl.rc('axes', unicode_minus=False)

plt.title('한글 제목')
plt.plot([10, 20, 30, 40], [1, 4, 9, 16])
plt.xlabel("엑스축 라벨")
plt.ylabel("와이축 라벨")
plt.show()
```

##### 객체마다 별도의 폰트 적용

폰트 패밀리, 색상, 크기를 정하여 플롯 명령의 `fontdict` 인수에 넣음

```python
font1 = {'family': 'NanumMyeongjo', 'size': 24, 'color': 'black'}
font2 = {'family': 'NanumBarunpen', 'size': 18, 'weight': 'bold', 'color': 'darkred'}
font2 = {'family': 'NanumBarunGothic', 'size': 12, 'weight': 'light', 'color': 'blue'}

plt.plot([10, 20, 30, 40], [1, 4, 9, 16])
plt.title('한글 제목', fontdict=font1)
plt.xlabel("엑스축 라벨", fontdict=font2)
plt.ylabel("와이축 라벨", fontdict=font3)
plt.show()
```

### 스타일 지정

```python
plt.title("'rs--' 스타일의 plot")
plt.plt([10, 20, 30, 40], [1, 4, 9, 16], 'rs--')
plt.show()
```

스타일 문자열은 색깔(color), 마커(marker), 선 종류(line style)의 순서로 지정

##### 색깔

색깔을 지정하는 방법은 색 이름 혹은 약자를 사용하거나 `#` 문자로 시작되는 RGB코드를 사용

전체 색깔 목록 - [http://Matplotlib.org/examples/color/named_colors.html](http://matplotlib.org/examples/color/named_colors.html) 

| 문자열    | 약자 |
| :-------- | :--- |
| `blue`    | `b`  |
| `green`   | `g`  |
| `red`     | `r`  |
| `cyan`    | `c`  |
| `magenta` | `m`  |
| `yellow`  | `y`  |
| `black`   | `k`  |
| `white`   | `w`  |

##### 마커

데이터 위치를 나타내는 기호를 마커(marker)라고 함

| 마커 문자열 | 의미                  |
| :---------- | :-------------------- |
| `.`         | point marker          |
| `,`         | pixel marker          |
| `o`         | circle marker         |
| `v`         | triangle_down marker  |
| `^`         | triangle_up marker    |
| `<`         | triangle_left marker  |
| `>`         | triangle_right marker |
| `1`         | tri_down marker       |
| `2`         | tri_up marker         |
| `3`         | tri_left marker       |
| `4`         | tri_right marker      |
| `s`         | square marker         |
| `p`         | pentagon marker       |
| `*`         | star marker           |
| `h`         | hexagon1 marker       |
| `H`         | hexagon2 marker       |
| `+`         | plus marker           |
| `x`         | x marker              |
| `D`         | diamond marker        |
| `d`         | thin_diamond marker   |

##### 선 스타일

선 스타일에는 실선(solid), 대시선(dashed), 점선(dotted), 대시-점선(dash-dit) 존재

| 선 스타일 문자열 | 의미                |
| :--------------- | :------------------ |
| `-`              | solid line style    |
| `--`             | dashed line style   |
| `-.`             | dash-dot line style |
| `:`              | dotted line style   |

##### 기타 스타일

라인 플롯에서는 앞서 설명한 세 가지 스타일 이외에도 여러가지 스타일을 지정할 수 있지만 이 경우에는 인수 이름을 정확하게 지정해야 함

사용할 수 있는 스타일 인수의 목록은 `Matplotlib.lines.Line2D` 클래스에 대한 다음 웹사이트를 참조

- [http://Matplotlib.org/api/lines_api.html#Matplotlib.lines.Line2D](http://matplotlib.org/api/lines_api.html#Matplotlib.lines.Line2D)

라인 플롯에서 자주 사용되는 기타 스타일

| 스타일 문자열     | 약자  | 의미           |
| :---------------- | :---- | :------------- |
| `color`           | `c`   | 선 색깔        |
| `linewidth`       | `lw`  | 선 굵기        |
| `linestyle`       | `ls`  | 선 스타일      |
| `marker`          |       | 마커 종류      |
| `markersize`      | `ms`  | 마커 크기      |
| `markeredgecolor` | `mec` | 마커 선 색깔   |
| `markeredgewidth` | `mew` | 마커 선 굵기   |
| `markerfacecolor` | `mfc` | 마커 내부 색깔 |

##### 스타일 적용 예

```python
plt.plot([10, 20, 30, 40], [1, 4, 9, 16], c="b", 
         lw=5, ls="--", marker="o", ms=15, mec="g", mew=5, mfc="r")
plt.title("스타일 적용 예")
plt.show()
```

### 그림 범위 지정

그림의 범위를 수동으로 지정하려면 `xlim` 명령과 `ylim` 명령을 사용

```python
plt.title("x축, y축의 범위 설정")
plt.plot([10, 20, 30, 40], [1, 4, 9, 16], 
        c="b", lw=5, ls="--", marker="o", ms=15, mec="g", mew=5, mfc="r")
plt.xlim(0, 50)
plt.ylim(-10, 30)
plt.show()
```

### 틱 설정

플롯이나 차트에서 축상의 위치 표시 지점을 틱(tick)이라고 함

틱에 써진 숫자 혹은 글자를 틱 라벨(tick label)이라고 함

수동으로 설정하려면 `xticks`, `yticks` 명령 사용

```python
X = np.linespace(-np.pi, np.pi, 256)
C = np.cos(X)
plt.plot("x축과 y축의 tick label 설정")
plt.plot(X, C)
plt.xticks([-np.pi, -np.pi / 2, 0, np.pi / 2, np.pi])
plt.yticks([-1, 0, +1])
plt.show()
```

틱 라벨 문자열에는 `$$` 사이에 LaTeX 수학 문자식을 넣을 수도 있음

```python
X = np.linespace(-np.pi, np.pi, 256)
C = np.cos(X)
plt.plot("x축과 y축의 tick label 설정")
plt.plot(X, C)
plt.xticks([-np.pi, -np.pi / 2, 0, np.pi / 2, np.pi],
           [r'$-\pi$', r'$-\pi/2', r'$0$',r'$+\pi/2', r'$+\pi$'])
plt.yticks([-1, 0, +1], ["Low", "Zero", "High"])
plt.show()
```

### 그리드 설정

그리드 설정은 `grid(True)`, `grid(False)`

```python
X = np.linespace(-np.pi, np.pi, 256)
C = np.cos(X)
plt.title("Grid 제거")
plt.plot("x축과 y축의 tick label 설정")
plt.plot(X, C)
plt.xticks([-np.pi, -np.pi / 2, 0, np.pi / 2, np.pi],
           [r'$-\pi$', r'$-\pi/2', r'$0$',r'$+\pi/2', r'$+\pi$'])
plt.yticks([-1, 0, +1], ["Low", "Zero", "High"])
plt.grid(False)
plot.show()
```

### 여러개의 선 그리기

x 데이터, y 데이터, 스타일 문자열을 반복하여 인수로 넘김

하나의 선을 그릴 때처럼 x 데이터나 스타일 문자열을 생략할 수 없음

```python
t = np.arange(0., 5., 0.2)
plt.title("라인 플롯에서 여러개의 선 그리기")
plt.plot(t, t, 'r--', t, 0.5 * t**2, 'bs:', t, 0.2 * t**3, 'g^-')
plt.show()
```

### 겹쳐그리기

복수의 `plot`명령을 하나의 그림에 겹쳐서 그릴 수 있음

```python
plt.title("복수의 plot 명령을 한 그림에서 표현")
plt.plot([1, 4, 9, 16],
        c="b", lw=5, ls="--", marker="o", ms=15, mec="g", mew=5, mfc="r")
# plt.hold(True) # <- 1.5 버전에서는 이 코드가 필요
plt.plot([9, 16, 4, 1],
        c="k", lw=3, ls=":", marker="s", ms=10, mec="m", mew=5, mfc="c")
# plt.hold(True) # <- 1.5 버전에서는 이 코드가 필요
plt.show()
```

### 범례

여러개의 라인 플롯을 동시에 그리는 경우에는 각 선이 무슨 자료를 표시하는지를 보여주기 위해 `legend` 명령으로 범례(legend)를 추가할 수 있음

범례의 위치는 자동으로 정해지지만 수동으로 설정하고 싶으면 `loc` 인수를 사용

인수에는 문자열 혹은 숫자가 들어감

| loc 문자열     | 숫자 |
| :------------- | :--- |
| `best`         | 0    |
| `upper right`  | 1    |
| `upper left`   | 2    |
| `lower left`   | 3    |
| `lower right`  | 4    |
| `right`        | 5    |
| `center left`  | 6    |
| `center right` | 7    |
| `lower center` | 8    |
| `upper center` | 9    |
| `center`       | 10   |

```python
X = np.linspace(-np.pi, np.pi, 256)
C, S = np.cos(X), np.sin(X)
plt.title("legend를 표시한 슬롯")
plt.plot(X, C, ls="--", label="cosine")
plt.plot(X, S, ls=":", label="sine")
plt.legend(loc=2)
plt.show()
```

### x축, y축 라벨, 타이틀

`xlabel`, `ylabel` 명령 사용

```python
X = np.linspace(-np.pi, np.pi, 256)
C, S = np.cos(X), np.sin(X)
plt.plot(X, C, ls="--", label="cosine")
plt.xlabel("time")
plt.ylabel("amplitude")
plt.title("Cosine Plot")
plt.show()
```

### 그림의 구조

Figure 객체, Axes 객체, Axis 객체 등으로 구성

Figure 객체는 한개 이상의 Axes 객체를 포함

Axes 객체는 두개 이상의 Axis 객체를 포함

![](https://user-images.githubusercontent.com/41600558/77513721-dc764e00-6eb8-11ea-8c82-b8df6590938e.png)

##### Figure 객체

원래 Figure를 생성하려면 `figure` 명령을 사용하여 그 반환값으로 Figure 객체를 얻어야 함

그러나 일반적인 `plot` 명령 등을 실행하면 자동으로 Figure를 생성

`figure` 명령을 명시적으로 사용하는 경우는 여러개의 윈도우를 동시에 띄워야 하거나(line plot이 아닌 경우), Jupyter 노트북 등에서(line plot의 경우) 그림의 크기를 설정하고 싶을 때

그림의 크기는 figsize 인수로 설정

```python
np.random.seed(0)
f1 = plt.figure(figsize=(10, 2))
plt.title("figure size : (10, 2)")
plt.plot(np.random.randn(100))
plt.show()
```

현재 사용하고 있는 Figure 객체를 얻으려면 `gcf` 명령을 사용

```python
f1 = plt.figure(1)
plt.title("현재의 Figure 객체")
plt.plot([1, 2, 3, 4], 'ro:')

f2 = plt.gcf()
print(f1, id(f1))
print(f2, id(f2))
plt.show()
```

##### Axes 객체와 `subplot` 명령



때로는 하나의 윈도우(Figure)안에 여러개의 플롯을 배열 형태로 보여야하는 경우도 있다. Figure 안에 있는 각각의 플롯은 Axes 라고 불리는 객체에 속한다. Axes 객체에 대한 자세한 설명은 다음 웹사이트를 참조한다.

- [http://Matplotlib.org/api/axes_api.html#Matplotlib.axes.Axes](http://matplotlib.org/api/axes_api.html#Matplotlib.axes.Axes)

Figure 안에 Axes를 생성하려면 원래 `subplot` 명령을 사용하여 명시적으로 Axes 객체를 얻어야 한다. 그러나 plot 명령을 바로 사용해도 자동으로 Axes를 생성해 준다.

`subplot` 명령은 그리드(grid) 형태의 Axes 객체들을 생성하는데 Figure가 행렬(matrix)이고 Axes가 행렬의 원소라고 생각하면 된다. 예를 들어 위와 아래 두 개의 플롯이 있는 경우 행이 2 이고 열이 1인 2x1 행렬이다. `subplot` 명령은 세개의 인수를 가지는데 처음 두개의 원소가 전체 그리드 행렬의 모양을 지시하는 두 숫자이고 세번째 인수가 네 개 중 어느것인지를 의미하는 숫자이다. 따라서 위/아래 두개의 플롯을 하나의 Figure 안에 그리려면 다음처럼 명령을 실행해야 한다. 여기에서 숫자 인덱싱은 파이썬이 아닌 Matlab 관행을 따르기 때문에 첫번째 플롯을 가리키는 숫자가 0이 아니라 1임에 주의하라.

```python
subplot(2, 1, 1)
# 윗 부분에 그릴 플롯 명령들
subplot(2, 1, 2)
# 아랫 부분에 그릴 플롯 명령들
```

`tight_layout` 명령을 실행하면 플롯간의 간격을 자동으로 맞춤

```python
x1 = np.linspace(0.0, 5.0)
x2 = np.linspace(0.0, 2.0)
y1 = np.cos(2 * np.pi * x1) * np.exp(-x1)
y2 = np.cos(2 * np.pi * x2)

ax1 = plt.subplot(2, 1, 1)
plt.plot(x1, y1, 'yo-')
plt.title('A tale of 2 subplots')
plt.ylabel('Damped oscillation')
print(ax1)

ax2 = plt.subplot(2, 1, 2)
plt.plot(x2, y2, 'r.-')
plt.xlabel('time (s)')
plt.ylabel('Undamped')
print(ax2)

plt.tight_layout()
plt.show()
```

만약 2x2 형태의 네 개의 플롯이라면 다음과 같이 그린다. 이 때 `subplot` 의 인수는 (2,2,1)를 줄여서 221 라는 하나의 숫자로 표시할 수도 있다. 

Axes의 위치는 위에서 부터 아래로, 왼쪽에서 오른쪽으로 카운트

```python
np.random.seed(0)

plt.subplot(221)
plt.plot(np.random.rand(5))
plt.title("axes 1")

plt.subplot(222)
plt.plot(np.random.rand(5))
plt.title("axes 2")

plt.subplot(223)
plt.plot(np.random.rand(5))
plt.title("axes 3")

plt.subplot(224)
plt.plot(np.random.rand(5))
plt.title("axes 4")

plt.tight_layout()
plt.show()
```

`subplots` 명령으로 복수의 Axes 객체를 동시에 생성할 수도 있다. 이때는 2차원 ndarray 형태로 Axes 객체가 반환된다.

```python
fig, axes = plt.subplots(2, 2)

np.random.seed(0)
axes[0, 0].plot(np.random.rand(5))
axes[0, 0].set_title("axes 1")
axes[0, 1].plot(np.random.rand(5))
axes[0, 1].set_title("axes 2")
axes[1, 0].plot(np.random.rand(5))
axes[1, 0].set_title("axes 3")
axes[1, 1].plot(np.random.rand(5))
axes[1, 1].set_title("axes 4")

plt.tight_layout()
plt.show()
```

##### Axis 객체와 축

하나의 Axes 객체는 두 개 이상의 Axis 객체를 가진다. Axis 객체는 플롯의 가로축이나 세로축을 나타내는 객체

- https://matplotlib.org/api/axis_api.html

여러가지 플롯을 하나의 Axes 객체에 표시할 때 y값의 크기가 달라서 표시하기 힘든 경우

- `twinx` 명령으로 복수의 y 축을 가진 플롯을 만들수도 있다. 
- `twinx` 명령은 x 축을 공유하는 새로운 Axes 객체를 만든다.

```python
fig, ax0 = plt.subplots()
ax1 = ax0.twinx()
ax0.set_title("2개의 y축 한 figure에서 사용하기")
ax0.plot([10, 5, 2, 9, 7], 'r-', label="y0")
ax0.set_ylabel("y0")
ax0.grid(False)
ax1.plot([100, 200, 220, 180, 120], 'g:', label="y1")
ax1.set_ylabel("y1")
ax1.grid(False)
ax0.set_xlabel("공유되는 x축")
plt.show()
```

