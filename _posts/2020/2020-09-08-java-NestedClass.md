---

title: 자바 - 중첩클래스(Nested Class)
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

# 중첩클래스(Nested class)

---

**중첩클래스란** 클래스 내부에 선언한 클래스를 말한다. 
특정 클래스 안에서만 사용되는 클래스가 있다면, 클래스를 따로 선언하는 것 보다 중첩 클래스로 만드는 것이 좋다. 
중첩 클래스를 사용함으로써 두 클래스의 멤버들은 서로 쉽게 접근할 수 있고, 외부로부터의 불필요한 노출을 줄여서 코드가 간결해진다. 즉, 코드를 유지보수하기 좋아진다.

```java
// 중첩 클래스의 형태

class Class{ // top level class 
 class NestedClass {} // nested class
}
```

-------

## 중첩클래스의 종류

1. static nested class
   - 바깥 클래스의 인스턴스에 종속되지 않는 클래스.
   - top level class와 사용법이 동일하다.
2. non- static nested class
   - 바깥 클래스의 인스턴스에 종속되는 클래스
   - 바깥 클래스의 인스턴스 없이는 생성할 수 없다.
3. local class
   - 특정 메서드 안에서만 사용되는 클래스
4. anonymous class(익명클래스)
   - 클래스의 이름이 없다.
   - 클래스를 정의하는 동시에 인스턴스를 생성해야 한다.
   - 클래스 이름이 없기 때문에 생성자를 정의할 수 없다.
   - 단 한 개의 인스턴스만 생성해서 사용할 경우 익명 클래스를 적용한다.

--------

## 1) static 중첩 클래스

static 멤버 클래스는 static으로 중첩 선언된 클래스를 말한다. static 멤버 클래스는 모든 종류의 필드와 메소드를 선언할 수 있다.

```java
// static 중첩 클래스
class A {
 static class C {
 C() {} //생성자
 int field; //인스턴스 필드
 static int field2; //정적 필드
 void method() {} //인스턴스 메소드
 static void method2() {} //정적메소드
 }
}
```

위의 예시 A 클래스 외부에서 정적멤버 중첩클래스 C 객체를 생성하기 위해서는 A객체를 생성할 필요가 없고 다음과 같이 C 객체를 생성하면 된다.

```java
A.C c = new A.C(); 
c.field = 3; // 인스턴스 필드 사용
c.method(); // 인스턴스 메소드 호출
A.C.field2 = 3; // 정적 필드 사용 
A.C.method2(); // 정적 메소드 호출
```

* **참고** : import static으로특정 클래스의 스태틱 멤버를 미리 선언해두면 멤버를 사용할 때 일일이 클래스 이름을 지정할 필요가 없다.

## 2) non-static 중첩 클래스

non-static멤버 클래스는 static 키워드 없이 중첩 선언된 클래스를 말한다. 인스턴스 필드와 메소드만 선언이 가능하다.(정적필드, 메서드 선언 불가)

non-static 중첩 클래스는 인스턴스 멤버이다. => 즉, 인스턴스가 있어야만 사용할 수 있다.

```java
// non-static nested class = inner class
package com.eomcs.oop.ex11.a;

public class Exam0210 {

  // 스태틱 멤버
  static int sValue;
  static void sm() {}

  // 인스턴스 멤버
  int iValue;
  void im() {}
  
  // non-static nested class 는 인스턴스 멤버이다.
  // 따라서 인스턴스가 있어야만 사용할 수 있다.
  class A {
    
    // 자바 컴파일러는 non-static nested class를 컴파일할 때 
    // 바깥 클래스의 인스턴스 주소를 담을 필드를 자동으로 생성한다.
    // 예)
    // Exam0210 outer;
    
    // 또한 이 inner class의 객체를 생성할 때 호출될 생성자를 자동으로 만든다.
    // 예)
    /*
    public A(Exam0210 arg0) {
      outer = arg0;
    }
    */
    
    void m1() {
      // 인스턴스 멤버(인스턴스 블록, 생성자, 인스턴스 메서드, inner class)는
      // 스태틱 멤버를 자유롭게 사용할 수 있다.
      sValue = 100; // OK
      sm(); // OK

      // 인스턴스 멤버는 같은 인스턴스 멤버를 사용할 수 있다.
      // 왜?
      // => inner class의 객체를 생성할 때 
      //    컴파일러가 자동으로 추가한 변수(예: outer)에 
      //    바깥 클래스의 객체 주소가 저장된다.
      // => 이렇게 inner class 객체에 보관된 바깥 클래스의 객체를 사용하려면
      //    다음과 같은 이름으로 변수를 사용해야 한다.
      //      "바깥클래스명.this"
      // => 다음 코드를 보자!
      //
      Exam0210.this.iValue = 100; // OK!
      Exam0210.this.im(); // OK!
      
      // inner 클래스에 같은 이름의 멤버가 없다면,
      // "바깥 클래스명.this"를 생략해도 된다.
      //
      iValue = 100; // OK!
      im(); // OK!
      
      // 주의!
      // => "바깥 클래스명"을 빼고 그냥 this 변수를 사용하면 
      //    이때의 this는 inner class의 인스턴스를 가리킨다.
      // => 바깥 클래스의 인스턴스를 가리키고 싶으면 반드시 바깥 클래스명을 지정해야 한다.
      //
      //this.iValue = 100; // 컴파일 오류!
      //this.im(); // 컴파일 오류!
    }
  }
  
  // 결론:
  // nested class가 바깥 클래스의 인스턴스 멤버를 사용한다면 
  // non-static nested class 로 정의하라!

  public static void main(String[] args) {
  }
}

```

> 일반적으로 top-level클래스 외부에서 중첩클래스(inner-class) 객체를 생성하는 일은 거의 없다. 대부분 top-level클래스 내부에서 중첩클래스 객체를 생성해 사용한다.



### 중첩클래스의 접근제한자

  중첩 클래스도 클래스의 멤버이기 때문에 필드나 메서드처럼 접근 제한자를 붙일 수 있다.

```java
public class Exam0310 {
  private static class A1 {} 
  static class A2 {}
  protected static class A3 {}
  public static class A4 {}

  private class B1 {} 
  class B2 {}
  protected class B3 {}
  public class B4 {}
}
```

​    그러나 로컬 변수처럼 로컬 클래스에는 접근 제어 modifier 를 붙일 수 없다.

```java
public class Exam0311 {
  static void m1() {
    //    private class A1 {} // 컴파일 오류!
    //    protected class A2 {} // 컴파일 오류!
    //    public class A3 {} // 컴파일 오류!

    class A4 {} // OK!
  }

  void m2() {
    //    private class B1 {} // 컴파일 오류!
    //    protected class B2 {} // 컴파일 오류!
    //    public class B3 {} // 컴파일 오류!

    class B4 {} // OK!
  }
}
```


