---
title: Object클래스
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
categories:
- JAVA
toc: true
toc_sticky: true
toc_label: on this page
description: Object클래스의 구조와 중요 메서드에 대한 설명
article_tag1: Java
article_tag2: Object Class
article_section: Java공부
meta_keywords: java, Object class
tags:
  - Java
  - Java basic
---

* Object 클래스는 자바의 최상위 클래스이다.
클래스를 정의할 때 수퍼클래스를 지정하지 않으면 컴파일러는 자동으로 Object를 상속받는다.
자바의 모든 클래스는 Object의 자손이기 때문에 당연히 Object의 메서드를 사용할 수 있다.

> Object 클래스의 주요 메서드

1) toString()
   => 클래스이름과 해시코드를 리턴한다.
   
2) equals()
   => 같은 인스턴스인지 검사한다.
   
3) hashCode()
   => 인스턴스를 식별하는 값을 리턴한다.
   
4) getClass()
   => 인스턴스의 클래스 정보를 리턴한다.
   
5) clone()
   => 인스턴스를 복제한 후 그 복제 인스턴스를 리턴한다.
   
6) finalize()
   => 가비지 컬렉터에 의해 메모리에서 해제되기 직전에 호출된다. C를 쓰던 사람들이 자주 호출하려는 명령. 존재하긴 하지만 없는 메서드라 생각하고 기억에서 지우자.

## toString() 메서드
---------------
Object에서 상속받은 메서드. 클래스 정보를 간단히 출력한다
패키지명.클래스명@16진수해시코드값
예시 ) `ch15.My@1e81f4dc`
Println()에 넘겨주는 값이 String타입이 아니라면 println()은 그 객체에 대해 toString()을 호출한 후 그 리턴값을 출력한다. 
```
    System.out.println(obj.toString());
    System.out.println(obj);
```
위 예시에서 두개의 코드는 같은 결과값을 출력한다. 즉 println()으로 객체의 값을 출력할 때는 toString()은 항상 생략되어 붙어있어서 번거롭게 toString()을 호출할 필요가 없다.

> 여기서 잠깐, 해시코드(hashcode)란?
인스턴스를 식별할 수 있는 코드이다. 주의해야할 점!!! **인스턴스 주소가 아니다!!** 자바는 절대로 메모리 주소를 알려주지 않는다. 단지 JVM이 임의로 붙인 식별코드일 뿐이다.

개발을 하다 보면 현재 인스턴스의 값을 간단히 확인하고 싶을 때 toString()을 오버라이딩 하여 쓴다.(오버라이딩 하지 않으면 위처럼 한번에 알아보기 힘든 값이 나오기 때문)
```java
@Override
    public String toString() {
      return "My3 [name=" + name + ", age=" + age + "]";
    }
    이런식으로 간단하게 오버라이딩 해준다.
```

## equals() 메서드
-------------------------
Object에서 상속받은 equals()메서드는 ==연산자와 마찬가지로 인스턴스가 같은지 비교한다.
**주의!** 인스턴스 내용물을 비교하는 것이 아니다. 내용물을 비교하고 싶다면 equals() 역시 오버라이딩 해줘야 한다.

```java
** 오버라이딩 전
 Member m1 = new Member("홍길동", 20);
 Member m2 = new Member("홍길동", 20);

    System.out.println(m1 == m2);
    System.out.println(m1.equals(m2));
    
```
결과값은 어떻게 나올까? 정답은 둘다 false다. 두개의 인스턴스가 다르기 때문이다.
그러나 String 인스턴스는 좀 다르다.
```java
String 인스턴스에서 equals()연산하기
	String s1 = new String("Hello");
    String s2 = new String("Hello");
    
    System.out.println(s1 == s2);
    System.out.println(s1.equals(s2));
```
결과값은 어떨까? 정답은 `s1 == s2 = false`, `s1.equals(s2) = true`이다. 
String클래스는 Object의 toString()을 오버라이딩 했기 때문이다. 인스턴스가 달라도 문자열이 같으면 true를 리턴하도록 오버라이딩 되어있다.
toString()이 오버라이딩 되어있는 대표적인 클래스는 String과 Wrapper클래스가 있다.

StringBuffer는 좀 다른 결과값을 출력한다.
```java
	StringBuffer sb1 = new StringBuffer("Hello");
    StringBuffer sb2 = new StringBuffer("Hello");
    
    System.out.println(sb1 == sb2); // false
    System.out.println(sb1.equals(sb2)); // false
```
왜냐? StringBuffer는 equals()를 오버라이딩 하지 않았기 때문이다. 이 내용에 대해서는 String클래스에서 더 자세히 다룬다.

## hashCode() 메서드
---------------------
Object클래스에서 상속받은 hashCode()는 인스턴스마다 고유의 4바이트 정수값을 리턴한다.
이 값은 toString()의 출력 값으로 사용된다.

* hashCode()의 목적
데이터가 같은지 빠르게 비교할 때 사용된다. 특정 수학 공식에 따라 값을 계산한다. 이런 이유로 hash값을 "디지털 지문"이라고 부른다. 대표적인 해시 알고리즘으로 SHA, MD, PGP등이 있다. **주의! 인스턴스 주소가 아니다!!**

hashCode()는 기본적으로 인스턴스마다 고유의 값을 리턴한다. 그래서 다음 출력은 비록 같은 값을 갖고 있다 하더라도 인스턴스가 다르기 때문에 해시코드 값이 다르다.
```java
    Score s1 = new Score("홍길동", 100, 100, 100);
    Score s2 = new Score("홍길동", 100, 100, 100);

	System.out.printf("%d, %d\n", s1.hashCode(), s2.hashCode());
    
    결과값 : 225534817, 1878246837
```
hashCode()를 직접 오버라이딩 하면 원하는 해시코드값을 리턴할 수 있다.
```java
@Override
    public int hashCode() {
      // 무조건 모든 Score 인스턴스가 같은 해시코드를 갖게 하자!
      return 1000;
    }
```
그러나 위 코드처럼 실제 데이터가 같은지 따지지도 않고 모든 인스턴스에 대해 같은 해시코드를 리턴하는 것은 아무 의미가 없다. 부질없는 짓이다!

### hashCode()응용 - HashSet, HashMap

#### HashSet클래스
집합의 기능을 수항한다. **중복 값을 저장하지 않는다.** 저장할 객체에 대해 hash코드로 중복 여부를 검사하고, 해시값이 다르면 데이터가 같아도 다른 값으로 취급한다. 또한 hash코드로 값을 저장할 인덱스를 결정하기 때문에 꺼낼때도 저장한 순서대로 꺼낼 수 없다.

같은 데이터를 가진 인스턴스에 같은 해시코드를 부여하고 싶다면? hashCode()와 equals()모두 오버라이딩 해야 한다!
```java
*** hashCode()와 equals()모두 오버라이딩 한 HashSet클래스

    Student s1 = new Student("홍길동", 20, false);
    Student s2 = new Student("홍길동", 20, false);
    Student s3 = new Student("임꺽정", 21, true);
    Student s4 = new Student("유관순", 22, true);

    System.out.println(s1 == s2);

    System.out.println(s1.hashCode());
    System.out.println(s2.hashCode());
    System.out.println(s3.hashCode());
    System.out.println(s4.hashCode());
    System.out.println("--------------------");

    // 해시셋(집합)에 객체를 보관한다.
    HashSet<Student> set = new HashSet<Student>();
    set.add(s1);
    set.add(s2);
    set.add(s3);
    set.add(s4);

    // 해시셋에 보관된 객체를 꺼낸다.
    Object[] list = set.toArray();
    for (Object obj : list) {
      Student student = (Student) obj;
      System.out.printf("%s, %d, %s\n",
          student.name, student.age, student.working ? "재직중" : "실업중");
    }
```
* 결론
같은 데이터를 가진 인스턴스를 비교하기 위해서는 equals()와 hashCode()둘다 오버라이딩 해주는 것이 좋다. 그래서 eclipse에는 우클릭-source-Generate hashCode() and equals() 기능을 지원하고 있다!

#### HashMap
`HashMap<K, V> 변수이름 = new HashMap<>();`
K에 Key값을 넣고, V에 인스턴스를 넣는다.
값을 저장할 때 사용한 key객체로 값을 찾아 꺼내는데, 마찬가지로 데이터가 같아도 인스턴스가 다르면 다른 key라고 간주하기 때문에 equals()와 hashCode()를 오버라이딩 해주어야 한다. HashMap을 쓸 때 마다 매번 오버라이딩 해주는 것이 귀찮기 때문에 보통 Key값으로 Wrapper 클래스와 String클래스 값을 준다! 

왜냐? Wrapper클래스와 String 클래스는 Object클래스에서 상속받은 equals()와 hashCode()메서드를 인스턴스가 다르더라도 인스턴스 필드의 값이 같으면 hashCode()리턴 값이 같고 equals()가 true를 리턴하도록 오버라이딩 되어있기 때문이다!


## getClass() 메서드
------------------
해당 클래스의 정보를 리턴한다.
```java
    My obj1 = new My();
    
    // 레퍼런스를 통해서 인스턴스의 클래스 정보를 알아낼 수 있다.
    Class<?> classInfo = obj1.getClass();
    
    // 클래스 정보로부터 다양한 값을 꺼낼 수 있다. 
    System.out.println(classInfo.getName());
    => 결과값 : com.eomcs.corelib.ex01.Exam0160$My
    
    System.out.println(classInfo.getSimpleName());
    => 결과값 : My
```
배열의 클래스 정보도 리턴한다.
```java
    String obj1 = new String();
    Class<?> classInfo = obj1.getClass();
    System.out.println(classInfo.getName()); // java.lang.String
    
    // 배열의 클래스 정보
    String[] obj2 = new String[10];
    classInfo = obj2.getClass();
    System.out.println(classInfo.getName()); //[Ljava.lang.String;
    
    int[] obj3 = new int[10];
    classInfo = obj3.getClass();
    System.out.println(classInfo.getName()); //[I

    float[] obj4 = new float[10];
    classInfo = obj4.getClass();
    System.out.println(classInfo.getName()); //[F
    
    double[] obj5 = new double[10];
    classInfo = obj5.getClass();
    System.out.println(classInfo.getName()); //[D
    
    System.out.println(new byte[10].getClass().getName()); //[B
    System.out.println(new short[10].getClass().getName()); //[S
    System.out.println(new long[10].getClass().getName()); //[J
    System.out.println(new char[10].getClass().getName()); //[C
    System.out.println(new boolean[10].getClass().getName()); //[Z
```
값을 한번 밖에 사용하지 않을 것이라면 위처럼 메서드를 호출하고 그 리턴값으로 또 호출하는 체인(Chain)방식으로 호출한다.

## Clone() 메서드
------------------
인스턴스를 복제할 때 호출하는 메서드이다. clone()은 protected이므로, 같은 패키지의 소속된 클래스이거나 상속 받은 서브클래스가 아니면 사용할 수 없다. 그러므로 clone()은 꼭 오버라이딩해야만 쓸 수 있다! 오버라이딩 후 public으로 접근제어 범위를 넓혀 다른 패키지 멤버도 쓸 수 있게 하는 것이 좋다. (수업 corelib ex01 Exam0170~Exam0174)
```java
class Score {}

	@Override
    public Score clone() throws CloneNotSupportedException {
      // 복제를 위한 코드를 따로 작성할 필요가 없다. 
      // JVM이 알아서 해준다. 
      // 그냥 상속 받은 메서드를 오버라이딩하고, 접근 권한을 public 으로 확대한다.
      // 리턴 타입은 해당 클래스 이름으로 변경한다.
      return (Score) super.clone(); }
      
 public static void main(String[] args) throws Exception {
 	Score s1 = new Score();
    Score s2 = s1.clone(); //런타임 에러!

```
하지만 이대로 로컬에서 clone()메서드를 실행하면 런타임 에러가 뜬다. clone()메서드의 사용을 활성화시키지 않아서 예외가 발생한 것이다. 단지 clone()을 오버라이딩 했다고 끝나는 것이 아니라, Score클래스에 복제 기능을 활성화시키는 설정을 해야한다.

인스턴스의 복제 기능을 활성화하려면 Cloneable 인터페이스를 구현해야한다. Cloneable을 구현하는 이유는, JVM에게 이 클래스의 인스턴스를 복제할 수 있음을 표시하기 위함이다. 이 표시가 안된 클래스는 JVM이 인스턴스를 복제해 주지 않는다. 즉 clone()을 호출할 수 없다.
```
static class Score implements Cloneable {} 
클래스 이름 옆에 선언해준다.
```
Cloneable선언 후 다시 clone() 메서드를 실행해보면 런타임 에러가 발생하지 않는다.

### shallow copy와 deep copy
* shallow copy
위의 Score예제에서 `System.out.println(s1 == s2);` 을 출력해보자. 결과값은 false이다. 왜냐? Object의 clone() 은 해당 객체의 필드 값만 복제한다. 그 인스턴스 변수가 가리키고 있는 객체는 복제하지 않는다! 이런 방식의 복제를 shallow copy(얕은 복제)라고 부른다. (내 도플갱어가 가족,집,재산 모두 공유하는 상태)

* deep copy
그렇다면 인스턴스 변수가 가리키고 있는 객체까지 복제하고 싶다면? 개발자가 직접 clone()메서드 안에 deep copy를 수행하는 코드를 작성해줘야 한다.
```java
  static class Engine implements Cloneable {
    int cc;
    int valve;

    public Engine(int cc, int valve) {
      this.cc = cc;
      this.valve = valve;
    }
  }
  
    static class Car implements Cloneable {
    String maker;
    String name;
    Engine engine;

    public Car(String maker, String name, Engine engine) {
      this.maker = maker;
      this.name = name;
      this.engine = engine;
    }

    @Override
    public Car clone() throws CloneNotSupportedException {
      // deep copy
      // => 포함하고 있는 하위 객체에 대한 복제를 수행하려면 다음과 같이
      //    개발자가 직접 하위 객체를 복제하는 코드를 작성해야 한다.
      //
      Car copy = (Car) super.clone();
      copy.engine = this.engine.clone();
      return copy;
    }
 }
```

