---
title: Unicast, Broadcast, Multicast, Anycast
author: cotes
date: 2025-04-15 00:34:00 +0800
categories: [Study, CS]
tags: [Study, CS, Network]
---

![Image](https://github.com/user-attachments/assets/f3b7132a-9dc1-4515-97b4-c01e3988f2a1)

출처: https://sharplee7.tistory.com/105

<br/>

---

<br/>

> ## Unicast

![Image](https://github.com/user-attachments/assets/ced86126-037d-43fe-8283-3b5313fa6754)

<br/>

- **one-to-one - 출발지와 목적지가 정확해야 하는 일대일 통신**
- 대상의 MAC address가 아닐 시 drop하기 때문에 다른 PC들의 성능을 저하하지 않는다.

<br/>

---

<br/>

> ## Broadcast

![Image](https://github.com/user-attachments/assets/b3f6123c-7cdd-4a84-8127-6cfdd6f72c63)

<br/>

- **one-to-all - 같은 네트워크에 있는 모든 장비들에게 보내는 통신**
- 해당 네트워크의 모든 PC들이 신호를 받으므로 수신 PC들이 수신 내용이 필요한 정보인지 확인하는 과정에서 CPU를 사용하게 되어 오버헤드가 발생한다. 따라서 과도한 브로드캐스트는 네트워크 및 PC 성능을 떨어뜨릴 수 있다. 
- Unicast로 통신하기 전 주로 상대방의 정확한 위치를 알기 위해 사용된다. 
- 로컬 네트워크 내에서 모든 호스트에 패킷을 전달해야 할 때 사용한다.


<br/>

---

<br/>

> ## Multicast

![Image](https://github.com/user-attachments/assets/a8f56904-2120-4cfa-bd57-08f36d097c18)

<br/>

- **one-to-many - 그룹에 속한 다수의 호스트로 패킷을 전송하기 위한 통신** 
- 여러명에게 보내야 할 경우에 사용하는 방식으로 유니캐스트와 브로드캐스트를 합쳐놓은 듯한 개념이다.
유니캐스트는 수신 PC가 많을수록 네트워크 부하가 커지며, 브로드캐스트는 해당 네트워크 전체에 보내므로 관련없는 PC에서는 CPU 사용률이 증가하는 문제점이 있다.
- 사내 방송이나 증권 시세 전송과 같이 단방향으로 다수에게 동시에 같은 내용을 전달해야 할 때 사용한다. 
- 라우터나 스위치에서 기능을 지원해야만 사용할 수 있다.



<br/>

---

<br/>

> ## Anycast

![Image](https://github.com/user-attachments/assets/137a3db4-3c7b-447d-8207-ff93b04b6366)

<br/>

- **one-to-any - 가장 가까운 Node와 통신**
- 유니캐스트와 비슷하지만 다른 네트워크에 연결된 수신 가능한 장비 중에서 가장 가까운 한 노드에만 전송한다.
동일한 애니캐스트 address가 서로 다른 node들의 Interface에 할당되어 있을 때, 해당 애니캐스트 address로 IPv6 Packet을 전송하면 Routing Protocol의 알고리즘에 따라 가장 가까이에 있다고 판단되는 node의 Interface로 전달된다. 
- 가장 가까운 게이트웨이를 찾는 애니캐스트 게이트웨이 기능에 사용하며 애니캐스트 게이트웨이의 성질을 이용해 가장 가까운 DNS 서버를 찾을 때 사용한다.
- IPv4에서 일부 기능 구현, IPv6 모두 구현이 가능하다. 


* 애니캐스트의 목적
    - 트래픽 분산
    - 네트워크 이중화
    - DDos 공격이 발생했을 때 서버가 받는 피해 최소화
    - Client와 Server 간의 물리적인 거리를 


<br/>

---

<br/>

> ## 📑 참고 자료
[뭉게뭉게 클라우드:티스토리](https://nice-engineer.tistory.com/entry/네트워킹-Unicast-Broadcast-Multicast-Anycast)