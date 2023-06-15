---
layout: single
title: "(SpringBoot) 배민 클론 코딩2"
categories:
  - SpringBoot
---

<br>

**깃허브 repository** : [SpringBoot Baemin practice](https://github.com/Molban2J/Baemin.git "github")

\* 참고한 블로그: [blog link](https://sumin2.tistory.com/10?category=900942 "github")

<br>
<br>
<br>

# MyPage, Login Page 만들기

<br>
<br>
<br>

## 목차

* [MyPage, Login Page 만들기](#mypage-login-page-만들기)

    * [User Controller 생성](#user-controller-생성)
    * [View 생성](#view-생성)

    <br>
* [회원가입](#회원가입)
    * [Controller, view 설정](#controller-view-설정)
    * [Validation](#validation)
    * [Mybatis 사용하기](#mybatis-사용하기)

<br>
<br>
<br>



## User Controller 생성

---

<br>

controller 패키지 밑에 UserController.java를 생셩해 줍니다.

<br>

```java
@Controller
public class UserController {
    @GetMapping("/myPage")
    public String myPage(){
        return "user/myPage";
    }

    @GetMapping("/login")
    public String login(){
        return "user/login";
    }
}
```

<div style="text-align:center; font-size:0.8em;">UserController.java</div>

<br>
<br>

## View 생성

---

<br>

 resources > templates 경로에 user 폴더를 만들고 login.html, myPage.html 생성해준다.

<br>

그리고 resources > static > css 경로에도 똑같이 user폴더 만들고 login.css, myPage.css 생성해준다.

밑에 소스들을 붙여 넣기한다.

<details>
<summary>myPage.html</summary>
<div markdown="1">

```html
<!DOCTYPE html>
<th:block th:insert="include/link.html"></th:block>
<link rel="stylesheet" href="/css/layout/nav.css">
<link rel="stylesheet" href="/css/user/myPage.css">

<th:block th:insert="include/header.html"></th:block>

<div class="wrap">

    <section class="title">
        <h1>my 배민</h1>
    </section>

    <!-- 콘텐츠 -->
    <main>
        <div class="container">

            <div class="grid_box">
                <div class="login_box">
                        <a th:if="${empty SPRING_SECURITY_CONTEXT }" href="/login"><span>로그인을 해주세요</span></a>
                    <div th:if="${!empty SPRING_SECURITY_CONTEXT }">
                        <div th:with="nickname=${#authentication.principal.user.nickname}"></div>
                        <a href="/user/myInfo"><span class="nickname" data-nickname=${nickname } >${nickname }</span></a>
                        <button type="button" class="logout">로그아웃</button>
                    </div>
                </div>


                <div>
                    <a href="/user/point" onclick="return loginCheck();">
	                       	<span class="img_box">
	                       		<img src="/img/icon11.png" alt="포인트">
	                       	</span>
                        <span>포인트</span>
                    </a>
                </div>


                <div>
                    <a class="updating" href="/myPage/coupon" onclick="return false;">
	               		  	<span class="img_box">
	                			<img src="/img/icon22.png" alt="쿠폰함">
	               			</span>
                        <span>쿠폰함</span>
                    </a>
                </div>


                <div>
                    <a class="updating" href="/myPage/gift" onclick="return false;">
	                 		<span class="img_box">
	                 			<img src="/img/icon33.png" alt="선물함">
	                 		</span>
                        <span>선물함</span>
                    </a>
                </div>


                <div>
                    <a href="/likes/store">
							<span class="img_box">
								<img src="/img/icon44.png" alt="찜한가게">
							</span>
                        <span>찜한가게</span>
                    </a>
                </div>


                <div>
                    <a href="/orderList">
							<span class="img_box">
								<img src="/img/icon55.png" alt="주문내역">
							</span>
                        <span>주문내역</span>
                    </a>
                </div>


                <div>
                    <a href="/user/myReview" onclick="return loginCheck()" >
							<span class="img_box">
								<img src="/img/icon66.png" alt="리뷰관리">
							</span>
                        <span>리뷰관리</span>
                    </a>
                </div>

            </div>

        </div>
    </main>

</div>

<!-- 콘텐츠 -->


<!-- 하단 메뉴 -->
<th:block th:insert="include/nav.html"></th:block>
<!-- 하단 메뉴 -->

<!-- 푸터 -->
<th:block th:insert="include/footer.html"></th:block>
<!-- 푸터 -->

<script type="text/javascript">

    $(".updating").click(function () {
        swal("업데이트 준비 중 입니다");
    })

    $(".logout").click(function () {
        location.href = "/logout";
    })

    function loginCheck(){
        const nickname = $(".nickname").data("nickname");
        if(!nickname) {
            swal("로그인 후 이용 가능합니다");
            return false;
        }
        return true;

    }

</script>

</body>

</html>

```

<div style="text-align:center; font-size:0.8em;">myPage.html</div>

</div>
</details>




<br>
<br>

> 여기서도 jsp를 thymeleaf로 바꿔줬다. include문은 이전 게시글처럼 똑같이 바꿔 주었고,

```html
<div class="login_box">
	  <c:if test="${empty SPRING_SECURITY_CONTEXT }">
	      <a href="/login"><span>로그인을 해주세요</span></a>
	  </c:if>
	                    
	                    
	  <c:if test="${!empty SPRING_SECURITY_CONTEXT }">
        <c:set var="nickname" value="${SPRING_SECURITY_CONTEXT.authentication.principal.user.nickname }" />
	      <a href="/user/myInfo"><span class="nickname" data-nickname=${nickname } >${nickname }</span></a>
				<button type="button" class="logout">로그아웃</button>
	  </c:if>
</div>
```

<div style="text-align:center; font-size:0.8em;">기존 코드</div>

<br>

```html
<div class="login_box">
    <a th:if="${empty SPRING_SECURITY_CONTEXT }" href="/login"><span>로그인을 해주세요</span></a>
    <div th:if="${!empty SPRING_SECURITY_CONTEXT }">
        <div th:with="nickname=${#authentication.principal.user.nickname}"></div>
        <a href="/user/myInfo"><span class="nickname" data-nickname=${nickname } >${nickname }</span></a>
        <button type="button" class="logout">로그아웃</button>
    </div>
</div>
```

<div style="text-align:center; font-size:0.8em;">수정 코드</div>

> c:if 는 th:if로 바꿔 줬다. 여기서는 이미 SPRING_SECURITY_CONTEXT가 null(empty)인지 체크하고 있기 때문에 따로 null 체크를 안해줘도 된다. 그리고 th:if는 c:if와는 다르게 단독으로 못쓰이기때문에 앞에 th:block 이나 div 태그같이 다른태그를 함께 붙여준다. 또한 **c:set**은 **th:with**로 바꿔줬다. <br> 그리고 기존의 ${SPRING_SECURITY_CONTEXT.authentication.principal.user.nickname } 부분을 {#authentication.principal.user.nickname}로 바꿔줬는데, 기존 코드도 작동하겠지만 수정 후 코드는 thymeleaf에서 Spring Security에 접근할 때 더욱 간편하기 위해 **#authetication**을 사용한다고 한다.

<br>

**코드 수정(2023-06-15)**

마이페이지에 접속하려니 오류가 나서 코드를 바꿔줬다.

```html
<div class="login_box">
    <a th:if="${empty SPRING_SECURITY_CONTEXT }" href="/login"><span>로그인을 해주세요</span></a>
    <div th:if="${!empty SPRING_SECURITY_CONTEXT }">
        <div th:with="nickname=${#authentication.principal.user.nickname}"></div>
        <a href="/user/myInfo"><span class="nickname" data-nickname=${nickname } >${nickname }</span></a>
        <button type="button" class="logout">로그아웃</button>
    </div>
</div>
```

<div style="text-align:center; font-size:0.8em;">기존 코드</div>

<br>
<br>

```html
<div class="login_box">
    <a th:if="${SPRING_SECURITY_CONTEXT == null }" href="/login"><span>로그인을 해주세요</span></a>
    <div th:if="${SPRING_SECURITY_CONTEXT != null }">
        <div th:with="nickname=${#authentication.principal.user.nickname}"></div>
        <a href="/user/myInfo"><span class="nickname" data-nickname="${nickname}">${nickname }</span></a>
        <button type="button" class="logout">로그아웃</button>
    </div>
</div>
```

<div style="text-align:center; font-size:0.8em;">수정 코드</div>

<br>
<br>

<details>
<summary>myPage.css</summary>
<div markdown="1">

```css
section.title {
    width: 100%;
}
 
section.title h1 {
    text-align: center;
    margin : 30px 0 30px 0 ;
}
 
/* 콘텐츠 */
 
main {
	/* min-height: 390px; */
}
 
main .container {
    max-width: 1200px;
    margin: 0 auto;
}
 
main .container .grid_box {
	margin: 0 auto 30px;
	display: grid;
	grid-template-columns: 1fr 1fr 1fr;
	grid-template-rows: 75px 1fr 1fr;
	width: 67%;
	text-align: center;
	border-right: 1px solid #ddd;
	border-bottom: 1px solid #ddd;
}
 
main .container .grid_box > div {
	border: 1px solid #ddd; 
	border-bottom: none;
	border-right: none;
}
 
main .container .grid_box .login_box {
    grid-column-end: span 3;
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0 30px; 
}
 
main .container .grid_box .login_box span {
	display: inline;
}
 
main .container .grid_box .login_box a {
	font-size: 2rem;
	font-weight: bold
}
	
main .container .grid_box .login_box a:after {
	content: '>';
	color: rgb(202, 198, 198);
	margin-left: 5px;
}
 
main .container .grid_box .login_box .logout {
	background: none;
	border: none;
	cursor: pointer;
	font-size: 1.5rem;
}
 
main .container .grid_box > div a span:last-child {
	padding-bottom: 15px;
	margin-top: -10px;
}
 
main .container .grid_box > div a span {
	display: block;
}
 
main .container .grid_box > div img {
	width: 130px;
}
 
 
@media (max-width:1023px) {
	main .container .grid_box {
		width: 80%;
	}
}
@media  (max-width:767px) {
	.wrap {
		min-height: calc(100vh - 277px);
	}
	
	main .container .grid_box {
		width: 90%;
		grid-template-rows: 50px 1fr 1fr;
	}
	
	main .container .grid_box > div img {
	    width: 100px;
	} 
}
 
 
@media  (max-width:480px) {
	.wrap {
		min-height: calc(100vh - 274px);
	}
	main .container .grid_box {
		width: 99%;
	}
	
	main .container .grid_box > div img {
	    width: 80px;
	} 
	
}

```

<div style="text-align:center; font-size:0.8em;">myPage.css</div>

</div>
</details>

<br>

<details>
<summary>login.html</summary>
<div markdown="1">

```html
<!DOCTYPE html>
<th:block th:insert="include/link.html"></th:block>

<link rel="stylesheet" href="/css/user/login.css">
</head>
<body>
<main>
    <div class="login_box">
        <a href="/"><img src="/img/bamin2.png" alt="이미지" class="bm_img"></a>

        <form action="/login" method="post">

            <div class="input_aera"><input type="text" name="username"  value="" required placeholder="이메일을 입력해 주세요" maxlength="30" ></div>
            <div class="input_aera"><input type="password" name="password" value="" required placeholder="비밀번호를 입력해 주세요" maxlength="30"></div>

            <input type="submit" value="로그인" class="login_btn" >

            <div class="box">
                <div class="continue_login">
                    <label for="continue_login">
                        <span>로그인 유지하기</span>
                        <input type="checkbox" id="continue_login" name="remember-me" >
                        <i class="fas fa-check-square"></i>
                    </label>
                </div>

                <div>
                    <span class="id_search"><a href="/find/id">아이디</a></span>
                    <span> ㅣ </span>
                    <span><a href="/find/password">비밀번호 찾기</a></span>
                </div>
            </div>
        </form>

        <div id="oauth_login">
            <div>
                <a href="/oauth2/authorization/kakao"></a>
            </div>

            <div>
                <a href="/oauth2/authorization/naver"></a>
            </div>

            <div>
                <a href="/oauth2/authorization/google"></a>
            </div>
        </div>

        <div class="join"><a href="/join" >회원 가입하러 가기</a></div>
    </div>
</main>


</body>
</html>

```

<div style="text-align:center; font-size:0.8em;">login.html</div>

</div>
</details>

<br>

<details>
<summary>login.css</summary>
<div markdown="1">

```css
main {
    width: 70%;
    margin: 40px auto;
    border: 1px solid #ddd;
    border-radius: 15px;
    padding-bottom: 40px;
}

.login_box {
    margin: 0px auto;
    text-align: center;
}

.bm_img {
    width: 50%;
}

.login_box .input_aera {
    margin: 10px auto;
    width: 80%;
}

.login_box input {
    margin: 3px auto;
    width: 100%;
    height: 40px;
    padding: 0 2%;
    border: 1px solid #ddd;
    box-sizing: border-box;
}

.login_box .login_btn,
#login_btn  {
    background: #2AC1BC;
    border-radius: 10px;
    width: 80%;
    height: 45px;
    color: #fff;
    cursor: pointer;
    font-size: 1.8rem;
    font-weight: bold;
}

.login_box .box {
    display: flex;
    justify-content: space-between;
    width: 80%;
    margin: 10px auto 40px auto;
}

.login_box .box .continue_login {
    direction: rtl;
}

.login_box .box i {
    font-size: 2rem;
    color: #999999;
}

input[type="checkbox"] {
    display: none;
}
input[type="checkbox"]:checked ~ i{
    color: #2AC1BC;
}

#oauth_login div {
    width: 300px;
    margin: 5px auto;
    height: 55px;
    border-radius: 5px;
    box-shadow: 0px 2px 3px 0px rgb(0 0 0 / 25%);
}


#oauth_login div a {
    display: block;
    width: 100%;
    height: 100%;
}

#oauth_login div:nth-child(1) a {
    background: url("/img/kakao_login_btn.png") no-repeat center;
    background-size: cover;

}

#oauth_login div:nth-child(2) a {
    background: url("/img/naver_login_btn2.png") no-repeat center;
    background-size: cover;
}

#oauth_login div:nth-child(3) a {
    background: url("/img/btn_google.png") no-repeat center;
    background-size: cover;
}

.join {
    margin-top: 20px;
}
.join a {
    color: rgb(43,206,203);
    font-weight: bold;
    font-size: 1.8rem;
    margin-top: 20px;
}

@media ( max-width :1023px) {
    main {
        width: 90%;
    }
}

@media ( max-width : 767px ) {
    html {
        font-size: 58%;
    }

    main {
        width: 99%;
    }

    .login_box .input_aera {
        width: 90%;
    }

    .login_box .login_btn {
        width: 90%;
    }

    .login_box .box {
        width: 90%;
        margin: 10px auto;
    }

    #oauth_login {
        display: flex;
        width: 230px;
        margin: 0 auto;
    }
    #oauth_login div {
        width: 60px;
        height: 60px;
        border-radius: 5px;
        box-shadow: 0px 2px 3px 0px rgb(0 0 0 / 25%);
    }

    #oauth_login div:nth-child(1) a {
        background: url("/img/btn_kakao_m.png") no-repeat center;
        background-size: cover;
    }

    #oauth_login div:nth-child(2) a {
        background: url("/img/btn_naver_m.png") no-repeat center;
        background-size: cover;
    }

    #oauth_login div:nth-child(3) a {
        background: url("/img/btn_google_m.png") no-repeat center;
        background-size: cover;
    }


}

@media (max-width: 480px) {
    html {
        font-size: 58%;
    }

}

```

<div style="text-align:center; font-size:0.8em;">login.html</div>

</div>
</details>

<br>

---

<br>
<br>
<br>
<br>
<br>

# 회원가입

<br>
<br>

## Controller, View 설정

---

UserController에 회원가입 매핑을 작성해준다.


```java
 @GetMapping("/join")
    public String join(){
        return "user/join";
    }
```

<div style="text-align:center; font-size:0.8em;">UserController.java에 추가</div>

<br>
<br>

그리고 resources > templates > user 경로에 join.html 생성해준다.

<br>

<details>
<summary>join.html</summary>
<div markdown="1">

```html
<!DOCTYPE html>
<th:block th:insert="include/link.html"></th:block>

<link rel="stylesheet" href="/css/user/login.css">
</head>
<body>
<main>
    <div class="login_box">
        <a href="/"><img src="/img/bamin2.png" alt="이미지" class="bm_img"></a>
        <form action="/join" method="post" >
            <div class="input_aera">
                <input type="text" name="username" class="username" maxlength="20"  placeholder="아이디를 입력해 주세요" >
                <span class="msg_box" th:value="${errorMsg?.username }"></span>
            </div>

            <div class="input_aera">
                <input type="password" class="password1" name="password" maxlength="20"  placeholder="비밀번호를 입력해 주세요">
            </div>

            <div class="input_aera">
                <input type="password" class="password2" maxlength="20"  placeholder="비밀번호를 한번더 입력해 주세요">
                <span class="msg_box" th:value="${errorMsg?.password }"></span>
            </div>

            <div class="input_aera">
                <input type="text" name="email" class="email"  placeholder="이메일을 입력해 주세요" >
                <span class="msg_box" th:value="${errorMsg?.email }"></span>
            </div>

            <div class="input_aera">
                <input type="text" class="nickname" name="nickname" maxlength="20"  placeholder="사용하실 닉네임을 입력해 주세요">
                <span class="msg_box" th:value="${errorMsg?.nickname }"></span>
            </div>

            <div class="input_aera">
                <input type=number name="phone" value="" class="phone" placeholder="'-' 없이 입력해 주세요" maxlength="11" >
                <span class="msg_box" th:value="${errorMsg?.phone }"></span>
            </div>

            <input type="submit" value="회원가입" class="login_btn" >
        </form>
    </div>

</main>
</body>
</html>


```

<div style="text-align:center; font-size:0.8em;">join.html</div>

</div>
</details>

<br>
<br>
<br>

![image]({{ site.baseurl }}/images/2023-06-13/20230613_113450.png)

<div style="text-align:center; font-size:0.8em;">회원가입 창</div>

<br>
<br>
<br>

이제 회원가입을 위한 post mapping을 작성해 준다.

```java
@PostMapping("/join")
    public String joinProc(Join join){
      System.out.println(join);
        return "redirect:/login";
    }
```

<div style="text-align:center; font-size:0.8em;">UserController.java</div>

<br>
<br>

Join 클래스는 아직 안만들었으니 dto 패키지를 만들어 그 안에 Join 클래스를 만들어준다. 


```java
@Getter
@Setter
@ToString
public class Join {

    private String username;

    private String password;

    private String email;

    private String nickname;

    private String phone;
}
```

<div style="text-align:center; font-size:0.8em;">Join.java</div>

<br>
<br>

![image]({{ site.baseurl }}/images/2023-06-13/20230613_115631.png)

<div style="text-align:center; font-size:0.8em;">회원 가입을 해본다.</div>

<br>
<br>

![image]({{ site.baseurl }}/images/2023-06-13/20230613_120005.png)

<div style="text-align:center; font-size:0.8em;">데이터가 잘 전달되는 모습</div>

<br>
<br>
아직 테이블도 만들지 않았고, 테이블에 삽입하는 sql도 작성하지 않았기 때문에 데이터 베이스에 들어간 것은 아니다.

<br>
<br>
<br>
<br>

## Validation

---

테이블에 엉뚱한 값이 들어가지 않도록 유효성 검사를 해줘야한다. 유효성 검사 라이브러리를 추가해준다.

```java
implementation 'org.springframework.boot:spring-boot-starter-validation'
```

<div style="text-align:center; font-size:0.8em;">Gradle 의존성 추가</div>

<br>
<br>

Join.java 코드를 수정해준다.

```java
@Getter
@Setter
@ToString
public class Join {
    @Pattern(regexp = "[A-Za-z0-9]{4,15}$", message = "아이디는 영어, 숫자 4 ~15자리로 입력 가능합니다")
    private String username;

    private String password;

    @Pattern(regexp = "^([0-9a-zA-Z_\\\\.-]+)@([0-9a-zA-Z_-]+)(\\\\.[0-9a-zA-Z_-]+){1,2}$", message = "올바른 이메일 형식이 아닙니다")
    private String email;

    @Pattern(regexp = "^[가-힣|a-z|A-Z|0-9|]+$", message = "닉네임은 한글, 영어, 숫자만 4 ~10자리로 입력 가능합니다")
    private String nickname;

    @Pattern(regexp = "^01([0|1|6|7|8|9])-?([0-9]{3,4})-?([0-9]{4})$", message = "휴대폰번호를 확인해 주세요")
    private String phone;
}
```

<div style="text-align:center; font-size:0.8em;">Join.java</div>

<br>
<br>

@Pattern은 Validation 라이브러리를 추가했을 때 사용할수 있는 애노테이션이다. regexp에 검사할 정규식을 입력해주고, 맞지않으면 메세지를 출력한다.

<br>

밑에 페이지는 각종 정규표현식을 정리해 놓은 사이트이다.

> [정규 표현식 모음](https://velog.io/@cherrycock/%EB%B3%B5%EC%82%AC%ED%95%B4%EC%84%9C-%EB%B0%94%EB%A1%9C-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-%EC%A0%95%EA%B7%9C%ED%91%9C%ED%98%84%EC%8B%9D%EB%AA%A8%EC%9D%8C "정규 표현식 모음")

<br>
<br>

UserController join post mapping도 수정해줍니다.

<br>

```java
@PostMapping("/join")
    public String joinProc(@Valid Join join, BindingResult bindingResult, Model model){
        System.out.println(join);
        if(bindingResult.hasErrors()){
            System.out.println("Error");

            List<FieldError> list = bindingResult.getFieldErrors();
            Map<String, String> errorMsg = new HashMap<>();

            for(int i = 0; i< list.size(); i++){
                String field = list.get(i).getField();
                String message = list.get(i).getDefaultMessage();

                System.out.println("필드 = " + field);
                System.out.println("메세지 = "+ message);

                errorMsg.put(field, message);
            }
            model.addAttribute("errorMsg", errorMsg);
            return "user/join";
        }

        return "redirect:/login";
    }
```

<div style="text-align:center; font-size:0.8em;">UserController.java</div>

<br>
<br>
<br>

> **@Valid** 어노테이션을 통해 Join 객체의 유효성을 검사하고 이에 대한 결과 값을 BindinResult 객체에 저장한다. 이후 hasError()를 통해 에러가 있다면 List에 FieldError를 담고, 이를 다시 Field와 DefaulMessage로 나누어 HashMap에 저장해 전달해준다. <br> **FieldError**는 유효성 검사중 발생하는 에러를 나타내는 클래스이다. 주로 폼에서 발생한 오류를 저장하고 전달하기위한 객체로 사용된다고 한다. 위 코드에서 보면 알수 있듯이 에러가 발생한 Field를 가져오는 **getField()** 메서드가 있다. 그리고 기본적으로 제공하는 에러 메세지인 **DefaultMessage**가 있다.<br> **DefaultMessage**는 오버라이딩을 통해 사용자 정의 메세지를 뿌려줄수도 있고, 번역할 수 있다고도 한다.(출처: chatGPT)


<br>
<br>
<br>

![image]({{ site.baseurl }}/images/2023-06-13/20230613_155944.png)

<div style="text-align:center; font-size:0.8em;">회원가입 창에 아무것도 입력하지 않고 가입하기 눌렀을때 모습. 콘솔창에 에러 메세지가 뜬다.</div>

<br>
<br>

 하지만 view에서 서버로 데이터를 전송하면서 나오는 것이기 때문에 애초에 view부분에서도 유효성 검사를 해줘야 한다.

 join.html 코드를 수정해 준다.


```html
<div class="login_box">
        <a href="/"><img src="/img/bamin2.png" alt="이미지" class="bm_img"></a>
        <form action="/join" method="post" onsubmit="return isSubmit.isSubmit();">
            <div class="input_aera">
                <input type="text" name="username" class="username" maxlength="20"  placeholder="아이디를 입력해 주세요" >
                <span class="msg_box" th:value="${errorMsg?.username }"></span>
            </div>

            <div class="input_aera">
                <input type="password" class="password1" name="password" maxlength="20"  placeholder="비밀번호를 입력해 주세요">
            </div>

            <div class="input_aera">
                <input type="password" class="password2" maxlength="20"  placeholder="비밀번호를 한번더 입력해 주세요">
                <span class="msg_box" th:value="${errorMsg?.password }"></span>
            </div>

            <div class="input_aera">
                <input type="text" name="email" class="email"  placeholder="이메일을 입력해 주세요" >
                <span class="msg_box" th:value="${errorMsg?.email }"></span>
            </div>

            <div class="input_aera">
                <input type="text" class="nickname" name="nickname" maxlength="20"  placeholder="사용하실 닉네임을 입력해 주세요">
                <span class="msg_box" th:value="${errorMsg?.nickname }"></span>
            </div>

            <div class="input_aera">
                <input type=number name="phone" value="" class="phone" placeholder="'-' 없이 입력해 주세요" maxlength="11" >
                <span class="msg_box" th:value="${errorMsg?.phone }"></span>
            </div>

            <input type="submit" value="회원가입" class="login_btn" >
        </form>
    </div>
    <script src="/js/util/util.js"></script>
    <script type="text/javascript" src="/js/user/join.js"></script>
```

<div style="text-align:center; font-size:0.8em;">join.html</div>

<br>
<br>

form 태그에 onSubmit 속성과 밑에 script태그를 삽입해줬다.

그리고 resources > static 경로에 js 폴더를 만들어주고 밑에 util과 user파일을 만들어준다. 각각의 파일 밑에 util.js, join.js를 생성해준다.


<details>
<summary>util.js</summary>
<div markdown="1">

```javascript
function usernameCheck(username) {
    const regUsername =  /^[A-Za-z0-9]{4,15}$/;

    if(regUsername.test(username)) {
        return true;
    } else {
        return false;
    }
}

function emailCheck(email){
    const regEmail = /^([0-9a-zA-Z_\.-]+)@([0-9a-zA-Z_-]+)(\.[0-9a-zA-Z_-]+){1,2}$/;

    if(regEmail.test(email)) {
        return true;
    } else {
        return false;
    }
}

function phoneCheck(phone){
    const regPhone = /^01([0|1|6|7|8|9])-?([0-9]{3,4})-?([0-9]{4})$/;
    if(regPhone.test(phone)) {
        return true;
    } else {
        return false;
    }
}


function nicknameCheck(nickname) {
    const regNickname = /^[가-힣|a-z|A-Z|0-9|]+$/;
    if (regNickname.test(nickname)) {
        return true;
    } else {
        return false;
    }
}



function lenthCheck(e, length) {
    if(e.value.length >= length) {
        return false;
    }

    $(this).off().focusout(function(){
        if(e.value.length > length) {
            e.value = "";
        }
    })

    return true;
}

```

<div style="text-align:center; font-size:0.8em;">util.js</div>

</div>
</details>

<br>
<br>

<details>
<summary>join.js</summary>
<div markdown="1">

```javascript
$(".login_btn").css("background", "#ddd");


const isSubmit = (function(){
    let usernameCheck = false;
    let passwordCheck = false;
    let emailCheck = false;
    let nicknameCheck = false;
    let phoneCheck = false;

    const setUsernameCheck = function(set){
        usernameCheck = set ? true : false;
        isSubmit();
    }
    const setpasswordCheck = function(set){
        passwordCheck = set ? true : false;
        isSubmit();
    }
    const setemailCheck = function(set){
        emailCheck = set ? true : false;
        isSubmit();
    }
    const setnicknameCheck = function(set){
        nicknameCheck = set ? true : false;
        isSubmit();
    }
    const setphoneCheck = function(set){
        phoneCheck = set ? true : false;
        isSubmit();
    }

    const isSubmit = function(){
        if(usernameCheck && passwordCheck && emailCheck && nicknameCheck && phoneCheck) {
            $(".login_btn").css("background", "#2AC1BC");
            return true;
        } else {
            $(".login_btn").css("background", "#ddd");
            return false;
        }
    }

    return {
        setUsernameCheck : setUsernameCheck,
        setpasswordCheck : setpasswordCheck,
        setemailCheck : setemailCheck,
        setnicknameCheck : setnicknameCheck,
        setphoneCheck : setphoneCheck,
        isSubmit : isSubmit
    }
})();





function overlapCheck(data) {
    /*
    let isUseable = false;
    $.ajax({
        url: "/overlapCheck",
        type: "get",
        data: data,
        async: false
    })
    .done(function(result){
        if(result == 0 ) {
            isUseable = true;
        }
    })
    .fail(function(){
        alert("에러");
    });

    return isUseable;
    */
}




function pwdCheck() {
    const password1 = $(".password1").val().replaceAll(" ", "");
    const password2 = $(".password2").val().replaceAll(" ", "");
    const msgBox = $(".password2").siblings(".msg_box");

    if(password1 && password2) {
        if(password1.includes(" ")  || password2.includes(" ")) {
            msgBox.text("비밀번호를 확인해 주세요");
            isSubmit.setpasswordCheck(false);
            return;
        }

        if(password1 != password2) {
            msgBox.text("비밀번호를 확인해 주세요");
            isSubmit.setpasswordCheck(false);
        } else {
            msgBox.text("");
            console.log("사용가능");
            isSubmit.setpasswordCheck(true);
        }
    }

}





$(".username").focusout(function(){
    const username = $(".username").val().replaceAll(" ", "");
    const msgBox = $(this).siblings(".msg_box");

    if(!username) {
        msgBox.text("아이디를 입력해주세요");
        isSubmit.setUsernameCheck(false);
        return;
    }

    if(!usernameCheck(username)) {
        msgBox.text("사용할수 없는 아이디입니다");
        isSubmit.setUsernameCheck(false);
        return;
    }

    const data = {
        value : username,
        valueType : "username"
    };


    if(overlapCheck(data)) {
        msgBox.text("사용 가능합니다");
        isSubmit.setUsernameCheck(true);
    } else {
        msgBox.text("이미 사용중인 아이디입니다");
        isSubmit.setUsernameCheck(false);
    }
});


$(".password1").focusout(function() {
    pwdCheck();
});

$(".password2").focusout(function() {
    pwdCheck();
});



$(".email").focusout(function() {
    const email = $(".email").val();
    const msgBox = $(this).siblings(".msg_box");

    if (!email) {
        msgBox.text("이메일을 입력해 주세요");
        isSubmit.setemailCheck(false);
        return;
    }

    if(!emailCheck(email)) {
        msgBox.text("사용 불가능합니다");
        isSubmit.setemailCheck(false);
    } else {
        msgBox.text("");
        isSubmit.setemailCheck(true);
    }
});




$(".nickname").focusout(function() {
    const nickname = $(".nickname").val();
    const msgBox = $(this).siblings(".msg_box");

    if (!nickname) {
        msgBox.text("닉네임을 입력 해주세요");
        isSubmit.setnicknameCheck(false);
        return;
    }

    if (!nicknameCheck(nickname)) {
        msgBox.text("닉네임은 한글, 영어, 숫자만 4 ~10자리로 입력 가능합니다.");
        isSubmit.setnicknameCheck(false);
        return;
    }

    let data = {
        value: nickname,
        valueType : "nickname"
    };

    if(!overlapCheck(data)){
        msgBox.text("이미 사용중인 닉네임입니다");
        isSubmit.setnicknameCheck(false);
    } else {
        msgBox.text("사용 가능합니다");
        isSubmit.setnicknameCheck(true);
    }

}); // nickname check




$(".phone").focusout(function() {
    const phone = $(".phone").val();
    const msgBox = $(this).siblings(".msg_box");

    if(!phone) {
        isSubmit.setphoneCheck(false);
        return;
    }

    if(!phoneCheck(phone)) {
        msgBox.text("휴대폰번호를 확인해 주세요");
        isSubmit.setphoneCheck(false);
    } else {
        msgBox.text("");
        isSubmit.setphoneCheck(true);
    }
});

```

<div style="text-align:center; font-size:0.8em;">join.js</div>

</div>
</details>

<br>
<br>
<br>
<br>

## Mybatis 사용하기

---

<br>
<br>

Mybatis 의존성을 gradle에 추가해준다.

```java
implementation 'org.mybatis.spring.boot:mybatis-spring-boot-starter:2.2.0'
```

<div style="text-align:center; font-size:0.8em;">build.gradle 의존성 추가</div>

<br>
<br>

그리고 나는 처음에 appliction.yml에 Oracle Driver 설정을 해줬기 때문에 데이터베이스에 대한 설정을 따로 하지 않겠다.

<br>
<br>

추가로 application.properties에 Mybatis 설정을 추가해준다.

```properties
# MyBatis
# mapper.xml 위치 지정
mybatis.mapper-locations:com/baemin/baemin/mybatis/*.xml
# model 프로퍼티 camel case 설정
mybatis.configuration.map-underscore-to-camel-case=true
# 패키지 명을 생략할 수 있도록 alias 설정
mybatis.type-aliases-package=com.baemin.baemin.dto
```

<div style="text-align:center; font-size:0.8em;">application.properties</div>

<br>
<br>

Mapper.xml을 생성해서 하는 방식이 있고, Mapper.java에 @Mapper 어노테이션을 작성해서 하는 방법이 있는데, Mapper.xml은 ecllipse에서 많이 해봤으므로 java클래스를 생성해서 하는 방법으로 하겠다.

<br>
<br>
<br>

먼저 UserController.java에 joinProc 매서드를 수정해준다.

<br>

```java
 @PostMapping("/join")
    public String joinProc(@Valid Join join, BindingResult bindingResult, Model model){
        System.out.println(join);
        if(bindingResult.hasErrors()){
            System.out.println("Error");

            List<FieldError> list = bindingResult.getFieldErrors();
            Map<String, String> errorMsg = new HashMap<>();

            for(int i = 0; i< list.size(); i++){
                String field = list.get(i).getField();
                String message = list.get(i).getDefaultMessage();

                System.out.println("필드 = " + field);
                System.out.println("메세지 = "+ message);

                errorMsg.put(field, message);
            }
            model.addAttribute("errorMsg", errorMsg);
            return "user/join";
        }
        userService.join(join);     //추가 됨
        return "redirect:/login";
    }

    //추가됨
    @ResponseBody
    @GetMapping("/overlapCheck")
    public int overlapChec(String value, String valueType){
        // value = 중복 체크할 값
        // valueType = username, nickname
        System.out.println(value);
        System.out.println(valueType);
        int count = userService.overlapCheck(value, valueType);

        System.out.println(count);
        return count;
    }
```

<div style="text-align:center; font-size:0.8em;">UserController.java > joinProc</div>

<br>
<br>

그리고 매핑 선언부 제일 위쪽에 코드를 추가해준다.

```java
 @Autowired
    private UserService userService;
```

<div style="text-align:center; font-size:0.8em;">UserController.java</div>

<br>
<br>

아직 Service를 만들지 않아서 빨간 글씨로 뜬다. 바로 service패키지 만들어서 UserService.java와 UserServiceImpl.java를 생성해준다.

추가로 dao 패키지를 만들고, UserDAO.java, UserDAOImpl.java를 생성해준다.

*참고로* UserService.java와 UserDAO.java는 Interface로 생성한다.

<br>
<br>


```java
public interface UserService {
    public void Join(Join join);

    public int overlapCheck(String value, String valueType);
}
```

<div style="text-align:center; font-size:0.8em;">UserService.java</div>

<br>
<br>

```java
@Service
public class UserServiceImpl implements UserService{

    @Autowired
    UserDAO userDAO;

    @Override
    public void Join(Join join) {
        userDAO.join(join)
    }

    @Override
    public int overlapCheck(String value, String valueType) {
       return userDAO.overlapCheck(value, valueType);
    }
}
```

<div style="text-align:center; font-size:0.8em;">UserServiceImpl.java</div>

<br>
<br>
<br>

```java
public interface UserDAO {

    void join(Join join);
    int overlapCheck(String value, String valueType);

}
```

<div style="text-align:center; font-size:0.8em;">UserDAO.java</div>

<br>
<br>

```java
@Repository
public class UserDAOImpl implements UserDAO{

    @Autowired
    private SqlSession sql;
    @Override
    public void join(Join join) {
        sql.insert("UserMapper.join" , join);
    }

    @Override
    public int overlapCheck(String value, String valueType) {
        Map<String, String> map = new HashMap<>();
        map.put("value", value);
        map.put("valueType", valueType);
        return sql.selectOne("UserMapper.overlapCheck", map);
    }
}
```

<div style="text-align:center; font-size:0.8em;">UserDAOImpl.java</div>

<br>
<br>
<br>
<br>

이제 sql문을 작성해줄 UserMapper.java를 생성해준다.

해당 클래스는 mybatis 패키지를 만들고 그 안에 Interface로 생성해준다.

```java
@Mapper
public interface UserMapper {
    @Insert("insert into BM_USER(ID, USERNAME, PASSWORD, EMAIL, NICKNAME, PHONE VALUES USER_ID_SEQ.NEXTVAL, #{username}, #{password}, #{email}, #{nickname}, #{phone}")
    public void join();

    @Select("SELECT COUNT(*) FROM BM_USER WHERE ${valueType} = #{value}")
    public int overlapCheck();
}

```

<div style="text-align:center; font-size:0.8em;">UserMapper.java</div>

<br>
<br>

> 원래 블로그 코드의 UserDAOImpl.java에서 user.join, user.overlapCheck를 사용한다. 여기서 'user'는 xml파일로 만들었을 때 설정해주는 **namespace**명인데, @Mapper 어노테이션을 쓰면 따로 namespace를 지정해 줄 수 없고 인터페이스 명이 곧 namespace가 된다고한다. 따라서 나는 인터페이스명으로 바꿔주었다. <br> *참고* : 찾다보니 Mybatis 3.4.0부터는 @Mapper(value = "namespace명") 형식으로 namespace를 지정할 수 있는 것 같다.

<br>
<br>
<br>

join.js 안에 주석 처리 되어있는 overlapCheck ajax함수가 있는데, 주석을 없애준다.

그리고 Oracle Database에 BM_USER테이블을 생성해준다.

![image]({{ site.baseurl }}/images/2023-06-13/20230613_175637.png)

<div style="text-align:center; font-size:0.8em;">Db</div>

<br>
<br>
<br>

이제 회원가입을 해보자.

라고 하려했으나 

![image]({{ site.baseurl }}/images/2023-06-15/20230615_103349.png)

<div style="text-align:center; font-size:0.8em;">회원가입</div>

<br>
<br>

폼에 다 입력 했음에도 회원가입 버튼이 활성화 되지 않는다.

유효성 검사에서 뭔가 잘못된 것 같다.

코드에는 문제가 없었다!

문제는 바로 경로에 있었는데, js파일을 만들때 user와 util 경로를 만들고 각각에 join.js , util.js를 넣어줘야하는데 두파일이 모두 user폴더에 있어서 그랬던 것이다... 다시 경로에 맞게 넣어줬다.

![image]({{ site.baseurl }}/images/2023-06-15/20230615_105814.png)

<div style="text-align:center; font-size:0.8em;">경로 확인</div>


