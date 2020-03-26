# PYTHON JSON 라이브러리 기초



## JSON 라이브러리

____________

 JSON은 JavaScript Object Notation의 약자로서 JavaScript 문법에 영향을 받아 개발된 LightWeight한 데이타 표현 방식

 Python은 기본적으로 JSON 표준 라이브러리를 제공

 JSON 라이브러리를 사용하면, Python 타입의 Object를 JSON 문자열로 변경할 수 있으며(JSON 인코딩), JSON 문자열을 다시 Python타입으로 변환할 수 있음(JSON 디코딩)

- Encoding

  ```python
  import json
   
  # 테스트용 Python Dictionary
  customer = {
      'id': 152352,
      'name': '강진수',
      'history': [
          {'date': '2015-03-11', 'item': 'iPhone'},
          {'date': '2016-02-23', 'item': 'Monitor'},
      ]
  }
   
  # JSON 인코딩
  jsonString = json.dumps(customer)
   
  # 문자열 출력
  print(jsonString)
  print(type(jsonString))   # class str
  ```

- Decoding

  ```python
  import json
   
  # 테스트용 JSON 문자열
  jsonString = '{"name": "강진수", "id": 152352, "history": [{"date": "2015-03-11", "item": "iPhone"}, {"date": "2016-02-23", "item": "Monitor"}]}'
   
  # JSON 디코딩
  dict = json.loads(jsonString)
   
  # Dictionary 데이타 체크
  print(dict['name'])
  for h in dict['history']:
      print(h['date'], h['item'])
  ```