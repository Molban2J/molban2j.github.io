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
  - 인터셉터 기능 구현 (5/10 ~~구현중~~ 완료)

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


---

---

### **5/11**

### 작가 테이블 구성, 푸터, 로고, 관리자 페이지

<br>

**작가 테이블 생성**

```
create table jm_nation(
nationId varchar(2) PRIMARY key,
nationName varchar(50)
);

insert into jm_nation values('01', '국내');
insert into jm_nation values('02', '국외');

create sequence author_seq start with 1 increment by 1;

create table jm_author(
authorId number primary key,
authorName varchar2(50),
nationId varchar2(2),
authorIntro long,
foreign key (nationId) references jm_nation(nationId)
);

insert into jm_author(authorId, authorName, nationId, authorIntro) values(author_seq.nextval,'유홍준', '01', '작가 소개입니다' );
insert into jm_author(authorId, authorName, nationId, authorIntro) values(author_seq.nextval,'김난도', '01', '작가 소개입니다' );
insert into jm_author(authorId, authorName, nationId, authorIntro) values(author_seq.nextval,'폴크루그먼', '02', '작가 소개입니다' );

```

그리고 이미지 폴더 생성

![spring web mvc]({{ site.baseurl }}/images/20230511_102930.png)

메인페이지(main.jsp) logo삽입

```jsp
<div class="logo_area">
	<a href="/main"><img src="resources/img/logo.png"></a>
</div>

```

-삽입 완료

![spring web mvc]({{ site.baseurl }}/images/20230511_103634.png)


---

## footer영역 제작

<br>

*일반 메인페이지(main.jsp)와 관리자 페이지의 메인(main.jsp) 둘다 동일한 푸터 적용

-main.jsp

```jsp
<!-- footer_nav 영역 -->
			<div class="footer_nav">
				<div class="footer_nav_container">
					<ul>
						<li>회사소개</li>
						<span class="line">|</span>
						<li>이용약관</li>
						<span class="line">|</span>
						<li>고객센터</li>
						<span class="line">|</span>
						<li>광고문의</li>
						<span class="line">|</span>
						<li>채용정보</li>
						<span class="line">|</span>
					</ul>
				</div>
			</div>
			<!-- footer 영역 -->
			<div class="footer">
				<div class="footer_container">
					<div class="footer_left">
						<img src="resources/img/logo.png">
					</div>
					<div class="footer_right">
						(주)BookShop		대표이사 : 임종민
						<br>
						사업자 등록번호 : 000-12-00000
						<br>
						대표전화 : 1588-1234(발신자 부담전화)
						<br><br>
						COPYRIGHT(C) <strong>molban2j.github.io</strong>	ALL RIGHTS
				  </div>
					<div class="clearfix"></div>
        </div>
      </div>

```

<br>

-main.css

```css
/* footer nav 영역*/
.footer_nav{
	width:100%;
	height: 50px;
}

.footer_nav_container{
	width:100%;
	height: 100%;
	background-color: #8ec0e4;
}

.footer_nav_container>ul{
	font-weight: bold;
	float:left;
	list-style: none;
	position: relative;
	padding-top: 10px;
	line-height: 27px;
	font-family: dotum;
	margin-left: 10px;
}
.footer_nav_container>ul>li{
	display: inline;
	width: 45px;
	height: 19px;
	padding: 10px 9px 0 10px;
}
.footer_nav_container>ul>span{
	margin: 0 4px;
}

/*footer 영역*/
.footer{
	width: 100%;
	height: 130px;
	background-color: #d4dfe6;
	padding-bottom: 50px;
}
.footer_container{
	width: 100%;
	height: 130px;
	margin: auto;
}
.footer_left>img{
	width: 150%;
	height: 130px;
	margin-left: -20px;
	margin-top: -12px;
}
.footer_left{
	float: left;
	width: 170px;
	margin-left: 20px;
	margin-top: 30px;
}
.footer_right{
	float:left;
	width: 680px;
	margin-left: 70px;
	margin-top: 30px;
}

```

footer적용된 모습

![이미지]({{ site.baseurl }}/images/20230511_105954.png)


---
## 상품, 작가 테이블 CRUD


<br>
먼저 작가 테이블을 만들었으니 AuthorVO 생성

![이미지]({{ site.baseurl }}/images/20230511_110912.png)

<br>
<br>

-상품 등록, 상품 관리, 작가 등록, 작가 관리, 회원 관리 url 매핑 매서드 작성(AdminController.java)

```java
 /* 상품 등록 페이지 접속 */
    @GetMapping("/goodsManage")
    public void goodsManageGET() throws Exception{
        logger.info("상품 등록 페이지 접속");
    }
    
    /* 상품 등록 페이지 접속 */
    @GetMapping("/goodsEnroll")
    public void goodsEnrollGET() throws Exception{
        logger.info("상품 등록 페이지 접속");
    }
    
    /* 작가 등록 페이지 접속 */
    @GetMapping("/autorEnroll")
    public void authorEnrollGET() throws Exception{
        logger.info("작가 등록 페이지 접속");
    }
    
    /* 작가 관리 페이지 접속 */
    @GetMapping("/autorManage")
    public void authorManageGET() throws Exception{
        logger.info("작가 관리 페이지 접속");
    }
```

-관리자 페이지 \<a> 태그 추가


<br>
변경전

```html
<!-- 네비 영역 -->
		<div class="admin_navi_wrap">
			<ul>
				<li><a class="admin_list_01">상품 등록</a></li>
				<li><a class="admin_list_02">상품 목록</a></li>
				<li><a class="admin_list_03">작가 등록</a></li>
				<li><a class="admin_list_04">작가 관리</a></li>
				<li><a class="admin_list_05">회원 관리</a></li>
			</ul>
		</div>
```


<br>
변경후 

```html
<!-- 네비 영역 -->
		<div class="admin_navi_wrap">
				<ul>
					<li><a class="admin_list_01" href="/admin/goodsEnroll">상품 등록</a></li>
					<li><a class="admin_list_02" href="/admin/goodsManage">상품 관리</a></li>
					<lI><a class="admin_list_03" href="/admin/authorEnroll">작가 등록</a></lI>
					<lI><a class="admin_list_04" href="/admin/authorManage">작가 관리</a></lI>
					<lI><a class="admin_list_05">회원 관리</a></lI>
				</ul>
			</div>
```

\<a>태그 색 변경 css

```css
.admin_navi_wrap li a:link {color: black;}
.admin_navi_wrap li a:visited {color: black;}
.admin_navi_wrap li a:active {color: black;}
.admin_navi_wrap li a:hover {color: black;}
```

---

## 각페이지 생성

jsp 생성

![이미지]({{ site.baseurl }}/images/20230511_112229.png)

css 생성

![이미지]({{ site.baseurl }}/images/20230511_112446.png)

*각각의 jsp는 main.jsp에서 조금씩 수정하는 방향으로 제작

---
<br>
기존 jm_author table 수정

```oracle
alter table jm_author add regDate date default sysdate;
alter table jm_author add updateDate date default sysdate;
```

기존 데이터 삭제후 다시 삽입
<br>
그리고 테이블이 수정됐으니 AuthorVO객체도 수정해줍니다.

---

## 작가 CRUD

<br>
작가 CRUD를 위한 mapper 인터페이스와 xml을 만들어줍니다.

<br>
AuthorMapper.java

![이미지]({{ site.baseurl }}/images/20230511_114334.png)

AuthorMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.shop.mapper.AuthorMapper">
	<insert id="authorEnroll">
		insert into jm_author(authorId, authorName, nationId, authorIntro) 
		values(author_seq.nextval,#{authorName},#{nationId},#{authorIntro} )
	</insert>
</mapper>
```

<br>
<br>
매퍼를 작성 했으니 그 다음 AuthorMapperTest.java를 만들어 테스트 해줍니다.

<br>

JunitTest <br>

![이미지]({{ site.baseurl }}/images/20230511_115444.png)

<br>

![이미지]({{ site.baseurl }}/images/20230511_115444.png)

테이블에도 잘 삽입된걸 확인했으니 service인터페이스를 작성
<br>
AuthorService.java

![이미지]({{ site.baseurl }}/images/20230511_120127.png)

<br>
AuthorServiceImpl.java

![이미지]({{ site.baseurl }}/images/20230511_120321.png)

<br>
<br>
<br>

service test 패키지를 만들어 테스트를 한번 더 해줍니다

![이미지]({{ site.baseurl }}/images/20230511_121403.png)

오류 발생!
<br>
bean객체를 제데로 인식하지 못하는 에러가 발생

AuthorService.java에 service라는 걸 인지시키기 위해 어노테이션 추가

![이미지]({{ site.baseurl }}/images/20230511_121529.png)

<br>
<br>
<br>
<br>

-AdminController.java에 작가등록 매핑 작성 

```java
@PostMapping("authorEnroll.do")
    public String authorEnrollPOST(AuthorVO vo, RedirectAttributes rttr) throws Exception{
    	//작가 등록 쿼리 실행
        authorService.authorEnroll(vo);
        rttr.addFlashAttribute("enroll_result", vo.getAuthorName());
    	return "redirect:/admin/authorManage";
    }
```

-adminEnroll.jsp에 \<form> 태그 작성 


```html
<div class="admin_content_wrap">
					<div class="admin_content_subject">
					<span>작가 등록</span>
						<div class="admin_content_main">
						<form action="/admin/authorEnroll.do" method="post">
							<div class="form_section">
								<div class="form_section_title">
									<label>작가 이름</label>
								</div>
								<div class="form_section_content">
									<input name="authorName">
								</div>
							</div>
							<div class="form_section">
                    			<div class="form_section_title">
                    				<label>소속 국가</label>
                    			</div>
                    			<div class="form_section_content">
                    				<select name="nationId">
                    					<option value="none" selected>=== 선택 ===</option>
                    					<option value="01">국내</option>
                    					<option value="02">국외</option>
                    				</select>
                    			</div>
                    		</div>
                    		<div class="form_section">
                    			<div class="form_section_title">
                    				<label>작가소개</label>
                    			</div>
                    			<div class="form_section_content">
                    				<input name="authorIntro" type="text">
                    			</div>
                    		</div>
                   		</form>
                   			<div class="btn_section">
                   				<button id="cancelBtn" class="btn">취 소</button>
	                    		<button id="enrollBtn" class="btn enroll_btn">등 록</button>
	                    	</div> 
						</form>
						</div>
						
					</div>
				</div>
```

<br>
버튼 액션을 위한 스크립트 처리

```html
<script>
/*등록 버튼*/
$("#enrollBtn").click(function(){
	$("#enrollForm").submit();
});

/*취소 버튼*/
$("#cancelBtn").click(function(){
	location.href="/admin/authorManage"
});
</script>
```

<br>
adminManage.jsp에서 작가 등록에 성공하면 메세지를 띄우도록 스크립트 처리

```html
<script>
$(document).ready(function(){
	let result = "${enroll_result}";
	checkResult(result);
	
	function checkResult(result){
		if(result == ''){
			return;
		}
		alert("작가 '${enroll_result}'을 등록하였습니다.");
	}
});
</script>
```

### 현재까지의 구현 

![이미지]({{ site.baseurl }}/images/20230511_131522.png)

---





















