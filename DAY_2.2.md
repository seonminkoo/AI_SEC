
![day2_2](https://user-images.githubusercontent.com/50771111/88466051-756ab380-cf03-11ea-972a-a8c19b88ad2f.jpg)

#### 매크로
윈도우만 WSAStartup 해줌!! -> 드라이버를 올림<br>
매크로 코드에서 ** connect(s,sa, Len(sa)) //연결, 서버의 accept로 도착<br>
send(s, comName(0), buf, 0) // 서버의 read로 보내줌

그렇다면 소켓이 뭐가 다른가
  문제는 동시에 여러명이 열면 어떻게 되는가
  => fork()로 멀티프로세싱 처리

### DLL 만들기
> 비주얼 스튜디오(파이썬으로 못만듦)
> 뉴 프로젝트 -> 콘솔 응용 프로그램 체크, DLL 체크로 생성
> DLL 생성
> message box(NULL, "Hello", "Test", MB_OK); //"Hello"라는 메세지 박스를 불르라는 의미

DLL은 혼자 프로그램 돌아갈 수 X
#### cmd
> rundll32.exe DLL파일 경로, main // rundll32.exe 라는 보안 프로그램이 있음, 강제적으로 부모 만들어주는 것!(가짜 main 생성)
=> **DLL Injection은 코드(메세지 박스)를 다른 프로그램에 삽입해서 실행될 수 있게 하는 것?!

### DLL Injection
DLL을 쓰는 프로그램 만들고 싶음
> LoadLibrary("testdll.dll"); //정상 프로그램에서 dll 쓸 수 있음
testdll.dll이 11자리이기 때문에 실행시키고 싶은 다른 프로그램에 11자리만큼 자리 만들어놔야함
> virtualAlloc(공간 할당할 프로그램의 pid, 자리수), ex. virtualAlloc(10, 11) //남의 프로그램에 11byte의 공간 할당됨 **=> x** <br>
> writeMemory(x, "내용(ex.aaa)"); //남의 메모리에 aaa 써짐 **=> y**
> GetProcAddress("&L-); //주소얻음 **=> z**
>CreateREmoteThread(z, y);

* 사용 함수
  - VirtualAllocEx
  - WriteProcessMemory
  - GetModuleHandle <- loadlibrary
  - GetProcAddress
  - RemoteThread





**snapshot**

#### cmd
>tasklist //프로그램의 pid 리스트
>tasklist | findstr 11772(pid) 

#### strings 프로그램
**cmd**
> strings injector.exe >a.txt //실행파일에 있는 문자열들이 a.txt에 들어가게 됨



## Driven by download: 자동으로 다운로드
1) 구축
  1-1) Tag 찾기, Script 실행
   - bmp, gif(동적-> 로직이 들어간다), png, jpg
   http://150.95.210.127/boan/
   
2) 로그 찾기
3) 분석


### Drive by download: 자동으로 다운로드
실습
tag, script 실행
확장 헤더
주석 활용
AA.jpg를 자바스크립트로 실행한다.
```
img태그로는 script는 실행되지 않는다. script태그에 jpg파일을 넣어야 실행됨
js파일 내부에 txt파일을 불러와 저장된 스크립트 다운로드
```
활용 ex
> nc이용 윈도우 쉘 연결
> 프로그램 강제 실행

1-1)
#### 이미지에 넣어
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
