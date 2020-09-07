---
title: 자바 - 반복자(Iterator)
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
categories:

- Java
toc: true
toc_sticky: true
toc_label: on this page
article_tag1: Java
article_tag2: basic
tags:
  - Java
  - Java basic
---

![java-logo](https://user-images.githubusercontent.com/68311188/92201199-e4e6a200-eeb6-11ea-9f5b-76b79db3564f.png){:.aligncenter}

# 반복자(Iterator)

------

**반복자(이하 Iterator)패턴이란?**

* 객체 목록을 관리하는 컬렉션(collection)에서 목록조회 기능을 별도의 객체로 캡슐화하는 설계 기법이다.

* 컬렉션의 관리 방식(data structure)에 상관없이 일관된 목록 조회 방법을 제공할 수 있다.

* 컬렉션을 변경하지 않고도 다양한 방식의 목록 조회 기법을 추가할 수 있다.

> 즉, 반복자는 컬렉션 종류와 관계없이 코드에 일관성을 부여하여 유지보수를 쉽게 만든다.



## Iterator 의 메서드

* hasNext() : boolean타입으로 다음 데이터가 있으면 true, 없으면 false를 리턴한다.
* next() : hasNext() 가 true를 리턴하면 next()로 값을 꺼낸다.
* remove() : next() 메서드로 호출한 데이터를 삭제한다.



### Iterator 예제

```java
Stack<String> stack = new Stack<>();

stack.push("aaa");
stack.push("bbb");
stack.push("ccc");
stack.push("ddd");
stack.push("eee");

Iterator<String> iterator = stack.iterator();

while(iterator.hasNext()){
    System.out.println("iterator.next()");
}

//결과 : eee, ddd, ccc, bbb, aaa

```

