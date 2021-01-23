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
import pymysql
from flask import Flask, jsonify
from flask_restful import reqparse, abort, Api, Resource

app = Flask(__name__)
api = Api(app)

# mysql 연결하기
db = pymysql.connect(
        user = 'root',
        passwd = 'devpass',
        host = '127.0.0.1',
        port = 3306,
        db = 'elice_flask_board',
        charset = 'utf8'
    )
cursor = db.cursor() .


parser = reqparse.RequestParser() # parser 변수를 통해 클라이언트로부터 전달 받는 인자들을 지정할 수 있습니다.
parser.add_argument("id") # key에 id, name을 입력했을 때 딕셔너리로 만들어준다
parser.add_argument("name")


class Board(Resource): # api를 잘 사용할 수 있게 도와주는 Resource 상속
    def get(self): 
        sql = "SELECT id, name FROM `board`"
        cursor.execute(sql)
        result = cursor.fetchall()
        return jsonify(status = "success", result = result)
        
    
    def post(self):
        args = parser.parse_args() # 입력했던 키와 값을 불러와서 args에 저장한다.
        sql = "INSERT INTO `board` (`name`) VALUES (%s)" # 어떤 문자열에 넣을 것인지 비워둠(%s)
        cursor.execute(sql, (args['name']))  #args['name'] 이 (%s)에 들어간다.
        db.commit() # db의 수정내용을 commit해야 db내용이 변경된다.
        
        return jsonify(status = "success", result = {"name": args["name"]})
        
    def put(self):
        args = parser.parse_args()
        sql = "UPDATE `board` SET name = %s WHERE `id` = %s"
        cursor.execute(sql, (args['name'], args["id"]))
        db.commit()
        
        return jsonify(status = "success", result = {"id": args["id"], "name": args["name"]})
    
    
    def delete(self):
        args = parser.parse_args()
        sql = "DELETE FROM `board` WHERE `id` = %s"
        cursor.execute(sql, (args["id"], ))
        db.commit()
        
        return jsonify(status = "success", result = {"id": args["id"]})

# API Resource 라우팅을 등록!
api.add_resource(Board, '/board')


if __name__ == '__main__':
    app.run()
```

# 웹 백엔드 구현 과제 2/3번 답안

```
import pymysql
from flask import Flask, jsonify
from flask_restful import reqparse, abort, Api, Resource

app = Flask(__name__)
api = Api(app)

db = pymysql.connect(
        user = 'root',
        passwd = 'devpass',
        host = '127.0.0.1',
        port = 3306,
        db = 'elice_flask_board',
        charset = 'utf8'
    )
cursor = db.cursor()


parser = reqparse.RequestParser()
parser.add_argument('id')
parser.add_argument('name')


class Board(Resource):
    def get(self):
        sql = "SELECT id, name FROM `board`"
        cursor.execute(sql)
        result = cursor.fetchall()
        return jsonify(status = "success", result = result)
        
    
    def post(self):
        args = parser.parse_args()
        sql = "INSERT INTO `board` (`name`) VALUES (%s)"
        cursor.execute(sql, (args['name']))
        db.commit()
        
        return jsonify(status = "success", result = {"name": args["name"]})
        
    def put(self):
        args = parser.parse_args()
        sql = "UPDATE `board` SET name = %s WHERE `id` = %s"
        cursor.execute(sql, (args['name'], args["id"]))
        db.commit()
        
        return jsonify(status = "success", result = {"id": args["id"], "name": args["name"]})
    
    
    def delete(self):
        args = parser.parse_args()
        sql = "DELETE FROM `board` WHERE `id` = %s"
        cursor.execute(sql, (args["id"], ))
        db.commit()
        
        return jsonify(status = "success", result = {"id": args["id"]})


parser.add_argument('id')
parser.add_argument('title')
parser.add_argument('content')
parser.add_argument('board_id')


class BoardArticle(Resource):
    def get(self, board_id=None, board_article_id=None):
        if board_article_id:
            sql = "SELECT id, title, content FROM `boardArticle` WHERE `id`=%s"
            cursor.execute(sql, (board_article_id,))
            result = cursor.fetchone()
        else:
            sql = "SELECT id, title, content FROM `boardArticle` WHERE `board_id`=%s"
            cursor.execute(sql, (board_id,))
            result = cursor.fetchall()
            
        return jsonify(status = "success", result = result)

    def post(self, board_id=None):
        args = parser.parse_args()
        sql = "INSERT INTO `boardArticle` (`title`, `content`, `board_id`) VALUES (%s, %s, %s)"
        cursor.execute(sql, (args['title'], args['content'], args['board_id']))
        db.commit()
        
        return jsonify(status = "success", result = {"title": args["title"]})
        
        
    def put(self, board_id=None, board_article_id=None):
        args = parser.parse_args()
        sql = "UPDATE `boardArticle` SET title = %s, content = %s WHERE `id` = %s"
        cursor.execute(sql, (args['title'], args["content"], args["id"]))
        db.commit()
        
        return jsonify(status = "success", result = {"title": args["title"], "content": args["content"]})
        
        
    def delete(self, board_id=None, board_article_id=None):
        args = parser.parse_args()
        sql = "DELETE FROM `boardArticle` WHERE `id` = %s"
        cursor.execute(sql, (args["id"], ))
        db.commit()
        
        return jsonify(status = "success", result = {"id": args["id"]})
        


# API Resource 라우팅을 등록!
api.add_resource(Board, '/board')
api.add_resource(BoardArticle, '/board/<board_id>', '/board/<board_id>/<board_article_id>')


if __name__ == '__main__':
    app.run(debug)
```
