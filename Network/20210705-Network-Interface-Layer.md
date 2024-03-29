## 하드웨어와 네트워크 인터페이스 계층

### 네트워크 인터페이스 계층의 역할

`네트워크 인터페이스 계층`은 네트워크의 하드웨어를 제어하는 부분이다. 여기서 말하는 하드웨어에는 `네트워크 어댑터`나 `LAN 케이블`, `광 케이블` 등을 포함한다. 이러한 하드웨어들을 제어하면서 상위의 인터넷 계층이 하위의 하드웨어 동작에 대해서는 신경 쓰지 않고 동작할 수 있도록 만들어주는 것이 네트워크 인터페이스 계층의 역할이다. 참고로, OSI 참조 모델에서는 프로토콜과 같이 소프트웨어와 관련된 부분을 `데이터 링크 계층`이라고 하고, 하드웨어와 관련된 부분을 `물리 계층`이라고 구분한다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FclMoyw%2FbtqCM9z7XEM%2FRITDKUafn38TBieA8wLIr0%2Fimg.png)

사진 출처 : [https://dad-rock.tistory.com/193](https://dad-rock.tistory.com/193)

#### 네트워크 인터페이스 계층의 프로토콜

네트워크 인터페이스 계층의 프로토콜 중에는 전화 회선을 사용해서 원격지와 접속하는 `PPP`와 동축 케이블이나 UTP 케이블을 사용하는 `이더넷`과 같은 프로토콜들이 있다. 그 외에도 IP 어드레스 정보를 이용하여 목적지의 MAC 어드레스를 알아내는 `ARP` 같은 프로토콜도 있다.

![image](https://user-images.githubusercontent.com/78870076/124416526-2a535480-dd92-11eb-9c67-681852da70e1.png)

사진 출처 : [https://mintnlatte.tistory.com/356](https://mintnlatte.tistory.com/356)

#### 네트워크 인터페이스 계층과 하드웨어와의 관계

트랜스포트 계층과 인터넷 계층에서 사용되는 TCP/IP는 특정 하드웨어에 의존하지 않도록 설계되어 있다. 반면, 네트워크 인터페이스 계층은 하드웨어와 떼려야 뗄 수 없는 관계다. 그래서 이더넷과 같은 기술에서는 소프트웨어 사양과 하드웨어 사양이 함께 존재한다.

### MAC 어드레스

네트워크 어댑터(NIC, Network Interface Card)에는 `MAC(Media Address Control)` 어드레스라고 하는 식별 번호가 부여되어 있다. 이 식별 번호는 제조사가  제조 단계부터 붙이는 것으로, 퍼블릭 IP 어드레스와 비슷하게 전 세계의 네트워크 장비들이 서로 구분될 수 있도록 할단된다.

![image](https://user-images.githubusercontent.com/78870076/124416733-9635bd00-dd92-11eb-8661-c225813c4ce8.png)

사진 출처 : [https://mr-zero.tistory.com/23](https://mr-zero.tistory.com/23)

![image](https://user-images.githubusercontent.com/78870076/124416881-e90f7480-dd92-11eb-95e6-549021abffad.png)

사진 출처 : [http://cactus.io/tutorials/ethernet/what-is-a-mac-address](http://cactus.io/tutorials/ethernet/what-is-a-mac-address)

- 네트워크 어댑터나 무선 LAN 클라이언트 장비, 무선 AP 장비들은 제조 과정에서 고유한 MAC 주소를 할당받는다.
- 이더넷으로 데이터를 전송할 때는 패킷에 목적지의 MAC 어드레스 정보를 설정한다.

MAC 어드레스는 유선 LAN의 이더넷이나 무선 LAN 외에도 단거리 통신에 사용되는 블루투스와 같은 다양한 데이터 통신에서 활용된다. 네트워크 인터페이스 계층이 보내는 데이터를 `프레임(Frame)`이라고 부르는데, 이 프레임 안에 송신지와 수신지의 MAC 어드레스 정보가 들어간다.

#### MAC vs IP

MAC 어드레스의 역할은 데이터를 전달할 목적지를 가리킨다는 점에서 IP 어드레스와 유사하다. 다만, IP 어드레스는 최종 목적지가 한번 설정되면 전송 과정 중에 변경되지 않지만, MAC 어드레스는 전송 과정 중에서 통신 경로상에 다음 장비의 어드레스로 교체된다는 점이 다르다.

- MAC 어드레스가 가리키는 곳은 다음 라우터 (새로운 프레임으로 교체)
- IP 어드레스는 변경되지 않음
- MAC 어드레스는 최종 목적지가 아니라 바로 다음 목적지를 가리킴

### 이더넷

유선 LAN은 통신 장비끼리 케이블로 연결하게 되는데, 이때 `이더넷(Ethernet)`라는 규격을 사용한다.

![image](https://user-images.githubusercontent.com/78870076/124416526-2a535480-dd92-11eb-9c67-681852da70e1.png)

수신 측의 네트워크 어댑터는 전압의 높낮이가 변화하는 신호를 받아 0과 1의 디지털 신호로 복호화한다. 이때 전압이 변화하는 타이밍에 맞춰 신호를 분석하려면 어디부터가 시작인지 기준점을 알아야 한다. 그래서 프레임 앞부분에는 `프라앰블(Preamble)`이라고 하는 패턴을 두어 신호의 시작을 알 수 있도록 만들어져 있다.

### 네트워크 허브

유선 LAN에서는 `네트워크 허브`라는 장비에 케이블을 연결해서 네트워크를 구성한다. 일반 가정에서는 인터넷 서비스 업체에서 임대한 초고속 인터넷 라우터를 사용하는 것이 일반적인데, 이 단말기에는 네트워크 허브 기능이 내장되어 있다. 네트워크 허브에는 리피터 허브, L2 스위치, L3 스위치 등이 있다.

#### 네트워크 토폴로지

네트워크의 구성 형태를 `네트워크 토폴로지`라고 부른다. 대표적인 몇 가지 패턴이 있는데, 이더넷은 그 중 스타형에 해당한다.

![image](https://user-images.githubusercontent.com/78870076/124417491-3dffba80-dd94-11eb-86a7-052dd7c29985.png)

사진 출처 : [https://kr.123rf.com/photo_13766028_네트워크-토폴로지의-그림-컴퓨터-네트워크-연결-링-버스-나무-메시-별-라인.html](https://kr.123rf.com/photo_13766028_네트워크-토폴로지의-그림-컴퓨터-네트워크-연결-링-버스-나무-메시-별-라인.html)

#### L2 스위치

`L2 스위치` 혹은 `스위칭 허브`는 오늘날 가장 많이 사용되는 네트워크 중계 기기다. L2는 OSI 참조 모델의 데이터 링크 계층을 의미하고, TCP/IP 모델에서는 네트워크 인터페이스 계층에 해당한다. L2 스위치는 포트에 연결된 각 호스트의 MAC 어드레스를 기억해 두었다가 통신할 당사자 간에만 데이터를 전달하기 때문에 통신 과정에서 다른 호스트가 보내는 패킷 신호와의 충돌을 피할 수 있다. L2 스위치를 사용하면 통신 회선이 통신 당사자 간에만 독립적으로 만들어지기 때문에 여러 통신이 동시에 이루어지더라도 패킷 충돌이 발생하지 않는다.

- 수신지의 MAC 어드레스를 확인한 후에 수신 측의 LAN 포트에만 신호를 보낸다.
- LAN 포트가 독립적으로 구성되어 있어서 동시에 여러 신호를 보낼 수 있다.

#### 브로드캐스트 도메인

`브로드캐스트 도메인(broadcast domain)`이란, 수신지의 주소가 브로드캐스트 어드레스일 때 데이터가 전달되는 범위를 의미한다. 그래서 L2 스위치로 네트워크를 구성하였다면 네트워크 전체가 브로드캐스트 도메인이 된다. 만약 네트워크에 연결된 호스트의 수가 많을 경우, DHCP나 OS의 파일 공유와 같은 브로드캐스트 통신이 자주 사용되면 네트워크 내에 대량의 패킷이 발생해서 망이 혼잡해질 수도 있다.

#### L3 스위치

`L3 스위치`는 OSI 참조 모델 중 네트워크 계층에 해당하고, TCP/IP 모델에서는 인터넷 계층의 기능까지 수행할 수 있는 네트워크 장비다. L3 스위치의 대표적인 기능으로는 `VLAN(Virtual LAN)`을 꼽을 수가 있는데, 이 기능은 LAN을 몇 개의 가상적인 네트워크로 분할해서 통신 효율을 높이기 위해 사용한다. VLAN을 구성하면 브로드캐스트 패킷을 VLAN 내의 호스트 범위로만 제한하여 전달할 수 있다.


![](https://kajabi-storefronts-production.kajabi-cdn.com/kajabi-storefronts-production/blogs/3184/images/WE7opKr4RvuT1sb7AWL6_Screen_Shot_2018-11-12_at_2.24.31_PM.png)

사진 출처 : [https://www.kwtrain.com/blog/vlan-security](https://www.kwtrain.com/blog/vlan-security)

- 같은 장비의 포트에 연결되어 있어도 서로 다른 네트워크처럼 구성할 수 있다.
- VLAN별로 서로 다른 IP 어드레스를 가지는 네트워크를 만들 수 있다.
- VLAN 내의 통신 방식은 L2 스위치와 같다.
- 라우터처럼 라우팅을 하면서 네트워크를 넘나들 수 있다.

### 무선 LAN

전파를 이용해서 네트워크를 구성하는 `무선 LAN`의 정식 규격명은 `IEEE 802.11`이다. 무선 LAN에서는 다른 통신 장비가 전파를 발신하고 있지 않은 것 을 확인한 후, 통신을 시작하는 `CSMA/CA(Carrie Sense Multiple Access with Collision Avoidance)`라는 방식을 사용한다.

### ARP

이더넷이나 무선 LAN으로 데이터를 보내려면 수신 측의 MAC 어드레스를 알고 있어야 한다. 이때 필요한 것이 `ARP(Address Resolution Protocol)`인데, 송신 측 장비는 `요청 패킷`에 수신 측의 IP 어드레스를 설정한 후 네트워크 전체에 브로드캐스트한다. 이어 이 요청을 받은 호스트들 중 수신지의 IP 어드레스가 자신의 IP 어드레스와 동일한 장비는 자신의 MAC 어드레스를 `응답 패킷`에 설정하여 응답하게 된다. 결국, 송신지 장비는 수신지 장비의 IP 어드레스 정보를 사용하여 MAC 어드레스도 알 수 있게 된다.

- ARP 요구 패킷을 브로드캐스트 방식으로 보낸다.
- 이 어드레스로 프레임을 보내면 네트워크 내의 모든 호스트가 받을 수 있다.
- ARP 응답 패킷을 보낸다.

![image](https://user-images.githubusercontent.com/78870076/124419500-93d66180-dd98-11eb-918d-173bb8c15a86.png)

사진 출처 : [https://heavyrainslab.tistory.com/m/6?category=564424](https://heavyrainslab.tistory.com/m/6?category=564424)

ARP 헤더에는 MAC 어드레스를 물어보는 송신지의 어드레스 정보와 MAC 어드레스를 알 수 없는 목적지의 어드레스를 담을 수 있는 필드가 있다. 헤더 내의 오퍼레이션  코드가 1인 경우는 요청을 의미하고, 2인 경우는 응답을 의미한다.

#### 프락시 ARP

`대리 ARP` 혹은 `프락시(proxy) ARP`는 호스트 대신 라우터가 ARP에 대해 응답하는 기능을 말한다. 서브넷 마스크 값이 다른 호스트가 있을 때 이 기능이 필요한데, 통상적인 ARP 요청은 같은 네트워크 내에 있는 호스트들에게는 브로드캐스팅되는 반면, 서브넷 마스크가 다른 호스트에는 전달되지 않기 때문이다. 그래서 그 사이에 있는 라우터가 프락시 ARP 기능으로 자신의 MAC 어드레스를 대신 응답하여 데이터를 중계할 수 있게 만든다.

- 목적지는 같은 네트워크 안에 있으니 ARP로 MAC 어드레스를 알아내자.
- 서브넷이 달라서 ARP 요청이 전달되지 않는다.
- 라우터를 경유해서 데이터가 도착한다.
