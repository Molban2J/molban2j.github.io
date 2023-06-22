---
layout: single
title: "(SpringBoot) 배민 클론 코딩4-스프링시큐리티 오류"
categories:
  - SpringBoot
---

<br>

**깃허브 repository** : [SpringBoot Baemin practice](https://github.com/Molban2J/Baemin.git "github")

\* 참고한 블로그: [blog link](https://sumin2.tistory.com/10?category=900942 "github")

<br>
<br>

이전 포스팅에서 이어진다.

<br>

<br>
<br>
<br>

## Error 해결하기

<br>
<br>

로그인이 제데로 작동하지 않는다..

우선 테이블 생성할 때 role 칼럼의 default 값이 잘못 됐다. 'ROLE_USER' -> 'USER' 로 고쳐준다.

```sql
  CREATE TABLE BM_USER (
    ID NUMBER PRIMARY KEY,
    USERNAME VARCHAR2(100) NOT NULL,
    PASSWORD VARCHAR2(100) NOT NULL,
    EMAIL VARCHAR2(50) ,
    NICKNAME VARCHAR2(50),
    POINT NUMBER DEFAULT 0,
    PHONE VARCHAR2(20) ,
    RATING VARCHAR2(50) DEFAULT 0,
    ROLE VARCHAR2(20) DEFAULT 'USER'  --수정
); 
```

<div style="text-align:center; font-size:0.8em;">테이블</div>

<br>
<br>

다시 회원가입을 하고 로그인을 했더니 그래도 이번에는 마이페이지로 잘 넘어간다.

![image]({{ site.baseurl }}/images/2023-06-16/20230616_121635.png)

<div style="text-align:center; font-size:0.8em;">로그인 후 마이페이지 화면</div>

<br>
<br>
<br>

하지만 다시 로그인 버튼이 생기는 것을 보니 제데로 세션을 물고 오질 못하는 것 같다. 확인해보니 역시나 세션이 이 없다.

![image]({{ site.baseurl }}/images/2023-06-16/20230616_123201.png)

<div style="text-align:center; font-size:0.8em;">f12로 세션 확인</div>

<br>
<br>

LoginSuccess.java 와 LoginFail.java에 출력문을 넣어보니 그래도 유저 정보는 잘 갖고 온다.

![image]({{ site.baseurl }}/images/2023-06-16/20230616_124016.png)

<div style="text-align:center; font-size:0.8em;">유저정보 잘 가져오는 모습</div>

<br>
<br>

그렇다면 일단 LoginFail에서 requestDispatcher가 제데로 작동 안하는 것으로 보이고, LoginSuccess에서는 myPage에서 **SPRING_SECURITY_CONTEXT != null** 이부분을 확인해 봐야할 것 같다.

<br>
<br>
<br>
<br>

> 문제가 도저히 해결이 안되서 라이브러리 문제인가 싶어, 새로 프로젝트를 생성하고 라이브러리는 처음에 의존성 주입할 때 넣어주고 나머지는 모두 똑같이 생성을 해봤다. 그랬더니 로그인이 잘 실행 되길래 그 프로젝트의 gradle을 복사해서 붙여 넣었더니 로그인이 잘 된다.

<br>

```java
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-security'
    implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
    implementation 'org.springframework.boot:spring-boot-starter-web'
    implementation 'org.thymeleaf.extras:thymeleaf-extras-springsecurity5'
    implementation 'org.mybatis.spring.boot:mybatis-spring-boot-starter:2.3.1'
    implementation 'org.springframework.boot:spring-boot-starter-validation'
    implementation 'org.jetbrains:annotations:24.0.0'
    compileOnly 'org.projectlombok:lombok'
    developmentOnly 'org.springframework.boot:spring-boot-devtools'
    runtimeOnly 'com.oracle.database.jdbc:ojdbc8'
    annotationProcessor 'org.projectlombok:lombok'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
    testImplementation 'org.springframework.security:spring-security-test'
}
```

<div style="text-align:center; font-size:0.8em;">build.gradle</div>

<br>



> 게다가 view에서 SPRING_SERCURITY_CONTEXT인식을 못하길래 계속 찾아보니 Thymeleaf에서는 SPRING_SERCURITY_CONTEXT를 인식을 못한다고 한다. 하지만 Spring Security와 호환성이 높아져서 #authentication으로 접근 가능하다고 한다. 또한 namespace를 활용하여 코드를 더 간결하게 할 수 있어서 수정을 해줬다.

<br>

```html
<div class="login_box">
    <div sec:authorize="!isAuthenticated()">
        <a href="/login"><span>로그인을 해주세요</span></a>
    </div>
    <div sec:authorize="isAuthenticated()">
        <a href="/user/myInfo"><span class="nickname" sec:authentication="principal.user.nickname"></span></a>
        <button type="button" class="logout">로그아웃</button>
    </div>
</div>
```

<div style="text-align:center; font-size:0.8em;">myPage.html</div>

<br>
<br>

```java
implementation 'org.thymeleaf.extras:thymeleaf-extras-springsecurity5'
```

<div style="text-align:center; font-size:0.8em;">sec namespace를 쓰기 위해서는 위 라이브러리를 꼭 넣어줘야한다.</div>

<br>
<br>

위의 !isAuthenticated() 대신 isAnonymous()를 써줘도 된다.

<br>
<br>

![image]({{ site.baseurl }}/images/2023-06-20/20230620_110449.png)

<div style="text-align:center; font-size:0.8em;">로그인 성공</div>

<br>
<br>
<br>

이제 남은 문제점은 로그인 실패시 화면이다.

 **문제인식** : 먼저 LoginFail에서 HttpServlet을 사용한다는 것인데, 여기서 **request.getRequestDispatcher** 가 jsp에서 사용하는 방식이라는 것이다. 그렇다고 sendRedirect를 사용하면 기존 setAttribute에서의 에러 메세지가 나오지 않는다. thymeleaf 엔진에 알맞은 방식을 찾아야겠다.

<br>

결국 마땅하게 찾지 못했다. 대신 주소에 error 파라미터 값을 직접 전달해 주는 방식으로 했다.


```java
@Component
public class LoginFail implements AuthenticationFailureHandler {
    @Override
    public void onAuthenticationFailure(HttpServletRequest request, HttpServletResponse response, AuthenticationException exception) throws IOException, ServletException {
        String loginFailMsg ="";
        if(exception instanceof BadCredentialsException || exception instanceof InternalAuthenticationServiceException){
            //request.setAttribute("loginFailMsg", "아이디와 비밀번호를 확인해 주세요");
            loginFailMsg = "?error=loginFailMsg";
            System.out.println("실패 메세지 전송");
        }
        System.out.println("로그인 실패");
        //request.getRequestDispatcher("/resources/templates/user/login.html").forward(request, response);
        response.sendRedirect("/login"+loginFailMsg);
    }
}
```

<div style="text-align:center; font-size:0.8em;">LoginFail.java 수정</div>

<br><br><br>


```html
<div th:if ="${param.error}">
    <script type="text/javascript">
        const msg = "아이디와 비밀번호를 확인해 주세요";
        swal(msg);
    </script>
</div>
```

<div style="text-align:center; font-size:0.8em;">login.html 수정</div>

<br><br><br>

이렇게하면 파라미터 값을 인식해서 오류 메세지를 띄운다.

<br><br>

![image]({{ site.baseurl }}/images/2023-06-20/20230620_121632.png)

<div style="text-align:center; font-size:0.8em;">로그인 실패(주소창에 파라미터  error 값을 갖고간다.)</div>

<br>
<br>
<br>

LoginFail부분에대해서는 좀 더 고민을 해봐야할 것 같다.

<br>
<br>
<br>
<br>




