[TOC]

# SQL - Day 1

## 1. 데이터베이스(DB)

데이터베이스는 체계화된 데이터의 모임입니다. 몇 개의 자료 파일을 조직적으로 통합하여 자료 항목의 중복을 없애고 자료를 구조화하여 기억시켜 놓은 자료의 집합체라고 할 수 있습니다.

- 데이터 중복 최소화
- 데이터 무결성
- 데이터 일관성
- 데이터 독립성
- 데이터 표준화
- 데이터 보안 유지

### SQLite

SQLite는 서버가 아닌 응용 프로그램에 넣어 사용하는 비교적 가벼운 DB이다. 구글 안드로이드 운영체제에 기본적으로 탑재된 DB이며, 임베디드 소프트웨어에도 많이 활용되고 있습니다. 로컬에서 간단한 DB 구성할 수 있으며, 오픈소스 프로젝트이기 때문에 자유롭게 사용할 수 있습니다. 설치[↗](https://www.sqlite.org/download.html)

> - 스키마 Schema : 데이터베이스에서 자료의 구조, 표현방법, 관계 등을 정의한 구조
> - 테이블 Table
> - 열 Column / 행 Row
> - 기본키 PK

#### SQL Keywords

> C : INSERT
>
> R : SELECT
>
> U : UPDATE
>
> D : DELETE

#### Datatype

SQLite는 동적 데이터 타입으로, 기본적으로 유연하게 데이터가 들어갑니다. BOOLEAN은 없으며 정수 0, 1으로 저장됩니다. `INTEGER, TEXT, REAL, NUMERIC, BLOB`



## 2. SQL 실습

```sql
sqlite3 tutorial.sqlite3
.databases
```

```sql
.mode csv
.import hellodb.csv examples
.tables
```

```sqlite
SELECT * FROM examples;
```

```sqlite
.headers on
.mode column
```

```sqlite
CREATE TABLE classmates (
    id INTEGER PRIMARY KEY,
    name TEXT
);
.tables
.schema table
```

```sqlite
DROP TABLE classmates;
```

```sqlite
CREATE TABLE classmates (
    name TEXT,
    age INT,
    address TEXT
);
.tables
.schema table
```



##### 00_intro.sql

```sqlite
-- PART1
-- .mode csv
-- .import hellodb.csv examples
-- .schema examples

-- 테이블 생성(CREATE)
CREATE TABLE classmates (
    name TEXT,
    age INT,
    address TEXT
);

-- PRIMARY KEY는 INTEGER만 사용가능!
CREATE TABLE classmates (
id INTEGER PRIMARY KEY,
name TEXT NOT NULL,
age INT NOT NULL,
address TEXT NOT NULL
);

CREATE TABLE tests (
id INTEGER PRIMARY KEY AUTOINCREMENT,
name TEXT NOT NULL
);


-- 데이터 추가(INSERT)
INSERT INTO classmates (name, age)
VALUES ('홍길동', 23);

INSERT INTO classmates
VALUES ('홍길동', 30, '서울');

INSERT INTO classmates (name, age, address)
VALUES ('김영희', 25, '서울');

INSERT INTO classmates
VALUES (2, '홍길동', 25, '서울');

INSERT INTO classmates
VALUES 
('홍길동', 30, '서울'),
('김철수', 23, '대전'),
('이파이', 24, '광주'),
('박로그', 25, '구미');

INSERT INTO classmates VALUES ('최지수', 45, '부산');

INSERT INTO tests (name) VALUES ('홍길동'), ('김철수');
INSERT INTO tests (name) VALUES ('김철수');

-- 데이터 조회
SELECT * FROM classmates;
SELECT rowid, * FROM classmates;
SELECT rowid, name FROM classmates;
SELECT rowid, name FROM classmates LIMIT 1;
SELECT rowid, name FROM classmates LIMIT 1 OFFSET 2;
SELECT rowid, name FROM classmates WHERE address='서울';
SELECT DISTINCT age FROM classmates;

-- 테이블 삭제
DROP TABLE classmates;


-- 데이터 삭제
DELETE FROM classmates WHERE rowid=4;
DELETE FROM tests where id=2;


-- 데이터 수정
UPDATE classmates SET name='홍길동', address='제주도' WHERE rowid=4;
```

```sqlite
-- PART2
-- .mode csv
-- .import users.csv users
-- .schema users


-- 데이터 조회
SELECT * FROM users WHERE age >= 30;
SELECT first_name FROM users WHERE age >= 30;
SELECT last_name, age FROM users WHERE age >= 30 AND last_name='김';

-- 표현식 Expressions
SELECT COUNT(*) FROM users;
SELECT AVG(age) FROM users WHERE age >= 30;
SELECT first_name, MAX(balance) FROM users;
SELECT AVG(balance) FROM users WHERE age >= 30;

-- LIKE (wild cards)
SELECT * FROM users WHERE age LIKE '2_';
SELECT * FROM users WHERE phone LIKE '02-%';
SELECT * FROM users WHERE first_name LIKE '%준';
SELECT * FROM users WHERE phone LIKE '%-5114-%';


-- ORDER
SELECT * FROM users ORDER BY age ASC LIMIT 10;
SELECT * FROM users ORDER BY age, last_name ASC LIMIT 10;
SELECT last_name, first_name FROM users ORDER BY balance DESC LIMIT 10;

-- GROUP BY
SELECT last_name, COUNT(*) AS name_count FROM users GROUP BY last_name;

-- ALTER : 테이블명 변경
CREATE TABLE articles (
title TEXT NOT NULL,
content TEXT NOT NULL
);

INSERT INTO articles VALUES ('1번제목', '1번내용');
SELECT rowid, * FROM articles;
ALTER TABLE articles RENAME TO news;

-- ALTER : 새로운 컬럼 추가
ALTER TABLE news ADD COLUMN created_at TEXT NOT NULL; -- 기존 데이터가 있어서, NULL값으로 새로운 컬럼을 넣을 수 없습니다.
ALTER TABLE news ADD COLUMN created_at TEXT;
INSERT INTO news VALUES ('title', 'content', datetime('now'));
ALTER TABLE news ADD COLUMN subtitle TEXT NOT NULL DEFAULT 1;
```





## 3. ORM 실습

```django
python manage.py migrate
sqlite3 db.sqlite3
.tables
.mode csv
.import users.csv users_user
SELECT COUNT(*) FROM users_user;
.schema users_user
# python manage.py shell_plus
```

##### SQL vs ORM - PART1

```sqlite
User.objects.all()
SELECT * FROM users_user;

User.objects.create(first_name='길동', last_name='홍', age=100, country='제주도', phone='010-1234-5678', balance=10000);
INSERT INTO users_use (first_name, last_name, age, country, phone, balance)
VALUES ('길동', '홍', 100, '제주도', '010-1234-5678', 10000);
-- DELETE FROM users_user WHERE id=102

User.objects.get(pk=101)
SELECT * FROM users_user WHERE id=101;


user = User.objects.get(pk=101)
user.last_name = '김'
user.save()
UPDATE users_user SET first_name='철수' WHERE id=101;


User.object.get(pk=101).delete()
DELETE FROM users_user WHERE id=101;
```

##### SQL vs ORM - PART2

```sqlite
User.objects.count()
SELECT COUNT(*) AS user_count FROM users_user;


User.objects.filter(age=30).values('first_name')
SELECT first_name FROM user_count WHERE age=30;
-- print(User.objects.filter(age=30).values('first_name').query) 쿼리를 확인합니다.


User.objects.filter(age__gte=30).count()
SELECT COUNT(*) FROM users_user WHERE age >= 30;


User.objects.filter(age=30, last_name='김').count()
SELECT COUNT(*) FROM users_user WHERE age = 30 AND last_name = '김';


-- ORM에서 'or' 활용하기
from django.db.models import Q
User.objects.filter(Q(age=30) | Q(last_name='김').count()
SELECT COUNT(*) FROM users_user WHERE age = 30 OR last_name = '김';

                    
User.objects.filter(phone__startswith='02-').count()
SELECT COUNT(*) FROM users_user WHERE phone LIKE '02-%';
                    

User.objects.filter(country='강원도', last_name='황').values('first_name')
SELECT first_name FROM users_user WHERE country='강원도' AND last_name='황';
```

```sqlite
User.objects.order_by('-age')[:10]
SELECT * FROM users_user ORDER BY age DESC LIMIT 10;


User.objects.order_by('balance')[:10]
SELECT * FROM users_user ORDER BY balance LIMIT 10;


User.objects.order_by('balance', '-age')[:10]
SELECT * FROM users_user ORDER BY balance ASC, age DESC LIMIT 10;


User.objects.order_by('-last_name', '-first_name')[4]
SELECT * FROM users_user ORDER BY last_name DESC, first_name DESC LIMIT 1 OFFSET 4;
```

##### SQL vs ORM - PART3

```sqlite
total_age = User.objects.aggregate(Avg('age'))
total_age.get('age__avg')
User.objects.aggregate(total_age_avg=Avg('age'))

SELECT AVG(age) FROM users_user;


User.objects.filter(last_name='김').aggregate(Avg('age'))
SELECT AVG(age) FROM users_user WHERE last_name='김';


User.objects.filter(country='강원도').aggregate(Avg('balance'))
SELECT AVG(balance) FROM users_user WHERE country='강원도';


user = User.objects.order_by('-balance').first()
user.balance
SELECT balance FROM users_user ORDER BY balance DESC LIMIT 1;
SELECT MAX(balance) FROM users_user;


User.objects.aggregate(Sum('balance'))
SELECT SUM(balance) FROM users_user;
```





















