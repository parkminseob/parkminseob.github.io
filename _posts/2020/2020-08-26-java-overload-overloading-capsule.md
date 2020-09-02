---
title: "자바 - 오버로딩, 오버라이딩, 캡슐화"
toc : true
categories:
  - Java
tags:
  - Java
  - Java basic
  - OOP
---

![java-logo](https://user-images.githubusercontent.com/68311188/91867408-9797e400-ecae-11ea-846c-22adf8b1d152.jpg)

* 객체지향을 잘 하고싶다면 모든 인스턴스를 사람처럼 생각해보자.  
`obj1.print3();`  
야 obj야 너 print3(); 해봐! (사람에게 일 시키듯)  
하지만 초보 개발자에게는 피연산자와 연산자 관점에서 프로그래밍 하는것이 더 직관적이고 실력 향상에 도움이 된다. 그러므로 개념이 잘 정립되지 않은 상태에서 인스턴스를 사람으로 생각하는건 도움이 되지 않는다.  
코딩레벨이 오르면 이야기를 쓰듯 코딩을 하게된다. 프로그램 하나가 메소드의 일대기를 쓰는것이다. (이렇게 생각하니까 프로그래밍도 그림과 같이 하나의 창작물이란 생각이 든다. 역시 세상의 모든 분야는 그렇게 멀리 떨어져있지 않다는 걸 새삼 느낀다.)

* 상속파트는 꼭 UML로 도식화 하여 이해해보자!

# 오버로딩
----------------------
* 오버로딩(많이 싣다)  
메소드 오버로딩이란 같은 일을하는 메소드에 대해서 파라미터는 다르지만 같은 이름을 부여함으로서 프로그래밍에 일관성을 제공하는 문법이다. 

```java
public class A {
static public void m() {
    System.out.println("m()");
  }
  // 파라미터의 타입이나 개수가 다르지만 이름이 같은 메서드를 여러 개 만들 수 있다.
  static public void m(int a) {
    System.out.println("m(int)");
  }

  static public void m(String a) {
    System.out.println("m(String)");
  }

  static public void m(String a, int b) {
    System.out.println("m(String,int)");
  }

  static public void m(int a, String b) {
    System.out.println("m(int,String)");
  }
    }
```

상속관계에서도 오버로딩이 있을 수 있다.
```java
public class B extends A {
  static void m(int a, int b, int c) {
    System.out.println("m(int,int,int)");
  }
  파라미터 형식이 다르기 때문에 오버로딩이다.
```
=> 즉 서브클래스에서 수퍼클래스에 있는 메서드와 같은 이름의 메서드를 만들어도 오버로딩이다. **파라미터 형식은 달라야한다!!**

* 대표적인 오버로딩 메서드
	 println
     Integer

# 오버라이딩
-------------------------
* 오버라이딩(재정의하다, 덮어쓰다)  
상속받은 메소드를 서브클래스 역할에 맞게끔 재정의하는 문법이다.  
오버라이딩은 기존 수퍼클래스의 메소드를 **덮어쓰는 것이 아니라** 재정의 하는 것이다!  
그래서 서브클래스에서 기존 수퍼클래스의 메소드를 불러낼 방법은 없지만 서브클래스의 생성자(super();)에서 불러낼 방법은 있다.

* 오버라이딩을 하기 위해서는 매서드 시그니쳐(method signature)가 일치해야한다.  
=> 메서드 시그너처(method signature) = 함수 프로토타입(function prototype)  
=> 메서드명, 파라미터타입/개수, 리턴타입이 일치해야 한다는 뜻

* annotation 문법  
`@Override`  
몇백개의 클래스와 소스파일을 만지다 보면 자잘한 실수는 찾기 힘들어진다. 그때 메소드를 재정의하며 오버라이딩 할 때 개발자의 실수를 막기위해 생긴 아주 특별한 문법이다. (오버라이딩이 오버로딩이 되지 않도록!)

아래 예제를 보자
```java
public class A {
  public void m() {
    System.out.println("A의 m() 호출!");
  }
}
---------------------------------------
public class A2 extends A {
  @Override // 컴파일러에게 오버라이딩을 제대로 했는지 검사하라고 명령한다.
  public void m() {
    System.out.println("A2의 m() 호출!");
  }

  public void x() {
    System.out.println("A2에서 추가한 메서드 x()");
  }
}
---------------------------------------
public class A3 extends A2 {
  public void y() {
    System.out.println("A3에서 추가한 메서드 y()");
  }
}
```
아래 예제는 위 메서드를 호출하는 로컬소스파일이다.
```java
public class Exam01 {
  public static void main(String[] args) {
    A a = new A();
    a.m(); // A의 멤버 호출. OK!
    //((A2)a).x(); // A 객체를 A2 객체라 우기면, 컴파일러는 통과! 실행은 오류!
    System.out.println("--------------------");
    
    A2 a2 = new A2();
    a2.m(); // A2가 수퍼 클래스인 A의 메서드 호출! OK!
    a2.x(); // A2의 메서드 호출! OK!
    System.out.println("----------------------");
    
    A a3 = new A2(); 
    a3.m(); // A2의 m() 호출. 
    	}
    }
    
    -----------------------------------------------
    public class Exam02 {
  public static void main(String[] args) {
    A a = new A3();
    a.m(); // A2의 m() 호출 
    System.out.println("--------------------");
  }
}
    
```
그림으로 표현하면 다음과 같다.
![](https://images.velog.io/images/minseobcms/post/54c0a182-c50c-4dea-98dc-9b71eb8279d5/20200812_202450.jpg)


- 오버라이딩이 되지 않은 예! (annotation의 중요성)
```java
public class B {
  void m(int a) {
    System.out.println("B의 m()");
  }
}
----------------------------
public class B2 extends B {
 
  void m(float x) {
    //이것은 오버라이딩이 아니라 오버로딩이 된 것이다.
    //즉 float 파라미터를 받는 m() 메서드가 추가된 것이다.
    //그런데 개발자는 오버라이딩(재정의)을 했다고 착각하고 사용할 것이다.
    //메서드 위에 @Override 명령을 붙여준다.
    System.out.println("B2의 m()");
  }
}
```
### 오버라이딩 접근범위
--------------------------
멤버의 접근 범위  
private      : 같은 클래스  
(default)    : 같은 클래스 + 같은 패키지  
protected    : 같은 클래스 + 같은 패키지 + 서브 클래스  
public       : 모두  

* 일단 메소드는 무조건 private으로 만든 다음에 오로지 공개해야 할 것만 public으로 만든다. 오버라이딩에서 접근범위를 확대할 순 있지만 접근범위를 좁힐 수는 없다.

#### final
* 클래스에 붙이면 상속 받을 수 없다.  
* 메서드에 붙이면 서브클래스에서 오버라이딩 할 수 없다.  
* final 필드는 인스턴스마다 개별적으로 관리하지 않기 때문에 보통 스태틱으로 만든다.  
* 파라미터 값에 final을 쓸 수 있다.
`public void m1(final int a) {}`
파라미터는 메서드가 호출될 떄 외부의 값을 받는 용도의 변수다. 메서드 안에서 파라미터 값을 임의로 변경하게 되면 처음 받은 파라미터 값을 사용하지 못하는 상황이 발생한다. 그래서 이런상황을 피하고자 보통 실무에서 파라미터를 final로 선언한다. 이클립스 속성란에 세이브 액션에서 파라미터값에 final을 기본으로 주는 설정도 있으니 그 중요성을 알 수 있다.

* 한국에서는 테스트를 꼼꼼히 하지 않는다. 테스트는 유저들의 몫.. 
 알파테스트 : 원래 의도한대로 기능이 돌아가는지 시나리오를 써서 테스트 하는 것
 베타테스트 : 사용자들에게 배포하여 테스트 해보는 것
    
### this. 과 super.
---------------------
```java
public class D {
  void m() {
    System.out.println("D의 m()");
  }
}

public class D2 extends D {
  @Override
  void m() {
    System.out.println("D2의 m()");
  }

void test() {
	this.m();
    super.m();
}
}
```

* this.메서드() 호출?
현재 클래스로부터 호출할 메서드를 찾는다.  
현재 클래스에 메서드가 없으면 수퍼 클래스에 메서드를 찾는다.  메서드를 찾을 때 가지 최상위 클래스로 올라간다.  

* super.메서드() 호출?
수퍼 클래스로부터 호출할 메서드를 찾는다.  
수퍼 클래스에도 메서드가 없으면 그 상위 클래스로 올라간다.  오버라이딩 하기 전의 메서드를 호출하고싶을 때 유용하다!

위 코드 예제와는 다르지만 그림으로 표현하면 다음과 같다.
![](https://images.velog.io/images/minseobcms/post/416b5034-631c-48b9-bc72-5d8fd9c259ca/20200812_195441.jpg)


# 캡슐화
-------------------------------
* 캡슐화가 필요한 이유
	아래 예제에 한 **환자**의 데이터를 입력했다.

```java
public static void main(String[] args) {
    Customer c1 = new Customer();
    c1.name = "홍길동";
    c1.age = 300;
    c1.weight = 100;
    c1.height = -50;
  }
```
이 값들은 인스턴스 변수에 들어갈 만한 값이긴 하나 환자라는 데이터로서는 유효하지 않다. 추상화가 무너졌다! 이를 방지하기 위해 캡슐화가 필요하다.  
 => 즉 캡슐화란, 인스턴스 변수에 추상화 목적에 맞는 유효한 값만 넣을 수 있도록 외부 접근을 제한하는 문법이다. 고수들의 프로그래밍은 방어적이다. 보안이 잘 되어있는 만큼 정교하다는 뜻!

## 세터(setter)와 게터(getter)
-------------------------------
개발자가 인스턴스 필드에 직접 접근하지 못하도록 인스턴스 필드를 private로 선언해준다. 아래 예제는 국영수 점수를 입력해 합계와 평균을 자동 계산하는 프로그램이다.

```java
class Score {
	private String name;
    private int kor;
    private int eng;
    private int math; 
    
    
 void compute() {
  this.sum = this.kor + this.eng + this.math;
  this.aver = this.sum / 3f;
     }
  }
```
필드의 값을 직접 설정할 수 없으니 필드값을 설정하는 메서드를 따로 만들어준다. 보통 필드 값을 설정하는 메서드는 set필드명()으로 이름짓는다.  
이런 메서드를 세터(setter)라고 부른다. 외부에서 호출할 수 있게 public으로 선언한다. 한국어 점수를 입력하는 세터메소드를 보자.
```java
  public void setKor(int kor) {
    this.kor = kor;
    this.compute();
  }
  ```
이제 로컬에서 세터를 통해 값을 입력하는 것이 가능해졌다. 값을 입력했으면 조회할 수도 있어야한다. 조회하기 위한 용도로 사용하는 메서드의 이름은 get필드명()으로 짓는다. 이런 메서드를 게터(getter)라고 부른다. 게터 또한 public으로 설정한다.
```java
public int getKor() {
    return this.kor;
  }
  ```
 하지만 유효성 검사를 하지 않았기 때문에 여전히 인스턴스 필드에 잘못된 값을 넣을 수가 있다. 세터 메서드에 유효성 검사를 해준다.
 ```java
   public void setKor(int kor) {
    if (kor <= 0 || kor >= 100) { 
      this.kor = 0;
      return;
    }
    this.kor = kor;
  }
 ```
**하지만!!!** 실무에서는 세터에서 유효 값을 검증하는 코드를 잘 넣지 않는다. 그냥 따로 인스턴스 필드의 값을 검증하는 메서드를 추가하여 처리한다. 그래서 실무에서의 세터메서드는 인스턴스 변수에 그냥 값을 넣는 경우가 많다. 이에 세터게터를 나누는 것에 회의감을 느끼는 개발자들도 있으나, 대부분 개발자들은 메서드에 기타 코드를 추가할 경우를 대비한 확장성을 고려해 세터게터를 쓰는 방법을 선호한다.
  







