---
title: 자바 - String클래스
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
description: String클래스의 구조와 중요 메서드에 대한 설명
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

## String 레퍼런스
String은 자바 기본 타입이 아니다. 클래스이다!  
`String s1;` 여기서 s1은 String 인스턴스 주소를 담는 레퍼런스이다.

* String 인스턴스
``` java
    s1 = new String("Hello");
	String s2 = new String ("Hello");
```
heap영역에 Hello문자 코드를 저장할 메모리를 만들고 그 주소를 리턴한다. 내용물의 동일 여부를 검사하지 않고 무조건 인스턴스를 생성한다!  
가비지가 되면 가비지 컬렉터에 의해 제거된다.

`System.out.println(s1 == s2);` 결과값은 false이다.

## String 문자열 리터럴
String constant pool(String 상수풀) 메모리 영역에 String 인스턴스를 생성한다. 내용물이 같으면 기존 인스턴스의 주소를 리턴한다.   
즉 메모리 절약을 위해 중복 데이터를 갖는 인스턴스는 생성하지 않는다! 상수풀에 생성된 메모리는 JVM이 끝날 때 까지 유지된다.
```java
String x1 = "Hello";
String x2 = "Hello";
System.out.println(x1 == x2); //두 객체는 같으므로 true
```

## intern() 메서드
----------------------------
String 인스턴스를 상수풀에 생성하는 메서드
```java
    String s1 = new String("Hello");

    // intern()
    // - 지정된 String 객체를 상수풀에서 찾는다.
    // - 있으면 그 String 객체의 주소를 리턴한다.
    // - 없으면 상수풀에 String 객체를 생성한 후 그 주소를 리턴한다.
    String s2 = s1.intern();
    String s3 = "Hello";

    System.out.println(s1 == s2); 
    // false(s1은 상수풀에 없기 때문에 상수풀에 새로운 객체를 생성 한 후 주소를 리턴하므로 s1과 s2는 다른 인스턴스이다.)
    System.out.println(s2 == s3); // true
    // 상수풀에 생성된 s2와 s3은 같은 문자열을 가진 동일한 객체이다.
```

## equals() 메서드
----------------------
String 인스턴스에 있는 값을 비교할 수 있다. equals()메서드는 Object클래스에서 상속받은 메서드로, 두 인스턴스가 같은지 비교한다.
 String 클래스는 이 메서드를 상속받아 오버라이딩 하여 두 문자열이 같은지 비교할 수 있다.
```java
    String s1 = new String("Hello");
    String s2 = new String("HELLO");
    String s3 = new String("HELLO");

	System.out.println(s2.equals(s3)); // true
    
    // equals()는 대소문자를 구분한다.
    System.out.println(s1.equals(s2)); //false

    // 대소문자 구분없이 문자열을 비교하고 싶다면,
    System.out.println(s1.equalsIgnoreCase(s2)); //true
```

## hashCode() 메서드
----------
Object에서 상속받은 hashCode()메서드는 인스턴스의 정보를 정수값으로 리턴한다.
때문에 인스턴스가 다르다면 다른 해시코드를 리턴하지만, String 클래스에서는 hashCode()를 오버라이딩 하여 문자열이 같은 경우 같은 hashCode()를 리턴하도록 되어있다.
```java
    String s1 = new String("Hello");
    String s2 = new String("Hello");

    System.out.println(s1.hashCode() == s2.hashCode()); // true
```
보통 equals()메서드와 함께 오버라이딩 되어 사용된다. HashMap, HashTable, HashSet과 같은 클래스에서 유용하게 쓰인다.


## StringBuffer 클래스
-------------------
StringBuffer클래스는 String과 같이 문자열을 출력하는 클래스이다.
```java
    StringBuffer b1 = new StringBuffer("Hello");
    StringBuffer b2 = new StringBuffer("Hello");
    
    System.out.println(b1 == b2); // false
    System.out.println(b1.equals(b2)); // false

```
어? equals()메서드를 썼는데도 결과값이 false가 나온다.  
왜일까? StringBuffer는 Object클래스에서 상속받은 equals()를 오버라이딩 하지 않았기 때문이다.  
StringBuffer에 들어있는 문자열을 비교하고 싶다면, toString()을 이용해 비교해야 한다.

```java
    StringBuffer b1 = new StringBuffer("Hello");
    StringBuffer b2 = new StringBuffer("Hello");

    // String s1 = b1.toString();
    // String s2 = b2.toString();
    // System.out.println(s1.equals(s2));
    System.out.println(b1.toString().equals(b2.toString()));
    
    => 결과값 : true
```

### mutable vs immutable
String과 StringBuffer의 가장 큰 차이점이다.   
String은 immutable객체로, 한번 객체에 값을 담으면 변경할 수 없다!
```java
    String s1 = new String("Hello");
    String s2 = s1.replace('l', 'x');
```
replace메서드를 써서 문자열 l을 x로 대체했다. 하지만 String은 immutable이므로, 원본 문자열이 바뀌는 것이 아니라 새로운 String 객체를 생성하여 리턴한다.  
반면에 StringBuffer클래스는 mutable로, 인스턴스의 데이터를 변경할 수 있다. 
```java
    StringBuffer buf = new StringBuffer("Hello");
    buf.replace(2, 4, "xxxx");
    // 원본을 바꾼다.
    // 여기서 2, 4는 2 이상 4 미만의 뜻을 가진다. 
    // 문자열은 0부터 시작하므로 ll부분이 xxxx로 대체된다.
    // 결과 : Hexxxxo

```

## toString() 메서드
---------------
Object에서 상속받은 toString()은 "클래스명@해시값"을 리턴한다.   String은 상속받은 toString()을 오버라이딩했다. 그래서 this주소를 그대로 리턴한다.
```java
    String s1 = new String("Hello");
    String s2 = s1.toString();
    System.out.println(s1 == s2); // true
    System.out.println(s2); // Hello
```

* println() 메서드에 넘겨주는 파라미터 값이 String 타입이 아닌 경우 println()메서드 내부에서 해당 값에 대해 toString()을 자동으로 호출하여 리턴한다.

### 기타 메서드 - valueOf()
`String s1 = String.valueOf();`
파라미터 안의 값을 모두 문자열로 만든다.


## String의 다양한 생성자 활용
----------------
String 인스턴스는 내부적으로 문자의 코드값을 저장할 char배열 또는 byte배열을 생성한다.  
 문자열을 다룰 일이 많으므로 String 문자배열을(특히 한글) 정확한 문자표에 따라 출력할 수 있도록 생성자에 바이트 배열을 넘겨줄 때 배열에 들어 있는 코드 값이 어떤 문자표 코드 값인지 알려줘야 한다.

``` java
byte[] bytes =
        {(byte) 0xb0, (byte) 0xa1, (byte) 0xb0, (byte) 0xa2, 0x30, 0x31, 0x32, 0x41, 0x42, 0x43};
        
String s1 = new String(bytes, "euc-kr"); //Hello
```