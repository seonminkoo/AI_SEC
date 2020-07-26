Run SQL Command Line <- oracle XE
> 기업에서는 toad 많이 사용

> select sysdate from dual
> edit <- sql 수정
### 개념(?)
**초기 저장 형태 - 파일**
> 검색이 힘듬
> 공유 힘듬
**ISAM(Indexed Sequential Access Method) 등장**
> 검색 속도가 빠름(데이터베이스보다도 빠름)
> 생산성이 좋지 않음(데이터 수정, 조회를 위해 프로그램 필요)
> 공유가 힘듬
**RDBMS**
> 생산성 높음
> 공유도 쉬움
> 속도 떨어짐 <- 튜닝 필요
> DB2, Oracle은 multi RW 가능
**빅데이터(하둡)**
> 메타데이터 활용
> 분석 - 머신러닝/딥러닝
**DW**
> OLAP
> 데이터 마이닝
**다양한 데이터를 수집(파이썬 활용)**
> 데이터의 종류(수치, 문자 등) 에 따라 분류
> 분포(대표값)
** 추리통계(귀무가설, 대립가설)**
> 신뢰도: P-Value
**통계적 분석**
>차이검정
>   - T-test
>     ANOVA
> 관계검정
>   - 상관분석
>   - 회귀분석
>   - 교차분석
**데이터마이닝 절차**
> 데이터 수집
> 전처리
> train/test set 분리
> X, Y값 설정
> train
> test

usbDeview
```
usbdeview /stext a.txt
```
usb 사용 기록 조회  
웹사이트 파일전송, 운영체제 파일 생성 기록과 함께 분석 가능  
BrowsingHistoryView  
브라우저 기록 조회  




### 파이썬 기초
언어를 공부할 때
1) 자료형
2) 문자열 처리 비교: 붙이기, 자르기, 변환
3) 파일입출력
4) 통신
5) IPC: Queue, Shared memort
### oracle 실습
**유저 생성**
```
Create user aiuser identified by "1234";
grant dba to aiuser;
connect aiuser/1234;
```

**테이블 생성**
```
CREATE TABLE course(
courseid varchar2(10) primary key,
coursename varchar2(100)
);
CREATE TABLE module(
moduleid varchar2(10),
modulename varchar2(200),
professor varchar2(20),
courseid varchar2(10),
constraint pkmodule primary key (moduleid, courseid)
);
CREATE TABLE schedule(
scheduleid varchar2(10),
studydate varchar2(8),
oracle - 엑셀 파일 입출력
studytime varchar2(4),
moduleid varchar2(10),
constraint pkschedule primary key (scheduleid, moduleid)
);
=========================
CREATE TABLE webcheck(
code varchar2(4),
result varchar2(2),
description varchar2(2000)
);
INSERT INTO webcheck VALUES('WA01', '0', '에러정보 노출');
INSERT INTO webcheck VALUES('WA02', '1', '다운로드 제약 없음');
INSERT INTO webcheck VALUES('WA03', '1', '웹서버 정보 노출');
```
**oracle - 엑셀 파일 입출력**
```
import cx_Oracle
import os
import pandas as pd
course = pd.read_excel('C:/Users/user/Documents/lecture/DB/test.xlsx',
sheet_name = 'course')
print(course)
print("\n")
module = pd.read_excel('C:/Users/user/Documents/lecture/DB/test.xlsx',
sheet_name = 'module')
print(module)
print("\n")
schedule = pd.read_excel('C:/Users/user/Documents/lecture/DB/test.xlsx',
sheet_name = 'schedule')
print(schedule)
print("\n")
con = cx_Oracle.connect("aiuser/1234")
cur = con.cursor()
'''
l = len(course)
for i in range(l):
sql = "INSERT INTO module VALUES ('" + course.values[i]
[0]+"','"+str(course.values[i][1])+"','"+str(course.values[i]
[2])+"','"+course.values[i][3]+"')"
print(sql)
cur.execute(sql)''' # 데이터 오라클에 입력
con.commit() # commit 미실행시 db에 반영 안됨
cur.close()
con.close()
```

**sql로 교집합 조회**
```
SELECT * FROM course a, module b, schedule c
WHERE a.courseid = b.courseid AND b.moduleid = c.moduleid;
```
NULL 값 조정 - IS (NOT) NULL  
**NVL: 치환**
```
CREATE TABLE emp(empno varchar2(10), sal number(10));
INSERT INTO emp VALUES('test1', 1000);
INSERT INTO emp VALUES('test2', 2000);
INSERT INTO emp(empno) VALUES(1000);
SELECT NVL(sal, (SELECT avg(sal) FROM emp)) FROM emp WHERE sal IS NULL;
```
#### DECODE: 얘도 치환
```
SELECT decode(empno, 'test1', 1) FROM emp;
```
### forecopy
#### $MFT 생성,
```
forecopy_handy -m .
analyzeMFT.py -f $MFT -e -o mft.csv
```
#### webcheck.txt (data)
```
WA05,A,AAAA
WA06,B,BBBBB
```
#### webcheck.ctl (sql)
```
load data
infile './webcheck.txt'
append
into table WEBCHECK
fields terminated by ','
trailing nullcols
(
code,
result,
description
)
### cmd 커맨드
```
sqlldr userid=aiuser/1234 control=webcheck.ctl log=webcheck.log
```
pandas, sklearn KMeans 알고리즘 활용 예제(matplotlib으로 visualization)
KMeans: clustering algorithm
코드 N드라이브에 있음

#### AI보안3일(8)
