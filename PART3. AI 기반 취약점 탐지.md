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

