---

title: 자바 - 익명클래스(anonymous class)
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

# 익명 클래스

## 문법

익명 클래스는 이름이 없는 클래스를 말한다.  이름이 없기에 클래스를 정의하자 마자 바로 인스턴스를 생성해야 한다. 일반적으로 인스턴스를 딱 한 개만 만들 때 익명 클래스로 정의한다.

익명클래스를 만들기 위해선 조건이 있다.

**어떤 클래스를 상속하거나 인터페이스를 구현해야 한다.**

문법은 다음과 같다.

```
[상속]
수퍼클래스 변수 = new 수퍼클래스(){...};

[구현]
인터페이스 변수 = new 인터페이스(){...};
```

* 여기서 수퍼클래스 변수는 이름이 없는 익명클래스 객체를 참조하고, 인터페이스 변수는 이름이 없는 구현 객체를 참조하게 된다.

* 익명클래스는 하나의 실행문이므로  반드시 세미콜론(;)을 붙여줘야 한다!



## 익명클래스 - 클래스 상속

```java
public class Exam0210 {
  // 클래스는 스태틱으로 선언해야만 스태틱 멤버에서 접근할 수 있다.
  static abstract class A {
    public abstract void print();
  }

  public static void main(final String[] args) {
    // 클래스를 상속 받는 익명 클래스 만들기
    // 문법:
    // => 클래스명 레퍼런스 = new 클래스명(생성자 파라미터, ...) {};
    //
    A obj = new A() {
      @Override
      public void print() {
        System.out.println("Hello2!");
      }
    };
    obj.print();
  }
}
```



## 익명클래스 - 인터페이스 구현

```java
public class Exam0120 {
  // 인터페이스의 경우 static으로 선언하지 않아도 스태틱 멤버에서 사용할 수 있다.
  interface A {
    void print();
  }

  public static void main(final String[] args) {
    // 익명 클래스로 인터페이스 구현하기
    // 문법:
    // => 인터페이스명 레퍼런스 = new 인터페이스명() {}
    //
    A obj = new A() {
      @Override
      public void print() {
        System.out.println("Hello!");
      }
    };

    obj.print();
  }
}
```

