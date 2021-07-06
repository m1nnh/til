## 트랜스포트 계층

### 트랜스포트 계층의 역할

`트랜스포트 계층`은 애플리케이션 계층과 인터넷 계층 사이에 위치한다. 인터넷 계층의 역할이 데이터를 수신지 컴퓨터까지 전달하는 것이라면, 트랜스포트 계층의 역할은 **컴퓨터가 받은 데이터를 애플리케이션까지 전달하는 것**이다.

- 애플리케이션으로부터 받은 데이터를 전달한다.
- 수신할 애플리케이션에 전달한다.

트랜스포트 계층은 포트 번호로 애플리케이션을 구분하게 된다. 할당된 포트 번호를 참고하여 데이터를 애플리케이션에 전달하게 된다.

#### TCP

- HTTP : 80
- SMTP : 25
- POP3 : 110

트랜스포트 계층의 TCP 프로토콜은 **수신자의 데이터가 정확하게 전달되도록 전송 속도를 조절 하거나 도달하지 않은 데이터를 재전송**한다.

#### UDP

- TFTP : 69
- NTP : 123
- SNMP : 161

VoIP나 동영상 스트리밍 서비스와 같이 실시간 통신이 필요하다면 **전송 속도**를 중요시하는 UDP를 사용한다.


![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fc0RjQP%2FbtqvW1PHcxn%2FueUFLSyAGqds1QpXA5adj1%2Fimg.png)

사진 출처 : [https://juni5184.tistory.com/10](https://juni5184.tistory.com/10)

사진을 보면 TCP 같은 경우는 클라이언트와 서버가 서로 메시지를 주고 받지만, UDP의 경우 별다른 처리를 하지 않는다.

### 포트 번호

포트를 재밌는 예시로 설명한 포스팅은 따로 해두었다.

[백엔드 기본기 잡기 2/5](https://minhyeok-rithm.tistory.com/entry/20210418-Network?category=854208)

트랜스포트 계층에는 인터넷 계층에서 전달한 다양한 종류의 패킷이 들어온다. 이 패킷들은 애플리케이션 계층에 있는 애플리케이션들에게 각각 전달되어야 하는데, 이때 데이터를 어떤 애플리케이션의 어느 프로토콜로 전달하지에 대해서는 포트 번호를 보고 판단할 수 있다. 포트 번호의 범위는 0 ~ 65535번까지 사용할 수 있고, `웰 노운 포트(Well-Known Ports)`, `레지스터드 포트(Registered Ports)`, `다이나믹 포트(Dynamic Ports)`의 세 종류로 구분된다. 이 중 웰 노운 포트는 애플리케이션 계층에서 많이 사용되는 대표적인 프로토콜의 수신 포트들이다.

주요 웰 노운 포트들은 서버 측에서 사용하는 포트이다. 서버 측에서 사용하는 포트들은 이미 정해져 있다. 하지만, 다이나믹 포트 같은 경우는 클라이언트가 사용하는 포트이다. 따라서 자동으로 할당되기 때문에 어떤 번호가 사용될지 미리 알 수 없다.

 ![image](https://user-images.githubusercontent.com/78870076/124265445-a4a48e80-db70-11eb-8f97-5dc9d412a419.png)

 사진 출처 : [https://cupjoo.tistory.com/54](https://cupjoo.tistory.com/54)

 클아이언트와 서버가 서로 통신하기 위해서는 먼저 클라이언트가 사용할 포트를 결정하고 이후 서버의 포트에 접속하게 된다. 따라서 만약에 사진에서 HTTP 페이지를 요청 할 때, 80번의 포트로 접속을 요청하고, 접속을 허용하면 페이지를 반환하고 포트를 반납하게 된다.

 서버 측의 포트 번호는 고정되어 있기 때문에 여러 클라이언트가 서버와 통신하는 과정에서 같은 포트로 요청이 몰리게 된다. 서버는 수신 대기를 위해 같은 포트를 사용하는 반면, 접속을 요청한 클라이언트 측은 서로 다른 IP 어드레스와 포트 번호를 사용한다. 서버는 이러한 클라이언트의 IP 어드레스와 포트 번호를 조합하여 클라이언트를 식별할 수 있기 때문에 여러 클라이언트와 통신하는 상황에서도 혼선이 발생하지 않게 된다.

 ### TCP가 정확하게 데이터를 전달하는 방법

 `TCP(Transmission Control Protocol)`는 프랜스포트 계층의 프로토콜의 하나로서 웹이나 이메일, FTP와 같이 정확한 데이터 전달이 필요한 통신에 사용된다. TCP는 데이터 전송에 **신뢰성**을 더하기 위해 데이터를 `세그먼트(Segment)`라는 단위로 분할하고, 전송 속도를 조정하며, 데이터가 제대로 전달되지 않았을 경우 재전송을 하게 된다.


![](https://madplay.github.io/img/post/2018-02-04-network-tcp-udp-tcpip-2.png)

사진 출처 : [https://velog.io/@hidaehyunlee/TCP-와-UDP-의-차이](https://velog.io/@hidaehyunlee/TCP-와-UDP-의-차이)

TCP의 세그먼트는 데이터 본체에 `TCP 헤더`가 붙은 형태로 구성된다. TCP 헤더에는 포트 번호나 일련 번호와 같은 정보가 포함되어 있다.


![](https://nesoy.github.io/assets/posts/20181010/1.png)

사진 출처 : [https://velog.io/@hidaehyunlee/TCP-와-UDP-의-차이](https://velog.io/@hidaehyunlee/TCP-와-UDP-의-차이)


<center>필드</center> | <center>내용</center> 
:--- | :---
Source Port / Destination Port | TCP로 연결되는 가상 회선 양단의 송수신 프로세스에 할당되는 포트 주소
Sequence Number | 송신한 바이트 수
ACK Number | 수신한 바이트 수
Header Length | TCP 헤더의 길이
Window Size | 한 번에 수신할 수 있는 데이터 크기
TCP Checksum | 데이터가 훼손되었는지 확인하기 위한 정보
Urgent Pointer | 긴급 데이터를 처리하기 위함

TCP 헤더  중 `컨트롤 비트(CWR 등등)`는 현재의 통신 상태를 표현하는 플래그 역할을 하며, 통신 상대에게 이 정보를 전달해서 TCP 통신을 제어하는 용도로 사용한다. 9개의 플래그 각각은 1비트 크기를 차지하며 ON/OFF 두 가지 상태를 표현한다.

<center>플래그</center> | <center>역할</center>
:--- | :---
CWR | 통신 경로가 혼잡해서 전송량을 줄여줄 것을 알려준다.
ECE | 통신 경로가 혼잡해서 수신할 수 없을 수도 있다는 것을 알려준다.
URG | 긴급 포인터에서 지정한 데이터를 즉시 처리해야 한다는 것을 알려 준다.
ACK | 이전 동작을 확인했다는 것을 알려준다. 확인 응답 번호와 조합해서 사용된다.
PSH | 수신 데이터를 즉시 애플리케이션 계층에 전달해야 한다는 것을 알려준다.
RST | 이상 상황이 발생하여 접속이 강제 중단되었다는 것을 알려준다.
SYN | 접속을 시작할 때 ON으로 설정한다.
FIN | 데이터 송신이 완료되어 통신을 종료하고 싶다는 것을 알려준다.

TCP 통신은 `커넥션 연결`에서 시작한다. 커넥션을 맺는 과정은 3단계로 진행되기 때문에 이것을 `3방향 핸드세이크`라고 부른다. 커넥션이 맺어지면 데이터를 전송할 수 있는 상태가 되고, 데이터 전송이 긑나면 커넥션을 끊는다.


![](https://www.redeszone.net/app/uploads-redeszone.net/2020/01/ataque-syn-3-way-handshake-655x281.png)

- 통신을 시작하고 싶을 때 `SYN`을 ON으로 설정한다.
- `ACK`는 데이터가 잘 도착했는지 확인 응답을 하기 위한 플래그이다. ACK가 ON으로 설정된 패킷이 응답으로 돌아오지 않으면 제대로 전달되지 않은 것이라고 판단한다.

커넥션을 맺을 때 송신 측과 수신 측은 원활한 통신을 위해 사전에 `일련번호`와 `최대 세그먼트 크기(MSS, Maximum Segment Size)`를 서로 합의하고 조율하는 과정을 거치게 된다. 커넥션을 맺는 과정에서 일련번호는 1씩 증가하는데, 데이터를 전송할 때는 여기에 전송한 데이터의 바이트 수만큼 더 더해진다. 또한, 데이터를 수신한 후에는 수신한 데이터의 바이트 수만큼을 확인 응답 번호에 더하기 때문에 일련번호와 확인 응답 번호를 확인하면 몇 바이트의 데이터를 주고 받았는지 알 수 있다. 인터넷에서 통신하다 보면 패킷 일부가 제대로 전달되지 않거나 패킷 자체는 전달되었더라도 응답 패킷이 전달되지 않는 경우가 발생할 수 있다. 송신 측에서는 일정 시간이 지난 후에도 수신 측으로부터 응답이 오지 않을 경우 송신 실패로 간주하고 최근에 정상적으로 응답을 받은 후부터 데이터를 재전송한다.

앞서 보낸 데이터에 대한 응답을 받은 후에 다음 데이터를 보내는 방식은 통신이 정상적으로 완료되기까지 다소 많은 시간이 소요된다. 대신, 응답을 기다리지 않고 연속된 데이터를 몰아서 보내면 전송 속도를 더 빠르게 향상시킬 수 있다. 단, 확인 응답 번호를 확인하는 데 시차가 발생한다. 연속해서 몰아 보내는 데이터의 양이 너무 많으면 수신 측이 제때 처리하지 못할 수 있다. 그래서 수신 측은 수신한 데이터를 일시적으로 보관할 수 있는 `버퍼(buffer)`라는 저장 영역을 가지고 있다. 수신 측은 TCP 헤더의 `윈도우 사이즈`에 이 버퍼의 크기를 설정하고 송신 측에 통보함으로써 어느 정도의 크기까지 받아 낼 수 있는지를 알려주게 된다.

수신 측은 도착한 패킷들을 버퍼에 쌓아 두는 것과 동시에 이미 버퍼에 쌓인 데이터를 순차적으로 꺼내서 처리하게 된다. 이때 만약 수신 측 컴퓨터의 성능이 낮다면 데이터가 들어오는 속도보다 처리하는 속도가 느려져 문제가 될 수 있다. 그래서 수신 측은 응답을 보낼 때 윈도우 사이즈를 설정하여 현재 어느 정도까지 수신할 수 있는지를 수시로 알려주게 된다. 이런 과정을 `흐름 제어(Flow Control)`라고 한다.

버퍼가 가득 차면 윈도우 사이즈가 0으로 설정되고 데이터 전송은 일단 멈추게 된다. 다시 전송을 재개할 시점을 알기 위해서 송신 측은 탐색 패킷 혹은 `윈도우 프로브(Window Probe)`라고 하는 패킷을 수신 측에 보내게 되고, 수신 측의 응답을 받아 현재 윈도우 사이즈를 확인한 후에 전송 재개 여부를 결정한다.

### UDP가 고속으로 데이터를 전달하는 방법

`UDP(User Datagram Protocol)`는 TCP에 비해 상당히 간단한 프로토콜로서 단순히 데이터를 보내는 역할만 한다. 통신 과정에서 데이터의 손실이 발생할 수 있는데, VoIP와 같은 음성 서비스나 동영상 스트리밍 서비스는 일부 데이터가 누락되거나 왜곡 되더라도 큰 문제가 없기 때문에 UDP를 주로 사용한다.

![image](https://user-images.githubusercontent.com/78870076/124272577-e2f27b80-db79-11eb-8eae-027df29246be.png)

사진 출처 : [https://gyoogle.dev/blog/computer-science/network/UDP.html](https://gyoogle.dev/blog/computer-science/network/UDP.html)

- Length : 헤더 길이와 데이터 길이의 합계
- Checksum : 데이터가 훼손 되었는지 확인하기 위한 정보

TCP에는 없는 기능으로 UDP에는 하나의 패킷을 여러 수신지에 전달하는 `브로드캐스트`와 `멀티캐스트`라는 기능이 있다. 특히, 브로드캐스트는 파일 공유나 DHCP와 같이 네트워크 내의 여러 컴퓨터나 통신 장비와 정보를 교환할 때 사용된다.

실시간 처리가 필요한 온라인 게임에서는 전송 속도가 우선인 `UDP`를 사용하긴 하지만, 데이터 전송의 신뢰성 역시 속도에 못지않게 중요하다. 이런 경우에는 애플리케이션 계층에서 `흐름 제어(Flow Control)`나 `혼잡 제어(Congestion Control)`를 구현해서 부족한 신뢰성을 보완하게 된다.