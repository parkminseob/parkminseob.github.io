---

title: 자바 - 예외(Exception)
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

# 예외(Exception)

**예외**란, 사용자의 잘못된 조작 또는 개발자의 잘못된 코딩으로 발생하는 프로그램 오류를 뜻한다. 예외처리를 배우기 전엔 예외가 발생하면 곧바로 JVM이 종료되었으나 예외 처리(Exception Handling)을 통해 프로그램이 강제 종료되지 않고 실행상태가 유지되도록 할 수 있다.



## 예외 떠넘기기 (Thorws)

throws는 메소드 선언부 끝에 작성되어 메소드에서 처리하지 않는 예외를 메소드를 호출한 곳으로 떠넘기는 역할을 한다. 이 메소드를 호출한 곳은 반드시 예외처리(try~catch)를 하거나 다시 throws를 사용해서 예외를 떠넘겨야 한다.

문법은 다음과 같다.

`throw [java.lang.Throwable 타입의 객체];`

```java
리턴타입 메소드이름(매개변수,...) throws 예외클래스1, 예외클래스2, ...{}

static void method() throws FileNotFoundException, RuntimeException {}
```

Throwable에는 두 부류의 서브 클래스가 있다.

1. java.lang.Error(시스템 오류)
   * JVM에서 발생된 오류이다. 
   * 개발자가 사용하는 클래스가 아니기 때문에 근본적으로 문제를 해결할 수 없다.
   * 이 오류가 발생하면 현재 상태를 백업하고 즉시 실행을 멈춰야 한다.
   * 예) 스택오버플로우, VM관련 오류, AWT윈도우 관련 오류, 스레드 종료 등
2. java.lang.Exception(어플리케이션 오류)
   * 개발자가 사용하는 클래스이다.
   * 개발자가 적절한 조치를 취한 후 계속 시스템을 실행하게 할 수 있다.
   * 예) 배열의 인덱스가 무효한 오류, I/O 오류, SQL 오류, Parse 오류, 데이터 포맷 오류 등



예외를 던질때는 메서드 선언부에 어떤 오류를 던지는지 모두 나열하여 선언해야한다. 

* Throwable클래스를 쓸수는 있지만 되도록 그 하위 클래스인 Exception을 사용하는 것이 좋다. 
* 발생하는 모든 예외를 Exception 하나로 퉁칠 수 있으나 호출자에게 어떤 오류가 발생하는지 정확하게 알려주는 것이 유지보수에 도움이 된다. 따라서 가능한 그 메서드에서 발생하는 예외는 모두 나열하는 것이 좋다.

* 상위 호출자가 모두 예외 처리를 하지 않아 main()에서 예외처리를 할 수도 있지만, main()의 호출자는 JVM이고, JVM은 main()에서 던지는 예외를 받는 순간 즉시 실행을 멈춘다. 따라서 main() 호출자에게 예외를 떠넘기는 것은 바람직하지 않다.



## 예외 처리 코드(try ~ catch~finally)

* try블록에 예외 발생 가능 코드가 위치한다.
* catch블록에서 그 예외를 받아 처리한다.

```java
public class Exam0430 {

  static void m(int i) throws Exception, RuntimeException, SQLException, IOException {
    if (i == 0)
      throw new Exception();
    else if (i == 1)
      throw new RuntimeException();
    else if (i == 2)
      throw new SQLException();
    else if (i == 3)
      throw new IOException();

  }

  public static void main(String[] args) {
    // - try ~ catch 를 사용하여 코드 실행 중에 발생된 예외를 중간에 가로챈다.
    //
    try {
      // try 블록에는 예외가 발생할 수 코드를 둔다.
      m(3);
      System.out.println("실행 성공!");
      // try 블록에 있는 코드를 실행하는 중에
      // 예외가 발생하면,
      // 그 예외 객체를 파라미터로 받을 수 있는
      // catch 문을 찾아 실행한다.
      //
    } catch (IOException e) {
      // catch 블록에서 그 예외를 받아서 처리한다.
      // 메서드가 던지는 예외 개수 만큼 catch 블록을 선언하면 된다.
      System.out.println("IOException 발생");

    } catch (SQLException e) {
      System.out.println("SQLException 발생");

    } catch (RuntimeException e) {
      System.out.println("RuntimeException 발생");

    } catch (Exception e) {
      System.out.println("기타 Exception 발생");
    }
  }
}
```

### **catch블록 작성시 주의할 점**

catch블록에서 여러개의 예외를 받을 때, 수퍼클래스 변수로 먼저 받으면 안됀다!

수퍼클래스 예외가 서브클래스 객체까지 다 받아 처리해버리기 때문에 아래에 위치한 서브클래스 catch블록까지 차례가 오지 않는다.

예외 객체를 정확하게 받고싶다면 서브클래스 예외부터 순차적으로 코드를 작성한다.

* catch블록에서 or 연산자를 이용해 여러 개의 예외를 묶어 받을 수 있다.

  ```java
  catch (RuntimeException | SQLException | IOException e) {}
  ```

  

### finally 블록

finally블록은 생략이 가능하다. 예외 발생 여부와 관계없이 항상 실행해야 할 내용이 있다면 finally블록을 작성한다. try블록과 catch블록에서 return문을 사용하더라도 finally블록은 **반드시** 실행된다. 그래서 finally블록에는 사용한 자원을 해제시키는 코드를 주로 둔다.

*자원이란? 파일, DB 커넥션, 소켓 커넥션, 대량의 메모리 등을 뜻한다.*

보통 자원을 해제시키는 메소드의 이름은 close()이다.



#### try-with-resources

매번 자원을 해제시키는 코드를 작성하기 귀찮을때는 try-with-resources 문법을 사용할 수 있다. 

단 java.lang.AutoCloseable 구현체에 대해서만 가능하다!

문법 `try (java.lang.AutoCloseable 구현체) {...}`

```java
public class Exam0630 {
  static void m() throws Exception {
    try (Scanner keyScan = new Scanner(System.in); // OK!
        // FileReader 클래스도 java.lang.AutoCloseable 구현체이다.
        FileReader in = new FileReader("Hello.java"); // OK!
    // 변수 선언만 올 수 있다.
    // if (true) {} // 컴파일 오류!
    ) {
      System.out.print("입력> ");
      int value = keyScan.nextInt();
      System.out.println(value * value);
    }
  }
  public static void main(String[] args) throws Exception {
    m();
  }
}
```

