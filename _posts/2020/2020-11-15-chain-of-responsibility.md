---
title: 디자인 패턴 - chain of responsibility
categories:
- design pattern
toc: true
toc_label: on this page
tags:
  - Java
---

# Chain of Responsibility

Chain of Responsibility디자인 패턴은 작업 요청을 받은 객체(sender)가 실제 작업자(receiver)에게 그 책임을 위임하는 구조에서 사용하는 설계 기법이다. 이 디자인 패턴의 장점으로는

- 작업자 간에 연결 고리를 구축하여 작업을 나누어 처리할 수 있다.

- 체인 방식이기 때문에 작업에 참여하는 모든 객체가 서로 알 필요가 없다.

- 오직 자신과 연결된 다음 작업자만 알면 되기 때문에 객체 간에 결합도를 낮추는 효과가 있다.

- 런타임에서 연결 고리를 변경하거나 추가할 수 있어, 상황에 따라 실시간으로 기능을 추가하거나 삭제할 수 있다.

- 보통 필터링을 구현할 때 이 설계 기법을 많이 사용한다.



## 디자인 패턴을 적용하기 전

![image](https://user-images.githubusercontent.com/68311188/99181812-5fb02100-2774-11eb-9568-eb3fb8cbee6d.png)

디자인 패턴을 적용하기 전에는 새로운 기능을 추가하기 위해서는 기존 클래스파일 사이사이에 코드를 집어넣어야 했다. 기존 코드 사이에 새 코드를 넣게 되면 가독성도 안좋을 뿐 아니라 유지보수도 힘들어진다.



## 디자인 패턴 적용 후

![image](https://user-images.githubusercontent.com/68311188/99181892-b74e8c80-2774-11eb-85bd-6b30c29c37d5.png)

쉽게 생각하자면 요청자가 필터1을 호출하면 필터1은 그다음 필터를 계속 호출한다. 제일 마지막에 있는 필터가 최종적으로 작업자를 호출하고 작업자가 수행안 작업을 순차적으로 리턴받아 요청자에게 전달한다. chain을 구현하면 작업자가 작업 수행 전/후에 기능을 삽입하기가 쉬워 유지보수가 굉장히 좋아진다.



### 구체적인 구조

![image](https://user-images.githubusercontent.com/68311188/99185584-28e70480-278e-11eb-816e-d0dc96d135ae.png)

필터가 다음 필터를 호출하는 것은 LinkedList 자료구조와 비슷하다.



## 구현

### 필터에게 제공할 정보를 다루는 Request 정의

```java
package com.eomcs.pms.handler;

import java.util.Map;

public class Request {
  String commandPath;
  Map<String, Object> context;

  public Request(String commandPath, Map<String,Object> context) {
    this.commandPath = commandPath;
    this.context = context;
  }

  public String getCommandPath() {
    return commandPath;
  }

  public Map<String, Object> getContext() {
    return context;
  }
}
```

- 사용자 입력이 들어오면 Request객체를 준비할 수 있도록 App에서 Request객체를 생성한다.

```java
Request request = new Request(inputStr, context);
```



### 커맨드 실행 전/후에 삽입되는 필터의 호출 규칙(인터페이스) 정의하기

#### CommandFilter인터페이스

필터 관리자가 호출할 메서드 규칙을 정의한다.

```java
public interface CommandFilter {
  void doFilter(Request request, FilterChain next) throws Exception;
}
```

#### FilterChain인터페이스

필터가 다음 필터를 실행시키기 위해 필터 관리자에게 요청하는 메서드 규칙을 정의한다.

```java
public interface FilterChain {
  void doFilter(Request request) throws Exception;
}
```



#### CommandFilterManager

필터들을 관리하고 실행 순서에 따라 필터를 실행시키는 클래스를 정의한다.

```java
package com.eomcs.pms.filter;

import com.eomcs.pms.handler.Request;

public class CommandFilterManager {
  // 역할 :
  // CommandFilter 구현체를 관리하고 실행시킨다.

  Chain firstChain;
  Chain lastChain;

  public void add(CommandFilter filter) {
    Chain chain = new Chain(filter);
    if(lastChain == null) {
      firstChain = lastChain = chain;
      return;
    }
    lastChain.nextChain = chain;
    lastChain = chain;
  }

  public FilterChain getFilterChains() {
    return firstChain;
  }

  private static class Chain implements FilterChain {
    CommandFilter filter;
    Chain nextChain;

    public Chain(CommandFilter filter) {
      this.filter = filter;
    }

    @Override
    public void doFilter(Request request) throws Exception {
      filter.doFilter(request, nextChain);
    }
  }
}
```



#### App에 FilterManager와 체인 실행하기

```java
// 필터 관리자 준비
CommandFilterManager filterManager = new CommandFilterManager();

// 사용자가 명령을 처리할 필터 체인을 얻는다.
FilterChain filterChain = filterManager.getFilterChains();

// 커맨드나 필터가 사용할 객체를 준비한다.
Request request = new Request(inputStr, context);

// 필터들의 체인을 실행한다.
if (filterChain != null) {
filterChain.doFilter(request);
}
```



### DefaultCommandFilter - 사용자가 입력한 명령을 처리할 커맨드를 찾아 실행시키는 필터

```java
import java.util.Map;
import com.eomcs.pms.handler.Command;
import com.eomcs.pms.handler.Request;

public class DefaultCommandFilter implements CommandFilter {
  @SuppressWarnings("unchecked")
  @Override
  public void doFilter(Request request, FilterChain next) throws Exception {

    // Request 보관소에서 context 맵 객체를 꺼낸다.
    Map<String, Object> context = request.getContext();

    // context맵에서 커맨드 객체가 들어 있는 맵을 꺼낸다.
    Map<String,Command> commandMap = (Map<String, Command>) context.get("commandMap");
    
    // 사용자가 입력한 명령에 따라 커맨드 객체를 실행한다.
    Command command = commandMap.get(request.getCommandPath());
    if (command != null) {
      try {
        command.execute(context);
      } catch (Exception e) {
        // 오류가 발생하면 그 정보를 갖고 있는 객체의 클래스 이름을 출력한다.
        System.out.println("--------------------------------------------------------------");
        System.out.printf("명령어 실행 중 오류 발생: %s\n", e);
        System.out.println("--------------------------------------------------------------");
      }
    } else {
      System.out.println("실행할 수 없는 명령입니다.");
    }
  }
}
```



### AuthCommandFilter - 로그인 유효성 검사를 하는 필터

```java
public class AuthCommandFilter implements CommandFilter {

  @Override
  public void doFilter(Request request, FilterChain next) throws Exception {
    if(request.getCommandPath().equalsIgnoreCase("/login")
        || request.getContext().get("loginUser") != null) {
      next.doFilter(request);
    } else {
      System.out.println("로그인이 필요합니다.");
    }
  }
}
```



이런식으로 Filter를 추가해나가면 최종적으로 App에 필터를 등록하는 것은 이렇게 된다.

```java
    // 필터 관리자 준비
    CommandFilterManager filterManager = new CommandFilterManager();

    filterManager.add(new LogCommandFilter(new File("command.log")));
    filterManager.add(new AuthCommandFilter());
    filterManager.add(new DefaultCommandFilter());

    // 사용자가 명령을 처리할 필터 체인을 얻는다.
    FilterChain filterChain = filterManager.getFilterChains();
```

기능 추가와 삭제, 위치변경이 굉장히 쉬워진 것을 알 수 있다.