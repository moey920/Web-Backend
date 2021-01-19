# RDB로 리소스 관리 및 저장하기

> Relation DataBase와 연동하여 데이터베이스에 리소스를 저장 및 관리하는 방법을 배웁니다.

## RDB와 Flask의 상호작용

### RDB란?

데이터베이스(Database)의 종류는 크게 관계형 데이터베이스(RDB)와 NoSQL(Not Only SQL)로 나뉘게 됩니다.

RDB(Relation Database)는 관계형 데이터 모델을 기반으로 한 데이터 베이스입니다. 다시 말해, **키(key)** 와 **값(value)** 들의 간단한 관계를 테이블화한 데이터베이스입니다. RDB의 특징은 아래와 같습니다.

- 데이터 독립성이 높습니다.
- 고수준의 DML을 사용하여 결합, 제양, 투영 등의 관계 조작에 의해 비약적으로 표현 능력을 높일 수 있습니다.
- 이들의 관계 조작에 의해 자유롭게 구조를 변경할 수 있습니다.

RDB의 종류
- Oracle
- MySQL
- MS-SQL
- DB2
- Maria DB
- Derby
- SQLite

RDB에는 여러 가지 종류가 있지만, 이번 실습에서는 MySQL 을 이용하여 진행하도록 하겠습니다.

RDB와 Flask의 상호작용
Flask에서 RDB를 연동하면 어떻게 될까요? Flask에서 입력 받은 내용들을 DB에 저장할 수 있어야 합니다.
- 효율적인 데이터 관리 기능

파이썬은 오픈 소스와 상용 데이터베이스에 대한 대부분의 데이터베이스 엔진을 위한 패키지를 가지고 있습니다.
본 과목에서는 sqlite3와 Flask 어플리케이션 안에 있는 SQLAlchemy을 사용하여 진행합니다.

> SQLAlchemy는 파이썬 코드에서 DB와 연결하기 위해 사용되는 라이브러리입니다.

- SQLAlchemy는 Flask에서 사용하는 SQL 패키지로 객체 관계형 매퍼입니다.
RDB의 구조와 객체지향언어인 Python의 구조 차이로 인해 발생하는 여러 어려운 Query를 효율적으로 관리할 수 있습니다.

- 웹 서버를 구현할 때, 데이터베이스가 없더라도 구현할 수 있습니다.
단, 저장되는 데이터의 보안과 무결성은 보장할 수 없게 됩니다.

- 웹 서비스에서 모든 데이터를 브라우저에 저장하여 사용할 수 있습니다. 하지만 이렇게 될 경우 위에서 설명한 것과 같이 데이터의 보안과 무결성을 보장할 수 없게 됩니다.

## 사용자 정보 검색

지난 장에서 실습한 내용을 이번 장에서는 DB를 연동하여 실습하겠습니다.

1. 실습에 사용할 DB는 sqlite입니다. 실습을 시작하기 앞서 임포트를 해줍니다.
```
import sqlite3
```

2. DB를 사용하기 위해서는 connect()를 한 상태에서 사용하고 사용이 끝나면 close()로 연결을 끊어줘야 합니다.
```
con =  sqlite3.connect('database.db')
con.execute('쿼리문')
con.commit()
con.close()
```

3. DB와 연결할 때 사용하는 객체를 생성하고 close()를 사용하는게 번거롭다면 다음과 같은 방법으로도 사용 가능합니다.
```
with sqlite3.connect("database.db") as con:
    con.execute("쿼리문")
    con.commit()
```

4. SELECT문 등을 사용해서 다량의 데이터를 추출할 때는 cursor()와 fetchall()을 사용합니다.
```
with sqlite3.connect("database.db") as con:
    cur = con.cursor()
    cur.execute("SELECT문")
    result = cur.fetchall()
```

이제 DB 내에서 SELECT FROM WHERE 쿼리문을 사용하여 사용자 정보를 검색하는 프로그램을 만들겠습니다.

```
#DATABASE
from flask import Flask, render_template, request, url_for, redirect
# 실습에 사용할 DB는 sqlite입니다. 실습을 시작하기 앞서 임포트를 해줍니다.
import sqlite3

app = Flask(__name__)
# DB를 사용하기 위해서는 connect()를 한 상태에서 사용하고 사용이 끝나면 close()로 연결을 끊어줘야 합니다.
conn = sqlite3.connect('database.db')
print ("Opened database successfully")
conn.execute('CREATE TABLE IF NOT EXISTS Board (name TEXT, context TEXT)') # 쿼리문
print ("Table created successfully")
name = [['Elice', 15], ['Dodo', 16], ['Checher', 17], ['Queen', 18]]
for i in range(4):
    conn.execute(f"INSERT INTO Board(name, context) VALUES('{name[i][0]}', '{name[i][1]}')")
conn.commit()
conn.close()
# DB와 연결할 때 사용하는 객체를 생성하고 close()를 사용하는게 번거롭다면 다음과 같은 방법으로도 사용 가능합니다.
# with sqlite3.connect("database.db") as con:
#     con.execute("쿼리문")
#     con.commit()
# SELECT문 등을 사용해서 다량의 데이터를 추출할 때는 cursor()와 fetchall()을 사용합니다.
# with sqlite3.connect("database.db") as con:
#     cur = con.cursor()
#     cur.execute("SELECT문")
#     result = cur.fetchall()

@app.route('/')
def board():
    con = sqlite3.connect("database.db")
    cur = con.cursor()
    cur.execute("select * from Board")
    rows = cur.fetchall()
    con.close()
    print("DB:")
    for i in range(len(rows)):
            print(rows[i][0] + ':' + rows[i][1])
    return render_template('board.html', rows = rows)

@app.route('/search', methods = ['GET', 'POST'])
def search():
    if request.method == 'POST':
        name = request.form['name']
        # database.db와 connect()한 객체 con을 만드세요.
        con = sqlite3.connect("database.db")
        
        # con에 cursor()값인 cur을 만드세요.
        cur = con.cursor()
        
        # cur에 form을 통해 전달 받은 name값이 DB에 있는지 찾는 쿼리문을 실행하세요
        cur.execute(f"SELECT * FROM Board WHERE name='{name}' or context='{name}'")
        
        # 쿼리문 실행결과를 fetchall()을 사용하여 rows에 저장하세요.
        rows = cur.fetchall()
        
        # con을 close()를 사용해 DB와의 연결을 종료하세요.
        con.close()
        
        print("DB:")
        for i in range(len(rows)):
            print(rows[i][0] + ':' + rows[i][1])
        return render_template('search.html', rows = rows)
    else:
        return render_template('search.html')

if __name__ == '__main__':
    app.run(debug=True)
```

## DB 사용자 추가

사용자를 추가할 때는 INSERT INTO 쿼리문을 사용합니다.

1. /add 경로와 add() 함수를 만들어 줍니다.
```
@app.route('/add', methods = ['GET', 'POST'])
def add():
```

2. 데이터베이스에 추가가 완료된다면 commit()을 하지만 중간에 문제가 생겼을 경우에는 이전 위치로 rollback()해야 하기 때문에 try~except문으로 commit()과 rollback()을 제어합니다.
```
try:
    name = request.form['name']
    context = request.form['context']
    with sqlite3.connect("database.db") as con:
        cur = con.cursor()
        cur.execute("INSERT INTO Board (name, context) VALUES (?, ?)", (name, context))
        con.commit()
except:
    con.rollback()
finally : 
    return redirect(url_for('board'))
```

3. 마지막으로 templates 폴더의 add.html 파일을 사용하여 사용자를 추가하는 프로그램을 완성합니다.
```
<form action = "/add" method = "POST">
     이름<br>
     <input type = "text" name = "name" /><br>
     내용<br>
     <input type = "text" name = "context" style="text-align:center; width:400px; height:100px;"/><br><br>
     <input type = "submit" value = "게 시" /><br>
 </form>
```

#### DB 추가, 검색 소스 코드
```
#DATABASE
from flask import Flask, render_template, request, url_for, redirect
import sqlite3
app = Flask(__name__)
conn = sqlite3.connect('database.db')
print ("Opened database successfully")
conn.execute('CREATE TABLE IF NOT EXISTS Board (name TEXT, context TEXT)')
print ("Table created successfully")
name = [['Elice', 15], ['Dodo', 16], ['Checher', 17], ['Queen', 18]]
for i in range(4):
    conn.execute(f"INSERT INTO Board(name, context) VALUES('{name[i][0]}', '{name[i][1]}')")
conn.commit()
conn.close()


@app.route('/')
def board():
    con = sqlite3.connect("database.db")
    cur = con.cursor()
    cur.execute("select * from Board")
    rows = cur.fetchall()
    print("DB:")
    for i in range(len(rows)):
            print(rows[i][0] + ':' + rows[i][1])
    return render_template('board.html', rows = rows)


@app.route('/search', methods = ['GET', 'POST'])
def search():
    if request.method == 'POST':
        name = request.form['name']
        con = sqlite3.connect("database.db")
        cur = con.cursor()
        cur.execute(f"SELECT * FROM Board WHERE name='{name}' or context='{name}'")
        rows = cur.fetchall()
        print("DB:")
        for i in range(len(rows)):
            print(rows[i][0] + ':' + rows[i][1])
        return render_template('search.html', rows = rows)
    else:
        return render_template('search.html')

@app.route('/add', methods = ['GET', 'POST'])
def add():
    if request.method == 'POST':
        try:
            # add.html의 form에서 name, context 값을 가져와 name, context에 저장하세요.
            name = request.form['name']
            context = request.form['context']
            
            with sqlite3.connect("database.db") as con:
                cur = con.cursor()
                # execute()를 실행해 쿼리문을 사용해 name, context값을 DB에 추가하세요.
                cur.execute("insert into Board (name, context) values (?, ?)", (name, context))
                # 수정된 DB가 반영되도록 commit()명령어를 추가하세요.
                con.commit()
                
        except:
            con.rollback()
        finally : 
            return redirect(url_for('board'))
    else:
        return render_template('add.html')

if __name__ == '__main__':
    app.run(debug=True)
```

## 중복 사용자 방지

중복 사용자를 방지하는 방법은 입력된 사용자 이름이 DB 상에 이미 있는지 확인하고 입력 여부를 제어하면 구현할 수 있습니다.

입력받은 사용자 이름이 중복될 경우에는 중복사용자임을 사용자에게 알리고 입력을 다시 받도록 프로그램을 구현하겠습니다.

중복임을 확인하는 데는 여러 방법이 있습니다.

1. 쿼리문을 통해 name이 일치하는 튜플을 추출해서 비교하는 방법이 있습니다.
2. 쿼리문을 통해 name이 일치하는 튜플 수를 쿼리문의 count()를 사용해 그 값이 1개 이상인지 확인하는 방법이 있습니다.

```
#DATABASE
from flask import Flask, render_template, request, url_for, redirect
import sqlite3
app = Flask(__name__)
conn = sqlite3.connect('database.db')
print ("Opened database successfully")
conn.execute('CREATE TABLE IF NOT EXISTS Board (name TEXT, context TEXT)')
print ("Table created successfully")
name = [['Elice', 15], ['Dodo', 16], ['Checher', 17], ['Queen', 18]]
for i in range(4):
    conn.execute(f"INSERT INTO Board(name, context) VALUES('{name[i][0]}', '{name[i][1]}')")
conn.commit()
conn.close()


@app.route('/')
def board():
    con = sqlite3.connect("database.db")
    cur = con.cursor()
    cur.execute("select * from Board")
    rows = cur.fetchall()
    print("DB:")
    for i in range(len(rows)):
            print(rows[i][0] + ':' + rows[i][1])
    return render_template('board.html', rows = rows)

@app.route('/search', methods = ['GET', 'POST'])
def search():
    if request.method == 'POST':
        name = request.form['name']
        con = sqlite3.connect("database.db")
        cur = con.cursor()
        cur.execute(f"SELECT * FROM Board WHERE name='{name}' or context='{name}'")
        rows = cur.fetchall()
        print("DB:")
        for i in range(len(rows)):
            print(rows[i][0] + ':' + rows[i][1])
        return render_template('search.html', rows = rows)
    else:
        return render_template('search.html', msg = "검색어를 입력해주세요.")

@app.route('/add', methods = ['GET', 'POST'])
def add():
    if request.method == 'POST':
        name = request.form['name']
        context = request.form['context']
        with sqlite3.connect("database.db") as con:
            cur = con.cursor()
            # 쿼리문을 활용하여 name이 DB 상에 존재하지 않으면 사용자를 추가하도록 조건문을 작성하세요.
            if not cur.executecur.execute(f"SELECT * FROM Board WHERE name='{name}'") :

                cur.execute(f"INSERT INTO Board (name, context) VALUES ('{name}', '{context}')")
                con.commit()
                
            else:
                # DB 상에 사용자가 있을 경우 else 조건문으로 중복사용자임을 알 수 있도록 add.html의 msg에 값을 넘기세요.
                return render_template('add.html', msg = "중복된 아이디입니다. 다른 아이디를 입력해주세요.")
        return redirect(url_for('board'))
    else:
        return render_template('add.html')
if __name__ == '__main__':
    app.run(debug=True)
```

## 게시판 내용 생성 및 조회

```
#DATABASE
from flask import Flask, render_template, request, url_for, redirect
import sqlite3
app = Flask(__name__)
conn = sqlite3.connect('database.db')
print ("Opened database successfully")
conn.execute('CREATE TABLE IF NOT EXISTS Board (name TEXT, context TEXT)')
print ("Table created successfully")
name = [['Elice', 15], ['Dodo', 16], ['Checher', 17], ['Queen', 18]]
for i in range(4):
    conn.execute(f"INSERT INTO Board(name, context) VALUES('{name[i][0]}', '{name[i][1]}')")
conn.commit()
conn.close()


@app.route('/')
def board():
    con = sqlite3.connect("database.db")
    cur = con.cursor()
    cur.execute("select * from Board")
    rows = cur.fetchall()
    print("DB:")
    for i in range(len(rows)):
            print(rows[i][0] + ':' + rows[i][1])
    return render_template('board.html', rows = rows)


@app.route('/search', methods = ['GET', 'POST'])
def search():
    if request.method == 'POST':
        # search.html의 form에서 name을 전달받아 name에 저장하고 database.db와 연결한 객체 con을 만드세요.
        name = request.form['name']
        con = sqlite3.connect("database.db")
        
        cur = con.cursor()
        # cur을 사용해 form을 통해 입력받은 name 값이 name 또는 context와 일치하는 튜플을 모두 찾는 쿼리문을 실행하세요.
        cur.execute(f"SELECT * FROM Board WHERE name='{name}' or context='{name}'")
        
        rows = cur.fetchall()
        print("DB:")
        for i in range(len(rows)):
            print(rows[i][0] + ':' + rows[i][1])
        return render_template('search.html', rows = rows)
    else:
        return render_template('search.html')


@app.route('/add', methods = ['GET', 'POST'])
def add():
    if request.method == 'POST':
        try:
            name = request.form['name']
            context = request.form['context']
            with sqlite3.connect("database.db") as con:
                cur = con.cursor()
                # form을 통해 입력받은 name, context를 DB에 추가하는 쿼리문을 작성하세요.
                cur.execute("INSERT INTO Board (name, context) VALUES (?, ?)", (name, context))
                
                con.commit()
        except:
            con.rollback()
        finally : 
            return redirect(url_for('board'))
    else:
        return render_template('add.html')

if __name__ == '__main__':
    app.run(debug=True)
```

### SQLite3를 사용하여 DB에 저장하는 프로그램

1. create에 해당하는 코드입니다.
```
try:
    name = request.form['name']
    context = request.form['context']
    with sqlite3.connect("database.db") as con:
        cur = con.cursor()
        cur.execute("INSERT INTO Board (name, context) VALUES (?, ?)", (name, context))
        con.commit()
except:
    con.rollback()
finally : 
    return redirect(url_for('board'))
```

2. read에 해당하는 코드입니다.
```
name = request.form['name']
con = sqlite3.connect("database.db")
con.row_factory = sqlite3.Row
cur = con.cursor()
cur.execute(f"SELECT * FROM Board WHERE name='{name}' or context='{name}'")
rows = cur.fetchall()
print("DB:")
for i in range(len(rows)):
    print(rows[i]['name'] + ':' + rows[i]['context'])
return render_template('search.html', rows = rows)
```

## 수정, 삭제를 포함한 게시판 기능 총 정리 소스 코드

```
#DATABASE
from flask import Flask, render_template, request, url_for, redirect
import sqlite3
app = Flask(__name__)
conn = sqlite3.connect('database.db')
print ("Opened database successfully")
conn.execute('CREATE TABLE IF NOT EXISTS Board (name TEXT, context TEXT)')
print ("Table created successfully")
name = [['Elice', 15], ['Dodo', 16], ['Checher', 17], ['Queen', 18]]
for i in range(4):
    conn.execute(f"INSERT INTO Board(name, context) VALUES('{name[i][0]}', '{name[i][1]}')")
conn.commit()
conn.close()


@app.route('/')
def board():
    con = sqlite3.connect("database.db")
    cur = con.cursor()
    cur.execute("select * from Board")
    rows = cur.fetchall()
    print("DB:")
    for i in range(len(rows)):
            print(rows[i][0] + ':' + rows[i][1])
    return render_template('board.html', rows = rows)


@app.route('/search', methods = ['GET', 'POST'])
def search():
    if request.method == 'POST':
        name = request.form['name']
        con = sqlite3.connect("database.db")
        cur = con.cursor()
        cur.execute(f"SELECT * FROM Board WHERE name='{name}' or context='{name}'")
        rows = cur.fetchall()
        print("DB:")
        for i in range(len(rows)):
            print(rows[i][0] + ':' + rows[i][1])
        return render_template('search.html', rows = rows)
    else:
        return render_template('search.html')


@app.route('/add', methods = ['GET', 'POST'])
def add():
    if request.method == 'POST':
        try:
            name = request.form['name']
            context = request.form['context']
            with sqlite3.connect("database.db") as con:
                cur = con.cursor()
                cur.execute(f"INSERT INTO Board (name, context) VALUES ('{name}', '{context}')")
                con.commit()
        except:
            con.rollback()
        finally : 
            con.close()
            return redirect(url_for('board'))
    else:
        return render_template('add.html')


@app.route('/update/<uid>', methods=['GET','POST'])
def update(uid):
    if request.method =='POST':
        name = request.form['name']
        context = request.form['context']
        with sqlite3.connect("database.db") as con:
            cur = con.cursor()
            # UPDATE ~ SET ~ WHERE을 사용해서 수정 기능을 수행하는 쿼리문을 작성하세요.
            cur.execute(f"UPDATE Board Set name='{name}', context='{context}'WHERE name ='{uid}'")
            
            con.commit()
        return redirect(url_for('board'))
    else:
        con = sqlite3.connect("database.db")
        cur = con.cursor()
        cur.execute(f"SELECT * FROM Board WHERE name ='{uid}'")
        row = cur.fetchall()
        return render_template('update.html',row=row)


@app.route('/delete/<uid>')
def delete(uid):
    with sqlite3.connect("database.db") as con:
            cur = con.cursor()
            # DELETE FROM ~ WHERE을 사용해서 삭제 기능을 수행하는 쿼리문을 작성하세요.
            cur.execute(f"DELETE FROM Board WHERE name='{uid}'")
            
            con.commit()
    return redirect(url_for('board'))


if __name__ == '__main__':
    app.run(debug=True)
```
