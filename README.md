# 오픈소스SW개론_기말과제2


## 1. 리눅스 명령어 : kill, job, ps, top 명령어 조사하기


### kill: 프로세스 종료하기
___응답이 없는 프로세스나 불필요한 프로세스를 강제로 종료하려면 해당 프로세스의 PID를 알아야한다.___
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

|SIGINT|2|종료|인터럽트로 사용자가 Ctrl + c를 입력하면 발생한다.|
|***|***|***|
|SIGQUIT|3|종료,코어덤프|종료신호로 사용자가 Ctrl + \을 입력하면 발생한다|
|'SIGKILL|9|종료|이 시그널을 받은 프로세스는 무시할 수 없으며 강제로 종료된다.|

- SIGALRM / 14 / 종료 / 알람에 의해 발생한다.

- `SIGTERM / 15 / 종료 / kill 명령이 보내는 기본 시그널이다.`

#### ~<pkill 명령으로 프로세스 종료하기>~
pkill 명령도 kill 명령과 마찬가지로 시그널을 보내지만, PID가 아니라 프로세스의 명령 이름(CMD)으로 프로세스를 찾아 종료한다. 
kill 명령과의 차이점은, 명령 이름으로 찾아 종료하므로 같은 명령이 여러 개 검색될 경우 한 번에 모두 종료된다는 것이다. 자신이 소유한 프로세스만 종료할 수 있다.

****

### top: 실시간으로 CPU 사용률을 체크해주는 도구
윈도우의 작업관리자와 비슷하며 리눅스를 사용하는 서버의 성능이나 현재 돌아가고 있는 상황을 볼 때 사용한다.

- 사용법: 리눅스에서 top 명령어를 치고 들어가면 된다. `root#>top`
<img width="642" alt="Screen Shot 2022-06-04 at 11 19 46 PM" src="https://user-images.githubusercontent.com/104884485/172006253-406082bc-3d6f-4653-a10b-c7da86563b01.png">
주황색은 현재 시스템의 상태를 보여주는 것이고, 시스템 시간이나 uptime 등등을 전체적으로 확인할 수 있다. 
또한 밑에 있는 빨간색 화면은 현재 실행하고 있는 프로세스 현황을 볼 수있다.

__top 정보 시스템 내용___
13:47:52: 현재 서버의 시간
9 days, 5:24: uptime(켜져있는 시간)
3 users: 유저
Load average: 현재 시스템이 얼마나 일을 하고 있는지 1분,5분, 15분 단위로 실행/대기 중인 프로세스 수를 나타내고 있음
Tasks : 프로세스 개수

__CPU__
- %us: 유저레벨에서 사용하고 있는 CPU 비중
- %sy: 시스템 레벨에서 사용하고 있는 CPU 비중
- %id: 유휴 상태의 CPU 비중
- %wa: 시스템의 I/O 요청을 처리하지 못한 상태에서의 CPU idle 상태인 비중

KiB Mem, Swap: 각 메모리의 상태 정보

#### 프로세스 상태 정보



****
### jobs: 현재 실행중인 작업 목록 보기

- 기능 : 백그라운드 작업을 모두 보여준다. 특정 작업 번호를 지정하면 해당 작업의 정보만 보여준다.
- 형식 : jobs [%작업 번호]
- %작업 번호 
  - %번호 : 해당 번호의 작업 정보를 출력한다.
  - %+ 또는 %% : 작업 순서가 +인 작업 정보를 출력한다.
  - %- : 작업 순서가 –인 작업 정보를 출력한다.
  -  사용 예 : jobs , jobs %1

#### <jobs 명령의 출력 번호>
항목 / 출력 예 /의미

작업 번호(항목) / [1](출력 예) / 작업 번호로서 백그라운드로 실행 할 때마다 순차적으로 증가한다.([1],[2],[3], ...)

작업 순서 / + / 작업 순서를 표시한다. + : 가장 최근에 접근한 작업  - : + 작업보다 바로 전에 접근한 작업  공백 : 그 외의 작업

상태 / 실행중 / 작업 상태를 표시한다. - 실행중 : 현재 실행 중이다. - 완료 : 작업이 정상적으로 종료되었다. - 종료됨 : 작업이 비정상적으로 종료되었다. - 정지됨 : 작업이 잠시 중단되었다.

명령/ sleep 100 & / 백그라운드로 실행 중인 명령이다.

#### <작업 전환 명령>
Ctrl + z 또는 stop[%작업번호] (명령) / 포그라운드 작업을 정지한다. (기능)
bg [%작업 번호] / 작업 번호가 지시하는 작업을 백그라운드 작업으로 전환한다.
fg [%작업 번호] / 작업 번호가 지시하는 작업을 포그라운드 작업으로 전환한다.

현재 포그라운드로 실행 중인 작업을 백그라운드로 전환하려면 우선 Ctrl + Z로 작업을 정지해야한다. 

#### <로그아웃 후에도 백그라운드 작업 계속 실행하기>
nohup
nohup 명령을 사용할 때는 반드시 백그라운드로 실행해야 한다. 
별도로 출력 방향 전환을 하지 않으면 명령의 실행 결과와 오류 메시지가 현재 디렉터리에 nohup.out 파일로 자동 저장된다.
- 기능 : 로그아웃 한 후에도 백그라운드 작업을 계속 실행한다.
- 형식 : nohup 명령& 

****

### ps: 현재 실행중인 프로세스의 정보를 출력한다.

#### <옵션>

  _<유닉스 옵션>_
  - e : 시스템에서 실행 중인 모든 프로세스의 정보를 출력한다.
  - f : 프로세스의 자세한 정보를 출력한다.
  - u uid : 특정 사용자에 대한 모든 프로세스의 정보를 출력한다.
  - p pid : pid로 지정한 특정 프로세스의 정보를 출력한다.
  
  _<BSD 옵션>_
  - a : 터미널에서 실행한 프로세스의 정보를 출력한다.
  - u : 프로세스 소유자 이름, CPU 사용량, 메모리 사용량 등 상세 정보를 출력한다.
  - x : 시스템에서 실행 중인 모든 프로세스의 정보를 출력한다.
  
  _<GNU 옵션>_
- -pid PID 목록 : 목록으로 지정한 특정 PID의 정보를 출력한다.
- 유닉스 옵션 : 묶어서 사용할 수 있고 붙임표로 시작한다.(예 : -ef)
- BSD 옵션 : 묶어서 사용할 수 있고 붙임표로 시작하지 않는다.(예 : aux)
- GNU 옵션 : 붙임표 두 개로 시작한다.(예 : --pid)


#### 프로세스의 상세 정보 출력하기 : -f 옵션
예 ) ps –f

__표 : ps –f의 출력 정보__
- UID(항목) / 프로세스를 실행한 사용자 ID (의미)
- STIME / 프로세스의 시작 날짜나 시간
- PID / 프로세스 번호 
- TTY / 프로세스가 실행된 터미널의 종류와 번호 
- PPID / 부모 프로세스 번호 
- TIME / 프로세스 실행 시간
- C / CPU 사용량 (%값)
- CMD / 실행되고 있는 프로그램 이름(명령)

#### 터미널에서 실행한 프로세스의 정보 출력하기 : a 옵션
출력 내용 중 STAT는 프로세스의 상태를 나타낸다.
예 ) ps a

__표 : STAT에 사용되는 문자의 의미__

- 문자 /의미 / 비고
- R(문자) / 실행 중 (의미)
- S / 인터럽트가 가능한 대기상태
- T / 작업 제어에 의해 정지된 상태
- Z / 좀비 프로세스
- STIME / 프로세스의 시작 날짜나 시간 
  - s / 세션 리더 프로세스 (BSD형식)
  - + / 포그라운드 프로세스 그룹(BSD형식)
  - l(소문자L) / 멀티스레드(BSD형식)


#### 터미널에서 실행한 프로세스의 상세 정보 출력하기 : a 옵션과 u 옵션
예 ) ps au


항목
의미
항목
의미
USER
사용자 계정 이름
VSZ
사용 중인 가상 메모리의 크기
%CPU
퍼센트로 표현한 CPU 사용량
RSS
사용 중인 물리적 메모리의 크기
%MEM
퍼센트로 표현한 물리적 메모리 사용량
START
프로세스 시작 시간

[표 : ps au의 출력 정보]



● 전체 프로세스 목록 출력하기(유닉스 옵션) : -e 옵션
예 ) ps –e | more , ps –ef | more

TTY의 값이 ?인 것은 대부분 데몬으로 시스템이 실행한 프로세스이다.
-e 옵션을 실행하면 출력 내용이 위로 스크롤 되어 프로세스 목록을 제대로 확인하기 어렵다. 따라서 출력 결과를 페이지 단위로 확인하려면 | 와  more 이나 less 명령을 함께 사용해야 한다.
전체 프로세스의 더 자세한 정보를 확인하려면 -e 옵션과 –f 옵션을 함께 사용해야 한다.(-ef)
TTY가 ?인 프로세스는 사용자 ID가 root이고, 스레드는 CMD에 []로 표시하여 구분한다.




● 전체 프로세스 목록 출력하기(BSD 옵션) : ax옵션
예 ) ps ax | more , ps aux | more

ax 옵션은 –e 옵션과 마찬가지로 시스템에서 실행 중인 모든 프로세스를 출력한다.
aux 옵션은 –ef 옵션처럼 시스템에서 실행 중인 모든 프로세스에 대한 자세한 정보를 출력한다.







● 특정 사용자의 프로세스 목록 출력하기 : -u 옵션
예 ) ps –u user1 , ps –fu user1

-u 옵션은 특정 사용자가 실행한 프로세스의 목록을 확인할 수 있다.
더 상세한 정보를 보고 싶다면 –f 옵션을 함께 사용한다.



● 특정 프로세스 정보 출력하기 : -p 옵션
예 ) ps –p 4882 , ps –fp 4882

-p 옵션과 함께 특정 PID를 지정하면 해당 프로세스의 정보를 출력할 수 있다. 이때 –f 옵션을 함께 사용하는 것이 좋다.





2.2 특정 프로세스 정보 검색하기


● ps 명령으로 특정 프로세스 정보 검색하기
예 ) ps –ef | grep bash



● pgrep 명령으로 특정 프로세스 정보 검색하기

pgrep   
   - 기능 : 지정한 패턴과 일치하는 프로세스의 정보를 출력한다.
   - 형식 : pgrep [옵션] 패턴
   - 옵션 
      -x : 패턴과 정확히 일치하는 프로세스의 정보를 출력한다.
       -n : 패턴을 포함하고 있는 가장 최근 프로세스의 정보를 출력한다.
       -u 사용자명 : 특정 사용자에 대한 모든 프로세스를 출력한다.
      -l : PID와 프로세스 이름을 출력한다.
      -t term : 특정 단말기와 관련된 프로세스의 정보를 출력한다.
   -사용 예 : pgrep bash



pgrep의 경우 –l 옵션을 지정해도 PID와 명령 이름만 출력된다.
더 자세한 정보를 검색하려면 pgrep 명령을 ps 명령과 연결해서 사용해야 한다. pgrep 명령으로 검색하려는 프로세스의 PID를 찾고 ps 명령으로 자세한 정보를 확인하는 것이다.
예) ps –fp $(pgrep –x bash)


만약 리눅스 시스템에서 동시에 여러 사용자가 실습하고 있을 때 위처럼 검색한다면 다른 사용자가 실행한 bash가 모두 검색될 것이다. 이때 –u옵션으로 사용자명을 지정하면 그 사용자의 프로세스 정보만 검색할 수 있다.
예) ps –fp $(pgrep –u user1 bash)

****


