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
