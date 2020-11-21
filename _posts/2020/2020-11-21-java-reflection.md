---
title: 자바 - 리플렉션(Reflection)
categories:
- java
toc: true
toc_label: on this page
tags:
  - Java
---

![java-logo](https://user-images.githubusercontent.com/68311188/92201199-e4e6a200-eeb6-11ea-9f5b-76b79db3564f.png)

# Java Reflection

## 의미

리플렉션이란 사전적 의미로 반사, 투영한다는 의미를 가지고 있다. 자바에서의 리플렉션은 **객체를 통해 클래스의 정보를 분석해 내는 프로그램 기법**을 뜻한다.

리플렉션 API를 이용하면 이미 로딩이 완료된 클래스에서 또 다른 클래스는 **동적으로 로딩(Dynamic Loading)**하여 생성자, 필드, 메서드를 사용할 수 있다.

## 사용법

리플렉션은 클래스의 이름만 알아내면 매우 쉽게 그 클래스의 인스턴스를 생성할 수 있다. 

클래스의 이름을 알아내는 코드 :  `Class.forName("클래스명").newInstance` 

아래 예제는 로컬에서 서버를 실행해 터미널로 클라이언트를 접속하도록 구현했다. 또한 Observer패턴을 적용한 것을 전제로 한다.

### 예제 - 1. RequestMappingListener클래스

```java
// 클라이언트 요청을 처리할 커맨드 객체를 준비한다.
public class RequestMappingListener implements ApplicationContextListener {

  Map<String,Object> context;

  @Override
  public void contextInitialized(Map<String,Object> context) {
    try {
      // 다른 메서드에서 context 맵 객체를 사용할 수 있도록
      // 인스턴스 필드에 저장한다.
      this.context = context;

      // 커맨드 클래스가 있는 패키지의 파일 경로를 알아내기
      // => Mybatis 에서 제공하는 클래스의 도움을 받는다.
      File path = Resources.getResourceAsFile("com/eomcs/pms/handler");

      // => 파일 경로에 URL 인코딩 문자가 들어 있으면 제거한다.
      String decodedFilePath = URLDecoder.decode(path.getCanonicalPath(), "UTF-8");

      // => URL 디코딩된 파일 경로를 가지고 새로 파일 경로를 만든다.
      File commandPackagePath = new File(decodedFilePath);

      System.out.println(commandPackagePath.getCanonicalPath());

      // 해당 패키지의 있는 커맨드 클래스를 찾아 인스턴스를 생성한다.
      Map<String,Object> commandMap = createCommands(commandPackagePath, "com.eomcs.pms.handler");

      // 커맨드 객체만 모아 놓은 상자를 context 맵이라는 큰 상자에 담는다.
      context.put("commandMap", commandMap);

      // 테스트 용 로그인 사용자 정보 가져오기
      //Member member = memberService.get("aaa@test.com", "1111");
      //context.put("loginUser", member);


    } catch (Exception e) {
      System.out.println("서비스 객체를 준비하는 중에 오류 발생!");
      e.printStackTrace();
    }
  }
```



### 2. createCommands 메서드

```java
  private Map<String,Object> createCommands(File packagePath, String packageName) {
    HashMap<String,Object> commandMap = new HashMap<>();

    File[] files = packagePath.listFiles((dir, name) -> name.endsWith(".class"));

    for (File f : files) {
      // 파일 정보를 가지고 클래스 이름을 알아낸다.
      String className = String.format("%s.%s",
          packageName,
          f.getName().replace(".class", ""));
      try {
        // 클래스 이름(패키지명 포함)을 사용하여 .class 파일을 로딩한다.
        Class<?> clazz = Class.forName(className);

        // 패키지에서 찾은 클래스가 Command 인터페이스를 구현한 클래스가 아니라면,
        // 생성자를 찾지 말고 다음 클래스로 이동한다.
        Class<?>[] interfaces = clazz.getInterfaces();
        boolean isCommand = false;
        for (Class<?> c : interfaces) {
          if (c == Command.class) {
            isCommand = true;
            break;
          }
        }

        if (!isCommand) continue; // 이 클래스는 Command 구현체가 아니다.

        // 커맨드 클래스에 붙여 놓은 @CommandAnno 애노테이션을 정보를 가져온다.
        CommandAnno commandAnno = clazz.getAnnotation(CommandAnno.class);

        // @CommandAnno 애노테이션이 클래스에 붙어 있지 않다면,
        // 해당 커맨드를 저장할 수 없기 때문에 객체를 생성하지 않는다.
        if (commandAnno == null) continue;

        // 클래스의 생성자 정보를 알아낸다.
        Constructor<?> constructor = clazz.getConstructors()[0];

        // 생성자의 파라미터 정보를 알아낸다.
        Parameter[] params = constructor.getParameters();

        // 생성자를 호출할 때 넘겨 줄 파라미터 값을 담을 배열을 준비한다.
        Object[] args = new Object[params.length];

        int i = 0;
        for (Parameter param : params) {
          args[i++] = findDependency(param.getType());
        }

        Object command = constructor.newInstance(args);
        System.out.println(command.getClass().getName() + " 객체 생성 성공!");

        // @CommandAnno 애노테이션에 지정한 커맨드 객체의 이름을 가져와서,
        // 커맨드 객체를 저장할 때 key 로 사용한다.
        commandMap.put(commandAnno.value(), command);

      } catch (Exception e) {
        System.out.println(className + " 로딩 중 오류 발생!");
      }
    }
    return commandMap;
  }
```



### 3. findDependency메서드

```java
  private Object findDependency(Class<?> type) {
    // context 맵에서 해당 타입의 객체를 찾는다.

    // 1) context 맵에 보관된 모든 객체를 꺼낸다.
    Collection<?> objs = context.values();

    // 2) 각 객체가 파라미터로 받은 타입의 인스턴스인지 확인한다.
    for (Object obj : objs) {
      if (type.isInstance(obj)) return obj;
    }
    return null;
  }
```



### 4. CommandAnno 애노테이션 인터페이스 생성

```java
// 커맨드 객체에 붙일 특별한 주석
// => 커맨드의 이름을 설정하는 용도이다.
// => JVM에서 객체를 생성할 때 사용할 것이다.
//
@Retention(RetentionPolicy.RUNTIME)
public @interface CommandAnno {
  String value(); // 이름을 저장하는 프로퍼티
}
```



### 5. CommandAnno를 적용한 HelloCommand 클래스

```java
@CommandAnno("/hello")
public class HelloCommand implements Command {

  @Override
  public void execute(Request request) {
    PrintWriter out = request.getWriter();

    out.println("안녕하세요!");
  }
}
```

이제 애노테이션으로 커맨드 이름을 지정할 수 있으며, 리플렉션을 구현하여 Command객체를 자동 생성한다.



### 실행 결과

![image](https://user-images.githubusercontent.com/68311188/99878704-a1e0d300-2c4a-11eb-9fa5-190f8764806d.png)



## 유념할 것

리플렉션은 본래가 어려운 개념이고 첫 술에 배부르려 하면 과한 욕심이다. 다만 리플렉션에 대해 꼭 알아둬야할 것은

- 클래스에 애노테이션을 붙이면 클래스 정보를 추출할 수 있구나!

- 그럼 우린 객체를 생성해서 자동으로 관리할 수 있구나!

정도가 되겠다. 당장은 코드 한줄한줄 이해하는 것 보다 전체적인 그림을 보는 것에 집중하자.

