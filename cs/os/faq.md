# 1. Convoy effect vs Starvation

## Convoy effect

* 몇 개의 시간이 오래 걸리는 프로세스로 인해 다른 프로세스의 실행이 느려지고 평균 대기시간이 길어지며 결과적으로 전체적인 프로세스의 성능을 저하시키는 현상
* FCFS 알고리즘을 사용할 때 나타날 수 있음

## Starvation

* 어떤 프로세스가 "무기한으로" 대기하며 CPU 소유권을 얻을 수 없음을 의미
* SJF 알고리즘을 사용할 때 나타날 수 있음
* 우선순위가 자꾸 밀려져서 해당 프로세스가 아예 실행이 안되는 것
    * aging을 통해 우선순위를 높이는 방법으로 해결
        * 실행되지 않은 프로세스라면 시간이 지나가면서 우선순위를 높여서 실행되게 만드는 것

# 2. busy wait이란?

* 프로세스, 스레드가 어떠한 일을 실행하기 전에 **만족하는 조건을 지속적으로 확인**하는 동기화 기술
* 운영체제에서 프로세스가 공유자원에 동시에 접근하는 것을 방지하는 상호배제를 달성하는데 사용
* busy_wait 상태에서는 우선순위에 따라 작업을 바꿀 수 없기 때문에 우선순위가 높은 작업을 완료해야할 때 비효율적
* 대기시간이 많이 걸릴 경우 계속해서 기다리는데 CPU 자원을 쓰기 때문에 자원낭비가 심함
* 대부분의 경우 busy_wait 은 안티패턴으로 분류

# 3. 운영체제와 펌웨어의 차이

1. 펌웨어는 ROM 이라고 불리는 비휘발성 메모리 하나를 사용 / 운영체제는 휘발성, 비휘발성 메모리를 계층화해서 사용
2. 펌웨어는 자유로이 프로그램을 설치할 수 없으며 미리 설치해놓은 프로그램을 기반으로 업데이트가 일어남. ROM에 해당 소프트웨어를 지우고 덮어쓰면서 업데이트가 일어남 / 운영체제는 정기적으로 업데이트가 되며
   운영체제 위에 프로그램을 자유로이 설치할 수 있음
3. 펌웨어의 종류로는 키보드, 세탁기 안에 들어가잇는 소프트웨어 등 / 운영체제의 종류로는 MacOS, windowOS 등