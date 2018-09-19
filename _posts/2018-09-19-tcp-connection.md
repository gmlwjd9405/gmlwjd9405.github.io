---
layout: post
title: '[Network] TCP 3-way handshaking과 4-way handshaking'
subtitle: 'TCP의 연결 설정 과정을 이해한다.'
date: 2018-09-19
author: heejeong Kwon
cover: '/images/network/3way-handshaking-main2.png'
tags: network tcp 3-way-handshaking
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

## Goal
> - TCP(Transmission Control Protocol)의 개념을 이해한다.
> - TCP의 특징을 이해한다.
> - TCP의 3-way handshaking를 이해한다.
> - TCP의 4-way handshaking를 이해한다.


## TCP(Transmission Control Protocol)란
### TCP란
TCP는 네트워크 계층 중 **전송 계층에서 사용하는 프로토콜** 로서, 장치들 사이에 논리적인 접속을 성립(establish)하기 위하여 연결을 설정하여 <span style="background-color: #e1e1e1">**신뢰성을 보장하는 연결형 서비스**</span> 이다.
* ![](/images/network/tcp-virtual-circuit.png){: width="450" height="330"}

### TCP의 특징
* 인터넷 상에서 데이터를 메세지의 형태(**세그먼트** 라는 블록 단위)로 보내기 위해 IP와 함께 사용하는 프로토콜이다.
  * TCP와 IP를 함께 사용하는데, IP가 데이터의 배달을 처리한다면 TCP는 패킷을 추적 및 관리한다.
* **연결형 서비스로** 가상 회선 방식을 제공한다.
  * 3-way handshaking과정을 통해 연결을 설정하고, 4-way handshaking을 통해 연결을 해제한다.
* 흐름제어 및 혼잡제어를 제공한다.
  * 흐름제어
    * 데이터를 송신하는 곳과 수신하는 곳의 데이터 처리 속도를 조절하여 수신자의 버퍼 오버플로우를 방지하는 것
    * 송신하는 곳에서 감당이 안되게 많은 데이터를 빠르게 보내 수신하는 곳에서 문제가 일어나는 것을 막는다.
  * 혼잡제어
    * 네트워크 내의 패킷 수가 넘치게 증가하지 않도록 방지하는 것
    * 정보의 소통량이 과다하면 패킷을 조금만 전송하여 혼잡 붕괴 현상이 일어나는 것을 막는다.
* 높은 신뢰성을 보장한다.
* UDP보다 속도가 느리다.
* 전이중(Full-Duplex), 점대점(Point to Point) 방식이다.
  * 전이중
    * 전송이 양방향으로 동시에 일어날 수 있다.
  * 점대점
    * 각 연결이 정확히 2개의 종단점을 가지고 있다.
  * 멀티캐스팅이나 브로드캐스팅을 지원하지 않는다.
* 연속성보다 신뢰성있는 전송이 중요할 때에 사용된다.


## TCP의 연결 설정 및 해제 과정
### 3-way handshake 란
TCP 통신을 이용하여 데이터를 전송하기 위해 네트워크 <span style="background-color: #e1e1e1">**연결을 설정(Connection Establish)** 하는 과정</span>
* 양쪽 모두 데이터를 전송할 준비가 되었다는 것을 보장하고, 실제로 데이터 전달이 시작하기 전에 한 쪽이 다른 쪽이 준비되었다는 것을 알 수 있도록 한다.
  * 즉, TCP/IP 프로토콜을 이용해서 통신을 하는 응용 프로그램이 데이터를 전송하기 전에 먼저 정확한 전송을 보장하기 위해 상대방 컴퓨터와 사전에 세션을 수립하는 과정을 의미한다.
* 예를 들어, A 프로세스(Client)가 B 프로세스(Server)에 연결을 요청
* ![](/images/network/3-way-handshaking.png)
<!-- {: width="450" height="250"} -->

1. A -> B: SYN
  * 접속 요청 프로세스 A가 연결 요청 메시지 전송 (SYN)
  * 송신자가 최초로 데이터를 전송할 때 Sequence Number를 임의의 랜덤 숫자로 지정하고, SYN 플래그 비트를 1로 설정한 세그먼트를 전송한다.
  * PORT 상태 - B: LISTEN, A: CLOSED
2. B -> A: SYN + ACK
  * 접속 요청을 받은 프로세스 B가 요청을 수락했으며, 접속 요청 프로세스인 A도 포트를 열어 달라는 메시지 전송 (SYN + ACK)
  * 수신자는 Acknowledgement Number 필드를 (Sequence Number + 1)로 지정하고, SYN과 ACK 플래그 비트를 1로 설정한 세그먼트를 전송한다.
  * PORT 상태 - B: SYN_RCV, A: CLOSED
3. A -> B: ACK
  * PORT 상태 - B: SYN_RCV, A: ESTABLISHED
  * 마지막으로 접속 요청 프로세스 A가 수락 확인을 보내 연결을 맺음 (ACK)
  * 이때, 전송할 데이터가 있으면 이 단계에서 데이터를 전송할 수 있다.
  * PORT 상태 - B: ESTABLISHED, A: ESTABLISHED

### 4-way handshake 란
TCP의 <span style="background-color: #e1e1e1">**연결을 해제(Connection Termination)** 하는 과정</span>
* 예를 들어, A 프로세스(Client)가 B 프로세스(Server)에 연결 해제를 요청
* ![](/images/network/4-way-handshaking.png)
<!-- {: width="450" height="250"} -->

1. A -> B: FIN
  * 프로세스 A가 연결을 종료하겠다는 FIN 플래그를 전송
  * 프로세스 B가 FIN 플래그로 응답하기 전까지 연결을 계속 유지
2. B -> A: ACK
  * 프로세스 B는 일단 확인 메시지를 보내고 자신의 통신이 끝날 때까지 기다린다. (이 상태가 TIME_WAIT 상태)
  * 수신자는 Acknowledgement Number 필드를 (Sequence Number + 1)로 지정하고, ACK 플래그 비트를 1로 설정한 세그먼트를 전송한다.
  * 그리고 자신이 전송할 데이터가 남아있다면 이어서 계속 전송한다.
3. B -> A: FIN
  * 프로세스 B가 통신이 끝났으면 연결 종료 요청에 합의한다는 의미로 프로세스 A에게 FIN 플래그를 전송
4. A -> B: ACK
  * 프로세스 A는 확인했다는 메시지를 전송


<mark>참고</mark>  ***포트(PORT) 상태 정보***
* **CLOSED**: 포트가 닫힌 상태
* **LISTEN**: 포트가 열린 상태로 연결 요청 대기 중
* **SYN_RCV**: SYNC 요청을 받고 상대방의 응답을 기다리는 중
* **ESTABLISHED**: 포트 연결 상태

<mark>참고</mark>  ***TCP Header 안의 플래그 정보***
* TCP Header에는 CONTROL BIT(플래그 비트, 6bit)가 존재하며, 각각의 bit는 "URG-ACK-PSH-RST-SYN-FIN"의 의미를 가진다.
  * 즉, 해당 위치의 bit가 1이면 해당 패킷이 어떠한 내용을 담고 있는 패킷인지를 나타낸다.
* **SYN(Synchronize Sequence Number)**
  * 연결 설정 / 000010
  * Sequence Number를 랜덤으로 설정하여 세션을 연결하는 데 사용하며, 초기에 Sequence Number를 전송한다.
* **ACK(Acknowledgement)**
  * 응답 확인 / 010000
  * 패킷을 받았다는 것을 의미한다.
  * Acknowledgement Number 필드가 유효한지를 나타낸다.
  * 양단 프로세스가 쉬지 않고 데이터를 전송한다고 가정하면 최초 연결 설정 과정에서 전송되는 첫 번째 세그먼트를 제외한 모든 세그먼트의 ACK 비트는 1로 지정된다고 생각할 수 있다.
* **FIN(Finish)**
  * 연결 해제 / 000001
  * 세션 연결을 종료시킬 때 사용되며, 더 이상 전송할 데이터가 없음을 의미한다.


<!-- # 관련된 Post
* Eclipse에서 Spring MVC 프로젝트 생성하기에 대해 알고 싶으시면 [Eclipse에서 Spring MVC 프로젝트 생성하기](https://gmlwjd9405.github.io/2018/05/07/spring-project-eclipse-setting.html)를 참고하시기 바랍니다. -->


# References
> - [http://needjarvis.tistory.com/157](http://needjarvis.tistory.com/157)
> - [http://hyeonstorage.tistory.com/286](http://hyeonstorage.tistory.com/286)
