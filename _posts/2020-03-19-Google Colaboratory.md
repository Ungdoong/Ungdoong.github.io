# Google Colaboratory

## Google Colab

__________

​	`Colab`은 `Jupyter Notebook`에 추가로 Python 소스코드를 Google의 클라우드 컴퓨팅 환경에서 GPU와 TPU를 무료로 사용할 수 있고 소스코드나 데이터를 `Google Drive`를 통해 불러오거나 저장할 수도 있는 개발 환경

​	딥러닝, M/L, 데이터 사이언스 분야에서 사용

​	https://colab.reserch.google.com/



### Google Drive의 ipynb 파일과  Colab 연결

1. ipynb 파일을 드라이브 상에 업로드
2. 해당 파일을 더블클릭 한 후 상단의 `연결 앱` 버튼을 누르고 `더 많은 앱 연결하기` 선택
3. 검색창에서 `colab`을 검색하고 `연결` 버튼을 클릭



### 코드 스니펫

​	재사용이 가능한 코드와 다양한 예제를 모아놓고 사용



### Google Drive 연동

```python
from google.colab import drive
drive.mount('/gdrive')

with open('/gdrive/My Drive/foo.txt', 'w') as f:
  f.write('Hello Google Drive!ㅡㅡ')
!cat '/gdrive/My Drive/foo.txt'
```



### GPU 와 CPU 처리시간 비교

​	상단메뉴의 `런타임` 내의 `런타임 유형 변경`에서 가속기 선택가능

```python
%tensorflow_version 2.x
import tensorflow as tf
device_name = tf.test.gpu_device_name()
if device_name != '/device:GPU:0':
  raise SystemError('GPU device not found')
print('Found GPU at: {}'.format(device_name))

%tensorflow_version 2.x
import tensorflow as tf
import timeit

device_name = tf.test.gpu_device_name()
if device_name != '/device:GPU:0':
  print(
      '\n\nThis error most likely means that this notebook is not '
      'configured to use a GPU.  Change this in Notebook Settings via the '
      'command palette (cmd/ctrl-shift-P) or the Edit menu.\n\n')
  raise SystemError('GPU device not found')

def cpu():
  with tf.device('/cpu:0'):
    random_image_cpu = tf.random.normal((100, 100, 100, 3))
    net_cpu = tf.keras.layers.Conv2D(32, 7)(random_image_cpu)
    return tf.math.reduce_sum(net_cpu)

def gpu():
  with tf.device('/device:GPU:0'):
    random_image_gpu = tf.random.normal((100, 100, 100, 3))
    net_gpu = tf.keras.layers.Conv2D(32, 7)(random_image_gpu)
    return tf.math.reduce_sum(net_gpu)
  
# We run each op once to warm up; see: https://stackoverflow.com/a/45067900
cpu()
gpu()

# Run the op several times.
print('Time (s) to convolve 32x7x7x3 filter over random 100x100x100x3 images '
      '(batch x height x width x channel). Sum of ten runs.')
print('CPU (s):')
cpu_time = timeit.timeit('cpu()', number=10, setup="from __main__ import cpu")
print(cpu_time)
print('GPU (s):')
gpu_time = timeit.timeit('gpu()', number=10, setup="from __main__ import gpu")
print(gpu_time)
print('GPU speedup over CPU: {}x'.format(int(cpu_time/gpu_time)))
```



### OpenCV를 사용한 그래픽 처리

​	OpenCV로 가공된 그래픽을 matplotlib를 사용해 이미지 버퍼 출력부분을 바꾸어 사용

```python
import cv2
import numpy as np
from matplotlib import pyplot as plt
import urllib
from skimage import io
url = 'https://user-images.githubusercontent.com/41600558/77033803-8f433980-69eb-11ea-9c23-b27a4f789eb2.jpg'
img = io.imread(url)
img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
plt.imshow(img)
plt.show()
```



### Face Detection

​	얼굴감지와 인식. face-recognition 1.3.0 패키지 사용

- 얼굴 인식 코드

```python
from google.colab import drive
drive.mount('/gdrive')

!pip install face_recognition

import cv2, os
import face_recognition as fr
from IPython.display import Image, display
from matplotlib import pyplot as plt
from skimage import io

image_path = '/gdrive/My Drive/colab/sample_img.jpg'
image = fr.load_image_file(image_path)
face_locations = fr.face_locations(image)

for(top, right, bottom, left) in face_locations:
  cv2.rectangle(image, (left, top), (right, bottom), (0,255,0), 3)

plt.rcParams["figure.figsize"] = (16, 16)
plt.imshow(image)
plt.show()
```



### Face Recognition

​	동일인인지 확인

- 얼굴영역만을 추출하여 리스트

  ```python
  plt.rcParams["figure.figsize"] = (1,1)
  
  known_person_list = []
  known_person_list.append(fr.load_image_file("/gdrive/My Drive/colab/sample1.jpg"))
  known_person_list.append(fr.load_image_file("/gdrive/My Drive/colab/sample2.jpg"))
  known_person_list.append(fr.load_image_file("/gdrive/My Drive/colab/sample3.jpg"))
  known_person_list.append(fr.load_image_file("/gdrive/My Drive/colab/sample4.jpg"))
  
  known_face_list = []
  for person in known_person_list:
  
    top, right, bottom, left = fr.face_locations(person)[0]
    face_image = person[top:bottom, left:right]
  
    known_face_list.append(face_image)
    
    for face in known_face_list:
    plt.imshow(face)
    plt.show()
  ```

- 동일인인지 확인하고자 하는 이미지 처리

  ```python
  unknown_person = fr.load_image_file("/gdrive/My Drive/colab/unkown.jpg")
  
  top, right, bottom, left = fr.face_locations(unknown_person)[0]
  unknown_face = unkown_person[top:bottom, left:right]
  
  plt.title("unkown_face")
  plt.imshow(unknown_face)
  plt.show()
  ```

- 비교 연산

  ```python
  enc_unknown_face = fr.face_encodings(unknown_face)
  
  plt.imshow(enc_unknown_face)
  plt.show()
  
  for face in known_face_list:
  
    enc_known_face = fr.face_encodings(face)
  
    distance = fr.face_distance(enc_known_face, enc_unknown_face[0])
  
    plt.title("distance: " + str(distance))
    plt.imshow(face)
    plt.show()
  ```

  