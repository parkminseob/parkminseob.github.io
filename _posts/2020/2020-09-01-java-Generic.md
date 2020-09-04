---
title: 자바 - 제네릭(Generic) 문법
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
article_section: Java공부
meta_keywords: Java
tags:
  - Java
  - Java basic
  - Generic
---

![](/home/sub/parkminseob.github.io/assets/images/post/java/java-logo.png)

# 제네릭(Generic)

제네릭의 사전적 의미는 "일반화하다" 이다.
제네릭 문법을 사용하면 단 하나의 클래스로 형변환 없이 특정 타입 전용 클래스로 만들 수 있다. 그래서 범용 박스, 제네릭(Generic)이라고 부른다.

## 제네릭 문법이 없다면?

 아래 예제는 각 타입의 객체를 저장하기 위해 각 타입에 대한 클래스를 정의한 코드이다.

```java
static class MemberBox {
    Member value;
    
    public void set(Member value) {
      this.value = value;
    }
    
    public Member get() {
      return value;
    }
  }

  static class IntegerBox {
    Integer value;
    
    public void set(Integer value) {
      this.value = value;
    }
    
    public Integer get() {
      return value;
    }
  }
  
  public static void main(String[] args) {
    
    // Member 객체를 저장하려면 MemberBox를 사용해야 한다.
    MemberBox box1 = new MemberBox();
    box1.set(new Member("홍길동", 20)); // 값 저장
    Member m = box1.get(); // 값 꺼내기
    System.out.println(m);
  }
```
이렇게 객체를 저장하려면 각 객체의 타입 별로 Box클래스를 생성해야 한다. 이런식으로 코딩을 하면 같은 일을 하는 클래스임에도 불구하고 객체 타입이 다르단 이유로 여러 개의 유사 클래스를 반복적으로 정의해야 하는 문제가 발생한다.


### 다형적 변수의 활용
다양한 객체를 저장하기 위해 인스턴스 변수 타입을 Object로 선언하면 문제가 해결 될까? 아래 예제를 보자.

```java
    static class ObjectBox {
    Object value;
    
    public void set(Object value) {
      this.value = value;
    }
    
    public Object get() {
      return value;
    }
  }
  
  public static void main(String[] args) {
    
    ObjectBox box1 = new ObjectBox();
    box1.set(new Member("홍길동", 20)); // 값 저장
    Member m = (Member) box1.get(); // 값을 꺼낼 때 형변환해야 한다.
    System.out.println(m);
     
    ObjectBox box3 = new ObjectBox();
    box3.set(100); // auto-boxing ==> Integer.valueOf(100)
    int i = (int) box3.get(); // 값을 꺼낼 때 primitive date type을 지정하면 
    // 자동으로 auto-unboxing으로 바꾼다.
    System.out.println(i);
  }
```
모든 객체를 담을 수 있는 Object로 선언하니 코딩이 한결 편해졌지만, 값을 꺼낼 때 원래의 타입으로 바꾸기 위해서 매번 형변환(type casting) 해야 하는 불편함이 있다.  
또, Object클래스는 모든 자바 객체를 받을 수 있기 때문에 다양한 객체를 보관할 수는 있지만, 특정 타입의 객체로 제한하는 것은 불가능하다.  
이런 문제를 해결하기 위해 나온 문법이 바로 **제네릭(Generic)** 이다.


## 제네릭 문법

* 제네릭의 특징
1. 다루는 타입을 제한할 수 있다. (개발자가 직접 타입을 지정 가능하다.)
2. 제네릭을 지정하면 그와 관련된 메서드의 타입 정보가 자동으로 바뀐다.

### 1) 문법 : 클래스명<타입명>
```java
    ArrayList<Member> list = new ArrayList<Member>();

     // => 레퍼런스 선언에 제네릭 정보가 있다면 new 연산자에서는 생략할 수 있다.
    ArrayList<Member> list2 = new ArrayList</*Member*/>(); // OK!

    ArrayList<Member> list3;
    list3 = new ArrayList<>(); // OK!

    // 제네릭의 인스턴스를 생성할 때 명확히 제네릭의 타입을 지정해 주는 것이 좋다.
    list = new ArrayList();
    // 이렇게 해줘도 컴파일은 통과하지만, 후에 오류가 발생활 확률이 매우 높다.
```

### 2) 제네릭을 Object(상속관계)로 적용 했을 때 

쉽게 이해하기 위해 예시를 한글로 들었다.
```java
    ArrayList<포유류> list1; 
    // 제네릭 타입을 Object(포유류)로 지정하면 
    // 그렇게 지정된 ArrayList 객체만 list1에 저장할 수 있다.

    list1 = new ArrayList<Object>();

    // ArrayList<포유류>로 만든 객체는 다음과 같이 포유류의 모든 객체를 담을 수 있다.
    list1.add(new 포유류());
    list1.add(new 개());
    list1.add(new 고양이());
    
    list1 = new ArrayList<개과>(); // 컴파일 에러!!
   // 이렇게 되면 다른 종류의 포유류를 담을 수가 없다!
   // 제네릭에서 정확하게 포유류를 담겠다고 선언했는데
   // 새로 만든 객체에서 오로지 개과만 다루겠다고 선언할 수 없는 것이다.
   // 이것은 다른 상속관계에서도 마찬가지다.

```

### 3) 제네릭의 타입을 제대로 지정해 주지 않았을 경우
```java
  static class A {}
  static class B1 extends A {}
  static class B2 extends A {}
  static class C extends B1 {}
  /*
   *   Object
   *     |
   *     A
   *    / \
   *   B1 B2
   *   |
   *   C
   */

  public static void main(String[] args) {
    // m1(ArrayList)
    // => 제네릭의 타입을 지정하지 않으면, 
    //    다음과 같이 다양한 종류의 ArrayList를 파라미터로 넘길 수 있다.
    //    m1(new ArrayList());
    //    m1(new ArrayList<A>());
          m1(new ArrayList<B1>());
    //    m1(new ArrayList<B2>());
    //    m1(new ArrayList<C>());
    System.out.println("실행완료!");
  }

  // 제네릭에 타입을 제대로 지정해 주지 않으면 어떻게 될까?
  static void m1(ArrayList list) {

    // 이렇게 모든 객체 값을 다 받을 수 있게 되버린다.
    list.add(new Object());
    list.add(new A());
    list.add(new B1());
    list.add(new B2());
    list.add(new C());
  }

```
### 4) 무엇이든 받을 수 있는 타입 <?>
<?>는 보이는 것 처럼 ? 안에 무슨 값이든 들어 갈 수 있다.  
컴파일러는 파라미터로 받은 ArrayList가 어떤 타입의 값을 다루는 지 알 수 없기 때문에
그 타입인지 검사해야 하는 메서드를 사용할 때는 컴파일을 명확하게 해줄 수 없다.  
따라서 컴파일 오류를 발생시킨다.
```java
  static void m1(ArrayList<?> list) {
    //    list.add(new Object());
    //    list.add(new A());
    //    list.add(new B1());
    //    list.add(new B2());
    //    list.add(new C());
  }
```
> 즉 <?>는 값을 조회할 때는 유용하나 값을 추가할때는 적절하지 못하다.

### 5) <? extends class>
이 문법은 class를 상속받은 ? 값을 받겠다는 뜻이다.

```java
  public static void main(String[] args) {
    // m1(ArrayList<? extends B1>)
    // => A 타입 및 그 하위 타입에 대해서 ArrayList 객체를 파라미터로 넘길 수 있다.
    //
    //m1(new ArrayList<Object>()); // 컴파일 오류!
    //m1(new ArrayList<A>()); // 컴파일 오류!
    m1(new ArrayList<B1>()); 
    //m1(new ArrayList<B2>()); // 컴파일 오류!
    m1(new ArrayList<C>()); 
  }
  // B1또는 B1을 상속받는 어떤놈이든 받겠다.
  static void m1(ArrayList<? extends B1> list) {
    // 파라미터로 받은 ArrayList가 구체적으로 어떤 타입의 값을 다루는 것인지
    // 결정되지 않았기 때문에 컴파일러는 다음 코드가 옳은지 검사할 수 없다.
    // 그래서 컴파일 오류가 발생한다.
    //list.add(new B1()); //컴파일 오류!

    Object obj1 = list.get(0);
    B1 obj2 = list.get(0);
    // C obj3 = list.get(0); // 컴파일 오류 
  }
```


> **결론!** 제네릭의 타입을 지정하지 않으면 특정 타입으로 제한하는 문법이 무용지물이 된다.  
따라서 제네릭으로 선언된 클래스를 사용할 때는 반드시 타입 파라미터 값을 지정하는 것이 좋다!   
제네릭 문법의 목적은 코드 안정성을 추구하는 것이다.  
원하는 타입이 아닌 다른 값을 지정하는 오류(타입오류)를 줄이기 위해 만든 문법이다.  
제네릭 문법의 대상은 **컴파일러**다.  
즉, 컴파일 단계에서 최대한으로 타입 오류를 잡아 내는 것이 목적이다. (JVM과는 상관이 없다!)



