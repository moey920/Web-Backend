# SQL Alchemy

> SQL Alchemy를 연동하여 데이터를 저장하는 스토리지를 단순화하고 더 강력하게 관리하는 방법을 학습합니다.

## ORM과 SQLAlchemy

### ORM이란?

ORM이란 객체 관계 매핑(Object Relational Mapping)을 의미하며 간단하게 데이터베이스 내의 테이블들을 객체화하여 각 DBMS(MySQL)에 대해서 CRUD 등을 공통된 접근 기법으로 사용할 수 있습니다. 
일반적으로 하나의 객체 정보를 다루기 위해서 여러 SQL 쿼리를 사용하게 되고, 프로그램이 커질수록 작성해야 하는 SQL 쿼리가 많아져 복잡해집니다. 
따라서 반복되는 쿼리를 객체 단위로 생성하여 이를 해결하고자 했고 이런 작업을 도와주는 것을 바로 ORM이라고 합니다. 
ORM을 사용하게 되면 따로 SQL문을 작성할 필요없이 객체를 통해 간접적으로 데이터베이스를 조작할 수 있습니다.

> 대표적인 Python ORM은 DjangoORM 과 SQLAlchemy 등이 있습니다.

#### 장점

- **개발 생산성을 증가시킬 수 있습니다.** 프로그래머는 DBMS에 대한 큰 고민없이, ORM에 대한 이해만으로 대부분의 CRUD를 다룰 수 있습니다.
- **유지보수성이 좋습니다.** 데이터 구조 변경시, 객체에 대한 변경만 이루어지면 됩니다.
- **코드 가독성이 좋습니다.** 객체를 통하여 대부분의 데이터를 접근 및 수정을 진행합니다

#### 단점

- 호출 방식에 따라 성능이 천차만별입니다.
- 복잡한 쿼리 작성시, ORM 사용에 대한 난이도가 증가합니다.
- DBMS(MySQL) 고유의 기능을 모두 사용하지 못합니다.

### SQL 쿼리와 ORM의 차이점

아래와 같은 데이터베이스 테이블이 있을 때 이름이 여왕이고 나이가 18살인 데이터를 삽입하려고 합니다. SQL 쿼리와 ORM이 어떻게 다른지 확인해볼까요?

- SQL 쿼리문
```
INSERT INTO 엘리스 (name, age) VALUES (‘여왕’, ‘18’);
```

- ORM
```
member = Member(name=‘여왕’, age=‘18’)
db.session.add(member)
```

> ORM으로 작성된 코드가 흔히 말하는 객체 지향 프로그래밍과 유사한 코드라는 것을 확인할 수 있습니다.

### SQLAlchemy란?

파이썬 ORM 라이브러리에서 가장 많이 사용되는 SQLAlchemy는 파이썬 코드에서 Flask를 데이터베이스와 연결하기 위해 사용됩니다. SQLAlchemy은 ORM으로 **데이터베이스 테이블을** 프로그래밍 언어의 **클래스로 표현**하게 해주고 테이블의 저장, 읽기, 업데이트, 삭제 등을 돕습니다.

#### 왜 사용하나요?
SQLAlchemy를 사용하면 SQL 쿼리를 사용하지 않고 프로그래밍 언어로 객체간의 관계를 표현할 수 있습니다. SQLAlchemy로 Model을 정의하고, 정의한 모델을 테이블과 Mapping할 수 있는 여러가지 방법을 쉽게 구현할 수 있습니다. 이는 데이터베이스의 종류와 상관 없이 일관된 코드를 유지할 수 있어 프로그램 유지 및 보수가 편리하고 SQL 쿼리를 내부에서 자동으로 생성하기 때문에 오류 발생률이 줄어드는 장점이 있습니다.

## Model 구현

앞서 본 코드에서 Member는 파이썬 클래스입니다. 클래스는 데이터베이스의 테이블과 매핑하여 사용되며 데이터베이스의 테이블과 매핑되는 이 클래스를 모델이라고 합니다.
```
member = Member(name='여왕', age='18')
db.session.add(member)
```

SQLAlchemy에 내장되어 있는 Model 클래스를 상속하여 이를 구현할 수 있으며 쿼리를 사용할 수 있습니다.
```
from flask_sqlalchemy import SQLAlchemy
db = SQLAlchemy()
class Member(db.Model):
name = db.Column(db.String(20), primary_key = True)
age = db.Column(db.Integer, nullable=False)
```

### 쿼리의 종류
쿼리의 종류와 사용법은 다음과 같습니다.

| 종류 | 사용법 |
|:---:|:---:|
| equal | == |
| not equal | != |
| like | like() |
| in | in_() |
| not in | ~ in_() |
| is null | ==None |
| is not null | !=None |
| and | & |
| or | \| |
| order by | order_by() |
| limit | limit() |
| offset | offset() |
| count | count() |
	
> equal (==): 이름이 “Elice”인 멤버를 검색합니다.
```
@app.route('/')
def list():
    member_list = Member.query.filter(Member.name == 'Elice')
    return " ".join(i.name for i in member_list)
```

> not equal (!=): 이름이 “Elice”가 아닌 멤버를 검색합니다.
```
@app.route('/')
def list():
    member_list = Member.query.filter(Member.name != 'Elice')
    return " ".join(i.name for i in member_list)
```

> like (like()): 이름이 “Elice”와 비슷한 멤버를 검색합니다.
```
@app.route('/')
def list():
    member_list = Member.query.filter(Member.name.like('Elice'))
    return " ".join(i.name for i in member_list)
```

> in: 이름이 “Elice”, “Dodo”에 포함되는 멤버를 검색합니다.
```
@app.route('/')
def list():
    member_list = Member.query.filter(Member.name.in_(['Elice', 'Dodo']))
    return " ".join(i.name for i in member_list)
```

> not in: 이름이 “Elice”, “Dodo”에 포함되지 않는 멤버를 검색합니다.
```
@app.route('/')
def list():
    member_list = Member.query.filter(~Member.name.in_(['Elice', 'Dodo']))
    return " ".join(i.name for i in member_list)
```

> is null: 이름이 비어있는 멤버를 검색합니다.
```
@app.route('/')
def list():
    member_list = Member.query.filter(Member.name == None)
    return " ".join(i.name for i in member_list)
```

> is not null: 이름이 비어있지 않은 멤버를 검색합니다.
```
@app.route('/')
def list():
    member_list = Member.query.filter(Member.name != None)
    return " ".join(i.name for i in member_list)
```

> and: 이름이 “Elice”이며 나이가 15살인 멤버를 검색합니다.
```
@app.route('/')
def list():
    member_list = Member.query.filter((Member.name == 'Elice') & (Member.age == '15'))
    return " ".join(i.name for i in member_list)
```

> or: 이름이 “Elice”이거나 나이가 15살인 멤버를 검색합니다.
```
@app.route('/')
def list():
    member_list = Member.query.filter ((Member.name == 'Elice') | (Member.age == '15'))
    return " ".join(i.name for i in member_list)
```

> order by: 나이순으로 정렬하여 검색합니다. 기본적으로 오름차순으로 정렬되지만 desc() 사용시 내림차순으로 정렬됩니다.
```
@app.route('/')
def list():
    member_list = Member.query.order_by(Member.age.desc())
    return " ".join(i.name for i in member_list)
```

> limit: 나이를 내림차순으로 정렬하되, limit_num의 크기만큼 반환합니다.
```
@app.route('/')
def list(limit_num = 5):
    if limit_num is None:
        limit_num = 5
    member_list = Member.query.order_by(Member.age.desc()).limit(limit_num)
    return " ".join(i.name for i in member_list)
```

> offset: 나이를 내림차순으로 정렬하되, off_set 크기만큼 앞에서 부터 생략하고 반환합니다.
```
@app.route('/')
def list(off_set = 5):
    if off_set is None:
        off_set = 5
    member_list = Member.query.order_by(Member.age.desc()).offset(off_set)
    return " ".join(i.name for i in member_list)
```

> count: 나이를 내림차순으로 정렬하고 나오는 튜플 수를 반환합니다.
```
@app.route('/')
def list():
    member_list = Member.query.order_by(Member.age.desc()).count()
    return str(member_list)
```

# Model을 사용하여 자료 추가

add.html 파일과 _add() 함수를 추가하면 데이터를 입력받고 입력받은 값을 모델을 사용해서 DB에 저장할 수 있습니다.

add.html에는 POST 메소드를 사용해서 form을 사용하여 입력받겠습니다.

form에서는 이름과 나이 두 값을 입력받습니다.

add()에서는 입력받은 name과 age를 사용하되 age를 숫자만 입력받도록 작성합니다.

DB에 새로운 튜플이 추가가 완료되면 _list() 주소를 반환하도록 redirect()를 사용합니다.

```
from flask import Flask, request, render_template, redirect, url_for
from flask_migrate import Migrate
from flask_sqlalchemy import SQLAlchemy
import config

db = SQLAlchemy()
migrate = Migrate()

app = Flask(__name__)
app.config.from_object(config)
db.init_app(app)
migrate.init_app(app, db)
from models import Member


@app.route('/list')
def _list():
    member_list = Member.query.all()
    return render_template('member_list.html', member_list=member_list)

# 1. 데이터를 추가할 경로와 함수를 만드세요.
@app.route('/', methods=['GET', 'POST'])
def _add():

    # 2. HTTP 메소드가 POST일 때 request로 부터 form 값을 받아오도록 코드를 작성하세요.
    if request.method == 'POST':
        name = request.form['name']
        # age = request.form['age']
        # 3. 나이를 입력 받을 때는 숫자만 입력받도록 try~except문을 추가하세요.
        try:
            age = int(request.form['age'])
        except:
            return "나이는 숫자만 입력하세요."
        
        # 4. 입력받은 이름, 나이를 DB에 추가하는 코드를 작성하세요.
        member = Member(name, age)
        db.session.add(member)
        db.session.commit()
        
        # 5. 추가가 완료되면 list를 보여주도록 redirect()를 사용하여 _list()로 이동하도록 코드를 작성하세요.
        return redirect(url_for('_list'))
        
    else:
        return render_template('add.html')
        
if __name__ == "__main__":
    app.run()
```


