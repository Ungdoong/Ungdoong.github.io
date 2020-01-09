# 191016(Wed)_MyBatis

1. SqlSessionFactoryBuilder 객체
   - 환경파일을 읽어와서 SqlSessionFactory 객체 생성

2. SqlSessionFactory 객체 
   - SqlSession 객체를 생성해 주는 factory 객체
   - openSession() 메소드로 SqlSession 객체를 얻을 수 있음

3. SqlSession 객체
   - 여러가지 쿼리를 실행할 수 있는 메소드를 가지고 있음
   - Mapper file에 등록된 sql을 실행시켜 주기 위한 메소드를 제공