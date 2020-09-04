---
title: 자바 - 인터페이스(Interface)
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
  - Interface
---

![java-logo](https://user-images.githubusercontent.com/68311188/92201199-e4e6a200-eeb6-11ea-9f5b-76b79db3564f.png)

# 인터페이스(Interface)

---

> **인터페이스란?**
>
> 개발 코드와 객체가 서로 통신할 때의 **호출 규칙(사용규칙)**을 정의한 것을 뜻한다.
> 만약 인터페이스가 없다면 각 클래스마다 비슷한 기능을 수행하는 메서드들의 사용법을 모두 알고 호출해야 할 것이다.
> 그래서 객체 사용 규칙을 정의하고, 클래스를 정의할 때 인터페이스 규칙에 따라 만든다면 일관된 방법으로 메서드를 호출할 수 있어 코드가 간결해지고 유지보수가 쉬워진다.



## 인터페이스 선언



* 인터페이스 구성요소
  1. 상수 필드 
  2. 추상 메서드

*  인터페이스는 객체로 생성할 수 없기 때문에 생성자를 가질 수 없다! new 명령으로 인스턴스 생성 불가!



### 1) 상수 필드 선언

public static final로 고정되어있다. 생략하더라도 컴파일 과정에서 자동으로 붙는다. 변수 선언과 동시에 특정 값으로 초기화 해야 한다.

`public static final int MAX_VOLUME = 10; `

### 2) 추상 메서드 선언

인터페이스를 통해 호출된 메서드는 최종적으로 **객체**에서 실행된다. 그렇기 때문에 인터페이스 메서드는 리턴 타입, 메서드 이름, 매개 변수만 적고 몸체{}는 없는 추상 메소드로 선언한다. 

인터페이스는 호출 규칙을 의미한다. 규칙은 당연히 공개되어야 한다! 그래서 인터페이스 메서드는 모두 public abstract이며 생략하더라도 컴파일 과정에서 자동으로 붙는다.

```java
public interface RemoteControl{
    public static final int MAX_VOLUME = 10; 
    public abstract void setVolume();
}
```



## 인터페이스 구현 

인터페이스 구현 클래스는 선언부에 implements키워드를 추가하고 인터페이스를 명시한다.  또한 추상 메서드들에 대한 실체 메서드를 반드시 구현해야 한다.

```java
public class Television implements RemoteControl{
    public void setVolume(int volume) {
        System.out.println("현재 TV 볼륨 : " + volume)
    }
}
```

구현 클래스가 작성되면 다음과 같이 new 연산자로 객체를 생성할 수 있다. 

```java
Television tv = new Television();
```

인터페이스 구현 객체를 사용하려면 인터페이스 변수를 선언한 뒤 구현 객체를 대입해야 한다. 객체 주소가 저장된 레퍼런스로 구현클래스가 구현한 메서드에 접근할 수 있다.

```java
RemoteControl rc;
rc = new Television();

rc.setVolume(7);
// 결과  => 현재 TV 볼륨 : 7
```



### 인터페이스 - default메서드

* default 메서드가 생긴 이유
  잘 쓰던 인터페이스의 규칙을 변경하면, 그 인터페이스를 따라 만든 모든 클래스를 변경해야 한다.
  규칙을 마음대로 변경하거나 제거, 추가하는 순간 그 인터페이스를 구현한 모든 클래스에서 컴파일 오류가 발생한다.
  기존 규칙을 변경하되 기존 구현체에는 영향을 끼치고 싶지 않을때 바로 default 메서드 문법을 사용한다. (JDK8에서 추가한 문법)

```java
default void method() {
//구현할 코드가 있으면 작성하고, 없으면 빈 채로 둔다.
}
```

* default 메서드의 단점:
  인터페이스에 선언하는 메서드는 기본이 추상 메서드이기 때문에
   그 인터페이스를 구현하는 클래스는 반드시 해당 메서드를 정의해야 한다.
  그러나 default 메서드는 이미 구현되어 있기 때문에 구현 클래스에서 반드시 정의할 필요는 없다.
  즉,  강제로 정의하게 만들 수 없는 것이 문제이다.

  그래서 default 메서드의 용도는 
  1) 기존에 정의된 인터페이스에 새 규칙을 추가할 때 
  2) 기존 프로젝트에 영향을 끼치지 않기 위함이다.
  따라서 새 인터페이스를 정의할 때는 default의 사용을 자제해야 한다.



### 인터페이스 - static 메서드

* 인터페이스도 static 메서드를 가질 수 있다. 
* 인터페이스의 스태틱메서드는 보통 그 인터페이스를 구현한 객체를 다루는 일을 한다.
* 반드시 클래스명으로 메서드를 호출해야 한다.
* 재정의가 불가능하다!

```java
public interface A {
  // 인터페이스도 static 메서드를 가질 수 있다.
  // => 특정 인스턴스에 종속되지 않고 작업하는 경우에 static 메서드로 정의한다.
  static String m1() {
    // 인스턴스 없이 수행하는 작업을 여기에 작성한다.
    return "안녕하세요!";
  }
}
```

```java
public class Exam0130 {
  public static void main(String[] args) {
  // 인터페이스 이름으로 바로 메서드 호출이 가능하다.
    System.out.println(A.m1());
  }
}

```

## 인터페이스 상속

구현 클래스는 여러개의 규칙(인터페이스)를 이행할 수 있다. 규칙들 중에서 메서드가 겹칠 수 있지만 상관없다.
왜? 어차피 구현된 메서드가 아니기 때문이다!

```java
public interface A {void m1();}
public interface B extends A {void m2();}
public interface C {void m3();}
public interface D {void m2(); void m4();}
public interface D2 {int m3();}
```

```java
public class Exam03 implements B, C, D {
  public void m1() {} // B의 수퍼인터페이스인 A 인터페이스 구현
  public void m2() {} // B와 D 인터페이스 구현
  public void m3() {} // C의 인터페이스 구현 
  public void m4() {} // D의 인터페이스 구현    
    
      public static void main(String[] args) {
    Exam03 obj = new Exam03();
    
    B obj2 = obj;
    obj2.m2(); // 여기에서는 B.m2() 이다.
    obj2.m1();
    
    C obj3 = obj;
    obj3.m3();
    
    D obj4 = obj;
    obj4.m2(); // 여기에서는 D.m2() 이다. 같은 메서드를 여러 인터페이스에서 공유할 수 있다.
    obj4.m4();
}
```

여러개 인터페이스를 구현하지 못하는 경우는 메서드 명과 파라미터가 같지만 리턴 타입이 다른 경우이다.

* 이유?
  클래스는 이름이 같고 파라미터 형식이 다른 메서드를 여러 개 정의할 수 있지만,(오버로딩) 
  이름이 같고, 파라미터 형식도 같은데 리턴 타입만 다른 메서드를 여러 개 정의할 수 없다.

```java
public class Exam04 implements C, D2 {
  // 리턴 타입만 다른 메서드를 여러 개 만들 수 없기 때문에
  // C인터페이스와 D2 인터페이스를 한 클래스에서 동시에 구현할 수 없다!
  // => C 규칙과 D2 규칙을 동시에 만족시킬 수 없다.
  //
  // 다음과 같이 정의하면 컴파일 오류이다!
  public void m3() {} // C의 인터페이스 구현 
  public int m3() {} // D2의 인터페이스 구현 
```

> 인터페이스 간접 구현
> 인터페이스를 구현한 클래스를 상속받은 클래스는 당연히 인터페이스를 구현한 것이 된다.



> **인터페이스와 추상클래스**
>
> 인터페이스를 직접 구현하면 인터페이스에 선언된 모든 메서드를 구현해야 하지만,
> 미리 인터페이스의 몇몇 메서드를 구현한 추상클래스를 상속 받는다면 서브 클래스는 좀 더 쉽게 인터페이스를 구현할 수 있다. 
>
> 즉, 인터페이스의 메서드가 많을 경우, 추상클래스에서 일부 메서드를 미리 구현함으로써 개발자가 좀 더 쉽게 인터페이스를 구현할 수 있게 도와주는 용도로 추상클래스 문법을 사용할 수 있다!