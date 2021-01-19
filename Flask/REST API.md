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
