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





