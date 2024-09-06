OSI 7 Layer Model(1) - Physical/Data Link
========================
OSI 7 Layer Model
--------
* 컴퓨터 네트워크 및 통신을 7개의 레이어(게층)으로 표현한 모델
* 각 계층은 하위 계층의 기능을 활용해 역할을 수행하고 상위 계층으로 처리 결과 전달
1. Physical
2. Data Link
3. Network
4. Transport
5. Session
6. Presentation
7. Application

=> Pleas Do Not Throw Sausage Pizza Away

### 1. Physical Layer
* 장치를 연결하기 위한 매체의 물리적인 사항
  * 전압, 주기, 시간, 전선의 규격, 거리 등
* 주요 단위: Bits
* 대표 구성 요소
  * 케이블/안테나/RF등 전송 매체, **허브, 리피터**   
   
  1. 허브
     * Physical Layer 단위에서 다수의 기기들을 연결해주는 장치
     * 특징
       * 에러 / 충돌(Collision) / 디바이스별 제어 기능 없음
       * 받은 내용을 그대로 전달 -> 무조건 Broadcast

### 2. Data Link Layer
* 물리적인 통신을 제어하여 디바이스와 디바이스간의 통신 및 전송을 안정화하기 위한 프로토콜
* 주요 단위: Frame
* 주요 구성 요소
  * **Mac Address, Switch**
* 주요 특징
  * CSMA/CD(Carrier-Sense Multiple Access with Collision Detection) 방식을 활용해서 각 디바이스간의 통신을 원활하게 연결
  * 대상을 구별하여 디바이스간의 통신을 지원(Unicast)
    * Broadcast도 지원
* MAC(Media Access Control) Address
  * 네트워크 인터페이스에 부여된 고유의 주소
    * 데이터가 지정한 대상에게 잘 전달될 수 있도록 대상 식별에 사용
  * 2개의 Hexadecimal(=byte) 단위로 6개를 나열 = 48 bits = 6bytes
    * 예: 00:1A:2B:3C:4D:5E
  * 두 파트로 구분
    * 첫 3개의 bytes는 OUI(Organizationally Unique Identifier): 제조사에 부여된 고유 식별자
    * 나머지 3개의 bytes는 NIC(Network Interface Controller): 네트워크 인터페이스별 고유 번호
  * **네트워크 인터페이스의 MAC Address는 고유의 값이며 변하지 않음**
    * IP는 변동
