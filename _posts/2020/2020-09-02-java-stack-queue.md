---
title: 자바 - Stack과 Queue
layout: single
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

![java-logo](https://user-images.githubusercontent.com/68311188/92201199-e4e6a200-eeb6-11ea-9f5b-76b79db3564f.png)

------------------------------
# 스택(Stack)

스택의 데이터 저장 방식은 아래와 같다.  
<center><img src = "https://user-images.githubusercontent.com/68311188/91978387-f7e65e80-ed5e-11ea-84fd-69b45fb78324.png"></center>

스택은 LIFO(Last In First Out) 방식으로 데이터를 넣고 꺼낸다.  
**LIFO란?** 후입선출, 제일 나중에 들어온 것을 먼저 꺼내는 방식이다.  
데이터를 넣는 것을 `push`라고 하고, 데이터를 꺼내는 것을 `pop`이라 한다.  
보통 입력한 역순으로 데이터를 꺼내야 하는 상황에서 이 자료구조를 사용한다.

* Stack이 쓰이는 예
  * JVM 스택 메모리 영역에서 메서드 호출을 관리할 때 
  * 웹 브라우저에서 이전 페이지로 따라 올라 갈 때
  * 자바스크립트에서 이벤트를 처리할 때 버블링 단계를 수행(조상) 엘리먼트를 따라 올라가면서 처리하는 것)

  ## Stack의 주요 메서드
  * push(E item) : 주어진 객체를 스택이 넣는다
  * peek() : 스택의 맨 위 객체를 가져온다. 객체를 스택에서 제거하지 않는다.
  * pop() : 스택의 맨 위 객체를 가져온다. 객체를 스택에서 제거한다.

```java
  public static void main(String[] args) throws Exception {
    MyStack<String> stack = new MyStack<>();
    stack.push("aaa");
    stack.push("bbb");
    stack.push("ccc");
    stack.push("ddd");
    stack.push("eee"); // aaa,bbb,ccc,ddd,eee
    print(stack);

    MyStack<String> stack2 = stack.clone();// aaa,bbb,ccc,ddd,eee
    print(stack2);

    System.out.println(stack2.pop()); // eee
    System.out.println(stack2.pop()); // ddd
    System.out.println(stack2.pop()); // ccc
    print(stack2); // aaa,bbb

    System.out.println("-----------------");
    print(stack);

  }
  static void print(MyStack<?> stack) {
    for (int i = 0; i < stack.size(); i++) {
      System.out.print(stack.get(i) + ",");
    }
    System.out.println();
  }
```

# 큐 (Queue)

큐의 자료 구조는 아래와 같다.
<center><img src = "https://user-images.githubusercontent.com/68311188/91979083-2c0e4f00-ed60-11ea-9020-33b8dd76a698.png"></center>

큐(queue) 는 FIFO(First In First Out) 방식으로 데이터를 넣고 꺼낸다.  
**FIFO란?** 선입 선출, 가장 먼저 들어온 것을 먼저 꺼내는 방식이다.  
데이터를 넣는 것을 `offer`라고 하고 목록의 맨 끝에 추가한다.  
데이터를 꺼내는 것을 `poll`이라 하고 목록의 맨 앞의 값을 꺼낸다.  
보통 입력한 순으로 데이터를 꺼내야 하는 상황에서 이 자료구조를 사용한다.

* 예)
  * 등록된 예약을 처리할 때 
  * 네트워킹에서 연결된 순서대로 소켓을 승인하고 처리할 때

  ## Queue의 주요 메서드
  * offer(E e) : 주어진 객체를 넣는다.
  * peek() : 제일 앞에 있는 객체를 가져온다. 객체를 큐에서 제거하지 않는다.
  * poll() : 객체 하나를 가져온다. 객체를 큐에서 제거한다.

```java
  public static void main(String[] args) throws Exception {
    MyQueue<String> queue = new MyQueue<>();
    queue.offer("aaa");
    queue.offer("bbb");
    queue.offer("ccc");
    queue.offer("ddd");
    queue.offer("eee"); // aaa,bbb,ccc,ddd,eee
    print(queue);

    MyQueue<String> queue2 = queue.clone();
    print(queue2); // aaa,bbb,ccc,ddd,eee

    System.out.println(queue2.poll());//aaa
    System.out.println(queue2.poll());//bbb
    System.out.println(queue2.poll());//ccc
    print(queue2); // ddd,eee

    System.out.println("--------------------");
    print(queue);
  }

  static void print(MyQueue<?> queue) {
    for (int i = 0; i < queue.size(); i++) {
      System.out.print(queue.get(i) + ",");
    }
    System.out.println();
  }
```

> Stack과 Queue모두 poll, pop을 통해 값을 꺼내면 본래의 값은 remove되어 없어진다.  
그러므로 출력할때 본래 값을 온전히 두고 출력하고 싶다면, shallow copy가 아닌 deep copy를 통해 값을 복사하여 꺼내는 것이 좋다.