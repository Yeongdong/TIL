# Elastic File System
* NFS 기반 공유 스토리지 서비스(NFSv4)
  * 따로 용량을 지정할 필요 없이 사용한 만큼 용량이 증가 <-> EBS는 미리 크기를 지정해야 함
* 페타바이트 단위까지 확장 가능
* 몇 천개의 동시 접속 유지 가능
* 데이터는 여러 AZ에 나누어 분산 저장
* 쓰기 후 읽기(Read After Write) 일관성
* Private Service: AWS 외부에서 접속 불가능
  * AWS 외부에서 접속하기 위해서는 VPN 혹은 Direct Connect 등으로 별도로 VPC와 연결 필요
* 각 가용영역에 Mount Target을 두고 각각의 가용영역에서 해당 Mount Target으로 접근
* Linux Only

## Amazon EFS 타입
* Regional: 리전 전체를 활용하여 고가용성과 내구성 확보
* One-Zone: AZ 하나만 활용하여 고가용성과 내구성은 떨어지지만 저렴한 가격

## Amazon EFS 퍼포먼스 모드
* General Purpose: 가장 보편적인 모드. 거의 대부분의 경우 사용 권장
* Max IO: 매우 높은 IOPS가 필요한 경우
  * 빅데이터, 미디어 처리 등

## Amazon EFS Throughput 모드
* Elastic Throughput(추천): EFS가 자동으로 Throughput을 조절 - 예측하기 어렵거나 적은 범위에서 변동이 있는 경우
* Bursting Throughput: 낮은 Throughput일때 크레딧을 모아 높은 Throughput일때 사용
  * EC2 T타입과 비슷한 개념
* Provisioned Throughput: 미리 지정한 만큼의 Throughput을 미리 확보해두고 사용
