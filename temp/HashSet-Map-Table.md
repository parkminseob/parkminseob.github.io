# HashSet

---

HashSet은 Set인터페이스에 있는 구현 클래스이다. 기본 생성자 호출 가능

HashSet에 문자열을 저장할 경우 같은 문자열을 갖는 String 객체는 동등한 객체로 간주되고 다른 문자열을 갖는 String 객체는 다른 객체로 간주된다.

이유? String크랠스가 hashCod()와 equals()메소드를 재정의해서 같은 문자열일 경우 hashCode()의 리턴값은 같게, equals()의 리턴값은 true가 나오도록 했기 때문이다.



# HashMap

----

HashMap은 Map클래스에 있는 구현 클래스이다.

<K, V> 키값, 값 모두 객체이다.

키는 중복저장될 수 없지만 값은 중복 저장될 수 있다.

HashMap키로 사용할 객체는 hashCode()와 equals()메소드를 재정의해서 동등 객체가 될 조건을 정해야한다.

보통 String많이씀



# HashTable

------

HashMap과 동일한 내부 구조를 가지고 있다. 

다른점 : HashMap과 다르게 HashTable 은 동기화된 메소드로 구성되어있기 때문에 멀티 스레드가 동시에 HashTable의 메소드들을 실행할 수 없고, 하나의 스레드가 실행을 완료해야만 다른 스레드를 실행할 수 있다. 그래서 멀티 스레드 환경에서 안전하게 객체를 추가, 삭제할 수 있어 안전하다.







