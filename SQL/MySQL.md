# MySQL

### About MySQL
- DBMS로 속도가 빠르고 대용량 데이터 처리가 가능    
- 표준 SQL 형식 사용
- 다양한 운영체제 지원
- 무료 사용 가능


### Install MySQL (in Ubuntu)
1. `# apt-get install mysql-server`: 설치 명령어
2. NCP의 ACG 설정에서 `3306 port` 추가
3. `systemstl start mysql` : mysql 실행 명령어
4. `/usr/bin/mysql -u root -p` : 접속 명령어 (root 패스워드와 동일)       
    `mysql` 명령으로도 접속 가능 (root 계정일때)

<br>

### DataBase
논리적으로 연관된 데이터를 모아 구조적으로 통합해 놓은 것

(Before) 파일 시스템의 단점
- 데이터 중복
- 데이터 불일치       
→ **DBMS** 발전 (DataBase Management System)


### DB 용어
- 테이블: 데이터의 집합
- 컬럼: 각 데이터의 속성
- 로우: 테이블 행에 저장된 정보

<br>

## SQL 

- 데이터베이스 목록 조회: `show databases;`
- 테이블 생성 + 한글 인코딩 : `CREATE DATABASE 이름 DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;`
- 사용할 데이터베이스 선택: `use DB명;`

### Create Table
- 테이블 생성: `CREATE TABLE 테이블명 ( 컬럼명1 데이터타입, 컬럼명2 데이터타입 );`
- 테이블 구조 확인: `desc 테이블명;`
- 테이블 목록 조회: `show tables;`


### Data Type
**문자형 데이터 타입** 
- `CHAR(n)`: 고정길이 데이터 타입 (공백은 채워짐)
- **`VARCHAR(n)`**: 가변길이 데이터 타입
- `TINYTEXT(n)`
- **`TEXT(n)`** or **`TEXT`**: 문자열 데이터 타입 (최대 65535byte)
- `MEDIUMTEXT(n)`
- `LONGTEXT(n)`
⇒ 글자당 영어는 1byte, 한글은 2byte


**숫자형 데이터 타입**
- `TINYINT(n)`
- `SMALLINT(n)`
- `MEDIUMINT(n)`
- **`INT(n)`**
- `BIGINT(n)`
- `FLOAT(길이, 소수)`
- `DECIMAL(길이, 소수)`


**날짜형 데이터 타입**
- **`DATE`**: 날짜(년도, 월, 일) 형태 / 0000-00-00
- `TIME`: 시간(시, 분, 초) 형태
- **`DATETIME`**: 날짜와 시간 형태의 기간 표현 데이터 타입 0000-00-00 / 00:00:00       
    ⇒ *회원가입*, *게시글 작성 시간* 등에 활용 
- `TIMESTAMP`: 날짜와 시간 형태의 기간 표현 데이터 타입
- `YEAR`: 년도 표현

**`null`** : 값이 존재하지 않음


<br> 


### CRUD
Create, Read, Update, Delete 

- Create: `INSERT INTO 테이블명 (컬럼1, 컬럼2) VALUES (값1, 값2);`
- Update: `UPDATE 테이블명 SET 컬럼1=값1, 컬럼2=값2 (WHERE 컬럼2=조건2);`
- Delete
  - 특정 데이터 삭제: `DELETE FROM 테이블명 WHERE 컬럼1=조건1;`
  - 테이블 내용 삭제: `DROP FROM 테이블명;`
  - 테이블 구조 삭제: `DROP TABLE 테이블명;`
  +) `DELETE` vs. `TRUNCATE` vs. `DROP`
    - delete: 내용 삭제
    - truncate: 공간 삭제
    - drop: 존재 삭제
- Read
  - `SELECT * FROM 테이블명;`
  - `SELECT 컬럼1 FROM 테이블명 WHERE 컬럼1=조건1;`

<br> 


**WHERE문 연산자**
- `=`
- `!=`, `<>`
- `>`, `>=`, `<`, `<=`
- `BETWEEN A AND B`
- `IN`, `NOTIN`
- `IS NULL`

**검색조건**
- `AND` : 모든 조건을 충족하는 행을 가져온다. ( 조건의 교집합 )
- `OR` : 하나의 조건만 만족하면, 해당되는 행을 가져온다. ( 조건의 합집합 )
- `IN` : 조건의 범위를 지정할 대 사용 ( OR을 단순화 시킨 형태 )
- `NOT` : 뒤에 오는 조건들의 역

**패턴 (LIKE)**       
특정 문자나 문자열의 포함 여부를 검색할 때 사용 
- `%` : 0개 이상의 어떤 문자
- `_` : 1개의 단일문자

<br>

---
### Tip! 
(실행 속도)    
- `like` > `between`        
- `>` > `>=`        

(SELECT문 우선순위)        
`SELECT` > `FROM` > ( `WHERE` > `ORDER BY` > `LIMIT` )





