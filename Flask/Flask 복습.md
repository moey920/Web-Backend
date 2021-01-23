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
- 무엇을 할 것인지 정하는 ***동사는 메서드에 입력하자**, uri는 명사 
- 채용시 RESTful API 개발자란 가독성을 높힌 REST 규칙에 맞춘 서버, 클라이언트 단을 개발해본 경험이 있는 사람을 찾는 것이다.
- 개발은 '팀워크'이다. 내 코드를 이해하기 쉽게 만들기!!! 
- 개발자들의 최대 고민은 변수명, url명 등이다. 최대의 가독성을 지닌 이름을 선택해야한다.
    - 변수명을 추천해주는 프로그램을 개발해보면 어떨까???

## HTML의 templets

- 여러 파일에서 동일한 코드의 반복을 피하기 위해서 사용
    ```
    {% extends "layout.html" %} {% block content %}
    <h1>This is my index page</h1>
    {% endblock %}
    ```
- url_for('') 에서 가장 중요한 점은 url을 쓰는 것이 아니라 함수명을 써야한다!
- 템플릿 문법인 {% %}은 자바스크립트 서버인 nodejs 등 여러 곳에서 동일한 방법으로 쓰인다(백엔드)
- html의 form 태그에서 지원하는 메서드는 GET, POST 밖에 없다. DELETE는 AJAX를 활용하여 개발한다.
- AJAX : 클라이언트가 서버에게 정보는 보내는 방식 중의 하나. 복사, 붙여넣기를 활용해서 사용한다. 데이터를 보내는 용도, 함수를 호출하지는 않는다.
- 클라이언트는 서버에 함수를 요청하지 않는다. 

# SSR(Server Side Rendering) vs CSR(Client Side Rendering)

> SSR : Front End Client 측에서 요청을 받아 모든 작업을 Server side에서 진행하고 html을 Rendering한다.

> CSR : Front End 에서 정보를 요청했을 때, Server는 정보를 찾기만 해서 돌려준다. 나머지 작업은 Front End에서 진행한다.(작업이 분배된다.)

* Rendering : 투박한 글(코드)를 볼 수 있게 다듬는 작업

정보를 관리해주는 Back End의 API, DB등의 서버와, 사용자가 원하는 기능은 Front End의 Client에서 구현

## 웹 어플리케이션의 구조

1. DB(Mongo)
2. API 서버(Flask)
3. Front End App(React.js)
4. 브라우저(Chrome)

## API 의 구조

1. Resource
2. HTTP Method
3. Message

```
from flask import Flask, jsonify
from flask_restful import reqparse, abort, Api, Resource

app = Flask(__name__)
api = Api(app)


class HelloElice(Resource): # Resource를 상속받아 어떤 http 메서드가 왔는지에 따른 함수를 정의한다.
    def get(self):
        # msg 변수에 dictionary type으로 메세지를 입력해보세요!!
        msg = None
        return jsonify(status = "success", result = msg)

# @app.route가 아닌 api.add_resource(클래스, url)로 사용하며 하나의 api를 다양한 url에 적용시킬 수 있다.

api.add_resource(HelloElice, '/')

if __name__ == '__main__':
    app.run()
```

## API를 GUI로 이용하기 - POSTMAN

> 다운로드 : <https://www.postman.com/downloads/>

1. Collection 생성 후 원하는 URL을 작성하고 KEY, VALUE를 입력하고 Send보내기

## flask restful api 패키지를 활용한 api 구현

```
from flask import Flask, jsonify
from flask_restful import reqparse, abort, Api, Resource

app = Flask(__name__)
api = Api(app)

# 임시 데이터베이스!
DB = {
    "1": {"msg": "Hello"},
    "2": {"msg": "Elice!"}
    #키 : 밸류
}

# parser 변수를 통해 클라이언트로부터 전달 받는 인자들을 지정할 수 있습니다.
parser = reqparse.RequestParser()
parser.add_argument("id") # 여기에 작성한 인자만을 받아올 수 있다.
parser.add_argument("msg")


class Board(Resource):
    def get(self):
        # DB에 있는 정보를 전부 다 json 형태로 반환
        return jsonify(status = "success", result = DB)
        
    
    def post(self):
        # 클라이언트로부터 전달 받은 정보, id와 msg를 가지고 있다
        args = parser.parse_args()
        # DB에 정보 추가
        # 값으로 입력한 id = 어떤 메세지?
        DB[args["id"]] = {"msg": args["msg"]}
        
        return jsonify(status = "success", result = DB[args["id"]])
        
    def put(self):
        # 클라이언트로부터 전달 받은 정보, id와 msg를 가지고 있다
        args = parser.parse_args()
        # DB에서 정보 수정
        DB[args["id"]] = {"msg": args["msg"]}
        
        return jsonify(status = "success", result = DB[args["id"]])
    
    
    def delete(self):
        # 클라이언트로부터 전달 받은 정보, id와 msg를 가지고 있다
        args = parser.parse_args()
        # DB에서 정보 삭제
        
        
        return jsonify(status = "success", result = args["id"])


# API Resource 라우팅을 등록!
api.add_resource(Board, '/board')


if __name__ == '__main__':
    app.run()
```
