---
title: 모두의 네트워크 Chap4 - 데이터 링크 계층
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
categories:
- network
toc: true
toc_sticky: true
toc_label: on this page
article_tag1: network
tags:
  - network
---

# 데이터 링크 계층 : 랜에서 데이터 전송하기1



## 데이터 링크 계층의 역할과 이더넷

* 랜에서는 데이터를 주고받으려면 데이터 링크 계층의 기술이 필요하다.

- 데이터 링크 계층은 네트워크 장비 간에 신호를 주고받는 규칙을 정하는 계층으로, 그 규칙들 중 일반적으로 가장 많이 사용되는 규칙이 **이더넷**이다.
- 이더넷은 허브와 같은 장비에 연결된 컴퓨터와 데이터를 주고 받을때 사용한다.

> 허브란?
>
> 물리계층에서 동작하는 네트워크 장비로, 여러개의 포트를 가지고 있어 컴퓨터 여러대와 통신을 할 수 있게 한다.
>
> 또한 허브는 컴퓨터에서 보낸 전기 신호가 허브에 도착하는 동안 노이즈의 영향으로 파형이 변경 될 때, 그 파형을 정상으로 되돌리는 기능도 한다.

* 허브를 사용하는 랜 환경에서는 특정한 컴퓨터 한 대에 데이터를 보내려고 해도 다른 모든 컴퓨터에 전기신호가 전달되어버린다. 이런 경우를 위해 데이터의 내용을 못 보게 하는 확실한 규칙이 이더넷이다.

- 보내려는 데이터에 목적지 정보를 추가해서 보내고 목적지 이외의 컴퓨터는 데이터를 받더라도 무시하게 되어있다.

- 허브는 들어온 데이터를 그대로 모든 포트에 보내기만 한다. 그래서 컴퓨터 여러대가 동시에 데이터를 보낼 경우 충돌(conflict)이 일어난다. 그래서 이더넷은 여러 컴퓨터가 동시에 데이터를 전송해도 충돌이 일어나지 않는 구조로 되어있다.

  > *어떤 구조로 되어있는가?*
  >
  > 데이터가 동시에 케이블을 지나가면 충돌할 수 밖에 없다. 그래서 데이터를 보내는 시점을 늦추는 것이다. 이처럼 이더넷에서 시점을 늦추는 방법을 **CSMA/CD**라고 한다.



- CSMA/CD란?
  - **CS**는 '데이터를 보내려고 하는 컴퓨터가 케이블에 신호가 흐르고 있는지 아닌지를 확인한다는' 규칙이다.
  - **MA**는 '케이블에 데이터가 흐르고 있지 않다면 데이터를 보내도 좋다'는 규칙이다.
  - **CD**는 '충돌이 팔생하고 있는지를 확인한다' 는 규칙이다.
  - 요즘에서는 CSMA/CD가 효율이 좋지 않다는 이유로 잘 쓰이지 않는다. 대신 스위치(switch)라는 네트워크 장비를 사용한다.



## MAC주소의 구조

>  MAC이란? 
>
> 랜 카드를 제조할 때 정해지는 물리적인 주소이다.

랜 카드는 비트(0과 1)을 전기신호로 변환한다. 이러한 랜 카드에는 MAC주소라는 번호가 정해져있다. 제조할때 새겨지기때문에 **물리 주소**라고 부르며, 전 세계에서 유일한 번호로 할당된다.



### MAC주소 규칙

- 48비트 숫자로 구성되어있다.
- 그중 앞 24비트는 랜 카드를 만든 제조사 번호고, 뒤 24비트는 제조사가 랜 카드에 붙인 일련번호이다.

> (00-23-AE)       -      (D9-7A-9A)
>
> 제조사 번호			제조사가 붙인 일련번호



### MAC주소를 사용한 통신

OSI모델이나 TCP/IP 모델에서는 각 계층에 헤더를 붙인다.

OSI모델에서는 데이터 링크 계층에 해당하고, TCP/IP 모델에서는 네트워크 계층에 해당하는데, 이 계층에서 **이더넷 헤더**와 **트레일러**를 붙인다.

- 이더넷 헤더

- 목적지의 MAC주소(6바이트), 출발지의 MAC주소(6바이트), 유형(2바이트) 이렇게 총 14바이트로 되어있다.

- 유형이란? : 이더넷으로 전송되는 상위 계층 프로토콜의 종류를 나타낸다.

  | 유형 번호 | 프로토콜           |
  | :-------- | ------------------ |
  | 0800      | IPv4               |
  | 0806      | ARP                |
  | 8035      | RARP               |
  | 814C      | SNMP over Ethernet |
  | 86DD      | IPv6               |

  유형 번호를 기억할 필요는 없지만 유형에 프로토콜 종류를 식별하는 번호가 들어간다는 것은 기억해두자.

- 트레일러 : FCS(Frame Check Sequence)라고도 하며, 데이터 전송 도중에 오류가 발생하는지 확인하는 용도로 사용한다.

- 이더넷 헤더와 트레일러가 추가된 데이터를 프레임이라고 한다.



### 데이터를 보내는 순서

1. 데이터 링크 계층에서 데이터에 이더넷 헤더와 트레일러를 추가하여 프레임을 만들고, 물리 계층에서 이 프레임 비트열을 전기 신호로 변환하여 네트워크를 통해 전송한다.
2. 허브는 수신한 데이터를 모든 포트의 컴퓨터로 전송ㅎ안다.
3. 하지만 이더넷 규칙에 따라 목적지 포트가 아닌 다른 컴퓨터들은 MAC주소가 다르기 때문에 데이터를 파기한다.
4. 만약 동시에 데이터를 전송한 경우 CSMA/CD방식을 통해 충돌을 해소한다.