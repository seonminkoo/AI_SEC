# DAY 4
## 

### 공부 키워드
```
chromedriver(웹 브라우저 관련 드라이버) + 셀레니움 =>  웹 페이지와 상호작용 필요할 때!!!
POM?
```

#### <참고문서>
* 실무로 배우는 빅데이터 기술 : http://www.yes24.com/Product/Goods/90367993
* 시작하세요! 데이터분석 with R : http://www.yes24.com/Product/Goods/62625013
* Machine Learning with R : http://www.yes24.com/Product/Goods/89018381
* 위키 독스 (주제로 검색) : https://wikidocs.net/
* 프로그래밍 : https://www.w3schools.com/

매일 데이터가 쌓이면 -> 로그가 쌓여 파일 배치 처리 => 시간 느려도 안정적으로 처리하면 됨, 하둡이나 분산 처리로 저장  
**실시간 데이터 처리 필요할 때**  
IOT 센서 장치 -> event monitering(실시간성) -> 실시간성 처리 필요하고 대규모 데이터이면 받는 순간 파일이 아니라 메모리로 옮겨서(MLDB?)   
이벤트가 발생하면 다른 솔루션에 넣거나, AI 적용 혹은 윈도우에 파일에 떨어지는 순간 읽어서 메모리에 읽어서 응용 서비스와 연계해서 바로 적용하고 그 다음에 데이터레이크에 넣는다  

#### service  
ex. 성적  
> 모델: 입력 데이터가 들어왔을 때 성적 분류하는것  
> 이 모델을 웹 서비스에 올리는 것  
> 데이터 시작부터 적절한 솔루션, 모델 만들고 서비스를 만드는 것!!  

## 시험!!
<빅데이터 특징>
```
* 3V : 크기( Volume), 다양상(Variety), 속도(Velocity)  => 빅데이터 레이크, 빅데이터(데이터)웨어하우스  
                                                        (빅데이터에서는 정합성보다 의미성이 더 중요한 것 같기는 함)    
   +  
 * 2V : 신실성(Veracity) => 빅데이터 마트(원천 데이터 수집 후 분석하기에 타당한(쉽고 좋은) 형태로 만들어 놓은 것)   
        시각화 (Visualization) => 인사이트(데이터의 특징 이해, 이상현상 발견, 현상 예측)   
   +   
   1V :  가치 (Value) => 비지니스의 문제를 해결 (예: 비용절감, 수익 창출)  
```
데이터 마트: 주제별로 데이터 모아놓은 것
빅데이터도 다 모아놓을 수 있지만 주제별로 필터링 할 수 있음 -> 데이터웨어하우스

----
#### <KDD 전처리>
```
결측치 :  NaN, NA  
  1) 1개 : 다른 데이터의 특징을 반영해서 적절히 추가.  
  2) N개 : 국, 영, 수 ==> 방법은 1개일 때와 동일한데, 데이터 영향도를 최소화 하기 위해.  
                         원본 데이터와 셔플링한다.  
과적합 : 모델이 학습 데이터에 너무 과하게 학습되어 일반성(품질)이 떠러지는 현상.                         
```

### 1. 수집(제일 중요!!)  
 - **데이터의 특징** : 실시간 성, 배치처리.  
 - **내부 데이터** : (1) 기간 시스템 (파일 로그, API, 시스템, RDB)    
                               => 삼바(고유폴더연결, 파일을 읽어오는거)  
                               => FTP로 특정 폴더에 업로드.  
                               => 전용 API (Web I/F 기반의 ESB, 전용 플러그인을 이용하는 EAI)    
                     (2) RDB (MySQL) : 테이블    
                               => View 테이블을 만들어서 연계.   

 - **외부 데이터** (웹 기반의 데이터)  
   (1) 외부에 위치한 시스템 : 내부 시스템과 비슷한.  
   (2) 소셜, 웹 :   
     - 크롤링, 스크라이핑 => 파이썬(urllib, beatifulsoap..), R (datafarme.read(URL)  
     - OpenAPI(ex. Naver API), Facebook API...(인증키 O)  
   (3) 공개된 데이터 셋:  
     - OpenAPI  
     - DataSet 자체를 공유 (공공, 연구)  

- **도메인별** : IoT 센서(실시간성)

### <적재/저장>
  - 대용량/실시간 처리  
  - 분산처리  
  => No-SQL(ex. 몽고디비-비정형데이터에 특화된), HDFS  
     분산처리 장점: 중복 저장이므로 노드 하나 삭제되도 O, 속도 좋음  
     분산처리 단점: 처리 과정에서 성능 떨어짐  
* 고민거리 : 데이터의 발생 속도/용량이 적재/저장 속도보다 크다면?  
    (캐시, 메시지 큐)  
    
### <처리>
 - 데이터를 선정, 변환, 통합... 절차  
  * Workflow를 관리하는 것이 필요하다. (선택)  
  
### <탐색/분석>  
- SQL (함수), R, 도구....  
-데이터마이닝 기법.

분류: 선을 그어서 나눔  
회귀: 통과하는 사람 예측?!  

### <응용>  
보고, 서비스 연계.  



  
-----
디렉토리리스팅?!  
intitle index of  
intitle index of hdd  

사이트URL/robots.txt
---
[메모]
https://www.netcraft.com/
https://www.gabia.com/
https://howsecureismypassword.net/



#### 하둡 실습


#### mongoDB 실습
bin 폴더 환경 변수 추가  

**cmd**
```
mongo  
show dbs  
use local  
show tables  
```
ex. 몽고디비 collection 만들기  
```
db.createCollection("WineList")

db.WineList.insert({WineID:'W001',WineName:'Latour',VintageYear:'1959',VintageID:0001,WineType: ({WineTypeID:'0001',WineTypeName:'WhiteWine'}),BottleType:375,Rating:'RP:96',Maturity:'Drink:Now',Price:2696,GST:2885,NewFlag:0})
```
결과  
> WriteResult({ "nInserted" : 1 })  

#### 전체 검색
```
db.WineList.find()  
```
> { "_id" : ObjectId("5f1e84f130bcb807c9d1fcda"), "WineID" : "W001", "WineName" : "Latour", "VintageYear" : "1959", "VintageID" : > > 1, "WineType" : { "WineTypeID" : "0001", "WineTypeName" : "WhiteWine" }, "BottleType" : 375, "Rating" : "RP:96", "Maturity" : > "Drink:Now", "Price" : 2696, "GST" : 2885, "NewFlag" : 0 }

#### 특정 검색
```
db.WineList.find({WineName:'Latour'})
```
