---
title: 자바 - 디자인 패턴 : 옵저버(Observer Design Pattern)
toc : true
categories:
  - java
tags:
  - design pattern
  - observer
---

![java-logo](https://user-images.githubusercontent.com/68311188/92201199-e4e6a200-eeb6-11ea-9f5b-76b79db3564f.png)

## 정의

**옵서버 패턴**(observer pattern)은 객체의 상태 변화를 관찰하는 관찰자들, 즉 옵저버들의 목록을 객체에 등록하여 상태 변화가 있을 때마다 메서드 등을 통해 객체가 직접 목록의 각 옵저버에게 통지하도록 하는 디자인 패턴이다. 주로 분산 이벤트 핸들링 시스템을 구현하는 데 사용된다. 발행/구독 모델로 알려져 있기도 하다.

쉽게 설명하자면, 한 프로젝트가 완료된 다음 시간이 지난 후 기존 프로젝트에 새 코드를 추가하고자 할 때 어떤 고객은 새로운 기능이 필요하지 않을 수도 있다. 이런 경우, 조건문을 추가하여 기능의  동작 여부를 제어할 수도 있을 것이다. 그러나 조건문을 쓰면 코드가 복잡해지고, 이미 디버깅과 테스트가 완료된 기존 코드를 변경하면 새 버그가 발생할 수도 있다.

이에 대한 해결책으로 기존 코드를 손대지 않고 새 기능을 추가하는 방법 중 하나가 Observer 디자인 패턴으로 설계하는 것이다. 

> Observer는 '관찰자' 라는 뜻으로 능동형 이지만, 실제로는 Listener형태의 수동형으로 돌아간다.



*** 정리**

***\*Observer 디자인 패턴\****은,

\- 특정 객체의 상태 변화에 따라 수행해야 하는 작업이 있을 경우,

 기존 코드를 손대지 않고 손쉽게 기능을 추가하거나 제거할 수 있는 설계 기법이다.

\- ***\*발행(publish)/구독(subscribe) 모델\**** 이라고 부르기도 한다.

\- 발행 측(publisher)에서는 구독 객체(subscriber)의 목록을 유지할 컬렉션을 가지고 있다.

\- 또한 구독 객체를 등록하거나 제거하는 메서드가 있다.

\- 구독 객체를 ***\*리스너(listener)\**** 또는 ***\*관찰자(observer)\**** 라 부르기도 한다.



![image](https://user-images.githubusercontent.com/68311188/95754430-fb65f180-0cdd-11eb-8f6d-c8d2e7b3dc60.png)

## Observer디자인 패턴을 이용한 예제

자동차의 시동을 켜고 끌때의 상황을 가정하여 Observer패턴을 적용해본다.

### CarObserver인터페이스 정의

인터페이스를 정의하여 호출 규칙을 만든다.

```java
public interface CarObserver {
  // 자동차 시동을 켤 때 호출될 메서드
  // => 보통 메서드의 이름은 동사로 시작하는데,
  // 옵저버에게 통지할 때 호출하는 메서드는
  // 명사구의 상태 이름으로 정의할 수 있다.
  void carStarted();

  // 자동차 시동을 끌 때 호출될 메서드
  void carStopped();
}

```

### Car클래스 정의

Car클래스에서 자동차의 상태 변경을 보고받을 관찰자들을 정의하고, 자동차의 행동을 관찰자들에게 보고하는 메서드를 정의한다.

```java
package com.eomcs.design_pattern.observer.after.f;

import java.util.ArrayList;
import java.util.List;

public class Car {

  // 관찰자의 객체 주소를 보관한다.
  List<CarObserver> observers = new ArrayList<>();

  // 자동차의 상태 변경을 보고 받을 관찰자(Observer)를 등록한다.
  public void addCarObserver(CarObserver observer) {
    observers.add(observer);
  }

  // 자동차의 상태 변경을 보고 받는 관찰자를 제거한다.
  public void removeCarObserver(CarObserver observer) {
    observers.remove(observer);
  }

  public void start() {
    System.out.println("시동을 건다.");

    // 자동차의 시동을 걸면,
    // 등록된 관찰자들에게 알린다.
    for (CarObserver observer : observers) {
      observer.carStarted();
    }
  }

  public void run() {
    System.out.println("달린다.");
  }

  public void stop() {
    System.out.println("시동을 끈다.");

    // 자동차의 시동을 끄면,
    // 등록된 관찰자들에게 보고한다.
    for (CarObserver observer : observers) {
      observer.carStopped();
    }
  }
}
```

### 기능 추가

자동차에 여러가지 기능을 추가하려면, 각 기능을 하는 클래스들을 따로 정의한다.

오일체크관찰자, 썬루프 관찰자, 안절벨트 관찰자 등..

```java
// 엔진오일 관찰자
public class EngineOilCarObserver implements CarObserver {

  @Override
  public void carStarted() {
    System.out.println("엔진 오일 유무 검사");
  }

  @Override
  public void carStopped() {}

}

```



```java
// 전조등 관찰자
public class LightOffCarObserver implements CarObserver {

  @Override
  public void carStarted() {}

  @Override
  public void carStopped() {
    System.out.println("전조등을 끈다.");
  }

}
```


### Test

기능을 추가할때마다 메서드를 추가하고, 차가 잘 굴러가는지 테스트한다.

```java
public class Test01 {
  public static void main(String[] args) {
    Car car = new Car();

    car.addCarObserver(new EngineOilCarObserver());
    car.addCarObserver(new LightOffCarObserver());

    car.start();

    car.run();

    car.stop();
  }
}
```