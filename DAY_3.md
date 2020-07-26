Run SQL Command Line <- oracle XE
> 기업에서는 toad 많이 사용

> select sysdate from dual
> edit <- sql 수정
### 개념(?) 
![day3_발전과정](https://user-images.githubusercontent.com/50771111/88477075-0e3b1680-cf78-11ea-8e4a-2d22649792e0.png)

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


![day3_시나리오](https://user-images.githubusercontent.com/50771111/88477090-3296f300-cf78-11ea-8570-87f6cffb6d59.png)

usbDeview(윈도우 레지스트리 내용 출력해놓은 것! 실시간X, 정확하지 않음) => USBDeview 텍스트 정보로 가져와야 함
```
usbdeview /stext a.txt
```
Devicename, Desc, Serial, 생성, 종료, 컴퓨터이름  
1) 시리얼 번호로 등록된 USB를 사용하지 않는사람 알 수 있음(건수)   
2) 직책별로 USB 사용 건수 알 수 있음(나이, 직무별로 USB 얼마나 쓰고 있는지 알 수 있음)  
3) 현재 상태에서 USB사용 사용내역(시리얼 번호)  

* 웹사이트 파일전송이력, 운영체제에서 파일 생성정보  
	BrowsingHistoryView와 결합하면 더 강력해짐!  
	실시간은?  WM_DEVICECHANGE //매체 정보에 대한 실시간은 가능하당!!  

a.txt를 엑셀처럼 정렬되게 변환해야함(수직->수평 변환) => 변환 프로그램 개발해야함(PLSQL)  

> 정리
> usb 사용 기록 조회  
> 웹사이트 파일전송, 운영체제 파일 생성 기록과 함께 분석 가능  
> BrowsingHistoryView  
> 브라우저 기록 조회  



### 준비
OracleXE 다운  
Run SQL 실행  
	1. connect system/1234 //로그인
	2. select * from dual
	   run
	3. edit //수정
  
### 파이썬 기초
언어를 공부할 때
1) 자료형
2) 문자열 처리 비교: 붙이기, 자르기, 변환
3) 파일입출력
4) 통신
5) IPC: Queue, Shared memort
* 생산성: 패키지(ex, PE파일 읽기)

## oracle
1) 파이썬 입력
  1-1) 파일시스템 정보를 Oracle DB에 입력
    - Master File Table: 파일 생성, 읽기, 수정 기록
    - forecopy_handy -m . , 엑셀(CSV)변환, 

  1-2) 엑셀 데이터를 Oracle DB에 입력
  1-3) USB 정보를 Oracle DB에 입력
2) 출력
  2-1) Oracle DB에서 읽어서 파이썬으로 출력
  2-2) 데이터 프레임을 출력
3) 시나리오
  > 취약점 정보를 구하고 입력, 출력, 조회(차트)
  > 파일 시스템 암호화 랜섬웨어
  > MSWord에 있는 파일 중에서 특정 단어가 있는 것을 조회하기
  > 로그 파일 분석
  
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
**DECODE: 얘도 치환**
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
```
### cmd 커맨드
```
sqlldr userid=aiuser/1234 control=webcheck.ctl log=webcheck.log
```
----
pandas, sklearn KMeans 알고리즘 활용 예제(matplotlib으로 visualization)
KMeans: clustering algorithm
코드 N드라이브에 있음



#### NULL.값 데이터 전처리 정리 뭐 의미하는지 추가하기
> select * from emp where sal is not null;  
> select nvl(sal,0) from emp where sak is null;  
> select nvl(sal, (select avg(sal) from emp)) from emp where sal  
> select avg(sal) from emp;  
> select decode(empnom, 'test1', 1) as aaa from emp;  //범주형 데이터 문자로  




#### 파이썬 서버 소켓 할 수 있는지!! 
             127.0.0.1  
    매크로 -----------> 서버(Listen)  
                     <컴퓨터이름찍힘> -------> DB에서 조회(select) ----->엑셀로 저장  
                     
SikulixIDE로 이미지 크롤링 가능  

