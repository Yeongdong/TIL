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
* MAC(Media Access Control) Address => **주민번호!**
  * 네트워크 인터페이스에 부여된 고유의 주소
    * 데이터가 지정한 대상에게 잘 전달될 수 있도록 대상 식별에 사용
  * 2개의 Hexadecimal(=byte) 단위로 6개를 나열 = 48 bits = 6bytes
    * 예: 00:1A:2B:3C:4D:5E
  * 두 파트로 구분
    * 첫 3개의 bytes는 OUI(Organizationally Unique Identifier): 제조사에 부여된 고유 식별자
    * 나머지 3개의 bytes는 NIC(Network Interface Controller): 네트워크 인터페이스별 고유 번호
  * **네트워크 인터페이스의 MAC Address는 고유의 값이며 변하지 않음**
    * IP는 변동

### 3. Network Layer
* 여러 노드의 경로를 찾고 올바르게 전달될 수 있는 기능과 수단을 정의
* 주요 단위: 패킷
* 주요 구성 요소
  * **Router, IP, ARP**
* 주요 특징
  * 서로 떨어진 Local Network간의 통신을 지원
    * 중간중간 Node들을 거쳐 목적지까지 도달할 수 있는 방법을 지원
* IP Address
  * Internet Protocol 상에서 통신 주체를 식별하기 위한 아이디
  * 두 가지 종류
    * IPv4: 32Bits(2^32 = 약 43억개)
      * 아이피를 최대로 활용하기 위해 사설(Private) IP와 공인(Public) IP로 분류
    * IPv6: 128Bits(2^128)
      * 엄청 많기 때문에 사설 IP 개념이 불필요함
    * MAC Address와 다르게 수시로 변동 가능
  * Classless Inter Domain Routing(CIDR)
    * IP는 주소의 영역을 여러 네트워크 영역으로 나누기 위해 IP를 묶는 방식
    * 여러 개의 사설망을 구축하기 위해 망을 나누는 방법
  * CIDR Notation
    * IP 주소의 **집합**
    * 네트워크 주소와 호스트 주소로 구성
    * 각 호스트 주소 숫자만큼의 아이피를 가진 네트워크망 형성 가능
    * A,B,C,D/E 형식
      * 예: 10.0.1.0/24, 172.16.0.0/12
      * A,B,C,D: 네트워크 주소 + 호스트 주소 표시
      * E: 0~32: 네트워크 주소가 몇 bit인지 표시
    * CIDR Block
      * 호스트 주소 비트만큼 IP 주소를 보유 가능
      * 예: 192.168.2.0/24
        * 네트워크 비트 24
        * 호스트 주소 = 32-24=8
        * 즉 2^8 = 256개의 IP 주소 보유
        * 192.168.2.0~192.168.2.255까지 총 256개 주소를 의미
  * Subnet Mask('/')
    * 어느 부분이 호스트 비트인지, 어느 부분이 네트워크 비트인지 구분해주는 Mask
      * AND 연산을 활용해 네트워크 주소를 필터링
      * AND 연산: 두 비트 중 둘다 1일때만 1
        * 예: 1 and 1 = 1, 0 and 1 = 0, 1 and 0 = 0
    * 네트워크 비트 수 만큼 1을 보유한 마스크를 IP에 적용하면 네트워크 비트만 추출 가능
* 라우터(Router)
  * 네트워크간에 패킷을 주고 받는 Layer 3 장치
  * IP 대역별 최적 경로를 수집 및 관리
    * 어떤 경로의 노드를 경유해야 가장 효율적으로 대상에 도착하는지 알고 있음(Route Table)
    * 이 경로를 바탕으로 특정 IP 주소가 대상인 패킷의 전달을 요청받을 때 해당 경로로 요청
  * 로컬 네트워크는 자신의 로컬 네트워크 주소가 아니라면 라우터로 전달
    * 확인 방법: 네트워크 주소가 같은지 확인(subnet mask 등)
  * 이후 Router는 패킷을 Frame안에 넣어서 최적 경로에 따른 다른 Router로 전달
  * Frame은 MAC Address가 필요함 -> IP 주소에 따른 Frame 확인 방법: ARP
* ARP(Address Resolution Protocol)
  * IP로 Mac 어드레스를 찾는 프로토콜
  * 순서
    * Broadcast (MAC Address FF:FF:FF:FF:FF:FF)로 IP 요청(이 IP 가진 컴퓨터 누구?)
    * 응답 받은 IP MAC Address를 기반으로 MAC 확정 후 테이블에 저장
* Network Layer 해결하지 못한 문제
  * 한번에 하나의 통신만 가능
    * 즉 여러 어플리케이션이 동시에 통신 불가능
    * 패킷 등의 순서 보장 / 유실에 대한 대응 불가능
### 4. Transport
* 통신 주체끼리 데이터 전달의 신뢰성을 확보하는 방법을 정의
* 주요 단위: 세그먼트
* 주요 구성 요소
  * **TCP/UDP**
* 주요 특징
  * Network Layer로 성립된 통신 위에서 실질적인 활용을 위한 다양한 기능을 정의
    * 패킷의 순서 보장, 에러 처리, 포트 기반 분할 등
* Transmission Control Protocol(TCP)
  * 패킷의 전달 과정에서 순서를 보장하고 유실되지 않도록 보장할 수 있는 통신 규약
    * 패킷 안에 세그먼트를 담아 주고 받아서 로직을 처리
  * 연결 지향
    * 즉 지속적으로 채널을 수립하여 전달 여부를 확인하고 무결성을 확인
    * 지속적으로 무결성을 확보하는 과정에서 비교적 느리고 복잡한 과정 필요
  * 주요 사용 사례
    * 웹 페이지(HTTP/HTTPS)
    * 이메일
    * 파일 전송
    * SSH 등
* Segment(세그먼트)
  * TCP/UDP 의 데이터 전달 단위
  * TCP 세그먼트의 주요 구성
    * Port(Source/Destination)
    * Sequence/Acknowledgement Number: 통신 주체끼리 데이터를 주고 받았는지 확인에 사용
    * Flags: Segment의 목적 등을 정의(예: ACK, SYN, FIN)
    * Window Size: 세그먼트를 만든 주체가 얼마나 많은 데이터를 받을지 전달
    * Urgent Pointer: 세그먼트의 중요도를 설정(먼저 처리해야하는 세그먼트 등)
    * 기타(checksum 등)
    * 실 데이터
* Port
  * IP 프로토콜에서 패킷을 올바른 프로세스로 라우팅하기 위한 논리적 단위
    * IP 주소가 동일하더라도 어플리케이션을 구분하기 위해 사용(다양한 어플리케이션 동시에 사용 가능)
  * TCP Port/ UDP Port로 구분
    * 각각 0~65535까지 정수 범위
    * Well Known Port: 주로 서버에서 사용하는 어플리케이션/프로토콜 별로 미리 지정된 포트
      * 주요 사용에 따라 표준으로 공통적으로 사용
      * 예:
        * 80: HTTP
        * 443: HTTPS
        * 22: SSH
        * 3306: Mysql
    * Ephemeral Port: 클라이언트에서 사용하는 포트로 연결할 때 마다 임의로 지정
* TCP Handshake
  * TCP 프로토콜에서 통신을 수립하고 서로를 인식하는 첫 과정
  * 보통 Three Way Handshake로 부르며 3가지 과정으로 구분
    * Syn(Synchronize): 첫 요청으로 사용할 첫 클라이언트 Sequence Number(CS)를 전달
    * Syn-Ack(Synchronize-Acknowledge): Syn에 대한 응답으로 CS+1과 서버 Sequence Number(SS)를 전달
    * ACK(Acknowledge): 마지막 단계로 연결이 수립되었음을 알려주며 CS+1과 SS+1을 전달
* User Datagram Protocol(UDP)
  * 빠르고 간단하게 데이터를 주고 받을 수 있는 방법을 정의
  * Connectionless
    * 연결 지향과는 달리 데이터의 무결성, 순서, 전달여부를 체크하지 않음
    * 즉 패킷이 순서대로 오지 않거나, 중복되거나, 아예 전달되지 않을 수 있음
  * 빠르고 간단
  * 주요 사용 사례
    * 스트리밍
    * 보이스톡
    * 온라인 게임

### 5. Session Layer
* TCP/IP Model(OSI 7 Model)
1. Network Access(Physical, Data Link)
2. Network(Network)
3. Transport(Transport)
4. Application(Session, Presentation, Application)

* 통신 주체끼리 연결이 유지할 수 있는 방법을 정의
* 예전의 컴퓨팅 환경에서 Layer 1, 2, 3, 4 이외의 차원에서 지속적인 연결(세션)이 수립될 수 있는 방법을 제공
  * 예: MAC Address(2)와 IP(3)주소와 포트(4)가 동일한 상황에서 어떻게 유저를 구분할 것인가?
* 현대에서도 마찬가지로 Layer 4 이상의 추가적인 차원에서 지속적인 연결(세션)을 수립할 수 있는 방법을 포함
  * 예: HTTP Cookie
* 몇몇 프로토콜의 경우 Session Layer 자체를 구현하지 않음
  * 예: FTP(IP가 바뀌면 다시 인증해야 함)

### 6. Presentation Layer
* 받은 데이터를 해석하는 방법을 정의
  * 파싱, 압축 해제, 복호화 등 Application Layer에서 사용할 수 있는 형식으로 변환을 담당

### 7. Application Layer
* 실제 받은 데이터를 처리하는 방법을 정의
  * 말 그대로 데이터를 가지고 무엇을 어떻게 처리할지에 관한 레이어
* 예: HTTP의 경우
  * Method(GET/POST/PUT/DELETE/HEAD/OPTION/PATCH)
  * Status Code(2xx,3xx,4xx,5xx)
  * Header
    * Host
    * User-Agent
    * Authorizations
    * Accept-Encoding
    * Content-Type
