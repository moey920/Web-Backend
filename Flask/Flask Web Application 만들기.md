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

- 블루프린트는 웹 애플리케이션에 등록할 수 있는 청사진, 경로, 기타 앱 관련 기능을 나타냅니다.
- 블루프린트는 웹 애플리케이션의 개체를 미리 요구하지 않고 기능을 정의할 수 있습니다. 또한, 파일을 여러개로 효과적으로 개발할 수 있게끔 할 수 있어 유지보수적인 측면에서 이점을 가집니다.
- 블루프린트 없이 flask 웹 애플리케이션을 만들 수 있습니다. 하지만 복잡한 웹 애플리케이션을 구현하고자 할 때 블루프린트를 사용하는 것을 권장합니다.

## 게시물 생성 및 읽기

리스트 자료구조를 사용하여 게시글을 추가하고 조회하는 프로그램을 만들겠습니다.

추가는 사용자로부터 입력받은 값을 리스트에 추가하는 작업을 하고 조회는 리스트의 값을 html파일로 보내 사용자가 볼 수 있도록 하는 작업입니다.

생성과 조회 모두 Board.html에서 이루어지도록 작성하겠습니다.

페이지는 하나지만 함수는 두 가지를 사용해야 합니다. 초기 페이지에서는 조회 기능을 /add 페이지에서는 추가 기능을 만들겠습니다.

### 조회

조회는 Board.html에서 Jinja2 문법을 사용하면 구현가능합니다.
```
{% for row in rows %}
<tr>
    <td>{{ loop.index }}</td>
    <td>{{ row[0] }}</td>
    <td>{{ row[1] }}</td>
</tr>
{% endfor %}
```

Jinja2를 사용하면 html 파일 내에서 파이썬처럼 제어문이나 변수 등을 사용할 수 있습니다.

if나 for을 사용할 때는 {%for 또는 if%} 태그를 사용합니다. 제어문이 끝날 때는 {%endfor 또는 endif%}로 마무리합니다.

변수를 사용할 때는 {{변수명}}형태로 사용합니다.

```return render_template('Board.html', rows = board)```

위와 같이 코드를 작성하면 리스트를 html 파일로 넘겨줄 수 있습니다.

### 생성

생성은 request를 사용합니다. html의 form을 사용해서 다음과 같이 코드를 짜면 파이썬 코드 내에서 입력받은 값을 사용할 수 있습니다.
```
<form action = "/add" method = "POST">
    이름<br>
    <input type = "text" name = "name" /><br>      
    <input type = "submit" value = "게 시" /><br>
</form>
```

파이썬 코드에서는

```name = request.form['name']```

이렇게 입력받은 값을 활용 가능합니다. 입력받은 값을 리스트에 추가하면 생성이 완료됩니다.

지시사항

1. board 리스트를 render_template를 사용하여 Board.html로 넘겨줘서 board의 내용을 조회할 수 있도록 index 함수를 완성하세요.

2. form을 통해 전달받은 name과 context를 board에 추가하는 코드를 추가하여 add() 함수를 완성하세요.

```
from flask import Flask, render_template, request, redirect, url_for
app = Flask(__name__)

board = []
@app.route('/')
def index(): # 조회 : 조회는 Board.html에서 Jinja2 문법을 사용하면 구현가능합니다.
    # board 리스트를 render_template를 사용하여 Board.html로 넘겨줘서 board의 내용을 조회할 수 있도록 index 함수를 완성하세요.\
    
    return render_template('Board.html', rows = board)


@app.route('/add', methods = ['POST'])
def add(): # 생성은 request를 사용합니다. html의 form을 사용해서 다음과 같이 코드를 짜면 파이썬 코드 내에서 입력받은 값을 사용할 수 있습니다.
    if request.method == 'POST':
        # form을 통해 전달받은 name과 context를 board에 추가하는 코드를 추가하여 add() 함수를 완성하세요.
        board.append([request.form['name'], request.form['context']])

        return redirect(url_for('index'))
        
    else:
        return render_template('Board.html', rows = board)


if __name__ == '__main__':
    app.run(debug=True)
```

## UPDATE & DELETE 구현

리스트 자료구조를 사용하여 게시글을 수정하고 삭제하는 프로그램을 만들겠습니다.

수정이나 삭제에서는 어떤 자료를 수정 또는 삭제할지를 알아야 합니다. name만 가지고 삭제를 할 경우에는 name이 중복된 원소들이 모두 삭제 될 수도 있는 삭제이상 현상이 발생할 수도 있습니다.

그래서 본 실습에서는 index를 사용해 수정과 삭제를 수행하겠습니다. 리스트 자료구조의 index는 중복없이 0부터 순서대로 정해집니다.
```
<td>
    <a href ="{{url_for('update', uid = loop.index)}}">수정</a> 
    <a href ="{{url_for('delete', uid = loop.index)}}">삭제</a>
</td>
```

위와 같이 html의 <a href> 태그를 통해 index를 같이 넘겨줄 수 있습니다. 단 저기서 loop.index는 1부터 세기 때문에 main에서 사용할 때는 uid-1 값을 사용해야 합니다.

delete()와 update()에 대한 함수를 각각 만들고 delete()는 리스트의 원소 삭제 기능을 추가하고, update에는 수정내용을 POST로 받아서 리스트의 원소 수정 기능을 수행하면 CRUD의 U와 D 기능 구현이 완료됩니다.
```
from flask import Flask, render_template, request, redirect, url_for
app = Flask(__name__)

board = []

@app.route('/')
def index():
    return render_template('list.html', rows = board)


@app.route('/add', methods = ['POST'])
def add():
    print(request.method)
    if request.method == 'POST':
        board.append([request.form['name'], request.form['context']])
        return redirect(url_for('index'))
    else:
        return render_template('list.html', rows = board)


@app.route('/delete/<int:uid>')
def delete(uid):
    # 매개변수 uid번째에 해당하는 리스트 원소를 제거하는 delete()함수를 완성하세요.
    
    del board[uid:uid+1]
    
    return redirect(url_for('index'))


@app.route('/update/<int:uid>', methods=['GET','POST'])
def update(uid): # 업데이트 제대로 동작하지 않음, 차우에 다시 수정하기.
    if request.method =='POST':
        # 매개변수 uid번째에 해당하는 리스트 원소를 입력받은 name, context로 수정하는 update() 함수를 완성하세요.
        
        board[uid] = [request.form['name'], request.form['context']]
        
        return redirect(url_for('index'))
    else:
        return render_template('update.html',index=uid,rows=board)


if __name__ == '__main__':
    app.run(debug=True)
```

## 인증(Authentication)이란?

인증(Authentication)은 유저의 identification을 확인하는 절차입니다. 한 마디로 정의하자면, 유저의 아이디와 비밀번호를 확인하는 절차입니다. 인증을 하기 위해선 먼저 유저의 아이디와 비밀번호를 생성할 수 있는 기능도 필요합니다. 우리가 생각하는 로그인 절차를 나열해봅시다.

1. 회원가입: 아이디와 비밀번호를 생성합니다.
    - 이때, 비밀번호 암호화 저장합니다.
2. 로그인: 아이디와 비밀번호를 입력합니다.
    - 입력한 비밀번호를 암호화 한 후, DB에 저장된 암호화 비밀번호와 비교합니다.
3. 일치하면 로그인 성공, 일치하지 않으면 에러가 발생합니다.
4. 로그인이 성공되면 access_tocken을 클라이언트에게 전송합니다.
5. 유저는 로그인 성공 후 다음부터는 access_tocken을 첨부하여 request를 서버에 전송합니다.

### 비밀번호 암호화

유저의 비밀번호는 절대 그대로 DB에 저장되지 않습니다. DB가 해킹 당할 경우 그대로 유출되기 때문입니다. 따라서, 유저의 비밀번호는 반드시 암호화하여 저장해야 합니다. 그럴 경우 DB가 해킹 당하더라도 그대로 노출되지 않기 때문입니다.

### 허가(Authorization)

허가(Authorization)은 유저가 요청하는 request를 실행할 수 있는 권한이 있는 유저인가를 확인하는 절차입니다. 예를 들어 해당 유저는 고객 정보를 볼 수 있지만 수정할 권한이 없는 경우입니다.

## 로그인 기능 구현

로그인 기능은 기존에 저장된 유저 정보로 로그인을 할 수도 있지만 회원가입 기능을 추가한 로그인하는 프로그램을 만들겠습니다.

1. 초기 페이지에서는 로그인이 되어있다면 loggedin.html로 넘어가도록 로그인이 되어 있지 않다면 index.html로 넘어가도록 코드를 작성합니다.
```
def home():
    if session.get('logged_in'):
        return render_template('loggedin.html')
    else:
        return render_template('index.html')
```

- 이 때 로그인 여부는 session을 사용하여 확인합니다.

2. index.html에는 사용자가 로그인 또는 회원가입을 할 수있도록 링크로 안내합니다.
```
<p>로그인이 필요한 서비스입니다.</p>
<a href= "login">로그인창으로</a><br>
<a href= "register">회원가입</a>
```

3. 아이디, 비밀번호는 dictionary 자료구조인 userinfo를 사용해서 저장하고 활용합니다.
```userinfo = {'Elice': '1q2w3e4r!!'}```

4. 회원가입 같은 경우는 입력 받은 username, password 값을 userinfo에 추가하고 login 창으로 redirect하도록 코드를 작성합니다.

5. 로그인은 입력받은 username이 userinfo에 있는지 1차적으로 확인하고 2차적으로는 username에 해당하는 value 값이 입력받은 password값과 같은지 비교를 합니다. 

아이디가 없거나 비밀번호가 틀리면 사용자에게 안내하도록 코드를 작성합니다.

6. 로그인이 성공한다면 session['logged_in'] 값을 True로 바꾸고 초기 페이지로 redirect 합니다.

7. 로그아웃은 session['logged_in'] 값을 False로 바꾸고 초기 페이지로 redirect 합니다.

```
# 1번을 해보세요!
from flask import Flask, request, render_template, session, url_for, redirect

app = Flask(__name__)
app.secret_key = 'super secret key'
app.config['SESSION_TYPE'] = 'filesystem'
userinfo = {'Elice': '1q2w3e4r!!'} # 아이디, 비밀번호는 dictionary 자료구조인 userinfo를 사용해서 저장하고 활용합니다.


@app.route("/")
def home(): # 이 때 로그인 여부는 session을 사용하여 확인합니다.
    if session.get('logged_in'): # 초기 페이지에서는 로그인이 되어있다면 loggedin.html로 넘어가도록
        return render_template('loggedin.html')
    else: # 로그인이 되어 있지 않다면 index.html로 넘어가도록
        return render_template('index.html')


@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        name = request.form['username']
        password = request.form['password']
        # 로그인 부분 오류 발생, 차후 수정 예정
        try: # 로그인은 입력받은 username이 userinfo에 있는지 1차적으로 확인하고
            if (name in userinfo):
                # 2차적으로는 username에 해당하는 value 값이 입력받은 password값과 같은지 비교
                if userinfo[name] == password :
                    #로그인이 성공한다면 session['logged_in'] 값을 True로 바꾸고 초기 페이지로 redirect 합니다.             
                    session['logged_in'] = True 
                    # redirect()를 사용해서 초기 페이지로 이동하세요.
                    return redirect(url_for('/'))

                    
                else: # 아이디가 없거나 비밀번호가 틀리면 사용자에게 안내하도록 코드를 작성합니다.
                    return '비밀번호가 틀립니다.'
            return '아이디가 없습니다.'
        except:
            return 'Dont login'
    else:
        return render_template('login.html')


@app.route('/register', methods=['GET', 'POST'])
def register(): # 회원가입 같은 경우는 입력 받은 username, password 값을 userinfo에 추가하고 login 창으로 redirect하도록 코드를 작성합니다.
    if request.method == 'POST':
        name = request.form['username']
        password = request.form['password']
        # userinfo에 입력받은 username은 key로 password는 value로 하여 추가하세요.
        userinfo[name] = password
        return redirect(url_for('login'))
        
    else:
        return render_template('register.html')


@app.route("/logout")
def logout():
    session['logged_in'] = False # 로그아웃은 session['logged_in'] 값을 False로 바꾸고 초기 페이지로 redirect 합니다.
    return render_template('index.html')
```

