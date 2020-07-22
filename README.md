# Pintos Projects
Operating System (HAEZ0003)

## Pintos Project #1 - Pintos 환경 구축 

- 개요
  * Pintos의 소스코드를 분석, 전체적인 구조를 이해하는 것을 목표로 한다.
  
- 내용
  * Pintos 운영체제가 부팅을 시작하여, alarm-multiple 테스트를 수행하기까지의 과정에 대해 (1) 함수 분석, (2) 자료구조 분석, (3) 프로그램 실행 경로 분석 등을 수행한다. 
  * 배포한 가상 머신 상의 ~/pintos/src/threads 디렉토리 내의 소스 코드를 대상으로 분석한다. 
  * 대상 소스 코드 중 조건부 컴파일을 위한 매크로 USERPROG와 FILESYS 해당 부분은 분석 대상에서 제외한다
  * “pintos –v -- run alarm-multiple” 명령을 실행시켰을 경우의 프로그램 수행에 대한 소스 코드를 분석하여 제출한다. 
  

## Pintos Project #2 – Alarm clock의 개선 

- 개요
  * Pintos 커널의 timer_sleep() 함수를 개선한다. 
- 프로젝트 내용
  * [문제정의] ‘devices/timer.c’에 정의되어 있는 timer_sleep() 함수를 개선한다. 현재의 timer_sleep()은 기능적으로는 정상 동작하나, busy waiting 방식으로 구현되어 있다. 이를 busy waiting 없이 수행하도록 수정하여 성능을 개선한다. 
  * [해결] void timer_sleep (int64 t ticks)를 호출한 쓰레드를 현재 시각 기준으로 ticks 시간이 경과할 때까지 block 시키도록 수정한다 (ticks 는 timer tick 단위로 표시). 이후 ticks timer tick 이상이 경과하면 해당 thread 를 ready 상태로 전이시키면 된다. 이를 위해 timer_sleep()에 의해 block 된 스레드들이 대기할 수 있는 리스트를 만들고, 매 번 timer interrupt가 발생할 때마다 (즉 timer interrupt handler 함수 timer_interrupt()에서) timer tick이 ticks 이상 지났는지 검사하는 방식으로 구현한다.
  * [테스트] 구현된 timer_sleep() 함수가 제대로 동작하는지 여부는 테스트 alarm-single, alarm-multiple, alarm-simultaneous, alarm-priority, alarm-zero, alarm-negative 를 사용하여 시험한다 (이 테스트들은 모두 timer_sleep() 함수를 사용하는 프로그램들이다.  따라서 원본 pintos 커널에서의 이 테스트들의 결과와 timer_sleep() 함수가 수정된 후의 결과가 동일하여야 한다). 


## Pintos Project #3 – Thread scheduling 

- 개요
  * Pintos에 우선순위 스케줄러(priority scheduler)를 구현한다. 또한 우선순위 스케줄링 시 발생할 수 있는 우선순위 역전 (priority inversion) 을 방지할 수 있는 priority inheritance(donation)기능을 구현한다. 

- 프로젝트 내용
  * [문제정의] (1) 현재 pintos에는 라운드로빈 스케줄러가 구현되어 있다. 이를 수정하여 스레드 별 우선순위에 따라 스케줄링 할 수 있는 우선순위 스케줄러를 새로 구현한다. 특히, preemptive dynamic priority scheduling을 구현한다. (2) 우선순위 스케줄링 방식의 문제점은 우선순위 역전 현상이 일어날 수 있다는 것이다. 이를 방지할 수 있도록 우선순위 스케줄러에 priority inheritance(donation) 기능을 구현한다. 단, 우선순위 역전 현상은 다양한 자원에 대해 발생할 수 있는 문제이나, 이번 과제에서는 세마포어와 관련된 우선순위 역전 현상만 해결한다. (3) 각 스레드가 자신의 우선순위를 확인하고 우선순위를 변경할 수 있도록 두 가지 함수 void thread_set_priority(int new_priority)와 int thread_get_priority(void)를 구현한다
  * [테스트] 구현된 우선순위 스케줄러가 제대로 동작하는지 여부는 테스트 priority-change, priority-preempt, priority-sema, priority-donate-one, priority-donate-multiple, priority-donate-multiple2, priority-donate-nest, priority-donate-chain, prioritydonate-sema, priority-donate-lower 를 사용하여 시험한다. 우선순위 스케줄링과 관련된 테스트 중 priority-fifo 와 priority-convar 에 대해서는 시험하지 않는다. 
