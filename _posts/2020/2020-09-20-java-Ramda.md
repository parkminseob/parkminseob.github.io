---

title: 자바 - 람다(Ramda)
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

# 람다(Ramda)

자바의 코딩 최소단위는 클래스이다. 메소드를 쓰려면 반드시 클래스 안에 메소드를 생성해야만 쓸 수 있다. 이러한 객체지향 코딩에 때문에 간단한 함수위주 코딩을 하고싶었던 자바 개발자들은 불편함을 느꼈다. 그래서 등장한것이 람다문법이다.

람다 문법은 메서드 한개짜리 인터페이스를 구현한 익명 클래스를 람다식을 통해 더 간단히 만들 수 있다.

## 람다 문법 규칙

* 추상메서드를 하나만 갖고있는 인터페이스에 대해 람다 문법으로 익명 클래스를 만들 수 있다.
* 추상메서드가 두개라면 람다 문법으로 구현할 수 없다.
* 여러개의 메서드가 있다 하더라도 추상메서드가 한개면 람다 문법 가능.
* 인터페이스가 아닌 추상 클래스는 람다 구현의 대상이 아니다!!
* 즉 람다는 메서드 한개짜리 인터페이스를 익명클래스로 구현한 문법이다.

### 람다 문법

```java
interface Player {
    void play();
  }

  public static void main(String[] args) {
      // 인터페이스를 구현한 익명클래스
    Player p1 = new Player() {
      @Override
      public void play() {
        System.out.println("익명 클래스");
      }
    };
    p1.play();

      // 익명클래스를 람다식으로 변환
    Player p2 = () -> System.out.println("익명 클래스");
    p2.play();

}
```



### 람다 파라미터

```java
public class Exam0130 {
  interface Player {
    void play(String name);
  }
  public static void main(String[] args) {
    // 1) 파라미터는 괄호() 안에 선언한다.
    Player p1 = (String name) -> System.out.println(name + "님 환영합니다.");
    p1.play("홍길동");

    // 2) 파라미터 타입을 생략할 수 있다.
    Player p2 = (name) -> System.out.println(name + "님 환영합니다.");
    p2.play("홍길동");

    // 3) 파라미터가 한 개일 때는 괄호도 생략할 수 있다.
    Player p3 = name -> System.out.println(name + "님 환영합니다.");
    p3.play("홍길동");
  }
}

```

* 파라미터가 여러개일 때는 괄호를 생략할 수 없다.

```java
public class Exam0140 {
  interface Player {
    void play(String name, int age);
  }

  public static void main(String[] args) {
    // 1) 파라미터는 괄호() 안에 선언한다.
    Player p1 = (String name, int age) -> System.out.printf("%s(%d)님 환영합니다.\n", name, age);
    p1.play("박민섭", 28);

    // 2) 파라미터 타입을 생략할 수 있다.
    Player p2 = (name, age) -> System.out.printf("%s(%d)님 환영합니다.\n", name, age);
    p2.play("최민섭", 27);

    // 3) 파라미터가 여러 개일 때는 괄호를 생략할 수 없다.
    //    Player p3 = name, age -> System.out.printf("%s(%d)님 환영합니다.",name, age);
    //    p3.play("최민섭", 29);
  }
}

```



### 람다 아규먼트(argument) 활용

아규먼트 자리에 lambda 문법을 바로 사용할 수 있다.

```java
public class Exam0312 {
  static interface Player {
    void play();
  }

  static void testPlayer(Player player) {
    player.play();
  }

  public static void main(String[] args) {
   	// 아규먼트에 익명클래스를 둘 수 있다.
    testPlayer(new Player() {
      @Override
      public void play() {
        System.out.println("실행!");
      }
    });
      
    // 익명클래스를 람다로 바꿀 수 있다.
    testPlayer(() -> System.out.println("람다 아규먼트 실행!"));
  }
}

```

아규먼트에 여러 개의 문장이 있는 경우 블럭 {}을 생략할 수 없다.

```java
public class Exam0330 {
  static interface Calculator {
    int compute(int a, int b);
  }
  static void test(Calculator c) {
    System.out.println(c.compute(100, 200));
  }
  public static void main(String[] args) {
    // 여러 문장을 실행하는 경우 블럭{}으로 감싸야 한다.
    test((a, b) -> {
      int sum = 0;
      for(int i = a; i <= b; i++) {
        sum += i;
      }
      return sum;
    });
  }
}

```



## 람다구현 -  메서드 레퍼런스

* 메서드 한 개짜리 인터페이스의 구현체를 만들 때 기존 메서드를 람다 구현체로 사용할 수 있다.
* 단, 인터페이스에 선언된 메서드의 규격(파라미터 타입 및 개수, 리턴 타입)과 일치해야 한다.
* 
* 문법 =>
  * 스태틱 메서드 : **클래스명::메서드명**
  * 인스턴스 메서드 : **인스턴스명::메서드명**

아래 예시는 스태틱 메서드로 람다 구현을 한 것이다.

```java

public class Exam0510 {
  static class MyCalculator{
    public static int plus(int a, int b) {return a + b;}
    public static int minus(int a, int b) {return a - b;}
    public static int multiple(int a, int b) {return a * b;}
    public static int divide(int a, int b) {return a / b;}

  }
  static interface Calculator{
    int compute(int x, int y);
  }
  public static void main(String[] args) {
    Calculator c1 = MyCalculator::plus;
      // 위 코드는 내부적으로 아래와 같다.
      //Calculator c1 = new Calculator() {
      //@Override
      //public int compute(int a, int b) {
       // return MyCalculator.plus(a, b);
      //}
   // };
    Calculator c2 = MyCalculator::minus;
    Calculator c3 = MyCalculator::multiple;
    Calculator c4 = MyCalculator::divide;

    System.out.println(c1.compute(200, 17));
    System.out.println(c2.compute(200, 17));
    System.out.println(c3.compute(200, 17));
    System.out.println(c4.compute(200, 17));
  }
}

```

인터페이스에 정의된 메서드가 생성자의 형식과 일치한다면 메서드 레퍼런스로 생성자를 지정할 수도 있다.

```java
public class Exam0710 {
  static interface ListFactory {
    List create();
  }
  public static void main(String[] args) {
    ListFactory f1 = ArrayList::new;

    // 인터페이스의 메서드를 호출하면
    // 지정된 클래스의 인스턴스를 만든 후 생성자를 호출한다.
    List list = f1.create(); // new ArrayList();

    System.out.println(list instanceof ArrayList);
    System.out.println(list.getClass().getName());
  }
}

```



### 람다 정리 - 인터페이스 구현체를 만드는 다양한 방법

```java
public class Exam0810 {

  interface Factory {
    Object create();
  }

  static class Car {}

  public static void main(String[] args) {
    // 1) 로컬 클래스로 인터페이스 구현체를 만든다.
    class CarFactory implements Factory {
      @Override
      public Object create() {
        return new Car();
      }
    }
    Factory f1 = new CarFactory();
    Car car = (Car) f1.create();

    // 2) 익명 클래스로 인터페이스를 구현체로 만든다.
    Factory f2 = new Factory() {
      @Override
      public Object create() {
        return new Car();
      }
    };
    Car car2 = (Car) f2.create();

    // 3) 람다로 인터페이스 구현체를 만든다.
    Factory f3 = () -> new Car();
    Car car3 = (Car) f3.create();

    // 4) 기존에 존재하는 메서드로 인터페이스 구현체를 만든다.
    Factory f4 = Exam0810::createCar;
    Car car4 = (Car) f4.create();

    // 5) 기존 클래스의 생성자로 인터페이스 구현체를 만든다.
    Factory f5 = Car::new;
    Car car5 = (Car) f5.create();

    System.out.println("완료!");
  }

  public static Car createCar() {
    return new Car();
  }
}

```

