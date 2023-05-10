---
layout: single
title:  "쇼핑 페이지 예제 클론코딩"
---

# 쇼핑 페이지 예제 클론코딩

### 수업시간에 진행한 쇼핑페이지 예제입니다.

현재 로그인, 회원가입 기능 구현 완료
<br> 

5/10
- 관리자 페이지, 인터셉터 제작중

**메인페이지(main.jsp)**

![메인 페이지]({{ site.baseurl }}/images/20230510_150448.png)

<br>

**로그인 페이지(login.jsp)**

![로그인 페이지]({{ site.baseurl }}/images/20230510_150529.png)

<br>

**회원가입 페이지(join.jsp)**

![회원가입 페이지]({{ site.baseurl }}/images/20230510_150605.png)

<br>


**관리자 페이지(admin.jsp)**

![관리자 페이지]({{ site.baseurl }}/images/20230510_142703.png)

<br>

---

## 메인페이지(main.jsp)

- 기능
  - top_gnb: Ajax의 비동기 시스템을 통해 로그아웃 버튼 누를시 페이지 리로드 없이 top_gnb가 로그인 여부에 따라 바뀜

  - login button: 로그인 창으로 이동

  - join button: 회원가입 창으로 이동
  - top_gnb:
    - 일반 회원 로그인 시: 로그아웃, 마이룸, 장바구니, 고객센터
    - 관리자 로그인 시 : 관리자 페이지, 로그아웃, 마이룸, 장바구니, 고객센터
    - 비로그인 시: 로그인, 회원가입
    - **여기서의 로그인은 ajax의 비동기 방식을 사용해서 "post"방식으로 페이지를 이동함**

---

## 로그인 창(login.jsp)

- 기능
  - 로그인 실패시 로그인 버튼 위에 로그인 실패 문장 나타남
  - BCryptPasswordEncoder 라이브러리를 통해 암호화된 사용자 비밀번호 검사후 일치 하면 로그인 성공.
  <br> -> main페이지로 이동

## 회원가입 창(join.jsp)

- 기능
  - 아이디: 아이디 중복 확인, 유효성 검사
  - 비밀번호 + 비밀번호 확인: 유효성검사, 일치 확인
  - 이메일:
    - 이메일 확인: 이메일 전송 라이브러리를 이용해 확인 메일을 보냄. 확인형식은 6자리 난수를 보내 그 수를 메세지 확인창에 입력하는 방식
    - 메세지 확인창: 수신 받은 6자리 임의의 수를 입력
  - 주소: 
    - 카카오 주소록API를 사용.
    - 주소록 API를 입력하고나면 상세주소 창에 focus()
  - 회원가입
    - 데이터베이스에 정보입력
    - 비밀번호는 BCryptPasswordEncoder를 통해 암호화 하여 데이터 베이스에 저장

---

## 관리자 페이지

- 기능
  - 상품 등록 (5/10 구현중)
  - 상품 목록 (5/10 구현중)
  - 작가 등록 (5/10 구현중)
  - 작가 관리 (5/10 구현중)
  - 회원 관리 (5/10 구현중)
  - top_gnb: 메인페이지, 로그아웃, 고객센터
  - 인터셉터 기능 구현 (5/10 구현중)

<br>

- **인터셉터**
1. spring web 라이브러리 추가 <br>

![spring web mvc]({{ site.baseurl }}/images/20230510_145456.png)
 
 <br>

2. src/main/java 밑에 com.shop.interceptor 패키지 생성 후 AdimInterceptor.java, LoginInterceptor.java 생성

![spring web mvc]({{ site.baseurl }}/images/20230510_145840.png)

<br>

3. servlet-context.xml에 \<Interpretors> 태그 삽입

```xml
<!-- 인터셉터 적용 -->
	<interceptors>
		<interceptor>
			<mapping path="/member/login.do"></mapping>
			<beans:bean id="loginInterceptor" class="com.shop.interceptor.LoginInterceptor"></beans:bean>
		</interceptor>
		<interceptor>
			<mapping path="/admin/**"></mapping>
			<beans:bean id="adminInterceptor" class="com.shop.interceptor.AdminInterceptor"></beans:bean>
		</interceptor>
	</interceptors>
```

4. AdimInterceptor.java, LoginInterceptor.java에서 상속받음

```java
implements HandlerInterceptor
```

관리자 창에서 관리자가 아니거나 로그인 상태가 아닐경우 접속을 혀용하지 않고 메인페이지로 돌려보내기 위해 Interceptor 사용

- AdimInterceptor.java

```java
public class AdminInterceptor implements HandlerInterceptor{
	@Override
	public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
			throws Exception {
		HttpSession session = request.getSession();
		MemberVO lvo = (MemberVO)session.getAttribute("member");
		if(lvo == null || lvo.getAdminCk() == 0) {  //로그인 상태가 아니거나 관리자 계정이 아닌경우
			response.sendRedirect("/main");		// 메인페이지로 리다이렉트
			return false;
		}
		
		return true;
	}
}
```



- LoginInterceptor.java

```java
public class LoginInterceptor implements HandlerInterceptor{
	@Override
	public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
			throws Exception {
		HttpSession session = request.getSession();
		
		session.invalidate();
		return true;
	}
}
```










