---
layout: single
title: "(SpringBoot) 배민 클론 코딩"
categories:
  - springboot
---

<br>

**깃허브 repository** : [SpringBoot class practice](https://github.com/Molban2J/SpringBootPractice.git "github")

\* 참고한 블로그: [blog link](https://sumin2.tistory.com/10?category=900942 "github")

<br>

## 스프링 부트 예제

Springboot로 프로젝트 만들기

배민 페이지 클론 코딩입니다.

Intellij, Oracle, Gradle 사용

<br>
참고 블로그에는 jsp를 사용했는데, 그래도 스프링 부트를 배웠는데 그에 맞게 thymeleaf로 바꿔서 해보려고한다.

<br>

## 프로젝트 생성

---

![image]({{ site.baseurl }}/images/2023-06-12/20230612_145456.png)

<div style="text-align:center; font-size:0.8em;">프로젝트 생성</div>

<br>
<br>

controller 패키지 생성 후 MainController 생성한다.

```java
 @RestController
public class MainController {

    @GetMapping("/")
    public String test() {
        return "test";
    }
}
```

<div style="text-align:center; font-size:0.8em;">MainController 생성</div>

생성 후 실행시켜 보자.

<br>
<br>

![image]({{ site.baseurl }}/images/2023-06-12/20230612_120635.png)

<div style="text-align:center; font-size:0.8em;">실행한 모습</div>

<br>
<br>
<br>

## 메인페이지 만들기

---

<br>
<br>

prefix와 suffix를 지정해 줘서 루트와 파일타입을 일일히 지정해줄 필요가 없게 설정해준다.

<br>

```java
spring:
  datasource:
    url: jdbc:oracle:thin:@127.0.0.1:1521:XE
    username: scott
    password: tiger
    driver-class-name: oracle.jdbc.driver.OracleDriver

  thymeleaf:
    prefix: classpath:templates/
    suffix: .html
    cache: false

```

<div style="text-align:center; font-size:0.8em;">application.yml에 추가</div>

<br>
그리고

MainController를 수정해준다.

<br>

```java
@Controller
public class MainController {

    @GetMapping("/")
    public String test() {
        return "home";
    }
}
```

<div style="text-align:center; font-size:0.8em;">MainController 수정</div>

<br>
<br>

그리고 resources 밑에 templates파일에 home.html파일을 생성해 준다.

![image]({{ site.baseurl }}/images/2023-06-12/20230612_121226.png)

<div style="text-align:center; font-size:0.8em;">home.html</div>

<br>
<br>

![image]({{ site.baseurl }}/images/2023-06-12/20230612_124338.png)

<div style="text-align:center; font-size:0.8em;">실행화면</div>

<br>
<br>

home.html에 다음 코드를 넣어준다.

```html
@Controller public class MainController { @GetMapping("/") public String test()
{ return "home"; } }
```

<div style="text-align:center; font-size:0.8em;">MainController 수정</div>

<br>
<br>
<br>

home.html과 css, header, footer등을 만들어준다.

기존 코드는 jstl을 사용했으나 나는 thymeleaf로 했으니 좀 수정해줘야한다.

<details>
<summary>home.html</summary>
<div markdown="1">

```html
<th:block th:include="include/link.html"></th:block>
<link rel="stylesheet" href="/css/layout/nav.css">
<link rel="stylesheet" href="/css/home.css">
<th:block th:insert="include/header.html"></th:block>
<!-- 콘텐츠 -->
<div class="wrap">
    <main>
        <section class="address_search">
            <div id="search_box">
                <div>
                    <input type="hidden" id="deleveryAddress1" placeholder="우편번호" value="${BMaddress.address1 }" name="address1" readonly>
                    <input type="text" value="${BMaddress.address2 }"
                           id="deleveryAddress2" readonly placeholder="주소를 입력해 주세요" name="address2"><br>
                </div>

                <div class="search_btn">
                    <label for="search_btn">
                        <i class="fas fa-search"></i>
                    </label>

                    <input type="button" name="search" id="search_btn">

                </div>

            </div>
        </section>
        <section class="category_box">
            <div class="box">
                <ul class="category">

                    <li>
                        <div>
                            <div class="img_box">
                                <img src="/img/pizza2.png" alt="이미지">
                            </div>
                        </div>
                        <div class="name">피자</div>
                    </li>

                    <li>
                        <div>
                            <div class="img_box">
                                <img src="/img/chicken1.png" alt="이미지">
                            </div>
                        </div>
                        <div class="name">치킨</div>
                    </li>

                    <li>
                        <div>
                            <div class="img_box">
                                <img src="/img/hamburger4.png" alt="이미지">
                            </div>
                        </div>
                        <div class="name">패스트푸드</div>
                    </li>

                    <li>
                        <div>
                            <div class="img_box">
                                <img src="/img/bunsik1.png" alt="이미지">
                            </div>
                        </div>
                        <div class="name">분식</div>
                    </li>


                    <li>
                        <div>
                            <div class="img_box">
                                <img src="/img/dessert2.png" alt="이미지">
                            </div>
                        </div>
                        <div class="name">카페/디저트</div>
                    </li>

                    <li>
                        <div>
                            <div class="img_box">
                                <img src="/img/cutlet1.png" alt="이미지">
                            </div>
                        </div>
                        <div class="name">돈까스/일식</div>
                    </li>

                    <li>
                        <div>
                            <div class="img_box">
                                <img src="/img/chinese1.png" alt="이미지">
                            </div>
                        </div>
                        <div class="name">중국집</div>
                    </li>


                    <li>
                        <div>
                            <div class="img_box">
                                <img src="/img/jockbal1.png" alt="이미지">
                            </div>
                        </div>
                        <div class="name">족발/보쌈</div>
                    </li>

                    <li>
                        <div>
                            <div class="img_box">
                                <img src="/img/jockbal2.png" alt="이미지">
                            </div>
                        </div>
                        <div class="name">야식</div>
                    </li>

                    <li>
                        <div>
                            <div class="img_box">
                                <img src="/img/bibimbap.jpg" alt="이미지">
                            </div>
                        </div>
                        <div class="name">한식</div>
                    </li>

                    <li>
                        <div>
                            <div class="img_box">
                                <img src="/img/jockbal3.png" alt="이미지">
                            </div>
                        </div>
                        <div class="name">1인분</div>
                    </li>

                    <li>
                        <div>
                            <div class="img_box">
                                <img src="/img/dosirac.jpg" alt="이미지">
                            </div>
                        </div>
                        <div class="name">도시락</div>
                    </li>
                </ul>
            </div>
        </section>
    </main>
</div>
<!-- 콘텐츠 -->


<!-- 하단 메뉴 -->
<th:block th:insert="include/nav.html"></th:block>
<!-- 하단 메뉴 -->

<!-- 푸터 -->
<th:block th:insert="include/footer.html"></th:block>
<!-- 푸터 -->

<script>
    $(".category li").click(function(){
        let address1 = $("#deleveryAddress1").val();
        if(!address1) {
            swal("배달 받으실 주소를 입력해 주세요");
            return false;
        }

        const index = $(this).index();

        location.href = "/store/" + (100+index) + "/" +address1;
    })

</script>


</body>

</html>

```

<div style="text-align:center; font-size:0.8em;">home.html</div>

</div>
</details>

<br>

파일에 header 나 body태그 등이 제데로 열리지 않거나 닫히지 않은 이유는 다른 파일을 include할때 그 파일에 포함되어 있기 때문이다.

<br>
<br>

밑에 파일들은 templates파일 밑에 include파일을 만들어 그 안에 만들어준다.

<details>
<summary>header.html</summary>
<div markdown="1">

<br>

```html

<style>
    header .admin_page_btn {
        font-size: 13px;
        position: absolute;
        right: 10px;
        top: 10px;
        font-weight: bold;
    }

    header .admin_page_btn a {
        border: 1px solid #ddd;
        border-radius: 10px;
        padding: 5px;
        background: #fff;
        font-size: 13px;
        display: block;
    }
</style>

</head>
<body >
<!-- <body> -->


<header>
    <div id="header">
        <a href="/"><img src="/img/baemin.jpg" alt="이미지"> </a>

        <th:if test="${SPRING_SECURITY_CONTEXT != null }">
            로그인중
        </th:if>

        <!-- 임시 -->
        <th:if test="${SPRING_SECURITY_CONTEXT.authentication.principal.user.role == 'ROLE_ADMIN' }">
            <div class="admin_page_btn">
                <div>
                    <a href="/admin/main">사장님 페이지</a>
                </div>
            </div>
        </th:if>
        <!-- 임시 -->

        <div class="menu_tab_box active">
            <div class="menu_tab">
                <span> </span>
                <span> </span>
                <span> </span>
            </div>
        </div>

    </div>
</header>
<!-- 헤더 -->



```

<div style="text-align:center; font-size:0.8em;">header.html</div>

</div>
</details>

<br>
<br>

<details>
<summary>footer.html</summary>
<div markdown="1">

```html
<footer>
  <div class="box">
    <div>이름</div>
    <div>깃허브</div>
    <div>전화번호</div>
    <div>이메일</div>
  </div>
</footer>
```

<div style="text-align:center; font-size:0.8em;">footer.html</div>

</div>
</details>

<br>
<br>

<details>
<summary>link.html</summary>
<div markdown="1">

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>배민</title>
    <meta
      name="viewport"
      content="width=device-width, initial-scale=1, user-scalable=no, maximum-scale=1"
    />
    <link rel="stylesheet" href="/css/layout/reset.css" />
    <link rel="stylesheet" href="/css/layout/header.css" />

    <link rel="stylesheet" href="/css/layout/footer.css" />

    <!-- 제이쿼리 -->
    <script
      type="text/javascript"
      src="http://code.jquery.com/jquery-latest.min.js"
    ></script>

    <!-- 스윗얼럿 -->
    <script src="https://unpkg.com/sweetalert/dist/sweetalert.min.js"></script>

    <!-- 폰트 어썸 -->
    <link
      rel="stylesheet"
      href="https://use.fontawesome.com/releases/v5.15.3/css/all.css"
      integrity="sha384-SZXxX4whJ79/gErwcOYf+zWLeJdY/qpuqC4cAa9rOGUstPomtqpuNWT9wdPEn2fk"
      crossorigin="anonymous"
    />

    <!-- 배민 폰트 -->
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link
      href="https://fonts.googleapis.com/css2?family=Jua&display=swap"
      rel="stylesheet"
    />

    <!-- 파비콘 -->
    <link
      href="/img/baemin_favicon.png"
      rel="shortcut icon"
      type="image/x-icon"
    />
  </head>
</html>
```

<div style="text-align:center; font-size:0.8em;">link.html</div>

</div>
</details>

<br>
<br>

<details>
<summary>nav.html</summary>
<div markdown="1">

```html
<nav>
  <ul>
    <li><a href="/"></a></li>
    <li><a href="/store/search"></a></li>
    <li><a href="/likes/store"></a></li>
    <li><a href="/orderList"></a></li>
    <li><a href="/myPage"></a></li>
  </ul>
</nav>
```

<div style="text-align:center; font-size:0.8em;">nav.html</div>

</div>
</details>

<br>
<br>

그리고 resources > static 위치에 css와 img폴더를 만들어 파일들을 넣어준다.

이 부분 설명은 생략하겠다.

<br>
<br>

실행해보니 오른쪽 위에 사장님 페이지나 보이지 말아야할 글자들이 보인다. th:if문들이 잘못되서 그런거같은데 header.html에서 코드를 수정해준다.

<br>

```html
<th:block th:if="${SPRING_SECURITY_CONTEXT != null }"> 로그인중 </th:block>

<!-- 임시 -->
<div
  th:if="${SPRING_SECURITY_CONTEXT.authentication.principal.user.role == 'ROLE_ADMIN' }"
  class="admin_page_btn"
>
  <div>
    <a href="/admin/main">사장님 페이지</a>
  </div>
</div>
```

<div style="text-align:center; font-size:0.8em;">header.html 수정</div>

<br>

수정했더니 오류메세지가 뜬다.

![image]({{ site.baseurl }}/images/2023-06-12/20230612_163647.png)

<div style="text-align:center; font-size:0.8em;">오류 화면</div>

<br>

아무래도 두번째 th:if에서 오류가 발생한 것 같다.

<br>

```html
<th:block th:if="${SPRING_SECURITY_CONTEXT != null }"> 로그인중 </th:block>

<!-- 임시 -->
<div
  th:if="${SPRING_SECURITY_CONTEXT.authentication.principal.authorities.contains('ROLE_ADMIN') }"
  class="admin_page_btn"
>
  <div>
    <a href="/admin/main">사장님 페이지</a>
  </div>
</div>
```

검색해서 이 방식으로도 수정해봤지만 authentication이 null인데 참조하려해서 오류가 발생하는 것 같다. 아직 권한기능은 없으니까 일단은 주석처리하고 넘어가겠다.

<br>
<br>
<br>

![image]({{ site.baseurl }}/images/2023-06-12/20230612_163647.png)

<div style="text-align:center; font-size:0.8em;">메인 화면</div>

<br>

검색창에 이상한 value값이 들어가 있는데, th:value를 안써줘서 그렇다. th:value는 null값을 참조 못하는데, 아직 전달해주는 값이 없어서 th:value로 바꾸면 오류가 나서 그냥 놔뒀다.

> 위 말했듯이 **th:value** 나 **th:field**는 null값을 참조하지 못한다. 그러려면 참조값이 null일 때와 null이 아닐 때의 조건문을 작성 해줘야한다. 하지만 이건 너무 번거롭다! 그래서 다른 방법이 있는데, 위 코드를 이용해 해보자면

```html
<input type="text" th:value="${BMaddress?.address1}" />
```

> 이런식으로 쓸 수 있다. 이러면 BMaddress == null 이라면 address1을 참조하지 않고 넘어간다.
<br>
그 외 방식으로는 삼항 연산자를 활용하는 방법이 있다.


```html
<input type="text" th:value="${BMaddress != ? BMaddress.address1 : defaultValue}" />
```

---

<br>
<br>
<br>

## 메인페이지2(쿠키, 세션으로 주소 저장)

---

<br>
<br>

주소를 입력하면 매장이 뜨도록 설계한다.

daum 주소 api를 사용한다.

<br>

다른 view에서도 사용할수 있게 따로 파일을 만들어 준다. 

코드도 살짝 수정해준다.

<details>
<summary>modifyAddress.html</summary>
<div markdown="1">

```html
<!DOCTYPE html>

<input type="hidden" id="sample2_extraAddress" placeholder="참고항목" readonly>


<!-- iOS에서는 position:fixed 버그가 있음, 적용하는 사이트에 맞게 position:absolute 등을 이용하여 top,left값 조정 필요 -->
<div id="layer"
     style="display: none; position: fixed; overflow: hidden; z-index: 2; -webkit-overflow-scrolling: touch;">
  <img src="//t1.daumcdn.net/postcode/resource/images/close.png"
       id="btnCloseLayer"
       style="cursor: pointer; position: absolute; right: -3px; top: -3px; z-index: 1"
       onclick="closeDaumPostcode()" alt="닫기 버튼">
</div>

<script src="//t1.daumcdn.net/mapjsapi/bundle/postcode/prod/postcode.v2.js"></script>

<script>
  // 우편번호 찾기 화면을 넣을 element
  var element_layer = document.getElementById('layer');

  function closeDaumPostcode() {
    // iframe을 넣은 element를 안보이게 한다.
    element_layer.style.display = 'none';

  }

  function modifyAddress() {
    new daum.Postcode(
            {
              oncomplete : function(data) {
                // 검색결과 항목을 클릭했을때 실행할 코드를 작성하는 부분.

                // 각 주소의 노출 규칙에 따라 주소를 조합한다.
                // 내려오는 변수가 값이 없는 경우엔 공백('')값을 가지므로, 이를 참고하여 분기 한다.
                var addr = ''; // 주소 변수
                var extraAddr = ''; // 참고항목 변수

                //사용자가 선택한 주소 타입에 따라 해당 주소 값을 가져온다.
                if (data.userSelectedType === 'R') { // 사용자가 도로명 주소를 선택했을 경우
                  addr = data.roadAddress;
                } else { // 사용자가 지번 주소를 선택했을 경우(J)
                  addr = data.jibunAddress;
                }

                // 사용자가 선택한 주소가 도로명 타입일때 참고항목을 조합한다.
                if (data.userSelectedType === 'R') {
                  // 법정동명이 있을 경우 추가한다. (법정리는 제외)
                  // 법정동의 경우 마지막 문자가 "동/로/가"로 끝난다.
                  if (data.bname !== ''
                          && /[동|로|가]$/g.test(data.bname)) {
                    extraAddr += data.bname;
                  }
                  // 건물명이 있고, 공동주택일 경우 추가한다.
                  if (data.buildingName !== ''
                          && data.apartment === 'Y') {
                    extraAddr += (extraAddr !== '' ? ', '
                            + data.buildingName : data.buildingName);
                  }
                  // 표시할 참고항목이 있을 경우, 괄호까지 추가한 최종 문자열을 만든다.
                  if (extraAddr !== '') {
                    extraAddr = ' (' + extraAddr + ')';
                  }
                  // 조합된 참고항목을 해당 필드에 넣는다.
                  document.getElementById("sample2_extraAddress").value = extraAddr;

                } else {
                  document.getElementById("sample2_extraAddress").value = '';
                }

                // 우편번호와 주소 정보를 해당 필드에 넣는다.
                $("#deleveryAddress1").val(data.zonecode);
                $("#deleveryAddress2").val(addr);

                // 추가
                console.log("data.zonecode = " + data.zonecode);
                console.log("addr = " + addr);

                $.ajax({
                  url: "/addressModify",
                  data: {address1 : data.zonecode , address2 : addr},
                  type: "post",
                  success: function(){
                    $(".address1").text(addr);
                    address1 = data.zonecode;
                  },
                  fail: function(){
                    alert("실패");
                  }
                })
                // 추가

                // 커서를 상세주소 필드로 이동한다.
                /* document.getElementById("deleveryAddress3").focus(); */

                $("#deleveryAddress3").focus();
                // iframe을 넣은 element를 안보이게 한다.
                // (autoClose:false 기능을 이용한다면, 아래 코드를 제거해야 화면에서 사라지지 않는다.)
                element_layer.style.display = 'none';

              },
              width : '100%',
              height : '100%',
              maxSuggestItems : 5
            }).embed(element_layer);

    // iframe을 넣은 element를 보이게 한다.
    element_layer.style.display = 'block';

    // iframe을 넣은 element의 위치를 화면의 가운데로 이동시킨다.
    initLayerPosition();
  }

  // 브라우저의 크기 변경에 따라 레이어를 가운데로 이동시키고자 하실때에는
  // resize이벤트나, orientationchange이벤트를 이용하여 값이 변경될때마다 아래 함수를 실행 시켜 주시거나,
  // 직접 element_layer의 top,left값을 수정해 주시면 됩니다.
  function initLayerPosition() {
    var width = 300; //우편번호서비스가 들어갈 element의 width
    var height = 400; //우편번호서비스가 들어갈 element의 height
    var borderWidth = 5; //샘플에서 사용하는 border의 두께

    // 위에서 선언한 값들을 실제 element에 넣는다.
    element_layer.style.width = width + 'px';
    element_layer.style.height = height + 'px';
    element_layer.style.border = borderWidth + 'px solid';
    // 실행되는 순간의 화면 너비와 높이 값을 가져와서 중앙에 뜰 수 있도록 위치를 계다.
    element_layer.style.left = (((window.innerWidth || document.documentElement.clientWidth) - width) / 2 - borderWidth)
            + 'px';
    element_layer.style.top = (((window.innerHeight || document.documentElement.clientHeight) - height) / 2 - borderWidth)
            + 'px';
  }
</script>

```

<div style="text-align:center; font-size:0.8em;">주소 api</div>

</div>
</details>

<br>
<br>

밑에 코드가 추가된 부분이다.


```html
// 추가
console.log("data.zonecode = " + data.zonecode);
console.log("addr = " + addr);
						
$.ajax({
    url: "/addressModify",
    data: {address1 : data.zonecode , address2 : addr},
    type: "post"
})
.done(function(){
    $(".address1").text(addr);
	address1 = data.zonecode;
})
.fail(function(){
    alert("실패");
})
// 추가

```

<div style="text-align:center; font-size:0.8em;">추가된 코드</div>

<br>

/addresModify로 주소를 보내고 주소를 세션과 쿠키에 저장하는 코드다.

<br>
<br>
<br>

MainController에 코드를 추가해준다.

<br>

```java
	@ResponseBody
	@PostMapping("/addressModify")
	public void addressModify(String address1, String address2, HttpServletResponse response, HttpSession session)
			throws UnsupportedEncodingException {
//		address1 = 우편번호
//		address2 = 주소
 
		System.out.println("address1 =" + address1);
		System.out.println("address2 =" + address2);
 
		String address = "{\"address1\" : \"" + address1 + "\",\"address2\" : \"" + address2 + "\"}"; 
		
		// 쿠키에 JSON으로 저장
		Cookie cookie = new Cookie("BMaddress", URLEncoder.encode(address, "UTF-8"));
 
		int age = 60 * 60 * 24 * 7; // 일주일
		cookie.setMaxAge(age);
 
		response.addCookie(cookie);
 
		// 세션에 map으로 저장
		Map<String, String> addMap = new HashMap<>();
		addMap.put("address1", address1);
		addMap.put("address2", address2);
		session.setMaxInactiveInterval(3600 * 3); // 3시간
		session.setAttribute("BMaddress", addMap);
	}


```

<div style="text-align:center; font-size:0.8em;">추가된 코드</div>

<br>
<br>


```java
	String address = "{\"address1\" : \"" + address1 + "\",\"address2\" : \"" + address2 + "\"}";

```

넘어온 주소를 JSON형태로 바꿔준다.


```java
Cookie cookie = new Cookie("BMaddress", URLEncoder.encode(address, "UTF-8"));
```

쿠키는 문자열만 저장할 수 있는데, 공백이나 특수 문자가 들어가면 오류가 나기때문에 URLEncoder로 변환한 후 저장한다.

쿠키 값을 꺼내줄 때는 URLDecoder로 변환해서 꺼내야한다.

<br>
<br>

![image]({{ site.baseurl }}/images/2023-06-12/20230612_174559.png)

<div style="text-align:center; font-size:0.8em;">메인 화면(주소창)</div>


<br>

![image]({{ site.baseurl }}/images/2023-06-12/20230612_174626.png)

<div style="text-align:center; font-size:0.8em;">메인 화면(주소창)2</div>