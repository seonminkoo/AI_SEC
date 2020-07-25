


## Driven by download: 자동으로
1) 구축
  1-1) Tag 찾기, Script 실행
   - bmp, gif(동적-> 로직이 들어간다), png, jpg
   http://150.95.210.127/boan/
   
2) 로그 찾기
                      여기까지 할거임
-----------------------------------
3) 분석

1-1)
### 이미지에 넣어
#### HxD
boangisal.jpg
> 두번째 줄 03 04 자리에 00 FF //확장헤더의 위치가 있는 자리, 여기에 숫자 넣으면 크기 커짐!! 작아질 수는 없음

boangisa_hack.jpg
> 확장 헤더 자리에 */ 있음 //   /*  */있어도 파일 깨지지 않음
> 헤더의 크기가 커짐 -> 길이를 맞춰주기 위해 AAA... 넣어준 것
> AAA... 끝나는 다음 부분에 다른 이미지 코드 넣어 이미지 바꿀 수 있음
> 어떤 이미지가 들어있어도 프로그램 실행에는 상관X
> 프로그램은 AAA.. 이전의 코드까지임! 자바스크립트

자바 스크립트를 실행하는 태그를 찾아야 하는데
AA.jpg

#### 인터넷에서(http://150.95.210.127/boan/) 이미지 우클릭 -> 소스 보기
> <image src = "boangisa_hack.jpg"></image> <br><br>    //크롬은 안됨, 익스플로러 9이상 안됨

#### HxD
boan_hack의 http://150.95.210.127/boan
virtual Box ip:    192.168.43.39/bbbbb
> 자리수가 다르므로 디렉토리 이름으로 자리수 맞춘 후 저장 => virtual box 이미지로 바뀜
> 이미지 공유 디렉토리에 넣기

#### kali_x86
> cd /var/www
> mkdir bbbbb
> cd bbbbb
이 디렉토리에 이미지 복사해서 넣어야 함<br>
bbbbb 폴더 안에 넣으면 서버 안에 들어온 것 

인터넷에서(http://150.95.210.127/boan/) boan_hack의 소스 보기해서 소스 전체 복사
> vi test.html 에 소스 붙혀넣기
**여기까지 웹은 뜬 것**
확인:
> service apache2 start
> service mysql start
익스플로러에서<br>
http://192.168.43.39/bbbbb/test.html
