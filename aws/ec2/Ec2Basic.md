# 1. EC2

## 1.EC2의 사용

* 서버를 구축할 때
    * 게임 서버, 웹서버, 어플리케이션 서버
* 어플리케이션을 사용하거나 호스팅하 ㄹ때
    * 데이터베이스
    * 머신 러닝
    * 비트코인 채굴
    * 연구용 프로그램
* 기타 다양한 목적
    * 그래픽 렌더링
    * 게임 등

## 2. EC2의 특성

* 초 단위 온디맨드 가격 모델
    * 온디맨드 모델에서는 가격이 초 단위로 결정
    * 서비스 요금을 미리 약정하거나 선입금이 필요 없음
* 빠른 구축 속도와 확장성
    * 몇 분이면 전 세계에 인스턴스 수백여대를 구축 가능
* 다양한 구성방법 지원
    * 머신러닝, 웹서버, 게임서버, 이미지 처리 등 다양한 용도에 최적화된 서버 구성 가능
    * 다양한 과금 모델 사용 가능
* 여러 AWS 서비스와 연동
    * 오토 스케일링, ELB, CloudWatch

## 3. EC2의 구성

* 인스턴스
    * 클라우드에서 사용하는 가상 서버로 CPU, 메모리, 그래픽카드 등 연산을 위한 하드웨어 담당
* EBS
    * Elastic Block Storage
    * 클라우드에서 사용하는 가상 하드디스크
* AMI
    * EC2 인스턴스를 실행하기 위한 정보를 담고 있는 이미지
* 보안 그룹
    * 가상의 방화벽

## 4. 실습

* 목표: EC2 한대를 프로비전하여 웹 서버 구성

## 5. EC2의 가격 정책

### On-Demand

* 실행하는 인스턴스에 따라 시간 또는 초당 컴퓨팅 파워로 측정된 가격을 지불
* 약정은 필요 없음
* 장기적인 수요 예측이 힘들거나 유연하게 EC2를 사용하고 싶을 때
* 한번 써보고 싶을 때

### Spot Instance

* 경매 형식으로 시장에 남는 인스턴스를 저렴하게 구매해서 쓰는 방식
    * 수요에 따라 내가 지정한 가격보다 낮으면 사용, 높으면 반환
* 최대 90% 정도 저렴
* 단 언제 도로 내주어야 할지 모름
* 인스턴스가 확보되고 종료되는 것을 반복해도 문제 없는 분산 아키텍쳐 필요
* 주로 빅데이터 처리, ML 등 많은 인스턴스가 필요한 경우 사용

### 예약 인스턴스(Reserved Instance-RI)

* 미리 일정 기간(1년~3년) 약정해서 쓰는 방식
* 최대 75% 정도 저렴
* 수요 예측이 확실할 때
* 총 비용을 절감하기 위해 어느 정도 기간의 약정이 가능한 사용자
* 주로 큰 기업

### 전용 호스트(Dedicated)

* 실제 물리적인 서버를 임대하는 방식
* 라이선스 이슈(Windows Server 등)
* 규정에 따라 필요한 경우
* 퍼포먼스 이슈(CPU Steal 등)

# 2. EBS

## 1. EBS 소개

* 가상의 하드 드라이브
* EC2 인스턴스가 종료되어도 계속 유지 가능
    * 인스턴스와 EBS(스토리지)가 네트워크로 연결되어있음
    * 인스턴스나 EBS를 언제든지 교체 가능
* 인스턴스 정지 후 재기동 가능
* 하나의 EBS를 여러 EC2 장착 가능(EBS Multi Attach)
* 루트 볼륨으로 사용시 EC2가 종료되면 같이 삭제됨
    * 단 설정을 통해 EBS만 따로 존속 가능
* EC2와 같은 가용영역에 존재
* 총 5가지 타입으로 제공

## 2. EBS 종류

| **타입**       | **범용타입** | **프로비전된 IOPS**          | **쓰루풋 최적화 HDD**                  | **Cold HDD** | **마그네틱**         |
|--------------|----------|-------------------------|----------------------------------|--------------|------------------|
| **이름**       | GP3      | IO2                     | ST1                              | SC1          | Standard         |
| **용량**       | 1GB~16TB | 4GB~16TB                | 500GB~16TB                       | 500GB~16TB   | 1GB~1TB          |
| **사용**       | 일반 범용    | IOPS가 중요한 애플리케이션/데이터베이스 | 쓰루풋이 중요한 어플리케이션/하둡/OLAP 데이터베이스 등 | 파일 저장소       | 백업/비주기적인 데이터 액세스 |
| **MAX IOPS** | 16,000   | 64,000                  | 500                              | 250          | 40~200           |

## 3. Snapshot

* 특정 시간에 EBS 상태의 저장본
    * EBS에 사진을 찍어둔 개념
* 필요시 스냅샷을 통해 특정 시간의 EBS를 복구 가능
* S3에 보관
    * 증분식 저장: 기존에서 변화된 부분만 저장한다.

## 4. AMI

* EC2 인스턴스를 실행하기 위해 필요한 정보를 모은 단위
    * OS, 아키텍쳐 타입(32-bit or 64-bit), 저장 공간, 용량 등
* AMI를 사용하여 EC2를 복제하거나 다른 리전 -> 계정으로 전달 가능
* 스냅샷을 기반으로 AMI 구성 가능

### 1. 구성

* 1개 이상의 EBS 스냅샷
* 인스턴스 저장 인스턴스의 경우 루트 볼륨에 대한 템플릿(예: 운영 체제, 애플리케이션 서버, 애플리케이션)
* 사용 권한(어떤 AWS 어카운트가 사용할 수 있는지)
* 블록 디바이스 맵핑(EC2 인스턴스를 위한 볼륨 정보 = EBS가 무슨 용량으로 몇 개 붙는지)

### 2. 타입

* EBS기반 or Instance Storage 기반

### 3. 타입에 따른 AMI의 생성 방법

* EBS: 스냅샷을 기반으로 루트 디바이스 생성
* 인스턴스 저장: S3에 저장된 템플릿을 기반으로 생성

# 3. EC2의 생명주기

## 1. Pending (시작 중)

- 인스턴스가 시작 요청을 받은 상태
- AWS가 해당 인스턴스의 자원을 할당하고 필요한 초기화 작업을 수행하는 단계
- 이 상태에서는 인스턴스가 아직 실행되지 않았으며, 사용자가 액세스할 수 없음

## 2. Running (실행 중)

- 인스턴스가 정상적으로 시작되어 운영 중인 상태
- 사용자는 인스턴스에 SSH 또는 RDP를 통해 접속하여 작업을 수행할 수 있음
- 이 상태에서 애플리케이션을 실행하거나 작업을 처리할 수 있음

## 3. Stopping (정지 중)

- 인스턴스를 중지 요청한 상태
- 운영 체제가 종료 절차를 진행 중이며, 인스턴스가 곧 "Stopped" 상태로 이동
- 인스턴스는 더 이상 작업을 처리하지 않으며, 데이터가 삭제되지 않고 EBS 볼륨 등은 유지

## 4. Stopped (정지됨)

- 인스턴스가 중지된 상태
- 인스턴스가 종료되었지만 EBS 볼륨은 유지되며, 다음 시작 시 기존 데이터를 사용할 수 있음
- 이 상태에서는 인스턴스의 CPU 시간에 대해 요금이 부과되지 않지만, EBS 볼륨에 대해서는 비용이 청구됨
- 필요에 따라 인스턴스를 다시 시작할 수 있음

## 5. Shutting-down (종료 중)

- 인스턴스가 종료 요청을 받고 종료 절차를 진행 중인 상태
- 이 상태는 잠시 유지되며, 종료가 완료되면 "Terminated" 상태로 전환

## 6. Terminated (종료됨)

- 인스턴스가 완전히 종료된 상태로, 인스턴스가 삭제되고 더 이상 존재하지 않음
- 이 상태가 되면 인스턴스를 복구할 수 없음
- 인스턴스의 모든 자원이 해제되고, EBS 볼륨이 삭제되었다면 데이터도 삭제됨

## 생명주기 상태 간 전이

- **Pending -> Running**: 인스턴스가 시작될 때.
- **Running -> Stopping**: 인스턴스를 중지할 때.
- **Running -> Shutting-down**: 인스턴스를 종료할 때.
- **Stopping -> Stopped**: 인스턴스가 완전히 중지되었을 때.
- **Stopped -> Running**: 정지된 인스턴스를 다시 시작할 때.
- **Shutting-down -> Terminated**: 인스턴스가 완전히 종료되었을 때.

## 추가 상태

- **Rebooting (재부팅 중)**: 인스턴스가 재부팅될 때 잠시 동안 표시되는 상태
- **Retired (사용 중지)**: AWS에서 인스턴스가 더 이상 사용되지 않도록 계획될 때 사용하는 상태

# 4. T 인스턴스

* burstable performance instances(버스트 가능한 성능 인스턴스)
    * 다른 인스턴스 타입처럼 고정된 용량의 CPU 자원을 제공하지 않음
    * CPU 크레딧 제도
        * CPU 사용량이 기본 수준(baseline) 이상(burst)이라면 크레딧을 차감
        * CPU 사용량이 기본 수준(baseline) 미만이라면 크레딧을 제공
    * 베이스라인: CPU 사용량을 통해 크레딧의 소모와 증가가 같은 지점
        * 인스턴스의 크기에 따라 다름
* 즉 평소에는 baseline 밑으로 유지해 크레딧을 모으고 꼭 필요한 상황에 크레딧을 소모해 Burst
* 크레딧이 없다면 최악의 경우 CPU 사용량이 5% 미만으로 제한됨

## CPU 크레딧과 베이스라인

* T인스턴스는 총 3가지 상태 중 하나
    * CPU 사용이 베이스라인보다 적은 경우: 크레딧 증가
    * CPU 사용이 베이스라인보다 많은 경우: 크레딧 소모
    * CPU 사용이 베이스라인보다 같은 경우: 크레딧 증감 없음
* CPU 크레딧: vCPU가 사용하는 시간 단위
* 예:
    * 1 CPU credit = 1 vCPU * 100% utilization * 1 minute
    * 1 CPU credit = 1 vCPU * 50% utilization * 2 minutes
    * 1 CPU credit = 2 vCPU * 25% utilization * 2 minutes
* 시간당 크레딧의 추가: 각 인스턴스 별로 지정된 베이스라인으로 계산
    * 공식: vCPU숫자 * 베이스라인 * 60분
    * 예: T3.nano의(baseline:5%) 경우 시간당 6크레딧 부여
        * 2 vCPU * 5% * 60 minute = 6 크레딧

## CPU 크레딧의 추가와 소모

* CPU 크레딧은 CPU 사용량이 베이스라인 미만일때 지속적으로 추가됨(millisecond 단위)
* CPU 사용량이 베이스랑니 이상일때 지속적으로 소모됨(millisecond 단위)
* 각 인스턴스 사이즈 별로 최대로 저장 가능한 크레딧의 한계가 존재
    * 일반적으로 시간당 얻을 수 있는 최대 크레딧 * 24
    * 예: t3.nano의 경우 6(시간당 얻는 크레딧) * 24 = 144 크레딧
    * 이 이상 얻는 크레딧은 버려짐

## T인스턴스 모드

* 스탠다드 모드
    * 베이스라인 이상으로 Burst가 발생할 경우 미리 저장된 크레딧을 소모해 CPU 사용
    * 크레딧이 없다면 베이스라인 이상으로 CPU사용 불가
* 언리미티드 모드
    * 제한없이 필요한만큼 CPU 사용
    * 베이스라인 이상으로 Burst가 발생할 경우 미리 저장된 크레딧을 소모해 사용
    * 크레딧이 없을 경우, 24시간 안으로 크레딧을 빌려 충당
    * 24시간 안에 베이스라인 미만으로 CPU를 유지해 추가된 크레딧으로 갚기 가능
    * 갚지 못한 크레딧은 비용을 지불해 충당
* T2는 스탠다드가 기본, T3는 언리미티드가 기본

## Launch Credit

* T2 인스턴스의 경우 Launch Credit을 제공
    * 인스턴스가 처음 생성될 때 제공
    * 생성 직후에 Burst로 진입할 경우를 대비
    * Standard 모드가 Default이기 때문
* T3의 경우 Unlimited Mode가 Default이기 때문에 Luanch Credit이 부여되지 않음
    * 즉 T3를 Standard 모드로 사용해서 생성할 경우, 바로 Burst 진입 불가

## T2인스턴스가 크레딧이 없을 경우

* 인스턴스의 응답이 없음
* 어플리케이션이 자주 멈춤
    * 웹서버의 경우 500 error 등
    * 정상 동작하다(크레딧이 충분할 때) 멈추다(크레딧이 부족할 때)를 반복
* 해결책
    * 인스턴스 사이즈 늘릭
    * 인스턴스 타입 바꾸기(예: M타입)
    * Unlimited 모드로 바꾸기
    * 계획적으로 CPU 사용하기

## 결론

* T 시리즈 인스턴스(특히 T2) 타입의 경우에는 사용 계획이 필요함
    * CPU 사용량에 대해 잘 계획해서 사용하면 이득을 볼 수 있지만 막 사용하면 오히려 손해임
    * 개발용 인스턴스 같은 경우에는 T타입이 적절
    * 라이브 서비스를 위한 인스턴스의 경우 T타입을 사용할 때 주의가 필요함
    * CPU Credit은 CloudWatch 지표로 모니터링 가능 -> 알람 등 처리 가능
* 사용량에 맞는 사이즈를 적절하게 사용 필요

# 5. 인스턴스 타입 변경

* 클라우드 컴퓨팅의 장점을 최대한 이용하여 프로비전 이후에도 적절한 타입/사이즈로 변경 가능
* 두 가지 변경
    * 사이즈 변경: 같은 인스턴스 패밀리에서 사용량에 따라 크기 조절(예: t3.micro -> t3.medium)
    * 패밀리 변경: 워크로드의 성격에 따라 다른 인스턴스 패밀리로 변경(예: t3.micro -> t5.large)
* 호환성 체크 필요
    * 가상화 타입(HVM, PV), 아키텍처(ARM, 32bit, ...), EBS 볼륨 개수 등
* 인스턴스 중지 필요
    * Spot 인스턴스는 변경 불가능
    * HealthCheck 불가능 -> Autoscale에 영향
* 참고사항
    * 인스턴스 아이디는 유지
    * Private IP는 유지하지만 Public IP는 변경(Elastic IP의 경우 고정)
* 인스턴스 스토어와 EBS 사용 인스턴스에 따라 과정이 달라짐
## 종료시 삭제 "예" -> "아니오" 변경 script
  * 중지된 인스턴스 -> CloudShell에서 실행
<pre>
<code>
aws ec2 modify-instance-attribute --instance-id {인스턴스_아이디} --block-device-mappings "[{\"DeviceName\":\"/dev/xvda\",\"Ebs\":{\"DeleteOnTermination\":false}}]"
</code>
</pre>
