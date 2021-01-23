> 플라스크를 어렵게 느끼는 이유 : 단일 파일이 아닌 html, python, DB 등을 적절히 사용해야 하기 때문에
- 요청에 대한 처리에 대해 확실히 이해하기
- app.py를 작성하다보면, url의 증가에 따라 코드가 길어지고 이를 효과적으로 관리하게 위해 분리하고, blueprint를 사용한다.
- jsonify : 플라스크에서 json 형식으로 요청에 대한 응답을 줄 때 사용

## 서버와 클라이언트

> 주체는 클라이언트 위주
- GET, DB(CRUD) 보통은 요청한 정보를 처리하기 위해 서버가 DB에 접속하여 정보를 읽고 반환한다. 
- 단순히 어떤 페이지를 요청 할 때는 DB를 통하지 않아도 된다. 

## API란? : 기능! 기능에 대한 함수 하나라고 이해해도 괜찮다.
- 백엔드를 공부할 땐 API에 대한 공부부터 시작하는 편이다.
1. 쇼핑몰
    - 회원가입
        - 회원가입 form(id, password 등 정보 입력 페이지) - API(기능) : ~경로로 들어왔을 때 ~html을 보여줄 것인가?
    - 물건 리스트 페이지
        - 물건 정렬 API
        - 물건 추천 API
    - 물건 상세 페이지
        - 물건 등록 API
        - 물건 삭제 API
    - 정보 업데이트 API
    - 사용자 삭제 API

## REST API
- 기능적으로는 같으나 DB의 CRUD와는 다르다.
- 가독성 있는 코드 작성 규칙에 대한 것임
- DB + 클라이언트 + 서버 3명이 있다고 생각하기
- http methods - GET, POST, PUT, UPDATE, DELETE ..
    - 경로를 /registeruser 와 같은 식으로 쓰지 말자
        ```
        @app.route('/registeruser') # 동사를 쓰지 말자
        def registeruser() :
            user = {}
            db.user.insert_one(user)
        ```
    - 아래와 같이 바꿔라
        ```
        @app.route('/user', methods=['DELETE']) # 동사를 쓰지 말자
        def user() :
            user = {}
            db.user.insert_one(user)
        ```
- REST하지 않아도 동작하는데 문제는 없으나, 코드를 이해하기 쉽게 만들자.
- 무엇을 할 것인지 정하는 동사는 메서드에 입력하자.
- uri는 명사 
- 채용시 RESTful API 개발자란 가독성을 높힌 REST 규칙에 맞춘 서버, 클라이언트 단을 개발해본 경험이 있는 사람을 찾는 것이다.
- 개발은 '팀워크'이다. 내 코드를 이해하기 쉽게 만들기!!! 
