# Elastic Load Balancer

* 다수의 서비스에 트래픽을 분산시켜주는 서비스
* Health check: 직접 트래픽을 발생시켜 Instance가 살아있는지 체크
* Autoscaling과 연동 가능
* 여러 가용영역에 분산 가능
* 지속적으로 IP 주소가 바뀌며 IP 고정 불가능: **항상 도메인 기반으로 사용**

## 1. 종류

### 1. Application Load Balancer

* 똑똑함 -> 가장 자주 사용
* 트래픽을 모니터링하여 라우팅 가능
    * 예: image.sample.com -> 이미지 서버로, web.sample.com -> 웹 서버로 트래픽 분산

### 2. Network Load Balancer

* 빠름
* TCP 기반 빠른 트래픽 분산
* Elastic IP 할당 가능(IP 고정시켜주는 서비스)

### 3. Classic Load Balancer

* 옛날
* 현재는 잘 사용하지 않음

### 4. Gateway Load Balancer

* 먼저 트래픽을 체크
* 가상 어플라이언스 배포/확장 관리를 위한 서비스(방화벽, 트래픽 분석, 캐싱, 인증, 로깅 등)

## 2. 대상 그룹(Target Group)

* ALB가 라우팅할 대상의 집합

### 1. 구성

* 3+1가지 종류
    * Instance
    * IP
    * Lambda
    * ALB
* 프로토콜(**HTTP, HTTPS**, gRPC 등)
* 기타 설정
    * 트래픽 분산 알고리즘, 고정 세션 등