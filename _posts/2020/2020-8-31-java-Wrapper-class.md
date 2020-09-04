---
title: 자바 - Wrapper클래스
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
description: Wrapper클래스의 구조와 설명
article_tag1: Java
article_tag2: String class
article_tag3: basic
article_section: Java공부
meta_keywords: Java
tags:
  - Java
  - Java basic
---

![java-logo](/home/sub/parkminseob.github.io/assets/images/post/java/java-logo.png)

# Wrapper(포장)클래스

자바는 primitive data type(*기본타입* byte, int, short, char, long, float, double, boolean)값을 다룰 때 기본 연산자 외에 좀 더 다양한 방법으로 다루기 위해 Primitive data type에 대응하는 클래스를 제공한다.
```	java
    Byte b = new Byte((byte)100);               // ==> byte
    Short s = new Short((short)20000);          // ==> short
    Integer i = new Integer(3000000);           // ==> int
    Long l = new Long(60000000000L);            // ==> long
    Float f = new Float(3.14f);                 // ==> float
    Double d = new Double(3.14159);             // ==> double
    Boolean bool = new Boolean(true);           // ==> boolean
    Character c = new Character((char)0x41);    // ==> char
```
이렇게 기본 타입에 대응하여 만든 클래스를 기본 데이터를 내부에 두고 포장하는 객체라고 해서 Wrapper클래스라고 부른다.  
Wrapper클래스가 없다면 정수를 받는 메서드, 부동소수점을 받는 메서드, 논리값을 받는 메서드를 따로따로 생성해줘야 한다. 자바는 이런 불편함을 없애기 위해 Wrapper클래스를 만든 것이다.  
 **즉, primitive type을 객체와 함께 다룰 수 있도록 만든 문법이다.**  
Wrapper클래스의 인스턴스를 생성할 때는 생성자 대신 클래스 메서드 valueOf()를 사용한다. 이유는 아래에서 후술한다.
```java
    Byte b2 = Byte.valueOf((byte)100);
    Short s2 = Short.valueOf((short)20000);
    Integer i2 = Integer.valueOf(3000000);
    Long l2 = Long.valueOf(60000000000L);
    Float f2 = Float.valueOf(3.14f);
    Double d2 = Double.valueOf(3.14159);
    Boolean bool2 = Boolean.valueOf(true);
    Character c2 = Character.valueOf((char)0x41);
```
> valueOf()는 기본 타입의 값을 문자열로 변환하는 기능을 가지고 있다. String클래스에서 매개 변수 타입별로 문자열로 변환될 수 있도록 오버로딩 되어있다.

----------------------------------------------
## 생성자 vs valueOf()
1. 생성자
new 명령으로 Integer 객체를 만들면 무조건 새 인스턴스를 생성한다.
```
    Integer i1 = new Integer(127);
    Integer i2 = new Integer(127);
    System.out.println(i1 == i2); // false
```
2. valueOf()
-128~127 범위의 수를 가지고 valueOf()를 호출하면 String 리터럴의 경우처럼 상수풀에 Integer객체를 생성한다.  
 *왜냐? -128~127 사이의 수는 자주 쓰이는 값이기 때문에 같은 값의 Integer객체가 여러개 생성되지 않도록 하는 것이다.*
```java
    Integer i3 = Integer.valueOf(127);
    Integer i4 = Integer.valueOf(127);
    System.out.println(i3 == i4); // true
    System.out.println(i1 == i3); // false
    
    // i3, i4는 같은 상수풀의 Integer객체를 참조하므로 결과값이 true, i1, i3은 다른 객체를 참조하므로 false다.
```

-128~127 범위를 벗어난 수는 무조건 새 인스턴스를 생성한다. 다루는 숫자가 매우 많기에 무조건 상수풀에 만들게 되면 메모리 낭비가 심해지기 때문이다.  
상수풀에 생성된 객체는 JVM이 종료되기 전까지 유지된다.(가비지가 되지 않음)  
그러나 heap에 생성된 객체는 주소를 잃어버리면 가비지가 되고 메모리를 좀 더 효율적으로 관리할 수 있다.
```java
    Integer i5 = Integer.valueOf(128);
    Integer i6 = Integer.valueOf(128);
    System.out.println(i5 == i6); // false
```

> 결론
Wrapper 객체의 값을 비교할 때 == 연산자를 사용하기보다는 equals()메서드를 이용해서 객체 값을 비교하는 것이 더 효율적이다!

-----------------------------------------------
## auto-boxing(오토박싱) / auto-unboxing(오토언박싱)

1. auto-boxing(오토박싱)

자바는 기본타입 값을 바로 wrapper클래스 인스턴스에 할당할 수 있다.  
`Integer obj = 100;`  
여기서 obj는 레퍼런스인데 어떻게 값을 바로 할당 할 수 있을까?  
답은 컴파일러가 내부적으로 위 코드를 `Integer obj = Integer.valueOf(100);`으로 바꾸기 때문이다.  
 즉 int값이 obj에 바로 저장되는 것이 아니라 내부적으로 Integer객체가 생성되어 그 주소가 저장된다.  
**이렇게 값을 자동으로 Integer객체로 만드는 것을 auto-boxing이라 한다.**

2. auto-unboxing(오토언박싱)

자바는 wrapper클래스 객체 값을 기본타입 변수에 할당할 수 있다.
```java
    Integer obj = Integer.valueOf(300);
    int i = obj; // ==> obj.intValue()
```
obj에 저장된 것은 int 값이 아니라 Integer객체의 주소이지만, 컴파일러가 내부적으로 obj.intValue()로 바꾼다.  
즉 obj에 들어있는 인스턴스 주소가 i에 저장되는 것이 아니라 obj 인스턴스에 들어 있는 값을 꺼내 i에 저장하는 것이다.  
**이렇게 Wrapper 객체 안에 들어있는 값을 자동으로 꺼내는 것을 auto-unboxing이라 한다.**

3. auto-boxing, auto-unboxing 응용
오토박싱, 오토언박싱을 이용하여 기본타입 또는 클래스의 인스턴스를 구분하지 않고 값을 편리하게 저장할 수 있다.
``` java
int i = 100;
Object obj;
obj = i; //오토박싱 규칙에 따라 Integer.valueOf(i) 문장으로 변환된다.
```

* Wrapper 클래스 정리
    wrapper 객체를 생성할 때는 new 를 사용하지 말고,  
    valueOf() 나 auto-boxing 기능을 이용하라.  
    값을 비교할 때는 반드시 equals()를 사용하자!
    
    '==' 연산자를 사용하면 안되나?  
    auto-boxing으로 객체를 만들 경우  
    -128 ~ 127 범위 내의 숫자인 경우는 캐시에 보관하기 때문에
    같은 값은 같은 객체이지만,  
    이 범위를 벗어나면 값이 같더라도 객체가 다르다.  
    따라서 일관성을 위해 값을 비교할 때 equals()를 사용하자!
   
    참고:  
    모든 wrapper 클래스는 String 클래스처럼  
    상속 받은 Object의 equals()를 오버라이딩 하였다.  
    즉 인스턴스를 비교하는 것이 아니라 값이 같은지를 비교한다.
