# Flask (Backend)

## 플라스크는 간결하다.

플라스크는 많은 사람이 **‘Micro Web Framework’** 라고 부릅니다. 여기서 칭하는 ‘Micro’는 ‘한 개의 파이썬 파일로 작성할 수 있다’ 또는 ‘기능이 부족하다’와 같은 의미가 아니라 **프레임워크를 간결하게 유지하고 확장할 수 있도록 만들었다는 뜻**입니다.

- Micro: 가벼운 기능 제공, 가볍게 배우고 가볍게 사용할 수 있으며 확장성이 넓다.
- Framework: 이미 작성된 코드의 모임인 **라이브러리 그 이상**의 의미, **어플리케이션을 개발하기 위한 일정한 틀을 제공해주는 기술**이다.

간단한 웹 사이트, 간단한 API 서버를 만드는 데에 특화 되어있는 Python Web Framework입니다. 요즘에는 **클라우드 컴퓨팅의 발달로 Docker, Kubernetes 와 접목해서 소규모 컨테이너 단위로 기능 별 개발 후, 한꺼번에 배포하는 방식이나 배포 후 기능을 추가하는 식으로 자주 사용하고 있습니다.**

## 플라스크의 장점이 무엇인가요?

- 가볍게 배울 수 있다!
    - HTML+CSS+Javascript 아시나요? Python 아시나요? 그렇다면 빠르게 배울 수 있습니다.
- 가볍게 사용할 수 있다!
    - 코드 몇 줄이면 뚝!딱! 만들 수 있습니다.
- 가볍게 배포할 수 있다!
    - pip를 사용하여 Flask를 다운받고 바로 배포하면 끝!

## 그렇다면.. 단점은 무엇인가요?

- Django에 비해서 자유도가 높으나, 제공해 주는 기능이 덜 합니다.
- 복잡한 어플리케이션을 만들려고 할 때 해야 할 것들이 많다.

### 결론은 가벼운 어플리케이션은 Flask, 무거운 어플리케이션은 Django를 사용하라는 말인가요?

정답은 없습니다. **Flask는 소규모의 어플리케이션을 빠르게** 만들 수 있고, **배포 환경에 따라 대규모 어플리케이션의 기능 확장의 역할**을 하기 쉬운 장점이 있습니다. 반면, **Django는 대규모 어플리케이션을 빠르게** 만들 수 있으며, **기본으로 제공 해 주는 기능이 많다는 장점**이 있습니다.

- Flask는 Django와는 다르게 대부분의 기능이 구현되어 있지 않는 대신 내가 원하는 기능만을 모듈을 사용하여 구현이 가능합니다.

| Flask | Django | 
|:---:|:---:|
| ORM 기능이 제공되지 않음 | ORM 기능이 내장 |
| 짧은 코드로 웹 서버 구동 | 자동으로 관리자 화면 구성 | 

* ORM(Object Relational Modeling): ORM은 데이터 베이스와 객체 지향 프로그래밍 언어 간의 호환되지 않는 데이터를 변환하는 프로그래밍 기법
- Flask는 ORM이 제공되지 않아 SQLAlchemy 등을 사용한다.


## Simple Web Server 띄우기

```
# 1. flask 패키지에서 FLASK를 임포트합니다
from flask import Flask

# 2. FLASK 객체 app을 선언합니다
# 여기서 __name__은 코드가 실행되면 해당 모듈명이 대입되는 변수입니다. 즉 __main__이 대입됩니다.
app = Flask(__name__)

# 3. route()를 사용해 웹페이지와 해당 페이지에서 작동할 함수를 매칭시킵니다.
# @app.route()는 해당 웹 페이지의 주소로 접속했을 경우 실행하는 함수를 표기할 때 사용합니다. 이것을 Routing(라우팅)이라고 부릅니다.
@app.route('/')


# 4. '/'에 매칭되는 함수 hello_elice()를 만들고 route() 밑에 작성합니다.
def hello_elice() :
    return "Hello Elice!"
    
# 5. 모듈명이 main일 때만 실행하도록 조건문을 추가합니다.
if __name__ == '__main__':
    app.run()
```

## HTTP란 무엇일까요?

- 모든 웹 브라우저에 있는 정보에 접근할 때는 URL을 통하여 접근합니다. 반대로 생각하면 URL을 모르는 정보에는 접근을 할 수 없다라는 것입니다.
> 위 설명이 현재 우리가 사용하는 WWW(World Wide Web) 의 기본적인 틀 
- URL을 통해 누군가에게 해당 정보를 요청하면, 요청한 정보를 누군가가 나에게 다시 전달해줍니다.
- HTTP는 Hypertext Transfer Protocal의 약자
    - Hypertext: 컴퓨터 화면이나 전자 기기에서 볼 수 있는 데이터이며, 다른 데이터와 연결될 수 있는 주소를 참조하고 있습니다.
    - Transfer: 사람들이 브라우저를 통해 확인하는 웹 상의 데이터는 HTTP에 의해 전달됩니다.
    - Protocal: 규칙 혹은 규약을 뜻합니다.

브라우저 주소창에 URL을 입력하면 그 데이터를 요청하고 보여주는 것은 브라우저의 역할입니다. 그리고 요청 받은 데이터를 가져오는 것은 웹 서버의 역할이며 HTTP는 바로 그 클라이언트와 서버 간의 규칙입니다. 이때, 클라이언트의 요청을 HTTP Request, 서버의 응답을 HTTP Response라고 합니다.
* HTTP는 연결 상태를 유지하지 않는 비연결성 프로토콜로 클라이언트에서 요청을 해야지만 서버에서 응답을 합니다.

### HTTP Methods

Flask는 HTTP 통신을 위해 아래와 같은 Methods를 제공합니다.

| Method | 설명 | 
|:---:|:---:|
| GET | 암호화되지 않은 형태의 데이터를 서버로 전송하는데 사용되는 가장 일반적인 방법 |
| HEAD | GET과 유사한 방법으로 Response Body를 포함하지 않고 사용 | 
| POST | 특정 양식의 데이터를 암호화하여 서버로 전송하는데 사용 | 
| PUT | 특정 대상의 데이터를 갱신(Update)하는데 사용 | 
| DELETE | URL에 지정된 대상을 삭제(DELETE)하는 데 사용됩니다. | 

### HTTP status code

HTTP status code(응답 상태 코드)는 특정 HTTP 요청이 성공적으로 완료되었는지 알려주는 코드입니다. 총 응답은 5개의 그룹으로 나누어집니다. (상태 코드는 section 10 of RFC 2616에 정의되어 있습니다.)

- 응답: 100
- 성공적인 응답: 200
- 리다이렉트: 300
- 클라이언트 에러: 400
- 서버 에러: 500
예를 들어, 우리가 가끔 알 수 없는 URL로 들어갔을 때, 404 Error 가 나오는 페이지를 많이 목격했을 것입니다.

## URL Routing이란?

URL Routing이란 **서버에서 특정 URL에 접속했을 시 보여줄 페이지를 정해주는 것**을 뜻합니다.
즉, **특정 URL을 일부 작업을 수행하기 위한 관련된 기능과 Mapping**하는 데 사용됩니다.

예를들어 https://academy.elice.io/explore 로 접속했을 때 홈화면을 보여주고 https://academy.elice.io/courses 로 접속했을 때 수강할 수 있는 과목들을 볼 수 있는 것은 모두 URL Routing이라는 작업을 통해 이루어집니다.

### Flask에서 URL 매핑은 어떻게 하나요?

Flask에서 URL Routing은 ```@app.route('')``` 를 통해 수행할 수 있습니다.
또한 ```@```를 통해 선언하는 방식을 **decorator(데코레이터)**라고 부릅니다.

```
from flask import Flask

# Flask 인스턴스 생성
app = Flask(__name__)

# '/' URL 접속 시, "This is Home Page" 문구 출력
@app.route('/’)
def home():
    return "This is Home Page"

# '/hello' URL 접속 시, "Hello Elice!" 문구 출력
@app.route('/hello’)
def hello():
    return "Hello Elice!"
```

### URL Mapping이란?

URL 매핑은 웹사이트 주소를 간략하게 치환하는 과정입니다.
아래의 예시와 같이 여러분이 ```home/randing.html``` 폴더에 홈 화면을 구성했다면, URL Mapping을 통해 접속할 수 있는 주소를 바꿀 수 있게 됩니다.

- 기존 경로: ```https://academy.elice.io/home/randing.html```
- URL 매핑 경로: ```https://academy.elice.io/explore```

### URL 매핑을 왜 하는건가요?

1. URL 주소를 가리므로 보안성이 향상됩니다.
2. 주소를 간결하게 함으로 편의성이 향상됩니다.

### URL의 규칙?

Flask의 URL 규칙은 Werkzeug의 라우팅 모듈에 기반합니다. 해당 라우팅 모듈의 기본 사상은 초기 HTTP 서버들에서 제공한 전례에 기반을 둔 잘 구성되고 유일한 URL을 보장합니다.

#### hello 끝점에 대한 정규 URL은 뒷 슬래시(/)를 포함합니다.

```
@app.route('/hello/')
def hello():
    return 'hello!!'
```
* url의 끝점에 /를 포함해라.

#### 뒷 슬래시(/) 없이 URL에 접근하면, Flask가 뒷 슬래시를 가진 정규 URL로 고칩니다.

```
@app.route('/bye')
def bye():
    return 'bye!!'
```

### url 매핑하기 - Routing

```
from flask import Flask


app = Flask(__name__)

#1번을 해보세요!
@app.route('/') # home
def index():
    return "Index Page"


#2번을 해보세요!
@app.route('/hello')
def hello():
    return "Hello Elice!"


if __name__ == '__main__':
    app.run()
```
### Variable Rules

> 플라스크에서는 route()를 사용해서 url path의 값을 활용할 수 있습니다. url path의 문자열, 정수, 서브경로를 다루는 실습

```
from flask import Flask

app = Flask(__name__)

# 문자열은 사용할 변수명을 <변수명>로 감싸주면 함수에서 전달받은 값을 활용할 수 있습니다.
@app.route('/user/<username>') # < 변수명>
def show_user_profile(username) :
    return 'User %s' % username
    
# 정수는 <int:변수명> 형태로 사용이 가능합니다.
@app.route('/post/<int:post_id>')
def show_post(post_id) :
    return 'Post %s' % post_id
    
# 서브경로는 <path:subpath> 형태로 사용이 가능합니다.
@app.route('/path/<path:subpath>') 
def show_subpath(subpath) :
    return "Subpath %s" % subpath
```

### 데이터 반환하기

> Flask에서는 데이터를 json 파일 형식으로 데이터를 교환합니다.

json 파일 형식은 웹 사이트 상에서 정보를 주고 받는 파일의 형식을 뜻합니다.
여러분이 익히 알고 있는 Dictionary(사전형) 형태와 유사한 구조를 이루고 있습니다.
예제 코드와 같이 Dictionary를 list로 감싸고 있는 형태입니다.

Flask 에는 데이터를 json 형식으로 바꿔주는 jsonify() 메소드가 있습니다.
jsonify()를 사용해서 데이터를 반환해봅시다.

```
from flask import Flask, jsonify

app = Flask(__name__)

@app.route('/')
def json():
    people = [{'name':'Elice', 'birth-year':2015}, 
              {'name':'Dodo', 'birth-year':2016},
              {'name':'Queen', 'birth-year':2017}]
    # jsonify()를 사용해서 people을 반환하세요.
    return jsonify(people)

if __name__ == '__main__':
    app.run()
```

### URL 설계하기 - URL Building

여러분은 웹 사이트의 기본 구조를 설계하고 사용자들이 웹 사이트에 접속하기 위해 URL 구조를 설계할 필요성이 있습니다.
만약 한 페이지에서 다른 페이지로 이동할 수 있도록 링크를 생성하려면 어떻게 해야 할까요?

Flask에서는 ```url_for()``` 메소드를 통해 이를 수행할 수 있습니다.
> url_for()
url_for() 함수는 특정 함수에 대한 URL을 동적으로 구축하는 데 사용할 수 있습니다. 
일반적으로 URL 끝점에 이동할 함수 이름과 점(.)을 접두사로 붙이는 것처럼 사용할 수 있습니다.

```
from flask import *  

# Flask 인스턴스 생성
app = Flask(__name__)  


# 홈 페이지로 이동
@app.route('/')
def home():
    return "주소창에 /user/admin과 /admin <br/> /user/student과 /student를 입력해보세요."


# 관리자 페이지로 이동
@app.route('/admin')
def admin():
    return "This is Admin Page"


# 학생 페이지로 이동
@app.route('/student')
def student():
    return "This is Student Page"


# redirect() 함수는 페이지에 다시 연결한다는 뜻으로 마치 페이지를 새로고침 한 것과 같은 동작합니다.
@app.route('/user/<name>')
def user(name):
    # 전달 받은 name이 'admin' 이라면?
    if name == 'admin':
        return redirect(url_for('admin'))

    # 전달 받은 name이 'student' 라면?
    if name == 'student':
        return redirect(url_for('student'))


if __name__ == '__main__':
    app.run()
```

- /user/student or /user/admin url로 접속했을 때 /student(@app.route('/admin')) or /admin(@app.route('/admin')) 페이지로 이동한다.

## REST란 무엇인가요?

> REST는 Representational State Transfer의 약자로, 자원을 이름(자원의 표현)으로 구분하여 해당 자원의 상태를 주고 받는 모든 것을 의미합니다. 한 마디로 정리하면, 자원(resource)의 표현(representation)에 의한 상태 전달입니다.

- 자원의 표현
    - 자원: 해당 소프트웨어가 관리하는 모든 것 (ex. 그림, 문서, 데이터 등)
    - 자원의 표현: 그 자원을 표현하기 위한 이름

- 상태(정보) 전달
    - 데이터가 요청되는 시점에서 자원의 상태(정보)를 전달합니다.
    - JSON 혹은 XML을 통해 데이터를 주고 받는 것이 보통의 전달 방식이다.

WWW와 같은 시스템을 위한 소프트웨어 개발 아키텍처의 한 형식입니다. REST는 기본적으로 웹의 기존 기술과 HTTP 프로토콜을 그대로 사용하기 때문에 웹의 장점을 최대한 활용할 수 있는 아키텍처 스타일입니다. **REST는 네트워크 상에서 클라이언트와 서버 사이의 통신 방식 중 하나**입니다.

### REST의 구체적인 개념

**HTTP URI(Uniform Resource Identifier)을 통해 자원을 명시**하고, **HTTP Method(POST, GET, PUT, DELETE)를 통해 해당 자원에 대한 CRUD Operation을 적용하는 것**을 의미합니다.

- REST는 자원 기반의 구조(ROA) 설계의 **중심에 자원**이 있고 **HTTP Method를 통해 자원을 처리하도록 설계**된 아키텍쳐를 의미합니다.
- **웹 사이트에 존재하는** DB, 이미지, 텍스트 등의 **모든 자원에 고유한 ID인 HTTP URI를 부여**합니다.
- CRUD
    - **C**reate: 생성(POST)
    - **R**ead: 조회(GET)
    - **U**pdate: 수정(PUT)
    - **D**elete: 삭제(DELETE)
    - HEAD: header 정보 조회(HEAD)

### REST의 장단점을 알아보자
장점

서버와 클라이언트의 역할을 명확하게 분리합니다.
여러가지 서비스 디자인에서 생길 수 있는 문제를 최소화합니다.
REST API 메시지가 의도하는 바를 명확하게 나타내므로 의도하는 바를 쉽게 파악할 수 있습니다.
HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용이 가능합니다.
HTTP 표준 프로토콜의 표준을 최대한 활용하여 여러 추가적인 장점을 함께 가져갈 수 있게 해줍니다.
단점

사용할 수 있는 메소드가 4가지 밖에 없습니다.
표준이 존재하지 않습니다.
REST가 왜 필요해요?
다양한 클라이언트의 등장
애플리케이션 분리 및 통합
최근 서버 프로그램은 다양한 브라우저(chrome, Internet Explorer 등)와 안드로이드폰, 아이폰과 같은 모바일 디바이스에서도 통신할 수 있어야 하기 때문입니다.

REST 구성 요소
자원(Resource): URI
: 웹 서버의 자원는 각자 이름을 가지고 있습니다. 따라서, 클라이언트는 이러한 이름을 통해 원하는 정보를 찾을 수 있습니다. 이때 서버 자원의 이름은 통합 자원 식별자 또는 URI라고 부릅니다.

행위(Verb): HTTP Method
: HTTP 프로토콜은 GET, POST, PUT, DELETE와 같은 메서드를 제공합니다.

표현(Representation of Resource)
: 클라이언트가 자원의 상태(정보)에 대해 요청하면 서버는 이에 적절한 응답을 보냅니다. REST의 하나의 자원은 JSON, XML, TEXT, RSS 등 여러 형태의 응답으로 나타낼 수 있습니다. 하지만, 보통 JSON이나 XML을 통해 데이터를 주고 받는 것이 일반적입니다.
