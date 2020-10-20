---
title: 모두의 네트워크 Chap7 - 응용 계층 
layout: single
author_profile: true
read_time: true
categories:
- network
toc: true
toc_sticky: true
toc_label: on this page
tags:
  - network
---

 # 응용 계층의 역할

우리는 웹 페이지를 볼 때는 웹 브라우저를 사용하고, 메일을 보낼때는 메일 프로그램(예 : Outlook)같은 프로그램을 사용한다. 

이러한 애플리케이션들은 응용계층에서 동작하며, **서비스를 요청하는 애플리케이션(클라이언트)**과 **서비스를 제공하는 애플리케이션(서버**)로 나뉜다.

응용계층에서는 클라이언트의 요청을 전달하기 위해 서버가 이해할수 있는 데이터로 변환하고 전송 계층으로 전달하는 역할을 한다.

또한, 클라이언트 측 애플리케이션(웹 브라우저, 메일 프로그램등)이 서버 측 애플리케이션(웹 서버, 메일 서버)과 통신하려면 응용 계층의 프로토콜을 사용해야 한다.

- 주요 응용 계층 프로토콜

| 프로토콜 | 내용           |
| -------- | -------------- |
| HTTP     | 웹 사이트 접속 |
| DNS      | IP주소 해석    |
| FTP      | 파일 전송      |
| SMTP     | 메일 송신      |
| POP3     | 메일 수신      |
