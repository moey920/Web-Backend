# REST API

> flask-restful을 활용해 restapi를 개발하는 방법을 학습합니다.

## REST API란?

앞에서 배운 **REST를 기반으로 서비스 API를 구현한 것을 REST API**라고 부릅니다. 최근 OpenAPI(누구나 사용할 수 있도록 공개된 API), 마이크로 서비스 등을 제공하는 기업에서는 대부분 REST API를 제공합니다.

## REST API의 특징

- REST 기반으로 시스템을 분산하여 확장성과 재사용성을 높여 유지보수를 편리하게 할 수 있습니다.
- REST는 HTTP 표준을 기반으로 구현하므로, HTTP를 지원하는 프로그램 언어로 클라이언트, 서버를 구현할 수 있습니다.

따라서, REST API를 구현하게 되면 클라이언트 뿐만 아니라, JAVA, C#, WEB 등을 이용해 클라이언트를 제작할 수 있습니다.

## REST API의 규칙

REST에서 가장 중요한 기본적인 규칙은 두 가지입니다. URI는 자원을 표현하는 데에 집중하고 행위에 대한 정의는 HTTP Method를 통해 하는 것이 REST한 API를 설계하는 핵심 규칙입니다.

1. URI는 정보의 자원을 표현해야 합니다.

    - 리소스명은 동사보다는 명사를 사용해야 합니다.
    - URI는 자원을 표현하는 데에 중점을 두어야 합니다.
    - GET같은 행위에 대한 표현이 들어가서는 안됩니다.
```
#안 좋은 예시
GET /getTodos/3
GET /todos/show/3

#좋은 예시
GET /todos/3
```

2. 자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE 등)으로 표현합니다.

주로 5가지의 Method(GET, POST, PUT, PATCH, DELETE)를 사용하여 CRUD를 구현합니다.

| Method | Action | 역할 |
|:---:|:---:|:---:|
| GET | index/retrieve | 모든/특정 자원을 조회 |
| POST | create | 자원을 생성 |
| PUT | replace | 자원의 전체를 교체 |
| PATCH | modify | 자원의 일부를 수정 |
| DELETE | delete | 모든/특정 자원을 삭제 |

```
#안 좋은 예시
GET /todos/delete/3

#좋은 예시
DELETE /todos/3
```

## RESTful이란?

RESTful은 **일반적으로 REST라는 아키텍처를 구현하는 웹 서비스를 나타내기 위해 사용되는 용어**입니다. 따라서, REST API를 사용하는 웹 서비스를 우리는 RESTful하다고 할 수 있습니다. RESTful은 REST를 REST답게 쓰기 위한 방법으로 누군가가 공식적으로 발표한 것이 아닙니다. **REST의 원리를 따르고 사용하는 시스템을 RESTful이라는 용어**로 칭하게 됩니다.

## RESTful의 목적은 무엇인가요?

쉽게 설명하자면 **이해하기 쉽고 쉬운 REST API를 만드는 것**입니다.
RESTful한 API를 구현하는 근본적인 목적이 성능 향상이 중점이 아니라 API의 이해도와 호환성을 높이는 것이 주된 목적입니다. 따라서, 성능이 중요한 상황에서는 굳이 RESTful한 API를 구현할 필요가 없습니다.

- 모든 요청을 POST로 받는게 아닌 GET, POST, PUT, DELETE 메소드를 활용해야 합니다.
- 응답에 대한 메타데이터는 Body에 포함 시키지 않고 최대한 HTTP 헤더에 포함되어야 합니다.
- 각 API에서 resource는 URL로 표시가 되며, 유일한 식별자로써 표현되야 하기 때문에 Endpoint가 하나여야 합니다.

## REST API 설계 기본 규칙

1. URI는 정보의 자원을 표현해야 합니다.

    - 자원은 동사보다는 명사를 사용해야 합니다.
    - 자원은 대문자보다는 소문자를 사용해야 합니다.
    - 자원의 도큐먼트 이름으로는 단수 명사를 사용해야 합니다.
    - 자원의 컬렉션 이름으로는 복수 명사를 사용해야 합니다.
    - 자원의 스토어 이름으로는 복수 명사를 사용해야 합니다.

```
#안 좋은 예시
GET /Student/3

#좋은 예시, 자원은 대문자보다는 소문자로 사용. 컬렉션 이름으로는 복수 명사
GET /students/3
```

2. 자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE 등)로 표현합니다.
- URI에 HTTP Method가 들어가면 안됩니다.
- 행위와 URI 분리되어 작성하는 것을 알 수 있다.

```
#안 좋은 예시
GET /students/delete/3

#좋은 예시
DELETE /students/3
```

- CRUD 기능을 나타내는 것은 URI에 사용하지 않습니다.

```
#안 좋은 예시
GET /students/show/3
GET /students/insert/4

#좋은 예시
GET /students/3
POST /students/4
```

- ```:id```는 하나의 특정 자원을 나타내는 고유값입니다.
    - ex) student를 생성하는 route: POST/students
    - ex) id=12인 student를 삭제하는 route: DELETE/students/12

> 도큐먼트: 객체 인스턴스나 데이터베이스 레코드와 유사한 개념
> 컬렉션: 서버에서 관리하는 디렉터리라는 자원
> 스토어: 클라이언트에서 관리하는 자원 저장소

## REST API 설계 규칙

기본적인 설계 규칙을 익혔다면 본격적인 설계 규칙을 알아보겠습니다.

1. 슬래시(/)는 계층 관계를 나타내는 데 사용합니다.
    - ex) https://academy.elice.io/classroom/teach

2. URI 마지막 문자로 슬래시(/)를 포함하지 않습니다.
    - ex) https://academy.elice.io/classroom/teach/ ← ( X )

- URI에 포함되는 모든 글자는 자원의 유일한 식별자로 사용되어야 합니다. URI가 다르다는 것은 불러오는 자원이 다르다는 뜻이고, 반대로 자원이 다르면 URI도 달라져야 합니다.
- REST API는 분명한 URI를 만들어 통신을 해야 하기 때문에 혼동을 주지 않도록 URI 경로의 마지막에는 슬래시(/)를 사용하지 않습니다.

3. 하이픈(-)은 URI 가독성을 높이는데 사용합니다.
불가피하게 긴 URI경로를 사용하게 된다면 하이픈(-)을 사용하여 가독성을 높입니다.

4. 밑줄(_)은 URI에 사용하지 않습니다.
밑줄은 보기 어렵거나 밑줄 때문에 문자가 가려지기도 합니다. 따라서, 가독성을 위해 밑줄은 사용하지 않습니다.

5. URI 경로에는 소문자를 사용합니다.
URI 경로에 대문자 사용은 지양합니다. RFC3986(URI 문법 형식)은 URI 스키마와 호스트를 제외하고 대소문자를 구별하도록 규정하였기 때문입니다.

6. 파일확장자는 URI에 포함하지 않습니다.
REST API에서는 메시지 내용의 포맷을 나타내기 위해 파일 확장자를 URI 안에 포함시키지 않습니다. 이를 대신하여 **Accept header**를 사용합니다.
    - ex) https://academy.elice.io/classroom/teach/111/python.png ← ( X )
    - ex) GET / classroom/teach/111/python HTTP/1.1 Host: academy.elice.io Accept: image/png ← ( O )

7. 자원 간에 연관 관계가 있는 경우 아래와 같이 작성합니다.
/자원명/자원ID/관계가 있는 다른 자원명
    - ex) GET : /students/{studentid}/classroom

| CRUD | HTTP verbs | Route |
|:---:|:---:|:---:|
| resource들의 목록 표시 | GET | /resource |
| resource 하나의 내용 표시 | GET | /resource/:id |
| resource 생성 | POST | /resource |
| resource 수정 | PUT | /resource/:id |
| resource 삭제 | DELETE | /resource/:id |


## CRUD 설계

CRUD는 Create, Read, Update, Delete의 제일 앞 문자를 하나씩 따와 만든 줄임말입니다. 데이터를 처리하는 시스템이 지속성을 갖기 위해 갖춰야 하는 기본적인 데이터 처리 4가지 기능입니다. 따라서, 시스템에서 데이터를 Create, Read, Update, Delete를 할 수 없다면 정상적으로 작동하는 시스템이 아니라는 것입니다.

이 용어는 데이터 베이스에서 나왔지만 사용자 인터페이스를 설계할 때도 사용될 수 있습니다. 사용자 인터페이스 자체가 시스템에서 오고가는 데이터를 사용자가 직접 눈으로 확인할 수 있도록 바꾼 것이기 때문에 시스템 개념과 인터페이스 개념의 구성원리는 다르지 않습니다.

앞으로 제작할 간단한 게시판을 만들기 위해 최소한의 기능을 구현할 예정입니다. 게시판의 사용 방법을 생각하면 크게 4가지가 있습니다.

- 게시판 내용 생성
- 게시판 조회
- 게시판 수정 및 추가
- 게시판 내용 삭제

CRUD에 빗대어 좀 더 자세히 알아보겠습니다.

- CREATE: 사용자는 글을 올릴 수 있는 게시판이 있고, 글을 생성할 수 있어야 합니다.
- READ: 사용자는 작성한 글을 조회할 수 있어야 합니다.
- UPDATE: 사용자는 작성한 글을 수정하거나 추가적으로 작성할 수 있어야 합니다.
- DELETE: 사용자는 작성한 글을 삭제할 수 있어야 합니다.

| CRUD | DB 명령어 |
|:---:|:---:|:---:|
| CREATE | INSERT |
| READ | SELECT |
| UPDATE | UPDATE |
| DELETE | DELETE |

## AJAX : Asynchronous JavaScript and XML

> 자바스크립트의 라이브러리 중 하나이며 비동기식 자바스크립트와 XML의 줄임말입니다.

- AJAX는 REST API를 손쉽게 구현하기 위해 사용되는 프레임워크이며 html 파일에서 간단히 사용하는 방법에 대해 알아보겠습니다.
- html 파일에서 $.ajax와 같은 형태로 사용되며 코드와 함께 어떠한 요소로 구성되는지 살펴보겠습니다.
```
$.ajax({
    type: 'POST',
    url: '{{url_for("ajax")}}',
    data: JSON.stringify(postdata),
    dataType : 'JSON',
    contentType: "application/json",
    success: function(data){
        alert('성공! 데이터 값:' + data.result2['id']+" " + data.result2['name']+ " " + data.result2['context'])
    },
    error: function(request, status, error){
        alert('ajax 통신 실패')
        alert(error);
    }
})
```

현재 POST 방식으로 생성된 데이터를 확인하고 있습니다. 각 요소의 의미는 아래와 같습니다.

| 요소 | 의미 |
|:---:|:---:|
| type | HTTP 메서드 종류 |
| url | 요청 URL |
| data | 서버로 보낼 데이터 |
| contentType | 서버로 보낼 컨텐츠의 유형 |
| success | 요청이 완료될 때 호출 |
| Text | 요청이 실패할 때 호출 |

이러한 AJAX를 이용할 때 Flask 코드가 어떻게 작성되어야 하는지 알아볼까요?

앞서 json 형태의 데이터를 반환하기 위해 json.dump()를 이용했는데, 이번에는 jsonify() 메소드를 이용해보겠습니다. 만약 실행 결과가 성공이라면 매개변수로 아래와 같이 넘겨주면 됩니다.
```
jsonify(result = "success")
```

마지막으로 POST 요청을 통해 얻은 데이터를 json 형식으로 얻기 위해 ```request.get_json()```을 이용하면 된다는 것을 기억하세요.

### REST API를 AJAX로 구현하는 방법

```
from flask import Flask, render_template, jsonify, request

app = Flask(__name__)

board = []

@app.route('/')
def index():
    return render_template('index.html', rows=board)


@app.route('/ajax', methods=['POST'])
def ajax():
    # 1. ajax() 메소드 내 data 변수에 request를 이용해 전달된 데이터를 json 형태로 저장하세요.
    # POST 요청을 통해 얻은 데이터를 json 형식으로 얻기 위해 request.get_json()을 이용하면 된다는 것을 기억하세요.
    data = request.get_json()
    board.append(data)
    
    # 2. ajax() 메소드에서 jsonify()를 이용해 결과를 반환하세요. 반환되는 내용은 아래와 같습니다.
    return jsonify(result = "success", result2 = data)
```
	
## SQL Alchemy

SQL Alchemy에 REST API를 적용하려고 합니다. 그러기 위해서는 SQLAlchemy()에서 반환되는 SQL 쿼리 결과를 JSON 형태로 변환할 수 있어야 합니다.

```데이터베이스 모델.query.all()```을 이용하면 현재 데이터베이스에 저장된 객체들의 row에 접근할 수 있습니다.

그리고 접근한 객체의 컬럼을 반복문을 이용해 확인할 수 있습니다.
```
for i in `데이터베이스 모델.query.all()`:
    print(i.name) # i의 컬럼인 name 출력
```
이처럼 반복문을 이용해 객체들의 이름과 인덱스를 함께 딕셔너리 형태로 저장해봅시다. 그리고 해당 딕셔너리를 json 형태로 변환해 HTTP 상태 코드와 함께 반환해봅시다.

지시사항
1. key값은 0부터 1씩 증가하고 value는 Member.query.all()의 name을 가지는 딕셔너리를 만드세요.

2. jsonify() 메소드를 이용해 상태 코드와 json으로 변환한 딕셔너리를 함께 반환하세요.

제출 결과

{
  "result": "{\"0\": \"elice\"}", 
  "status": 200
}
Copy
Tips
만약 2개의 데이터가 있으면 아래처럼 제출 결과가 나옵니다.
{
  "result": "{\"0\": \"elice\", \"1\": \"test\"}", 
  "status": 200
}
	
	
	
