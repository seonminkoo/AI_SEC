# DAY 1

수집, 전처리, 분석(95%, 80%), 에러율,
분석값 수정,,82% 로 좋아지면 쓰는 이유O => 쓰는 이유 의미 있어야함!!!

## AI보안
악성코드 분류 => ** 챌린지 위해서 1~5과정 자동화 되어야 함!
1. 악성만들어 : 구조
2. 로그 (?), 100%
3. 로그 가져와 변환: DB -> 디비에 넣고 튜닝해가면서 원하는 형태로 변환 //성능때문에!!, DECODE, COR
4.  분석: 입력값 주어지면 출력값이 예측됨
  - 통계적분석기법
  - 기법을 고정해놓는 경우가 많은데(딥러닝, 의사결정나무) 이러면 안됨!
5. 대응: 방화벽, 파일삭제

P.1
방향성을 변수로 두면 인식률 높아짐
  1. deep
  2. 노드 -> 베르누이, 나이브 베지니안
인공지능 자동화 필수!!!
데이터 3법 : 데이터를 거래하는게 가능하다
각 병원에 진료 기록이 있으면 빅데이터 진료 센터를 만들어 각 병원의 정보 담음 -> 제약 회사에 팜
금융권의 거래 정보, 대출 정보 -> P2P 대출

P.3
AWS

P.5
## 정보시스템 구조
### 가. 네트워크 서브넷

네트워크 서브넷 공부하기
1. 기업의 네트워크 구성도 볼 수 있어야함
2. 구성도에 따라 어떻게 뚫고 나가는지 알아야 함 -> 뭘 하는지 알아야함
3. 각각의 로그가 어떻게 남는지, 어떤 정보가 남는지 알아야함 -> 업무 파악하기


ex. 대한민국 서울 강남구 => 255.255.255.0 -> 나머지는 필요하면 정해서 쓰라는 의미 없는 => 00000000 //shift 연산 위해 2의 배수만 씀
- (삼성동, 역삼동, 대치동, 논현동, 신사동 )
- 00. 01. 10. 11. => 2자리, 네트워크 id
- => 255.255.255.00

.01 ->64
10. ->128
11 -> 192 => 신사동 | <= 한남대교(Gateway)
                 방화벽 //목적지에 가기 위해 방화벽 통과해야함
                 
 
> 홈페이지에 접근하는 사용자 유형은 여러가지(ex. 사용자, 직원)  
> 기업은 세그먼트를 만들어놔야함(사용자 유형이 접근 가능한 부분과 접근 불가능한 부분으로, 인터넷존, 서버존)
> FireWall(방화벽) 놓고 웹이니깐 웹서버(Apache나 II Server) 나옴
> 그 다음에 WAS(얘는 없을 수도 있음, 세션 관리, 연결 관리)

> 서버존에는 DB 들어있음, 기업의 애플리케이션 서버 들어있을 수도
> 업무존(직원용)에는 인프라담당, 개발자, 업무 현업(ex, 총무팀)이 있음
> 개발 서버와 개발 DB 들어있는 세그먼트
> 기업은 혼자 돌아갈 수 없기 때문에 연계 시스템 필요, 대외 연계 세그먼트  
> 	서버가 하나 있어서 외부로 나감(전용선이든 뭐든), ex, 어떤 애는 보증 보험과 연계하고 어떤 애는 뉴스와 연계하고..)  
> 	이것이 FEP  

> 사용자가 방화벽을 통해 웹서버로 들어옴  
>	FireWall에서 Inbound 80 Allow 해줘서 웹서버(Apache)까지 들어옴,   
>	Apache와 WAS 연결 : 웹서버 Inbound로 연결해주기 위해 서버에 TCPWrapper 존재(10.10.10 Allow)  

> 서버존(DB)에 들어가기 위해 방화벽 Inbound 10,10,1,11(WAS ip) Allow  
>	모든 기업은 DB 앞에 방화벽 같은 DB접근제어 존재!!, DB 출입과 어디에 접근했는지 기록 남음  
> => 보안 솔루션인 1)방화벽, 2)Wrapper, 3)DB 접근제어의 로그가 필요한지 봐야함  

> WAS와 방화벽 사이에 센서하나 넣어 전기선 여러 갈래로 가능  
> 대외연계 세그먼트 안의 서버를 하나 빼서 연결 -> 4)IDS(TMS), Snort 보안 솔루션 필요!  

#### 시나리오: 업무 세그먼트의 인프라 담당자가 인터넷존의 서버에 접근해야함
	> Outbound 열려있어야 함  
	> 인프라 담당자 -> 방화벽 -> DB접근 제어 -> 서버 ;이 경로로 가면 서버 접근제어(DB접근제어)에서 막힘, why? 서버 접근 제어는 ip 확인하고 결정하는데 인프라 담당자의 ip는 Allow하지 않으므로 차단됨
	> 따라서 인프라 담당자 -> "WAS" -> 방화벽 -> DB접근 제어 -> 서버 순서로 들어가야 접근 가능

클라이언트를 가장 쉽게 공격하는 방법은 인터넷 존에 메일 서버를 하나 만들어 공격자가 메일 서버를 통해 인프라 담당자에게 악성메일 보내는 것  
대외연계는 전용선을 사용하므로 인터넷존 X  
업무 세그먼트 구간에서 나오는 모든 패킷을 잡음 -> APT 솔루션(네트워크와 컴퓨터 내부(EDR))  


#### ex. 인터파크 쇼핑몰
> 판매자가 EDI를 불러 상품 정보를 등록! EDI의 역할은 상품 정보와 이미지 정보가 있음  
> 이미지는 사이즈가 커서 DB에 넣지 않고 스토리지를 만들어서 따로 넣음 -> NFS(공유폴더)  
> 상품 조회만 하고 사지 않는 고객 있기 때문에 CDN 필요, CDN = Cache Server  
> 택배 보내기 위해 택배 회사와 연결 해야함 -> 실시간 필요X(부하걸리므로!), 데이터 묶을 때 사용하는 프로토콜인 ftp로 택배 회사와 연결 되어 있음,   
	  > ftp 들어갈 때 id/pw 필요!, id/pw는 대외 연계 세그먼트 안의 서버에 들어있음  
	  > 택배 회사 입장에서 업체 별로 전용선 열어둘 수 없기 때문에 인터넷임!!  
	  > ID/pw가 업체별로 다르므로 한 번 정의된 pw는 잘 안바뀜!!   
> 판매자가 EDI로 들어온다고치면 판매자인지 id/pw로 인증 -> id/pw 바꾸지 않음! 판매자 업체 입장에서는 받은 키를 가지고 들어가는데 이 키가 영원함  

#### ex. 인터파크 도서, 티켓, 투어
> 각 업무는 독립적, 회원 관리는 독립적! -> 쇼핑몰이 업무를 파티션하기 좋음  
> 쿠팡이 90%만 aws로 전환한 이유는 10%가 공통 업무이기 때문에 중요해서!  

웹에서 PC 프로그램 실행할 수 없음, PC용 프로그램은 웹을 조작할 수 있지만 반대는 불가! PC프로그램이 부모, 웹이 자식  

Active X          = >     Non-Active X  
익스플로러만 가능   익스플로러 이외도 가능  
설치 필요	              설치 필요  
		                    1) EXE  
		                    2) HTML5  
		     
=> 둘 다 PC용 프로그램(증명서 발급 hts, Oz Report)  
=> 공격할 때 웹만 보지 않고 피시용 프로그램을 타겟으로 하면 관점이 달라진다!  

#### 침입탐지, 악성코드 AI  
1) 침입탐지  
   - 방화벽 로그, 서버로그, DB접근제어, NAC로그, 망분리 로그  
   - PC 로그(인프라 담당자에서 어떤 일이 일어났는지)  
2) 악성코드  
   - 프로그램 정보, USB, 인터넷 다운로드, 메일 다운로드, 공유 폴더  
   - 프로그램 구조  
   - 행위(Event Driven, Message Driven)  



iptables -A INPUT -s 10.0.2.15 -j ACCEPT
 

iptables -P INPUT DROP

1) 공격탐지,대응

netsh

/etc
cd/etc
vi test.txt

vi hosts.deny

## ALL:ALL
## (프로그램이름): (IP주소)
## sshd: 10.10.10.10 만 거부하겠다는 뜻

vi hosts.allow  
sshd: 10.0.2.15(Kali_x86에서 ifconfig)  


네트워크 만들 때 1.세그먼트 -> VPC

tcpdump -i eth0 -v host www.naver.com -w test.cap  
tcpdump -i eth0 -v net 10.10 -w test.cap   
tcpdump -i eth0 -v src 10.10.10.10 and dst 20.20.20.20 -w test.cap   

snort -> 환경변수 설정까지  
snort -i 2 -c rules\test2.rule -l log  


vi /etc/sshd_config


#### test.rule
> alert icmp any any -> any any (msg:"Snort Test";sid:100001;)  
> alert tcp any any -> any any (msg:"Naver Log";content:"naver";nocase;sid:100002;)  
> threshold: type threshold, track by_dst, count 3, seconds 30;sid:100002;)  

apt-get install vsftpd 

#### ssh 
> 1) KaliLinux에서 vi etc/ssh/sshd_config 들어가서 PermitRootLogin yes
> 2) service ssh restart
> 3) Kali_x86에서 ssh 10.0.2.6(KaliLinux에서 ifconfig해서 확인한 값) 

응답에 대해서 snort 되었다.

hping 3 10.10.10.10 -S --flood --rand-source //디도스 공격 됨


### PasswordChecker, WinSpy 다운로드!!
1) WinSpy에서 Find 들어가 PasswordChecker 검색하면 Handle 나옴  
2) Style은 특성값에 불과함  
3) Messages는 전체 프로그램(PasswordChecker)의 핸들에 넘어감,   
	ex) WM_CLOSE  
	이렇게 하면 PasswordChecker 꺼질 것 같은데 안없어짐  
	메모장을 띄워서 WM_CLOSE 보내면 없어짐 -> if MSG == WM_CLOSE 이면 msg  
				PasswordChecker는 if MSG == WM_CLOSE 이면 무시 로 코딩 되어 있는 것  
edit(비밀번호 치는 창) Find해서 보면 edit에 번호쳐도 WinSpy의 text에는 아무것도 없음 -> 시각화 효과로 있는 것처럼 보여주는 것  
다시 WIN_CLOSE 보내면 edit에 쓴 번호 지워짐  
=> MessageDriven System: 날아오는 메세지에 따라 행위를 해주겠다  

MessageDriven ex2. WM_CHAR  
lParam: 1 -> 글자 1개 찍힘, 10 -> 10개 찍힘  

Injector: 변조해도 메모장은 모르기 때문에 윈도우 자체는 마음대로 변조 가능  
	


 
#### tail -f \var\log/\apache2/\access.log
#### 호출한 웹페이지, IP, 시간, 브라우저 정보, 성공 여부

 

v$sqltext  
    
금융권 인증서 SSL 사용함,  
은행권의 인증서는 보이면 안됨!, OZ_Report 할 때 이름 보이면 안됨 => Hooking 한 번이면 끝남  
              

                 
