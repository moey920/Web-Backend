# 플라스크 웹 애플리케이션 만들기

## 렌더링 템플릿

플라스크 내에서 **html 파일을 사용해** 웹 사이트를 제작할 수 있습니다. 이 때 사용하는 것이 렌더링 템플릿입니다.
html 코드들을 플라스크내에 사용하는 것보다 파일을 따로 만들어 라우팅하는 방법이 유지보수에서 이점을 가져갈 수 있습니다.
플라스크는 ```render_template()```을 이용해 html 파일을 렌더링하여 브라우저에 보여줄 수 있습니다.

다음은 템플릿을 렌더링하는 방법에 대한 간단한 예입니다.
```
from flask import render_template

@app.route('/hello/')
@app.route('/hello/<name>')
def hello(name=None):
    return render_template('hello.html', name=name)
```
hello 함수의 URL에 접근할 때, **hello.html을 반환**하며 해당 **html 파일의 매개변수 name에 함수의 매개변수인 name을 전달**합니다.

Flask는 templates폴더에서 템플릿을 찾기 때문에 **출력할 템플릿은 항상 templates폴더 안에 존재해야 합니다.**

* 플라스크에서는 렌더링 템플릿을 사용하지 않고도 HTML 코드를 통해 화면에 원하는 그래픽을 구성하여 보여줄 수 있습니다.
```
@app.route('/')
def hello():
    return "<h1>안녕하세요</h1>"
```

## Jinja2

렌더링 템플릿을 통해 화면을 출력할 때 html에서는 아래와 같은 Jinja2 문법으로 해당 내용을 띄울 수 있습니다.
```
{% for row in rows %}
<tr>
    <td>{{ loop.index }}</td>
    <td>{{ row[0] }}</td>
    <td>{{ row[1] }}</td>
</tr>
{% endfor %}
```
두 구분자에 대해 설명을 하겠습니다. 먼저 ```{% ... %}``` 같은 경우 if문이나 for문 같은 **제어문을 할당하는 구분자**로 위의 예시에서는 **전달 받은 rows를 반복문을 이용해 접근**합니다. 그리고 변수나 표현식의 결과를 출력하는 구분자인 ```{{ ... }}```을 이용해 **rows의 항목에 접근**할 수 있습니다.

jinaja2의 자세한 사용법을 확인해보세요.<https://jinja.palletsprojects.com/en/2.11.x/templates/>

## 블루 프린트

블루 프린트는애플리케이션에 등록 할 때 실행할 작업을 기록해두고 사용하는 청사진을 의미합니다. Flask의 요청으로 URL을 생성 할 때 화면을 출력하는 함수를 블루 프린트와 연결합니다. 아래는 블루 프린트의 예시입니다.

블루 프린트는 웹 애플리케이션의 개체를 미리 요구하지 않고 기능을 정의할 수 있습니다. 또한, 파일을 여러개로 나누어 유지 보수 측면에서 매우 효과적으로 개발할 수 있게끔 할 수 있습니다

블루 프린트 없이 flask 웹 애플리케이션을 만들 수 있습니다. 하지만 **여러 기능이 포함된 웹 애플리케이션을 제작하고자 한다면 블루프린트를 사용하는 것을 권장합니다.**
```
from flask import Blueprint, render_template, abort
from jinja2 import TemplateNotFound

simple_page = Blueprint('simple_page', __name__,
                        template_folder='templates')

@simple_page.route('/', defaults={'page': 'index'})
@simple_page.route('/<page>')
def show(page):
    try:
        return render_template('pages/%s.html' % page)
    except TemplateNotFound:
        abort(404)
```
```@simple_page.route 장식자```를 이용해 함수를 바인딩하면 **구현한 show() 함수를 통해 블루 프린트가 동작**하게 됩니다. 만약 블루 프린트 등록하려면 아래와 같이 ```register_blueprint()``` 함수를 이용하면 됩니다.
```
from flask import Flask
from yourapplication.simple_page import simple_page

app = Flask(__name__)
app.register_blueprint(simple_page)
```
다만 앞으로의 실습은 블루 프린트는 사용하지 않고 있으니 참고로 봐주시면 됩니다.
