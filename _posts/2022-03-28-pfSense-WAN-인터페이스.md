---
layout: post
title: proxmox 내 pfSense 브릿지(vmbr)가 인터넷에 연결이 안될 경우 해결방법
hide_title: false
feature-img:
thumbnail:
color: black
bootstrap: true
tags: [2022, short, proxmox, pfSense] # 태그는 작성하면 알아서 분류됨
author: ccppoo
---

## 당시 상황

{% include aligner.html images="posts/22-03-28/proxmox-pfsense-1.png" caption="" column=1 row=2 %}

이 그림은 아래에도 다시 나오지만, proxmox 내에 pfsense를 설치해서 Guest OS와 컨테이너들이

pfSense를 통해서 네트워킹을 할 수 있게 만든 것이다.

pfSense를 가상화 했을 때 반드시 모든 VM과 컨테이너들이 다시 켜질 때

start order=1로 해서 제일 먼저 활성화 할 수 있도록 설정해야한다.

PoF(Point of Failure), 즉 위와 같은 구조는 어느 한곳이 망가지면 모든 곳이 망가지는 장애에 매우 취약한 구조다.

Proxmox내 pfSense를 설치하는 것은 외국 커뮤니티에서도 Single Point of Failure 때문에 설왕설래하는 주제이긴 하지만,

VPN과 DNS 구축에 상당한 편리함을 주기 때문에 (모놀로식 서버 구성...ㅋㅋ)

그리고 솔직히 돈만 많으면 랜포트 4개 있는 미니 PC를 구입해서 방화벽을 구축하는 편이 더 쉽긴하지만

현실적이고 금전적인 문제 때문에 이렇게 구성하는 것이다.

## 문제상황 : DHCP, DNS 작동하는데 인터넷 안됨

pfSense를 proxmox 내 가상화 시켜서

호스트 머신의 WAN 인터페이스를 pfSense가 대체해 모든 게스트 OS 또는 컨테이너가

pfSense를 통해서 네트워크를 사용할 수 있도록 설정했을 때 연결이 되었으나 외부망으로 나가지 못하는 경우가 있다.

```ps
>> ping google.com
```

처럼 DNS를 사용하는 작업의 경우 IP 주소는 제대로 받아오나 인터넷에 연결은 안되는

상당히 이상한 상황이 일어나는 경우가 있다.

**DHCP도 작동하고**

**DNS도 작동하는데**

네트워크 연결이 안되는 이상한 상황에 처음 직면하면 뭐라고 검색하기도 억울한 상황에 놓이는데

해결 방법은 의외로 간단하다.

## 해결방안

pfSense의 Network Interface, WAN Interface를 보면 아래와 같이 IP 주소가 잘 작성되었는지 확인해야한다.

{% include aligner.html images="posts/22-03-28/wan_interface.png ,posts/22-03-28/wan_interface2.png" caption="WAN interface를 볼 때 ↑↑" column=1 row=2 %}

내가 직접 설정하지 않았지만, IPv4 Upstream gateway가 다른 IP로 되어있었다는 것을 확인할 수 있었다.

바꾸기 전에는 pfSense 내부의 gateway의 주소로 되어 있었다.

{% include aligner.html images="posts/22-03-28/proxmox-pfsense-1.png" caption="" column=1 row=2 %}

그림으로 그리면 위와 같은 상황이다.

**그래서**

위 '연결된 공유기/gateway/DHCP' 이 부분에 그림 상으로 **192.168.0.33**을 붙여넣고

위 'IPv4 Upstream gateway'이 부분에 그림 상으로 **192.168.0.1**을 넣으면 된다.

상식적으로 토폴로지만 잘 알고 있으면 이상한 gateway IP가 있으니 바꾸면 되지만,

이 문제는 아무래도 pfSense 자체가

공유기나 방화벽 전용 PC, Mini PC에 가상화가 아닌 베어매탈에 설치되는 것이 보편적이기 때문에

위 그림과 같이 VM 내 가상 내부망의 DHCP를 자처함과 동시에 자신의 인터넷 공급자인, Upstream Gateway로부터 또 다시 IP를 받는 것이기 때문에

자동으로 값을 넣어준 pfSense 자체 문제라기 보단, 가상머신에 설치하는 나와 여러분의 문제다.

## 왜 DNS와 DHCP는 정상적으로 작동했을까?

우선 DNS의 경우 Upstream으로부터 상속 받아오도록 설정(아마 default 값)했기 때문에

정상적으로 DNS 쿼리는

VM2(10.1.1.22) → 10.1.1.1 ( NAT ) 192.168.0.33 → 192.168.0.1

순서로 갔기 때문에 정상적으로 작동했을 것이고

DHCP는 뭐... 일단 네트워크를 pfSense가 연결되어 있는 vmbr1(브릿지)를 통해서 공급했기 때문에

정상적으로 작동했을 것이다.

하지만, Upstream Gateway IP가 내부망 Gateway로 값이 적혀 있어서 (즉 원래 ISP에서 제공하는 GateWay IP가 있었어야 한다)

인터넷이 연결이 안되었던 것이다.

끝!

\# proxmox pfSense
