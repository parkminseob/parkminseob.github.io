```java
package com.eomcs.pms.web.listener;

import javax.servlet.annotation.WebListener;
import javax.servlet.http.HttpSession;
import javax.servlet.http.HttpSessionEvent;
import javax.servlet.http.HttpSessionListener;
import com.eomcs.pms.domain.Member;
import com.eomcs.pms.service.MemberService;

@WebListener
public class AutoLoginListener implements HttpSessionListener{

  // 그 클라이언트에 대해서 세션이 만들어 질때 딱 한번 호출되서 자동 로그인 된다.
  @Override
  public void sessionCreated(HttpSessionEvent se) {
    System.out.println("세션생성됨ㅋㅋ");
    try {
      HttpSession session = se.getSession();
      MemberService memberService =
          (MemberService) session.getServletContext().getAttribute("memberService");
      Member member = memberService.get("aaa@aaaa", "1111");
      session.setAttribute("loginUser", member);
    } catch (Exception e) {
      e.printStackTrace();
    }
  }
}
```

