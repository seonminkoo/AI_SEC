# DAY 2

## 공격자 공격 방법 종류
  - EXE 파일
    1. 악성코드 생성도구(metasploit): 서버 백신 //서버백식은 악성코드 못잡은 경우 O
    2. DLL Injection
  - URL
  - MACRO
  - 웹 취약점
  - 모바일


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
매크로 코드에서 ** connect(s,sa, Len(sa)) 나오면 서버의 accept로  

