# 오픈소스SW개론_기말과제2

## 1. 리눅스 명령어 : top, ps, jobs, kill 명령어 조사하기

### kill: 프로세스 종료하기
응답이 없는 프로세스나 불필요한 프로세스를 강제로 종료하려면 해당 프로세스의 PID를 알아야한다. 
ps –ef나 ps aux 명령으로 프로세스의 정보를 확인하면 PID와 PPID를 알 수 있다. 
프로세스를 종료하는 데는 kill이나 pkill 명령을 사용한다. 이 명령들은 프로세스에 시그널을 보내어 프로세스를 종료한다.

- 기능 : 지정한 시그널을 프로세스에 보낸다.
- 형식 : kill [-시그널] PID


#### <시그널> 
시그널은 프로세스에 무언가 발생했음을 알리는 간단한 메시지이다. 
시그널을 받은 프로세스는 기본적으로 종료된다.
15번 시그널은 일반적으로 프로세스 종료이지만, 시그널을 무시하거나 다른 동작을 하도록 지정되어 있다면 프로세스가 종료되지 않을 수도 있다. 
kill 명령에서 시그널을 지정하지 않을 경우 15번 시그널로 간주된다. 
9번 시그널은 강제 종료이므로 무조건 종료되지만 좀비 프로세스의 경우 9번 시그널을 받아도 종료되지 않을 수 있다.

#### <주요 시그널 표>
- SIGHUP(시그널) / 1(번호) / 종료(기본처리) / 터미널과의 연결이 끊어졌을 때 발생한다.

- "SIGINT / 2 / 종료 / 인터럽트로 사용자가 Ctrl + c를 입력하면 발생한다."

- SIGQUIT / 3 / 종료,코어덤프 / 종료신호로 사용자가 Ctrl + \을 입력하면 발생한다.

- 'SIGKILL / 9 / 종료 / 이 시그널을 받은 프로세스는 무시할 수 없으며 강제로 종료된다.'

- SIGALRM / 14 / 종료 / 알람에 의해 발생한다.

- 'SIGTERM / 15 / 종료 / kill 명령이 보내는 기본 시그널이다.'

#### ~<pkill 명령으로 프로세스 종료하기>~
pkill 명령도 kill 명령과 마찬가지로 시그널을 보내지만, PID가 아니라 프로세스의 명령 이름(CMD)으로 프로세스를 찾아 종료한다. 
kill 명령과의 차이점은, 명령 이름으로 찾아 종료하므로 같은 명령이 여러 개 검색될 경우 한 번에 모두 종료된다는 것이다. 자신이 소유한 프로세스만 종료할 수 있다.

****

### jobs: 현재 실행중인 작업 목록 보기

- 기능 : 백그라운드 작업을 모두 보여준다. 특정 작업 번호를 지정하면 해당 작업의 정보만 보여준다.
- 형식 : jobs [%작업 번호]
- %작업 번호 
- %번호 : 해당 번호의 작업 정보를 출력한다.
- %+ 또는 %% : 작업 순서가 +인 작업 정보를 출력한다.
- %- : 작업 순서가 –인 작업 정보를 출력한다.
- 사용 예 : jobs , jobs %1


항목
출력 예 
의미
작업 번호
[1]
작업 번호로서 백그라운드로 실행 할 때마다 순차적으로 증가한다.([1],[2],[3], ...)
작업 순서
+
작업 순서를 표시한다.
+ : 가장 최근에 접근한 작업
- : + 작업보다 바로 전에 접근한 작업
공백 : 그 외의 작업
상태
실행중
작업 상태를 표시한다.
- 실행중 : 현재 실행 중이다.
- 완료 : 작업이 정상적으로 종료되었다.
- 종료됨 : 작업이 비정상적으로 종료되었다.
- 정지됨 : 작업이 잠시 중단되었다.
명령
sleep 
100 &
백그라운드로 실행 중인 명령이다.

 [표 : jobs 명령의 출력 정보]


● 작업 전환하기


명령
기능
Ctrl + z 또는 stop[%작업번호]
포그라운드 작업을 정지한다.
bg [%작업 번호]
작업 번호가 지시하는 작업을 백그라운드 작업으로 전환한다.
fg [%작업 번호]
작업 번호가 지시하는 작업을 포그라운드 작업으로 전환한다.

[표 : 작업 전환 명령]

현재 포그라운드로 실행 중인 작업을 백그라운드로 전환하려면 우선 Ctrl + Z로 작업을 정지해야한다. 


● 로그아웃 후에도 백그라운드 작업 계속 실행하기

nohup
   - 기능 : 로그아웃 한 후에도 백그라운드 작업을 계속 실행한다.
   - 형식 : nohup 명령& 

nohup 명령을 사용할 때는 반드시 백그라운드로 실행해야 한다. 별도로 출력 방향 전환을 하지 않으면 명령의 실행 결과와 오류 메시지가 현재 디렉터리에 nohup.out 파일로 자동 저장된다.


