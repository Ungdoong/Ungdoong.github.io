# MySQL Workbench Import & Export

### 학습목표

​	공공데이터를 DB에 import 하고 가공 후 csv파일로 export

### 준비물

 1. 도로명 주소데이터 다운로드

    http://www.juso.go.kr/addrlink/addressBuildDevNew.do?menu=match

	2. 보건복지부 선별진료소 데이터 다운로드

    https://www.data.go.kr/dataset/15043008/fileData.do

	3. MySQL workbench 6.10 / MySQL server 5.7

	4. Notepad++

### 학습내용

##### 도로명 주소데이터

		1. 구분자를 확인 (`|`) 후 `:`로 치환
  		2. 데이터 내 칼럼명이 존재하지 않으므로 최상단에 각 칼럼명 기입(각 칼럼명도 구분자로 구분)
  		3. Notepad++ 에서 파일 인코딩 방식을 UTF-8로 변환
  		4. 파일형식을 `.csv`로 변경
  		5. workbench에 addr 테이블을 생성하고 데이터에 맞추어 칼럼 추가
  		6. 기존 칼럼에 더하여 `등록자ID` 와 `등록일자` 칼럼 추가
  		7. 칼럼을 지정하여 `csv`파일 `import`
  		8. 엔트리에서 `시도명` 칼럼기준으로 `value`가 `서울특별시`가 아닌 `row` 삭제
  		9. 가공 후 테이블 `export wizard`를 이용하여 `export`

### 

##### 보건소복지부 선별진료소 데이터

  		1. 구분자를 확인 (`,`) 후 `:`로 치환
  		2. Notepad++ 에서 파일 인코딩 방식을 UTF-8로 변환
  		3. workbench에 medical 테이블을 생성하고 데이터에 맞추어 칼럼 추가
  		4. 기존 칼럼에 더하여 `등록자ID` 와 `등록일자` 칼럼 추가
  		5. 칼럼을 지정하여 `csv`파일 `import`
  		6. 엔트리에서 `시도명` 칼럼기준으로 `value`가 `서울특별시`가 아닌 `row` 삭제
  		7. 가공 후 테이블 `export wizard`를 이용하여 `export`

