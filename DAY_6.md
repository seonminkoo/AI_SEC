# DAY 6
## 안드로이드 개발과 보안

[안드로이드 앱]  
- Native  
- 하이브리드 앱  
- 웹 앱.   

모바일에서는 SQLLite 많이 씀  

1. 액티비티는 각각 다 독립  
2. 액티비티는 시스템이 관리하는데 특정 시간동안 응답없으면 죽인다.  

> SDK 파일 폴더로 만들어서 공유해서 쓰면 팀플 할 때 환경 달라서 오류나는 경우 없어짐  

> androiManiFest 파일에 모든 정보가 들어 있음  

<참고>  
평균 ---> 분산/표준편차 -> 공분산 -> 상관계수  
정규분포. : 중심극한정리.  (모집단에서 무작위 추출할 경우 추출하는 개수-회수-가 충분히 크면  
                                            그 표본은 정규분포를 따른다.)




C:\Android_SDK\Sdk\platform-tools 의 abd.exe -> 안드로이드를 제어할 수 있음(apk 설치, shell 사용 등등)
폴더 환경 변수 설정 필요!  
cmd 에서  
> abd  //abd 실행  
> adb devices //어떤 에뮬레이터 있는지  
> adb shell //에뮬레이터의 쉘로 들어감  
> exit //shell 나가기  

안드로이드는데이터를 저장할 때 public으로 되어 있음   
public 공간은 삭제해도 남아있는 것과 지워지는 것 2가지로 나뉘어짐 
private에는 인증서 같은게 저장됨!!  
1. 그런데 에뮬레이터는 동작할 수 있기 때문에 보안이 막음!!  
2. TCP dump 같은 리눅스 기반의 동작 사용할 수 있음  

#### 1. 이미지 뷰어 설치

adb 쉘 모드  

설치: adb install apk이름  
> adb install ImageViewer_app-debug.apk

> cd data  
> cd data  
> ls  
> 패키지 이름 나옴  
> 패키지 폴더로 들어가면 파일 볼 수 있음  

#### 2. TCP dump
tcpdump 파일 제공해주심  

> adb push 파일명 저장할곳(/sdcard/Download)  



