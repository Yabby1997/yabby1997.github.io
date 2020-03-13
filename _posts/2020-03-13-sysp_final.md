---
layout: post
title: "📚 시스템 프로그래밍 기말고사 정리"
description: "2019년 2학기 전공 시스템프로그래밍 기말고사 공부 정리"
date: 2020-02-14
feature_image: images/thumbnail_sysp.png
tags: [CNU, System Programming, C, Final]
---

# 기말고사대비

# 9

컴퓨터의는 반복적으로 인스트럭션을 실행함. jump, branch, call, return 등의 제어 흐름의 변경이 있지만 그렇게 instruction에 정의되지 않은 예외적인 흐름이 발생하면 그걸 처리해줄 매커니즘이 필요하다. 

**하위 : 예외 → 시스템 이벤트에 대한 반응. 제어의 흐름 변경. HW, OS SW 함께 사용**

**상위 : 문맥전환 → OS SW, HW타이머로 구현?**

**상위 : 시그널 → OS SW로 구현** 

상위 : nonlocal 점프 → 할일 없을거라고 했었음. 

### 예외상황

특정 이벤트가 발생하면 그에대한 반응으로 제어가 OS커널로 옮겨감. 유저프로그램은 제어권을 잃게된다. ( divide by zero, arithmetic overflow, page fault, ctrl + c, 등)

커널은 OS에 자리를 차지하고있는 코드라고 보면됨. 유저가 짠 코드에서 OS에 미리 짜여진 어떤 코드로 제어가 넘어간다.

예외가 처리되면 원래위치로 돌아가거나 원래위치의 다음으로돌아가거나 종료됨.

### 예외테이블

각 이벤트에 해당하는 예외번호로 만들어진 예외테이블이 있다. 예외번호 k가 exception table의 index가 됨. 즉 k번째 예외 발생시 예외 테이블의 k 번째가 호출되는거. 이걸 인터럽트 벡터라고 부름.

비동기형 예외 : **ASYNCHRONOUS EXCEPTION : INTERRUPT** → 외부사건으로부터 시그널이 발생, 핸들러실행 후 다음 수행해야했을 명령어로 복귀한다.  **처리후에 현재, 다음, 종료 3가지옵션**

입출력 인터럽트 : ctrl + c, z, 네트워크패킷입력, 디스크섹터읽기

하드리셋, 소프트리셋(ctrl+alt+delete) 인터럽트 **우리가 다룬건 인터럽트?/turn call**

동기형 예외 : traps, faults, aborts **SYNCHRONOUS** **EXCEPTION**

**traps** : 의도적인 명령어의 결과로 발생 : systelcall, breakpoint, special instruction **처리 후 다음으로** 간다

**faults** : 정정할 수 없는 에러로 발생 : page fault, protection fault, floating point exception. fault일으킨놈 다시실행(**현재로 돌아감**)하거나 복구불가이면 abort

page fault : 가상 메모리가 하드디스에 위치해서 접근이 안될때 발생함. page handler가 하드디스크에있는 page를 메모리에 로드해줘야하는데 이 때 발생하는 오류. 이거는 정정이 가능함 따라서 다시 시도한다. 

**aborts** : 복구 불가 에러. 하드웨어 오류같은거. 패리티에러, 시스템체크에러 등. 복귀불가능이니까 종료하는수밖에..

### 시스템 콜

아주 아랫단에서 이뤄져야 하는일들을 해주는 함수? syscall instruction은 syscall 번호를 %rax에서 가져오고 그 외에 다른 인자들은 rdi, rsi, rdx, r10, r8, r9를 사용함 리턴도 rax에 되니까 다른 call이랑 다른건 syscall 번호가 rax에 들어간다는거.

### 프로세스

논리적인 제어의 흐름. CPU와 메모리를 독점하는것 처럼 보이지만 여러 프로세스가 나눠가져간다.  → 컴퓨터는 존1나게 빠르다. 서로 교대로 실행됨. **주소공간은 가상메모리로 관리?이건무슨소리냐**

앞서말했던 문맥전환이 여기서 사용되는것. CPU와 메모리처럼 자원을 노나쓴다.  스위치할때 레지스터에 있던걸 메모리에 저장해서 비워두는거임. 왜냐면 다른 프로세스가 들어와서 똑같은 공간을 써야하니까? 현대에서는 멀티코어 프로세서기때문에 여러개의 CPU가 돌아가잖아? 그래서 메인메모리는 공유하고 CPU에 각기 레지스터랑 처리하는 회로가 따로있으니까 ㅇㅇ 그렇게돌아감.

### 동시성 프로세스 (concurrent)

각 프로세스는 앞서말했듯 끊어지면서 실행됨(그렇게 안보이지만) 따라서 여러 프로세스들이 드문드문 실행되는데, 이렇게 여러 프로세스의 실행 시간이 서로 중첩되는 경우를 동시에 실행된다(concurrent)고 한다. 

[The difference between concurrent and parallel processing](https://youtu.be/N6JZAf3BJgU)

parallel은 멀티코어인 경우를 말하는거. concurrent하게 parallel로 돌아갈 수 있는거지 

프로세스가 서로 overwrap되어야함. 21페이지에 B랑 C가 concurrent가 아닌 이유는 **B가 끝난 이후에 C가 실행되기 때문에 서로 겹치지 않아서야!!!!**

연습문제 1
A 0~2, B 1~4, C 3~5이므로 A와 B, A와 C, B와 C가 overwrap된다. concurrent 하다.

위 슬라이드에서는 뚝뚝 끊어지는거처럼 보이게 해놨지만 실제로 이건 존나빠르게 문맥전환이 이뤄지기때문에 사용자 관점에서는 그냥 서로 병렬적으로 실행된다고 볼 수 있음. **멀티태스킹, 타임슬라이싱**

### **문맥전환 (context switch)**

프로세스는 운영체제의 커널이 관리함 아까 앞에서 사용자에서 커널로 제어가 옮겨간다고 한거랑 같은듯. 커널은 프로세스가 아니다??? **유저프로세스의 일부분으로 실행**

프로세스가 다른 프로세스로 넘어가는게 문맥전환

하나의 유저프로세스에서 다른 유저프로세스로 넘어갈 때 운영체제의 커널이 사용되는것???? 즉 유저프로세스(유저코드)-문맥전환(커널코드)→유저프로세스(유저코드) 라고 볼 수 있을 듯.

### 프로세스 제어

`pid_t getpid()` : 함수 호출한 프로세스의 pid 반환

`pid_t getppid()` : 함수 호출한 프로세스의 부모 pid를 반환 

int fork() : 부모와 동일한 자식 프로세스를 만듬. 자식과 부모가 분기했으므로 부모의경우와 자식의경우가 반환값이 다른데, 부모의 경우는 자식의 pid를 리턴받고, 자식의경우는 0을 리턴받는다. 0을 받으면 끝단이고 0이아니면 그게 자식 pid라고보면될듯.

    void fork1(){
    	int x = 1;
    	pid_t pid = fork();        //fork를 통해 부모 자식 분기!
    	if(pid == 0){              //분기했으므로 흐름은 두가지인데, 0인경우는 자식인경우
    		printf("child has x = %d\n", ++x);    //자식의 x 는 1 더해져서 출력이므로 2 출력
    	}
    	else{
    		printf("parend has x = %d\n", --x)    //부모의 x 는 1 빼져서 출력되므로 0 출력
    	}
    	printf("bye from process %d with x = %d\n", getpid(), x);    //부모와 자식 모두 끝날땐 이걸 만나기때문에 부모와 자식 pid와 x값이 각각 한줄씩 두줄 출력될것
    }

child has x = 2
parent has x = 0
bye from process "parent pid" with x = 0
bye from process "child pid" with x = 2

    void fork2(){
    	printf("L0\n");
    	fork();
    	printf("L1\n");
    	fork();
    	printf("Bye\n");
    }

fork를 했지만 반환을 안받은것 뿐, 동일하다.
{% include image_caption.html imageurl="/images/mogakko_9_1.png" title="" caption="수행결과" %}

{% include image_caption.html imageurl="/images/sysp_final/_2019-11-22__9.21.02.png" title="" caption="" %}

    void fork3(){
    	printf("L0\n");
    	fork();
    	printf("L1\n");
    	fork();
    	printf("L2\n");
    	fork();
    	printf("Bye\n");
    }

시점이 완벽하게 맞춰질수는 없는듯.

{% include image_caption.html imageurl="/images/sysp_final/_2019-11-22__9.29.53.png" title="" caption="" %}

    void cleanup(void){
    	printf("cleaning up\n");
    }
    
    void fork6(){
    	atexit(cleanup);        //atexit함수로 exit시에 실행할 함수 등록
    	fork();
    	exit(0);
    }

fork로 분기했으니까 2개의 흐름이있고 결국엔 exit하니까 cleanup이 두번 불릴거같은데?

{% include image_caption.html imageurl="/images/sysp_final/_2019-11-22__9.33.17.png" title="" caption="" %}

정확합니다!

    //연습문제2
    
    int main(){
    	int x = 1;
    	if(fork() == 0)
    		printf("printf1 : x = %d\n", ++x);
    	printf("printf2 : x = %d\n", --x);
    	exit(0);
    }

자식은 두줄출력 부모는 한줄출력으로
printf1 : x = 2
printf2 : x = 1
printf2 : x = 0

{% include image_caption.html imageurl="/images/sysp_final/_2019-11-22__9.38.09.png" title="" caption="" %}

{% include image_caption.html imageurl="/images/sysp_final/_2019-11-22__9.40.56.png" title="" caption="" %}

실질적으로 종료되었는데 부모에서 정리가 안된게 좀비
결국 실질적인 정리는 부모가 다 해줘야함. 
정리안하면 init process가 정리해주긴 하는데, 장기간 동작하면 좀비 컨트롤을 잘해줘야함. 서버같은것들은 이런거에 취약하겠죠?

    void fork7(){
    	if(fork() == 0){
    		printf("terminating child pid = %d\n", getpid());
    		exit(0);
    	}
    	else{
    		printf("running parent pid = %d\n". getpid());
    		while(1)
    	}
    }

{% include image_caption.html imageurl="/images/sysp_final/_2019-11-22__10.00.49.png" title="" caption="" %}

안끝난다. while loop이 무한 loop라 끝나지 않는건 이해가되는데, 그냥 부모가 안끝나니까 안죽는거 아닌가..? ← ㅇㅇ 아닌듯 보니까 프로세스가 살아있음 parent는 loop에 빠져 자식을 정리를 안해줘서그런건가?

부모가 정상종료가 안되어서 자식은 종료되었는데도 남아있는거 그래서 defunct. 부모죽이면 같이죽음 

    void fork8(){
    	if(fork() == 0){
    		printf("running child pid = %d\n", getpid());
    		while(1);
    	}
    	else{
    		printf("terminating parent pid = %d\n", getpid());
    		exit(0);
    	}
    }

자식이 종료되지 않아 발생하는 좀비 프로세스 
부모는 죽었는데 자식은 계속돌아가고있따. pid로 확인가능 

{% include image_caption.html imageurl="/images/sysp_final/_2019-11-22__10.10.47.png" title="" caption="" %}

# 10

종료된 프로세스(자원을 모두 반환한 프로세스)라고해도 부모가 제거해줄때까지 종료된 상태로 남아있게 된다(자원은 반환했어도 PID는 점유하는 상태) 부모가  자식을 청소 안하면 init으로 청소해야함. 그러다 부모가 종료되면 종료된 자식들은 커널이 알아서 init으로 청소해준다. 명시적으로 제거하려면 **wait**함수를 쓴다.

`int wait(int *child_status)` : 현재 프로세스의 자식 중 하나 종료시까지 대기. 반환값은 종료된 자식의 pid, child_status에 종료시킨 시그널 저장, 커널이 pid제거 해줌 → 명시적인 삭제

    void fork9(){
    	int child_status;
    	if(fork() == 0){
    		printf("HC : hello from child\n");
    	}
    	else{
    		printf("HP : hello from parent\n");
    		wait(&child_status);
    		printf("CT : child has terminated\n");
    	}
    	printf("bye\n");
    	exit();
    }

자식 부모 분기해서 HC랑 HP중 뭐 하나 먼저실행되고 HC→HP or HP→HC
HC종료 bye
그다음 CT→bye될듯

{% include image_caption.html imageurl="/images/sysp_final/_2019-11-22__10.38.11.png" title="" caption="" %}

정확하다이거야~~~~~

    void fork10(){
    	pit_t pid[10];
    	int i;
    	int child_status;
    	for(i = 0; i < 10; i++)
    		if((pid[i] = fork()) == 0)
    			exit(100 + i);
    	for(i = 0; i < 10; i++){
    		pid_t wpid = wait(&child_status);
    		if(WIFEXITED(child_status))
    			printf("child %d terminated with exit status %d\n", wpid, WEXITSTATUS(child_status));
    		else
    			printf("child %d terminate abnormally\n", wpid);
    	}
    }

10 개 다 정상종료될거같은데..?
WIFEXITED가 정상종료를 확인하는거. wait함수로 기록한걸 기반으로 정상 종료를 확인한다. wait을 통해 종료된거니까 정상종료 맞지
**exit함수에 어떤 값이 인자로 들어왔냐가 또 WEXITSTATUS로 반환받아지네... 몰랐던 사실..!**

{% include image_caption.html imageurl="/images/sysp_final/_2019-11-22__10.46.10.png" title="" caption="" %}

    void fork11(){
    	pit_t pid[10];
    	int i;
    	int child_status;
    	for(i = 0; i < 10; i++)
    		if((pid[i] = fork()) == 0)
    			exit(100 + i);
    	for(i = 0; i < 10; i++){
    		pid_t wpid = waitpid(pid[i], &child_status, 0);
    		if(WIFEXITED(child_status))
    			printf("child %d terminated with exit status %d\n", wpid, WEXITSTATUS(child_status));
    		else
    			printf("child %d terminate abnormally\n", wpid);
    	}
    }

{% include image_caption.html imageurl="/images/sysp_final/_2019-11-22__10.56.13.png" title="" caption="" %}

특정 pid 를 기다리게 했으므로 배열 순으로, 즉 배열에 만들어진 자식 순으로 제거가되겠지. 그래서 위에거는 걍 지들맘대로 순서없이 제거됐지만 여기서는  순서대로 제거된거다 이말이야~

    //연습문제 1. waitpid
    
    int main(){
    	if(fork() == 0){
    		printf("a");
    	}
    	else{
    		printf("b");
    		waitpid(-1, NULL, 0);
    	}
    	printf("c");
    	exit(0);
    }

a→c(자식), b→c(부모) 두가지 줄기가 나온다. 근데 부모에서 모든 자식이 끝날 때 까지 기다리니까 부모가 끝나기 전에는 자식이 다 끝나야함
a, b, c(자식) → c(부모) 가 되어야 할듯. 따라서 가능한 출력은
a b c c
a c b c
b a c c
~~b c a c~~ **불가능하지 왜냐면 b만나오고 a는 한번도 안나온상태에서 a를 종료시킬 수는 없으니까 마지막 c 가 자식의 c가 되어버림 따라서 위의 3가지경우만 가능**

{% include image_caption.html imageurl="/images/sysp_final/_2019-12-07__12.22.47.png" title="" caption="" %}

{% include image_caption.html imageurl="/images/sysp_final/_2019-11-22__11.05.12.png" title="" caption="" %}

왠진 모르겠지만 acbc만 나온다. 암튼 뭐 맞는듯

    //연습문제 waitpid2
    
    void waitpidtest2(){
    	int status;
    	pid_t pid;
    	
    	printf("hello\n");
    	pid = fork();
    	printf("%d\n", !pid);
    	if(pid != 0){
    		if(waitpid(-1, &status, 0) > 0){
    			if(WIFEXITED(status) != 0)
    				printf("%d\n", WEXITSTATUS(status));
    		}
    	}
    	printf("bye\n");
    	exit(2);
    }

hello
1(자식)
0(부모)
bye(자식 exit)
2(부모에서 자식 종료 대기 후 exit status 출력)
bye(부모 exit)

{% include image_caption.html imageurl="/images/sysp_final/_2019-11-22__11.13.05.png" title="" caption="" %}

어케맞췄노 시ㅂ련아

unsigned int sleep(unsigned int secs) : secs 초 만큼 정지시킴. 정상종료 → 다 기다리고 끝나면 0리턴, 그외에 중도정지면 남은 secs가 반환된다.
`int pause(void)` : signal을 받을 때 까지 쳐 잔다. 

    //연습문제 3 sleep
    
    unsigned int snooze(unsigned int secs){
    	unsigned int actuallSlept = sleep(secs);
    	printf("Slept for %u of %u secs.\n", secs - actuallSlept, secs);
    }

중단시에 중단되면서 몇초까지 sleep 했는지 출력하는게 핵심인거같은데, 쉘환경에서 실행해야하는건지 잘안됨. 

{% include image_caption.html imageurl="/images/sysp_final/_2019-11-23__10.01.57.png" title="" caption="" %}

tsh 에서 해도 마찬가지인데???

`int execve(char *filename, char **argv[] char **envp[])` : filename을 환경변수 envp를 이용해 인자 argv로 실행함. argv배열이 code, data, stack을 덮어씌운다. 이게 아마 문맥전환인것같음. argv랑 envp는 둘다 문자열이니까(?) 널문자로 끝나는 포인터 배열. 에러발생시 리턴됨. 

### 쉘 shell

쉘에서 입력은 루프에서 처리한다. 게임엔진이랑 똑같음. 무한루프 돌면서 입력을 받는 부분이 main에 있다. utility와 built-in 명령어를 입력받아서 실행하는 것. builtin은 쉘에서 지원하는 명령어들을 이야기하는거고 유틸리티는 설치된거, 쉽게생각하면 외부 프로그램이라고 보면 된다!!!

job : shell에서 명령어를 통해 생성된 프로세스. FG와 BG 모두 job이고 그룹으로 나눌수도있다. 리눅스에서 foreground작업에 IO가 집중되는데 이를 멈추는건 control+Z로 sittstp 시그널을 보내는거고 이렇게 정지가된걸 다시 실행하려면 sigcont로 하면된다고 함. FG로 할지 BG로 할지도 결정 가능. 일반적으로 프로세스는 그냥실행시키면 포어그라운드로 돌아감. 백그라운드로 돌리고싶으면 &을 추가하면된다. 

sleep 도 쓸 수 있다???

eval 함수에서는 커맨드를 받아 그 커맨드가 빌트인인지, 외부 유틸리티인지를 확인해서 처리하는데 이 때 자식프로세스를 만들어 자식이 수행하도록 함. 부모에서는 만약 자식이 수행하는게 포어그라운드면 제어권을 자식이 가져갔으므로 기다려줘야하고, 그게아니라면 백그라운드이므로 부모가 제어를 계속 유지한다. 

쉘은 포어그라운드가 종료되면 알아서 제거를 해주지만, 백그라운드잡은 그렇지 않다. ←좀비프로세스가 남아서 메모리의 누수가 발생하고 궁극적으론 커널 메모리 부족까지 일으킬 수 있음. signal을 통해 제거해준다.

# 11

시그널 : 어떠한 이벤트가 시스템에 발생했음을 전달하는 메시지

예외상황과 인터럽트를 커널에서 정수 ID로 추상화한것(1~30) 시그널을 보내주면 프로세스가 이를 받아 처리하는데, 전달되는 시그널정보는 시그널 ID와 전달유무. 이를 바탕으로 프로세스가 시그널을 처리하도록 한다. 

- divide-by-zero : SIGFPE
- terminate ctrl+c : SIGINT
- 자식이 종료됨 : SIGCHLD

시그널을 받으면 프로세스는 그에대한 반응을 해야함. **무시, 종료, 캐치**가있다.

**캐치** : 유저수준의 함수인 시그널 핸들러가 시그널에 대응한다. 

대기 : 전송했지만 수신하지 않은 시그널. 동일한 시그널은 하나만 대기할 수 있음.  비트벡터를 사용해서 대기 시그널들을 나타낸다.???

블럭 : 특정 시그널을 거절시킬 수 있음. SIGKILL은 거절할 수 없다. 불가항력

커널이 프로세스 컨텍스트에 pending, block 비트 벡터를 가지고 대기시그널과 블럭시그널들을 표시해둔다. pending 벡터의 k번째 비트가 1이면 k번째 시그널이 대기중이라는 뜻임. sigprocmask를 통해서 block해봤잖아? 그거처럼 시그널 블럭도 이뤄짐

프로세스그룹 : 프로세스들은 각기 프로세스 그룹에 속해있다. 자식은 부모의 복제이기때문에 자식은 기본적으로는 부모와 같은 그룹에 속함. 바꿔주고자하면 임의로 바꾸는것도 가능하다. 

`getpgrp()` : 프로세스그룹 리턴

`setpgid()` : 프로세스 그룹 설정

`kill -9 pid or pgid` : pid혹은 pgid에 해당하는 프로세스를 죽임

### 시그널 보내기

SIGINT SIGTSTP시그널이 발생하면 **포어그라운드 프로세스 그룹의 모든 작업으로 시그널이 전달된다.** 

유저입력으로 시그널발생 →shell에서 시그널캐치→포어그라운드그룹으로 시그널전달

그룹이 다같이 종료/중지된다 이거야~

    void fork12_original(){
        pid_t pid[10];
        int i, child_status;
        for(i = 0; i < 10; i++)
            if((pid[i] = fork()) == 0)
               while(1);
        for(i = 0; i < 10; i++){
            printf("killing process %d\n", pid[i]);
            kill(pid[i], SIGINT);                       //10개 순회하며 SIGINT보낸다.
        }
        for(i = 0; i < 10; i++){
            pid_t wpid = wait(&child_status);           //자식 종료되면 그 pid 받아오고 뭐로 종료되었는지 기록
            if(WIFEXITED(child_status))                 //종료시그널을 보니 SIGINT로 종료된거라면 출력 즉 이것도 10번 출력될듯
                printf("child %d terminated with exit status %d\n", wpid, WEXITSTATUS(child_status));
    	      else
                printf("child %d terminated abnormally\n", wpid);
        }
    }

{% include image_caption.html imageurl="/images/sysp_final/_2019-11-23__2.03.31.png" title="" caption="" %}

내 예상
killing process 0000
child 0000 terminated with exit status 2
위와같은거 10번 반복

틀렸다. 사실 틀린게 맞음. 정상종료 아니면 비정상종료임. 할거 다하고 끝나야 (exit) 정상종료잖아? 그니까 이건 비정상종료가 뜬다. 만약 내가 원하는대로하려면 아래처럼..

    void fork12_configured(){
        pid_t pid[10];
        int i, child_status;
        for(i = 0; i < 10; i++)
            if((pid[i] = fork()) == 0)
               while(1);
        for(i = 0; i < 10; i++){
            printf("killing process %d\n", pid[i]);
            kill(pid[i], SIGINT);                       //10개 순회하며 SIGINT보낸다.
        }
        for(i = 0; i < 10; i++){
            pid_t wpid = wait(&child_status);           //자식 종료되면 그 pid 받아오고 뭐로 종료되었는지 기록
            if(WIFEXITED(child_status))                 //종료시그널을 보니 SIGINT로 종료된거라면 출력 즉 이것도 10번 출력될듯
                printf("child %d terminated with exit status %d\n", wpid, WEXITSTATUS(child_status));
            else if(WIFSIGNALED(child_status))
                printf("child %d terminated with SIGINT signal %d\n", wpid, WTERMSIG(child_status));
            else
                printf("child %d terminated abnormally\n", wpid);
        }
    }

{% include image_caption.html imageurl="/images/sysp_final/_2019-11-23__1.55.23.png" title="" caption="" %}

signal을 통해 종료된 경우, WIFSIGNALED를 통해 출력하도록 하게하면 됨. WTERMSIG를 통해서 종료시킨 시그널 번호도 얻어올 수 있따.

커널이 예외처리에서 프로세스로 다시 넘어올때 pending돼었고 block되지 않은것들을 계산한다. (대기중이고 블락되지않은것) 만약 없다면(0이라면) 그냥 제어를 프로세스에 넘겨줘버리면 되지만, 그렇지 않다면 1인 비트를 찾아가 그 비트에 해당하는 시그널을 수신해서 그 시그널에대해서 처리를 진행함. 이걸 반복해서 pnb가 0이되도록 만들어야 대기중인 시그널이 모두 처리되는거지. 이게 다 해결되면 프로세스에 돌려주면된다. 

디폴트 액션 : 각 시그널들은 기본적으로 사전 정의된 동작들이 있음. 

- 프로세스 종료
- 종료 후 core dump
- 정지 (SIGCONT입력시까지)
- 무시

signal()함수를 통해 기본동작도 수정이가능하다 SIGSTOP이랑 SIGKILL은 막을수없는 예외임

{% include image_caption.html imageurl="/images/sysp_final/_2019-11-23__2.12.38.png" title="" caption="" %}

우리 과제에서 Signal함수를 통해 해당 signal의 기본 동작을 각 핸들러로 돌린것을 확인할 수 있음. 저거 주석처리하고한거랑 주석 다시 풀고 한거 밑에 비교
우리과제에서 이렇게 하는 이유는 이미 기본동작 default action이 정의되어있는데 그걸 그대로 따라가면 쉘이종료가되어버린다. 그니까 그걸 막으려고 이렇게 재정의해준거!

{% include image_caption.html imageurl="/images/sysp_final/_2019-11-23__2.14.14.png" title="" caption="" %}

`handler_t *signal(int signum, handler_t *handler)` : signum에 해당하는 시그널에 대해 핸들러를 handler(주소에 위치한 핸들러)로 변경해준다.
handler 부분에 `SIG_IGN`이 들어가면 시그널 무시, `SIG_DFL`이 들어가면 기본 시그널로 다시 복귀시킨다..!? 테스트해봅시다.

{% include image_caption.html imageurl="/images/sysp_final/_2019-11-23__2.23.55.png" title="" caption="" %}

{% include image_caption.html imageurl="/images/sysp_final/_2019-11-23__2.23.39.png" title="" caption="" %}

각기 핸들러를 새로 정의해준거 뒤에다가 SIG_DFL로 다시설정해주니 보다시피 주석 제거한거랑 똑같이, 즉 핸들러 지정 안한거랑 똑같이 작동한다. 

signum에 해당하는 시그널 수신하면 handler에 해당하는 핸들러로 제어가 이동하게한다 (catching, 처리한다) 핸들러가 return 을 만나면 제어권은 시그널이 발생하며 중단되었던 원래의 프로세스로 넘어가는것~~

    void int_handler(int sig){
    	printf("process %d received signal %d\n", getpid(), sig);
    	exit(0);
    }
    
    void fork13(){
    	pid_t pid[10];
    	int i, child_status;
    	signal(SIGINT, int_handler);    //int_handler로 보내버렷~~
      for(i = 0; i < 10; i++)
    	  if((pid[i] = fork()) == 0) 
    	    while(1);
    	for(i = 0; i < 10; i++){
    		printf("killing process %d\n", pid[i]);
        kill(pid[i], SIGINT);                       //10개 순회하며 SIGINT보낸다.
      }
      for(i = 0; i < 10; i++){
    	  pid_t wpid = wait(&child_status);           //자식 종료되면 그 pid 받아오고 뭐로 종료되었는지 기록
    	    if(WIFEXITED(child_status))                 //종료시그널을 보니 SIGINT로 종료된거라면 출력 즉 이것도 10번 출력될듯
    	      printf("child %d terminated with exit status %d\n", wpid, WEXITSTATUS(child_status));
    	    else
            printf("child %d terminated abnormally\n", wpid);
        }
    }

{% include image_caption.html imageurl="/images/sysp_final/_2019-11-23__3.05.06.png" title="" caption="" %}

좀 애매하긴한데 아무튼 예상대로 결과가 나오기는 함

### **시그널 핸들러 이상동작????? 뭐가문제인지 잘 이해 안됨.**

    int ccount = 0;
    
    void child_handler(int sig){
    	int child_status;
    	pid_t pid = wait(&child_status);
    	ccount--;
    	printf("received signal %d from process %d\n", sig, pid);
    	sleep(2);
    }
    
    void fork14(){
    	pid_t pid[10];
    	int i, child_status;
    	signal(SIGCHLD, child_handler);
    	for(i = 0; i < 10; i++){
    		if((pid[i] = fork() == 0){
    			sleep(1);
    			exit(0);
    		}
    	}
    	while(ccount > 0){
    		pause();
    	}
    }
     

{% include image_caption.html imageurl="/images/sysp_final/_2019-12-04__12.34.39.png" title="" caption="" %}

{% include image_caption.html imageurl="/images/sysp_final/_2019-12-04__12.46.39.png" title="" caption="" %}

위 WNOHANG사용, 아래 sleep만 사용
자식들의 종료가 계속해서 발생하는데 handler는 한번에 하나의 시그널만 처리가 가능하고 대기 큐가 따로 없기때문에 시그널 처리에 헛발질이 발생한다. 
`waitpid(-1, &child_status, WNOHANG)` 에서 -1옵션을 줘서 아무 자식프로세스나 기다리게하고 WNOHANG을 통해 종료할거없을땐 즉각끝내게해서 자식프로세스 종료시 즉각 처리, 부모는 다른일을 바로한다. `while(ccount > 0) pause();` 를 통해서 부모는 다시 시그널이 발생하기를 기다려주면서 보조를 맞춘다. 

반면 `wait(&child_status)`는 맹목적으로 자식이 끝나는 걸 기다린다 **자식이 끝날때까지 부모가 블락된다.** 즉 자식이 종료될 때 까지 다른걸못하므로, 자식은 우후죽순으로 끝나는데 핸들러가 대응해줄수가 없다. → 실질적으론 종료됐지만 처리가 안되는 좀비프로세스가 생기게된다. 

**(자식)—시그널—>(커널)               (부모)
(자식)←-종료——(커널)—전달-→(부모)** 
자식완전종료까지 기다리는게 wait. 그동안 종료되는 다른 자식들은 커널이 정상정으로 종료시키지만 부모가 다른 자식 종료를 기다리기때문에 부모에서 전달이 안된다. 즉 핸들러에서 처리가 안됨. waitpid와 WNOHANG을 써주면 커널이 자식을 종료시키는걸 따로 기다리지 않기때문에 즉각즉각 처리가되고 부모가 어디에 붙잡히지않으니까 정상적으로 다 처리됨.

    //내부 발생 이벤트 처리 프로그램
    
    int beeps = 0;
    
    void bombHandler(int sig){
    	prinf("BEEP\n");
    	fflush(stdout);
    	
    	if(++beeps < 5)
    		alarm(1);
    	else{
    		printf("BOOM!\n");
    		exit(0);
    	}
    }
    
    int main(){
    	signal(SIGALRM, bombHandler);
    	alarm(1);
    	while(1);                          //while이 여기없으면 main이 죽어버리면서 끝남 
    }

{% include image_caption.html imageurl="/images/sysp_final/_2019-11-23__5.03.33.png" title="" caption="" %}

위에서 본 것처럼 핸들러가 처리하는 과정에서 또다시 인터럽트로 핸들러로 갈 수 있다. 여기서는 재귀식으로 처리했지만, 다른 핸들러로 가는것도 마찬가지로 가능함. 모든 핸들러가 종료되면 메인으로 돌아가서 실행됨.

묵시적 블록 : 커널은 현재 처리중인 시그널과 동일한 타입의 대기 시그널은 블록 → 진행중인 시그널이랑 같은게 들어오면 묵시적으로 블록된다

명시적 블록 : sigprocmask함수를 이용해 명시적으로 블록할 수 있다.

`int sigprocmask(int how, const sigset_t *set, sigset_t *oldset)` : how → SIG_BLOCK : blocked에 set을 추가, SIG_UNBLOCK : blocked set에서 set을 제거, SIG_SETMASK : blockedset이 set

`sigemptyset()` : 빈 시그널 집합 생성

`sigfillset()` : 집합의 시그널을 모두 1로 설정

`sigaddset()` : 집합에 특정 시그널을 1로

`sigdelset` : 집합에 특정 시그널을 0으로 

pdf 24페이지 참조

시그널 핸들러를 작성하는건 존나 까다롭다. 지맘대로 툭툭 튀어나오는거니까 힘들수밖에.. 언제 어디서 시그널이 수신될지 직관적이지 않고 시스템마다 처리방법이 다른데 메인과 함깨돌아가면서 전역변수들도 맘대로갖다씀 그래서 문제가됨.

    pid_t pid;
    int counter = 2;
    
    void handler1(int sig){
    	counter = counter -1;
    	printf("%d, counter);
    	fflush(stdout);
    	exit(0);
    }
    
    int main(){
    	signal(SIGUSR1, handler1);
    
    	printf("%d", counter);
    	fflush(stdout);
    
    	if((pid = fork()) == 0){
    		while(1){};
    	}
    	kill(pid, SIGUSR1);
    	waitpid(-1, NULL, 0);
    	counter = counter + 1;
    	printf("%d", counter);
    	exit(0);
    }

출력을 212로 예상했으나 213이나왔다. 근접했지만 한가지 놓친게 
부모 자식이 분기하면서 counter는 모두 2로 가져간다. 따라서 자식이 handler를 통해 1로떨어진게 출력된 후 부모에서 1증가한다고해서 2 - 1 + 1이되는게아니라 2, 2-1, 2+1이 되는것!

{% include image_caption.html imageurl="/images/sysp_final/_2019-11-27__1.48.12.png" title="" caption="" %}

race condition : concurrent한 프로그램에서 여러 프로세스가 동시에 하나의 자원에 접근하면 문제가 발생할 수 있다. 따라서 동시진입의 가능성을 제거해주어야 한다. 

27페이지랑 29페이지에서 차이는 emptyset을 만들어 시그널 넣어주고 fork 전후에 block unblock을 끼워준거

### race condition

동일한자원에 동시접근하면 문제가 생긴다. ((pid = fork()) == 0) 이라고하면 pid가 fork 될 때 concurrent하면 0인친구들이 많아져서 동시접근문제가 생길수있따? 

전역변수에 접근할땐 시그널을 블럭시킨다. 

마찬가지로 포크할때도 시그널을 블럭해준다.

그냥뭔가 race condition이 발생할 수 있는 부분의 전후로 블락을해줘야하나?

{% include image_caption.html imageurl="/images/sysp_final/_2019-11-29__12.40.44.png" title="" caption="" %}

{% include image_caption.html imageurl="/images/sysp_final/_2019-11-29__12.40.57.png" title="" caption="" %}

포크하고 addjob되는거보다 자식이 먼저 실행될수도있는데, 자식이 만약 실행되어서 바로 중지되어버리면 addjob이 실행되기도 전에 , 즉 global 변수인 job list에 추가되기도전에 제거가 수행될 수 있다. concurrent한 컴퓨터에서 여러 프로세스들이 동시에 수행되다보니 이런 경주문제가 발생할 수 있는 것. 그래서 자식이 실행될때는 signal의 영향을 받지 못하게하는것!
SIGCHLD핸들러에서도 delete전후로 마스크해준다.

{% include image_caption.html imageurl="/images/sysp_final/_2019-11-29__2.16.34.png" title="" caption="" %}

{% include image_caption.html imageurl="/images/sysp_final/_2019-11-29__2.16.22.png" title="" caption="" %}

main에서 child가 fork 되어나오면 pid는 0이고 수행이된다음에 SIGCHLD가 불리면, (이 이전까지는 블락되어있어서 문제가 안된다) handler에서 pid를 종료된 pid, 즉 0이 아닌값으로 바꿔놓게되고, 바꿔놓게 되는순간 부모프로세스에서 while문이 중단되면서 다음 행으로 넘어가게되는것. 이런식으로 명시적으로 시그널을 기다리게할 수 있다. 

{% include image_caption.html imageurl="/images/sysp_final/_2019-11-29__8.24.11.png" title="" caption="" %}

{% include image_caption.html imageurl="/images/sysp_final/_2019-11-29__8.31.45.png" title="" caption="" %}

suspend를 사용하는편이 pause를 쓰는것보다 낫고(race가 발생하지 않는다), sleep을 쓰는거보다 빠르다. 

시그널은 프로세스 수준의 예외방법. 유저가 따로 정의할수도 있지만. 오버헤드가 커서 성능에 좋지않고, 큐가아니므로 시그널이 대기할수없다. 또 concurrent한 컴퓨터 개념이므로 시그널간에 발생하는 race에 주의해야함.

# 12

메모리는 무한하지 않고, 참조버그는 치명적이며, 성능은 일정하지않다(그래보이지만)

프로그램은 코드를 통해서 대략적으로 메모리 사용량을 알 수 있지만, 런타임에서 크기가 결정되는 것들의 메모리는 동적으로 할당되어야한다. 

동적으로 메모리가 필요하면 malloc으로 heap에 할당하고 free를 통해 해제한다.  자바는 가비지컬렉터가 안쓰는거같으면 알아서 수거해가는 방식

스택은 위에서 RSP가 내려오는 방식이지만 힙은 밑에서 BRK가 올라가는 방식이다. 

(밑) text→data→bss→heap→libraries→stack→user code(kernel virtual memory)

스택에서 push pop하듯 malloc free를 하고, 공간이 부족할경우는 brk에 값을 더해서 크기를 확보한다. 그게 sbrk함

`malloc` : 입력된 사이즈만큼의 블록 포인터 반환. 8바이트에 맞춰줌

`realloc` : 입력된 블록포인터의 사이즈를 바꿈.

`free` : 할당된 메모리 포인터를 받아서 제거함
메모리는 워드단위로 할당된다. 1차원 메모리배열에서 하나는 한워드임 

비어있고 공간이 충분하면 거기에 새로할당한다. 중요한건 할당이랑 반환이 모두 일련의 순서가 있는게 아니라는것. 요청될 때마다 처리되므로 순서가 없다. 

요청의 순서를 버퍼에 저장해순서를 바꿀 수 없음 따라서 할당 당시에 프리메모리 중 공간이 충분한곳에 할당해야함 ← 또 **맞춤요건에 만족해야함 8바이트 정렬한다.** 

공간이용률과 시간성능 모두 좋은 malloc 과 free가 좋은것이다. 단편화의 발생을 최소화 하는 방향으로 가야함 

시간성능, 처리량 throughput을 높이기위해서는 malloc, realloc, free의 처리시간이 최소화가 되어야함. 

또 지역성을 확보해야한다. 시간적으로 인접한건 공간적으로 인접해 가져다 사용하기에 유리해야함. 

견고성 : 유효한 포인터에 대해수행되는지 확신할 수 있어야함. (체크할 수 있어야 함)

메모리 한 블럭의 크기 안에 헤더, 페이로드, 패딩이 들어간다. 그거 다 합친 사이즈가 블럭의 사이즈가 되는 것.

연습문제 1
헤더4바이트, 블록 크기 8배수
malloc(1) → 헤더 4 + 페이로드 1 = 5 인접한 8배수는 8 따라서 패딩 3바이트, 블록헤더는 사이즈 8 + 할당 1 = 9 → 0x9
malloc(5) → 헤더 4 + 페이로드 5 = 9, 인접한 8배수는 16. 따라서 패딩 7바이트, 헤더는 사이즈 16 + 할당 1 = 17 → 0x11
malloc(12) → 헤더 4 + 페이로드 12 = 16, 인접 8배수는 16, 따라서 패딩 없음, 헤더는 사이즈 16 + 할당 1 = 17 → 0x11
malloc(13) → 헤더 4 + 페이로드 13 = 17, 인접 8배수는 24, 따라서 패딩 7바이트, 헤더는 사이즈 24 + 할당 1 = 25 → 0x19

간접리스트로 메모리가 구성될 때 free 블럭을 찾는 방식은 크게 3가지가 있다.

first fit, 최초할당 : 처음부터 일일히 검색해서 맨처음 크기가 맞는곳에 할당하는 방법 → 모든 블럭을 순회해야하므로 모든 블럭에 비례한 상수시간이 걸림. 이렇게하면 문제가 **시작부분에 작은 조각이 다수 발생할 수 있다는데, 이해가 안됨**

    p = start;
    while((p < end) && ((*p & 1)||(*p <= len)))    //p가 메모리 끝보단 작고, 할당이 되어있거나, 들어가기에 사이즈가 너무 작다면 할당 못한다. 
    	p = p + (*p & -2);                           //현재 위치의 사이즈만큼 더해주면 다음위치로 이동한다. 

현재 블럭의 주소가 가리키는곳은 블럭의 첫부분으로, 블럭의 사이즈와 할당 여부를 기록하고있다. 따라서 1과 and 해주면 할당여부를, -2혹은 -7과 and 해주면 사이즈를 얻어올 수 있음. 

next fit , 다음할당 : 이전 검색이 종료된 위치부터 검색을 한다. 기본적인 방식은 first fit과 동일하다고 볼 수 있다. 

best fit : 최적할당 : 리스트 검색하며 가장 근접한 크기를 선택한다. 가장 근접한 크기의 free블락을 선택하기때문에 단편화를 가장 적게해주지만 탐색에 시간이 오래걸리므로 느리다. 

헤더매크로

`#define SIZEMASK (~0x7)` 7을 not연산하면 1111 1000이 나옴. 이거로 and 연산해서 사이즈 얻어오는데 쓴다.

`#define PACK(size, alloc ((size)|(alloc)` size에 alloc여부를 쓰는듯 

`#define getSize(x) ((x)->size & SIZEMASK)` size와 SIZEMASK의 and 연산을 통해 사이즈를 얻어온다.

    struct{
    	unsigned allocated : 1;
    	unsigned size : 31;
    }Header;

사실 이건 뭐하는건지 잘 모르겠다...

할당을 free시키는 과정이 더 까다롭다. 할당했는지 나타내는 allocate 비트만(할당플래그) 0으로 표시해서는 문제가 해결되지 않는다. free 블럭 두개가 연속한경우 공간이 있지만, 두 블럭으로 나뉘어있기 때문에 탐색시 찾지 못하게 되는것. 

free가 발생하면 앞 뒤의 free블럭이있다면 연결해주면 된다.

    void free_block(ptr p){
    	*p = *p & -2;
    	next = p + *p;        //p의 주소에 p의 사이즈를 더함, 다음 블럭
    	if((*next & 1) == 0)  //next의 할당이 0, 즉 free라면
    		*p = *p + *next;    //p와 next의 사이즈 정보를 합쳐서 p에 통합
    }

그치만 우리가 앞서 사용해왔던 방식의 간접 리스트는 헤더만 존제하기때문에 앞의 블럭은 확인할 수 없다. 앞의 블럭도 확인해주기 위해선 양방향 연결이 필요하다.

양방향 연결 : 경계 태그 Boundary Tags

리스트를 역방향으로 따라갈 수 있도록 하기 위해 추가적인 공간을 소모해 footer를 만들어준다. (헤더와 내용은 동일함)

헤더와 푸터에대해서 조금 부연설명
헤더는 사이즈정보와 할당정보를 모두 가지고있다. 원랜 free할때도 주소랑 사이즈를 받아야하는데, 헤더와 푸터에 메타정보를 같이 기록할 것이기 때문에 별도의 사이즈는 안받아도 되는거임. 그리고 푸터는 헤더랑 똑같은 정보인데 굳이 넣는이유가 앞에서 앞 블럭과 합치는게 불가능했던걸 해결하려면 앞으로 탐색할 필요도 있기 때문. 
현재 블럭 헤더 -4 하면 앞 블럭 푸터
현재 블럭 푸터  +4 하면 뒷 블럭 헤더

allocated→free→allocated

allocated→free→free     →    allocate→free

free→free→allocated     →    free→allocated

free→free→free               →    free

# 13

블럭할당 정책 : 3가지 할당 정책이 있지만(first fit, next fit, best fit) 세가지 모두 아주 이상적인건 없었다. 처리량과 단편화의 최소화 사이에서의 절충이 요구됨. 

블럭을 나누는 방식과 통합하는 방식이 세부적으로 좀다름. 언제 블럭을 나눌거고 얼마나 단편화되는걸 허용할것인지부터, 언제 통합할건지 (free시 바로 통합되는게 앞서 본것이고, 특정 시점, 예컨대 **단편화의 양으로 판별**하는것과 **검색시 통합**하는 등을 기준으로 시간을 지연시키는것도 있다.

간접리스트는 앞서 보았듯 선형적인 시간을 갖는다. (공간 복잡도는 어떤 할당 정책을 선택했느냐(first fit, next fit, best fit)에 따라 달라진다. 

실제 malloc/free구현에서는 사용하지 않고 특수경우에만 사용함

**그래도 앞서 구현한 블럭 나누기, 경계태그로 free 블럭 합치기는 universal하게 이용됨**

연습문제 1. 메모리 블록 설계
single word allign : 
allocated, header, footer ⇒ 최소 1 + 4 + 4 = 9, 인접 4배수 12 → padding 3, 최소크기 12
free, header, footer ⇒ 최소 0 + 4 + 4 = 8, 인접 4배수 8 → padding 0, 최소크기 8
double word allign :
allocated, header ⇒ 최소 1 + 4 = 5 인접 8배수 8 → padding 3, 최소크기 8
free, header, footer ⇒ 최소 0 + 4 + 4 = 8, 인접 8배수 8 → padding 0, 최소크기 8

### 직접리스트

간접리스트에서는 블럭의 크기를 이용해 메모리 블럭을 이동하는 방식으로 연결리스트를 이뤘다.

직접리스트는 가용 블록 내부에 포인터를 두어서 일반적인 연결리스트와 더 유사하게 사용한다. 

이전에는 할당된 리스트도 관리했지만 직접리스트에서는 사용 가능한 **가용 블록의 리스트만을 관리** 한다. 그리고 다음 가용블럭은 할당된 블럭에 의해 실실제로 연결되어있지 않기때문에 간접 리스트에서처럼 **블럭사이즈로 순회할수없다. 실제 연결리스트처럼 포인터 주소를 활용해야함.** 

블록 연결은 경계태그를 사용한다. 가용블럭의 데이터영역에 안쓰는 부분을 활용해서 pred와 succ의 주소를 저장하는데 사용, 실제 블럭들의 순서와는 무관하게 주소로 연결되어있는 구조가된다. 

free들의 pred와 succ를 통해 탐색하며 사이즈를 확인할 수 있고, 그를 통해 할당해줄 수 있다. 할당해주게되면 splitting을 통해 짤라주고, 잘려져 나온 새로운 가용 블럭에 대해서 주소작업을 다시해주면 된다. 

- 반환된 블럭은 가용리스트의 맨 앞에 넣어준다. LIFO방식을 취함(스택) 상수시간만 필요하지만, 단편화가 심하다
- 가용블럭의 주소를 기준으로 정렬되도록 삽입하면됨. 단점으론 리스트를 탐색하여 주소에 알맞은 공간에 넣어줘야한다는것. 그치만 단편화는 덜하다.

Explicit(직접리스트) LIFOdd : free가 발생하면 free된 가용공간이 맨 앞 노드가된다. 이전의 노드는 새로운 맨 앞 노드(방금 free된 친구)의 다음 노드가 된다. 

모든 블럭 수가 아닌 가용 블럭수에 성능이 비례하므로 메모리가 많이 차있는 상태에 성능이 빠른편이다. (가용블럭이 적을수록 빠르다는거). 블록을 리스트에 추가 제거하는 작업이 복잡함. 주소를 기반으로 연결리스트가 구현되므로 블록마다 링크포인터의 저장을위해 2워드씩 추가로필요함. → 이것도 실제로 쓰이진않고 구분가용리스트랑 같이쓰임

**구분가용리스트** :  블록들이 유사한 크기의 가용블럭들끼리 따로 리스트를 만든다. (클래스를 구분한다) 이렇게하면 필요한 크기에 알맞는 블럭을 찾는데 유리하니까 탐색의 속도가 빠르다. 또 알맞은 블럭을 찾았을 때 가용 공간을 자르는데도 유리하다. 안자를 확률이 꽤 높기때문. 

그치만 사이즈별로 리스트가 따로있다보니 여러 리스트를 관리해야한다는 trade off는 피할 수 없다. 

**블록할당방법**

n사이즈의 리스트가 비어있지 않다면 리스트의 첫블록으로 할당해준다. 간접방식 직접방식 모두 사용가능

그리고 만약 n사이즈의 블럭을 찾는데 리스트가 비어있다면 가용 블럭이 없는거니까 새 페이지를 할당받아온다 (sbrk로 힙을 늘린다) 그 페이지의 모든블럭으로 가용리스트를 만들고 위에서한것처럼 첫 블록으로 할당해준다. 

**블록반환방법**

가용블럭으로 만들고 해당 클래스의 가용리스트에 추가

**장점**

가용리스트 기준으로 찾으니까 알맞는걸 빨리찾을 수 있다. 그래서 매우 빠른 상수시간의 처리시간을 가지며 메모리이용률도 좋음

{% include image_caption.html imageurl="/images/sysp_final/_2019-11-30__10.07.24.png" title="" caption="" %}

malloc은 payload의 시작점을 반환한다..?

# 14

링킹 ← 리눅스에서는 ld

코드와 데이터를 다른 부분에 저장하고 실제 하나의 파일처럼 동작하도록 메모리에 로딩되도록 하는것. **컴파일시, 런타임시**에 수행될 수 있다.

하나의 프로그램을 분할하여 작성할 수 있게되어 커다란 프로젝트에서 일부 수정이 발생했을 때 전체를 컴파일할 필요 없이, 수정된 부분만 컴파일되면 된다. 

sum은 sum에서는 정의가되어있고 main에서는 선언과 호출이 되고 있다. 

여러 파일의 소스코드가 각자 컴파일된다. 이를 relocatable이라고 함 

relocatable들을 엮어서(링크해서) 실행가능한(executable)한 파일을 만들어내는것이 링커의 역할

장점 1. 여러 파일에 소스를 나누어 작성 가능함

**시간적으로 효율적**이다 : 수정이 없는 파일들을 **재 컴파일 할 필요는 없다**.  수정된거만 컴파일하고 다시 링크해주면 된다.

여러 파일을 동시 컴파일 할 수 있다.

**공간적으로 효율적**이다 : 자주 쓰는 기능을 모아놓을 수 있다. 

**정적링킹 :** 컴파일타임에 사용하는 라이브러리 코드를 미리 적재함

컴파일코드 안에 필요한 라이브러리 코드가 모두 들어가있어서 어케보면 좋을 수 있는데, 다른 프로그램이 동일 코드를 실행하면 문제가 심각해짐. 동일한 기능을 하는 코드가 **메모리에 중복 적재**된다. 

**동적링킹 :** 런타임에 그때그때 사용하는 라이브러리 코드를 적재함?

컴파일코드 안에 라이브러리를 포함시키지 않고 필요한 라이브러리는 그 때 그 때 가져다 쓰는 것. 위의 메모리 **중복 적재가 해결**됨. 

단점이라 한다면 바로바로 갖다쓸수있는게 아니라 공유된 라이브러리 부분을 찾아가야하니 약간의 오버헤드가 발생한다는 점. 그러나 공간적인 측면에서 정적링킹은 너무나도 손해다. 모든 프로그램이 아닌 라이브러리만 따로 업데이트될 수 있는 이유이기도 함. 

장점 2. 라이브러리를 통해 코드 재활용성을 늘린다. 

### 심볼 해석

기본적으로 소스를 쪼개서 사용하므로 서로 다른 파일에 정의된 심볼들을 참조 및 관리해야한다. 링커는 이 심볼들을 정의 참조한다. 

오브젝트 파일의 **심볼테이블에 심볼 정의** : 이름과 크기, 위치정보를 갖는다.  심볼 해석 시 링커가 심볼테이블을 통해 심볼을 참조하도록 해준다. 

### 재배치

하나의 프로젝트가 여러개의 오브젝트 파일로 쪼개어져 있는 형태이다. 이런 데이터와 코드 영역을 하나로 합쳐야 실행할 수 있다 (executable). 각 o파일이 relocatable인 이유가 얘네들을 재배치해서 executable하게 만들어줘야 하기 때문임. 따라서 각 **o파일 (relocatable)에서의 심볼 위치가 executable에서의 최종적인 위치로 재배치되어야함.** 

오브젝트파일 ELF

relocatable : .o 다른 relocatable과 연결될 코드와 데이터영역들. .c로부터만들어짐

shared : .so DLL이라고 보면 됨. 특수케이스. 필요할 때 적재되어 동적으로 링크

executable : .out 코드와 데이터영역이 하나로 합쳐진거 relocatable들이 엮어진 것

{% include image_caption.html imageurl="/images/sysp_final/_2019-12-12__2.07.33.png" title="" caption="" %}

{% include image_caption.html imageurl="/images/sysp_final/_2019-12-12__6.14.37.png" title="" caption="" %}

심볼분류

전역심볼 : 해당 모듈에서 정의되어 다른 모듈에서 참조할 수 있는 심볼. **non static 함수, 전역변수**

외부심볼 : 다른 모듈에서 정의되어 해당 모듈에서 참조하는 심볼. **전역심볼을 다른데서 갖다쓰면** 외부심볼

지역심볼 : 해당 모둘에서 정의되어 해당 모듈에서만 참조하는 심볼. **static함수와 전역변수 걍 static이면 일단 지역이다!??!?!?**

**그냥 지역변수는 어짜피 링커가 신경쓸게 아니니까 심볼처리 자체를 안하는거같음**

static : 지역, 전역에서 모두 사용 가능. 선언된 범위에 따라 달라진다는거같음. **전역이냐 지역이냐 이런걸 타지 않는다.**  프로그램과 라이프사이클을 같이한다. 단 선언된 스코프는 따라감. 해당 스코프 안에서만 사용 가능하다. 

심볼분류 revisited
전역심볼 : nonstatic함수와 nonstatic 전역변수
지역심볼 : static 모두
don't care : 지역변수

{% include image_caption.html imageurl="/images/sysp_final/_2019-12-12__11.49.05.png" title="" caption="" %}

{% include image_caption.html imageurl="/images/sysp_final/_2019-12-12__11.52.18.png" title="" caption="" %}

링커는 지역변수는 care하지 않는다. 

단 지역변수더라도 static 변수라면 bss와 data에 저장되는 변수로 심볼을 부여받는 지역심볼이다. 이점에 유의해야함. 위에 **심볼분류 revisited**를 확인하자.

{% include image_caption.html imageurl="/images/sysp_final/_2019-12-12__5.22.36.png" title="" caption="" %}

링커가 중복된경우 
스트롱이 있다면 무조건무조건이야 strong한 심볼을 따라간다. 
없다면 weak중 랜덤하게 선택한다. 

strong심볼 : 함수(프로시져)는 무조건 strong하다. 초기화된 변수도 strong함. strong은 여러개가 될 수 없겠죠?

weak심볼 : 초기화 되지 않았다면 weak하다. extern이로 선언된 것도 마찬가지  **pdf 22페이지 퍼즐 보기**

relocation : 여러 relocatable object파일의 .text와 .data영역 (코드와 데이터)를 하나의 .text와 .data로 합쳐주기위해 위치를 잡아주는 과정 → executable로 만드는 과정 

relocation이 이뤄지기 전에는 심볼의 정확한 주소를 알 수 없다. 상대적인 주소같은게 들어간다. 

라이브러리 

함수들을 하나의 소스에 만들어서 링크한다 → 시공간적으로 비효율적임 안쓰는게 섞이니까

각 함수를 각 파일에(c→o) 구현하고 필요한걸 다 include한다. → **재료 : 프로그래머 분말 25g**

**해결책 :**

**정적라이브러리** : 아카이브파일 .a → 오브젝트파일(개별 함수들)을 엮어서 인덱스를통해 참조하도록 한다. 링커의 강화버전. 심볼 참조시 아카이브의 테이블을 훑어서 심볼이 있는지 확인, 있다면 사용하도록 링크해준다.**이 훑는 과정이 근데 순서가 있음** 

c파일을 o파일로 만들어서 archive로 엮을 수 있다. executable이랑 비슷한듯?

libc.a는 기본 포함 라이브러리.  libc의 경우처럼 정적라이브러리 앞에는 lib이 붙는다. 

vector.h를 include하면 libvector.a를 링크한다. 

{% include image_caption.html imageurl="/images/sysp_final/_2019-12-12__3.54.48.png" title="" caption="" %}

{% include image_caption.html imageurl="/images/sysp_final/_2019-12-12__3.56.34.png" title="" caption="" %}

gcc -static -o prog2c main2.o -L -lvector ← 여기에 쓰이는 순서가 곧 확인 순서다. 아카이브가 뒤에 와야함 

위에서 말했듯 정적 링크는 **하나로 엮어서 실행파일을 만들**기때문에 크기가 굉장히 커진닭

.o와 .a가 주어진 순서대로 스캔을함.main이 먼저 읽히고 필요한 심볼을 아카이브에서 찾아야한다. 그렇지 않으면 순서대로 스캔하기때문에 문제가될 수 있음. 

**공유라이브러리** : 어짜피 라이브러리처럼 자주쓰는걸 묶어놨다면 존나 중복해서 쓸텐데 그걸 중복해서 메모리에 올리는건 비효율이다. 컴파일시에 executable에 포함되게 하지 말고 런타임에 필요하면 가져다 쓸 수 있는 동적인 라이브러리를 만들자 → **Dynamic Link Library .so (shared object)**

실행될 때 동적으로 링크된다. (load time linking) 

실행 중에도 링크될 수 있다 (런타임 링킹, dlopen())

{% include image_caption.html imageurl="/images/sysp_final/_2019-12-12__4.10.34.png" title="" caption="" %}

링커가 **명시적으로 링크하는건 똑같은데 load후에 필요하면 가져다 쓰는** 방식이다. 그니까 실행파일에 다 포함되어있는게 아니라 실행파일은 없이 만들어놓고 **필요할 때 동적으로 so를 가져다 써라 이거지**  이게 load time에 동적링크하는거고 더 심화적으로 run time 에 링크할 수도 있다. 

런타임에 동적으로 링크하는법은 아래와 같다. 

{% include image_caption.html imageurl="/images/sysp_final/_2019-12-12__4.14.44.png" title="" caption="" %}

이번엔 에센셜한 libc만 가져다가 링크하고 libvector는 진짜 **쓸일이 생기면 dlopen으로 가져다가 쓴다. 말그대로 런타임에 동적으로 갖다쓰는거.** 

# 족보

### 단편화

- 내부 단편화 : 가용블럭의 크기가 할당할 데이터보다 큰 블럭에 그냥 데이터를 할당하게되면 실제 쓰는 공간보다 블럭이 큰데 거기에 다른 데이터를 할당할 수 없게됨. 이걸 내부단편화라함 
**splitting을 통해 해결한다.**

    {% include image_caption.html imageurl="/images/sysp_final/_2019-12-07__4.28.16.png" title="" caption="" %}

    오버헤드도 단편화에 포함되는 것 같음.

- 외부 단편화 : 가용블럭의 전체 크기 합은 충분히 크지만, 실질적으로 얘네가 다 쪼게져있어서 더 할당할 수 없는 경우 **coalesce 되지 않은 free블럭의 연속을 생각하면 좋다. 
coalescing을 통해 해결한다.**

## coalesce

- 즉시 통합 : free 시 coalesce
- 지연 통합 : malloc시 가용리스트를 검색하는 시점, 혹은 외부단편화가 일정 양을 넘어서는 때에 일괄 통합하는 방식

## waitpid하면..

{% include image_caption.html imageurl="/images/sysp_final/_2019-12-07__12.13.48.png" title="" caption="" %}

선을 ...으로 표시해주자. 

## addjob after deletejob

{% include image_caption.html imageurl="/images/sysp_final/_2019-12-07__1.05.23.png" title="" caption="" %}

child의 실행이 더 빨라서 delete가 먼저일어나고 add가 일어나면 add된 job은 누가삭제해줌? → 좀비가 되어버린다. 

## zombie

좀비프로세스 자체는 메모리를 많이 안잡아먹지만, PID의 수가 제한적이라 좀비가 너무많아지면 가용 PID가 모두 소모되어 문제가 발생한다.

## 각 방식별 특징

- 간접 리스트(implicit) : 크기를 통해 모든 블럭을 연결한 리스트로 관리함
    - 모든 블럭을 순회하다보니 **모든 블럭의 개수에 해당하는 상수시간**이 걸린다.
- 직접 리스트(explicit) : 크기로 간접적으로 모든블럭을 연결한게 아니라 주소로 가용블럭들만 연결해서 관리함
    - 가용블럭들로만 이뤄진 리스트를 순회하며 찾다보니 모든 블럭을 탐색하는 간접리스트보다는 탐색이 빠르다. **가용블럭 수만큼에 해당하는 상수시간**이 걸린다.
- 구분 리스트(segregated) : 직접리스트처럼 주소로 리스트를 관리하는데, 크기별로 여러 리스트를 관리함
    - 크기별로 리스트를 운용하니까 대강 알맞은 크기 찾기에 유리하다. 따라서 **삽입에 시간이 빠를 뿐 만 아니라 단편화도 적은편**에 해당한다. first-fit방식으로도 best-fit에 맞먹는 성능 utilization도 좋다고는 안했다!!
    - LIFO : 블럭을 리스트 맨 앞에 배치한다 따라서 반환하면 주소 계산할필요 없이 바로 삽입되니까 빠르게 작동한다. 그치만  주소정렬보단 단편화가 늘어날 가능성이 있다.
    - 주소정렬 : LIFO방식이랑 달리 모든 가용블럭들의 주소를 비교해서 주소순으로 리스트가 관리되도록 삽입하므로 LIFO보다는 느리다. 그래도 단편화는 LIFO보다 좀 낫다 하네요

## 예외

interrupt검색해봐 거기 장황하게 나옴

- interrupt : 사용자가 발생시키는 비동기형. **현재 다음 종료**
- trap : 시스템콜, 브레이끼 등. **다음**으로
- fault : 오버플로우, 페이지폴트 등 처리가능한거. **현재**로
- abort : 하드웨어적인 문제, 복구 불가. **종료**

## SIGPROCMASK의 인자들

{% include image_caption.html imageurl="/images/sysp_final/_2019-12-13__5.00.43.png" title="" caption="" %}

{% include image_caption.html imageurl="/images/sysp_final/_2019-12-13__6.15.18.png" title="" caption="" %}

{% include image_caption.html imageurl="/images/sysp_final/_2019-12-13__6.15.54.png" title="" caption="" %}
