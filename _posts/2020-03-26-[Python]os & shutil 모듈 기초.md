# [Python]os & shutil 모듈 기초



## os 모듈

____________

`os` 모듈은 운영 체제와 상호 작용하기 위한 수십 가지 함수들을 제공

`dir()`, `help()` 등

## shutil(shell utility) 모듈

_______________

일상적인 파일과 디렉터리 관리 작업을 위해, `shutil`모듈은 사용하기 쉬운 `os` 모듈보다 더 고수준의 인터페이스를 제공

- 파일 복사

  ```python
  shutil.copy('sample.pdf', 'Temp')
  ```

- 디렉터리 복사

  ```python
  shutil.copytree('Original', 'Original-Copy')
  ```

- 파일 이동

  ```python
  shutil.move('Sample.pdf', 'Temp')
  ```

- 디렉터리 이동

  ```python
  shutil.move('Original', 'Original-Copy')
  ```

- 파일과 디렉터리 이름 변경

  `move()`함수 이용

- 파일과 디렉터리 영구 삭제

  ```python
  # 파일
  shutil.remove('Sample.pdf')
  
  # 디렉터리
  shutil.rmtree('Original-Copy')
  ```

### 파일 지정