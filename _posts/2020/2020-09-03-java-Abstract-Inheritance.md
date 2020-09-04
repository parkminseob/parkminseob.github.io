---
title: 자바 - 추상클래스(Abstract)
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
  - Inheritance
---

# 추상클래스(Abstract)

----

![java-logo](/home/sub/parkminseob.github.io/assets/images/post/java/java-logo.png)

상속에는 크게 두가지 종류가 있다.

1. 전문화(specialization)
   가장 많이 사용하는 방법으로 수퍼 클래스를 상속 받아 서브 클래스를 만드는 것이다.
   수퍼클래스에 새 특징을 추가하거나 새 기능을 추가하여 더 특별한 일을 수행하는 서브클래스를 만든다.
   그래서 이런 상속을 "특수화/전문화(specialization)"이라 부른다.
2. 일반화(generalization)
   리팩토링 과정에 수행하는 방법이다.
   서브클래스들의 공통 분모를 추출하여 수퍼클래스(추상 클래스)를 정의하는 방법을 말한다.
   그래서 이런 상속을 "일반화/표준화(generalization)"이라 부른다. 이 포스트에선 일반화 상속, 즉 추상클래스에 대해서 설명할 것이다.



## 추상클래스 선언



추상클래스를 선언할 때는 클래스 선언에 `abstract` 키워드를 붙여야 한다. `abstract`를 붙이면 new 연산자를 이용해서 객체를 만들지 못하고, 상속을 통해서만 자식 클래스를 만들 수 있다.

```java
public abstract class Car {
    // 필드
    // 생성자
    // 메서드
}
```

추상클래스도 일반 클래스와 마찬가지로 필드, 생성자, 메서드 선언을 할 수 있다. **new 연산자로 생성자를 직접 호출할 수는 없지만** 서브클래스가 생성될 때 super()를 호출해서 추상 클래스 객체를 생성하므로 추상 클래스도 생성자가 반드시 있어야 한다.

* 추상클래스 Car예제

```java
public abstract class Car {

  public Car() {
    super();
  }

  public void start() {
    System.out.println("시동 건다!");
  }

  public void shutdown() {
    System.out.println("시동 끈다!");
  }

  public void run() {
    System.out.println("달린다.");
  }

}
```

추상 클래스 Car를 상속받은 서브클래스 Sedan과 Truck클래스에서는 필요한 메서드를 재정의 해서 쓸 수 있다.

* Sedan클래스

```java
public class Sedan extends Car {
  @Override
  public void run() {
    System.out.println("쌩쌩 달린다.");
  }

  public void doSunroof(boolean open) {
    if (open) {
      System.out.println("썬루프를 연다.");
    } else {
      System.out.println("썬루프를 닫는다.");
    }
  }
}

```

* Truck 클래스

```java
public class Truck extends Car {
  @Override
  public void run() {
    System.out.println("덜컹 덜컹 달린다.");
  }

  public void dump() {
    System.out.println("짐을 내린다.");
  }
}
```

## 추상메서드 선언



위 예제에서 Sedan과 Truck클래스에서 공통적으로 run()메서드를 사용하는 것을 알 수 있다. 

추상클래스의 목적은 실체를 가진 서브클래스가 공통적으로 가져야 할 필드와 메소드들을 통일하는 데 목적이 있기 때문에 실체가 가지고 있는 메소드의 실행 내용이 동일하다면 추상클래스에 메소드를 작성하는 것이 좋다.

다만 모든 run()메서드가 각 클래스에서 동일한 동작을 하지는 않기 때문에, 추상클래스에서 **추상 메서드**를 정의하는 것이다.

추상 메서드는 abstract키워드와 함께 메서드의 선언부만 있고 메서드 실행 내용인 중괄호 {} 가 없다.

* 추상메서드 정의

```java
public abstract class Car{
	public abstract void run();    
}
```

추상클래스 설계시 하위 클래스가 반드시 실행 내용을 채우도록 강제하고 싶은 메서드가 있을 경우 해당 메서드를 추상 메서드로 선언한다. 서브클래스는 반드시 추상 메서드를 재정의 해서 실행 내용을 작성해야 한다.