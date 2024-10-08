# 1. Autoscaling(오토스케일링)

## 1. 스케일링

### 1. Vertical Scale(Scale Up)

* 성능이 올라가지만 비용은 기하급수적으로 올라간다.

### 2. Horizontal Scale(Scale Out)

* 성능에 따라 비용이 일정하게 올라간다.
* 클라우드 환경에서는 스케일 아웃을 고려해야 한다.

## 2. AWS Auto Scaling
### 1. 목표
* 정확한 수의 EC2 인스턴스를 보유하도록 보장
    * 그룹의 최소 인스턴스 숫자 및 최대 인스턴스 숫자
        * 최소 숫자 이하로 내려가지 않도록 인스턴스 숫자를 유지(인스턴스 추가)
        * 최대 숫자 이상 늘어나지 않도록 인스턴스 숫자 유지(인스턴스 삭제)
    * 다양한 스케일링 정책 적용 가능
        * 예: CPU의 부하에 따라 인스턴스 크기를 늘리기
* 가용 영역에 인스턴스가 골고루 분산될 수 있도록 인스턴스를 분배
### 2. 오토스케일링의 구성
* 시작 구성(launch configurations)/시작 템플릿(launch template): **무엇**을 실행시킬 것인가?
    * EC2의 타입, 사이즈
    * AMI
    * 보안 그룹, Key, IAM
    * 유저 데이터
* 모니터링: **언제** 실행시킬 것인가? + 상태 확인
    * 예: CPU 점유율이 일정 %을 넘어섰을 때 추가로 실행 or 2개 이상이 필요한 스택에서 EC2 하나가 죽었을 때
    * CloudWatch (and/or) ELB와 연계
* 설정: **얼마나 어떻게** 실행시킬 것인가?
    * 최대 / 최소 / 원하는 인스턴스 숫자
    * ELB와 연동 등