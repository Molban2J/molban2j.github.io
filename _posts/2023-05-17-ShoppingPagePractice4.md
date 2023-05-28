---
layout: single
title:  "쇼핑 페이지 예제 클론코딩4(상품등록,위지윅,Datepicker)"
---

<br>

**깃허브 repository** : [shopping page practice](https://github.com/Molban2J/ShoppingPagePractice.git "github")

<br>

개요: 상품등록 창과 상품 관리 창 제작, 그리고 상품 등록창에서 위지윅, Datepicker 에디터 적용

<br>
<br>

## 상품 등록

### 1. 상품 테이블 생성

상품 등록을 위한 테이블을 생성해줍니다(Oracle)

```sql
create table jm_book(
    bookId number primary key,
    boonName varchar2(50) not null,
    authorId number,
    publeYear Date not null,
    publisher varchar2(70) not null,
    cateCode varchar2(30),
    bookPrice number not null,
    bookStock number not null,
    bookDiscount number(2,2),
    bookIntro clob,
    bookContent clob,
    regDate date default sysdate,
    updateDate date default sysdate
);

create sequence jm_book_seq start WITH 1;
```

<div style="text-align:center; font-size:0.8em;">book table과 sequence 생성</div>

> bookDiscount number(2,2)의 의미<br>
앞의 2는 자릿수가 최대 2인걸 의미하고, 뒤의 2는 소수점 밑 두자리 수까지 유효하다는 의미이다.

<br>
<br>

### 2. 카테고리 테이블 생성

```sql
create table jm_bcate(
    tier number(1) not null,
    cateName varchar2(30) not null,
    cateCode varchar2(30) not null  primary key,
    cateParent varchar2(30),
    foreign key(cateParent) references jm_bcate(cateCode)
);
```

<div style="text-align:center; font-size:0.8em;">category table생성</div>

이 예제에서는 카테고리 분류를 세단계로 분류한다(교보문고 분류법 참고했다고 함).
<br>
1단계는 국내/국외, 2단계는 철학, 인문, 사회, 과학, 교양 등 큰 분류, 3단계는 2단계의 세분화라고 합니다.

### 3. 데이터 삽입

카테고리 데이터를 삽입해줍니다.

![image]({{ site.baseurl }}/images/20230517_124012.png)


<div style="text-align:center; font-size:0.8em;">카테고리 삽입 완료</div>

---

<br>

### 4. VO 생성

테이블을 생성 했으니 이제 VO객체 생성해줍니다.

com.shop.model 패키지 밑에 BookVO.java 생성

![image]({{ site.baseurl }}/images/20230517_124609.png)


<div style="text-align:center; font-size:0.8em;">BookVO.java</div>

---

<br>
<br>


### 5. Mapper 생성
AdminMapper 생성해줍니다.
AdminMapper.java , AdminMapper.xml , AdminService.java , AdminServiceImpl.java 생성


![image]({{ site.baseurl }}/images/20230517_125126.png)

<div style="text-align:center; font-size:0.8em;">AdminMapper.java , AdminMapper.xml , AdminService.java , AdminServiceImpl.java 생성
</div>

그리고 bookEnroll mapper 작성해줍니다.

```xml
<insert id="bookEnroll">
		insert into jm_book(bookId, bookName, authorId, publeYear, publisher, cateCode,
		bookPrice, bookStock, bookDiscount, bookIntro, bookContent)
		values(jm_book_seq.nextval, #{bookName},#{authorId},
		#{publeYear},#{publisher},#{cateCode},#{bookPrice},#{bookStock},#{bookDiscount},#{bookIntro},#{bookContent})
	</insert>
```

<div style="text-align:center; font-size:0.8em;">bookEnroll sql문 작성</div>

그리고 test 실행

![image]({{ site.baseurl }}/images/20230517_143505.png)

<div style="text-align:center; font-size:0.8em;">test실행
</div>

<br>

이제 service에 등록하고 controller에도 등록합니다.

처음에 먼저 goodsEnroll을 Get방식으로 매핑해놨으니, Post매핑을 등록해줍니다.

```java
	@PostMapping("/goodsEnroll")
	public String goodsEnrollPOST(BookVO book, RedirectAttributes rttr) throws Exception {
		logger.info("goodsEnroll 실행");
		adminService.bookEnroll(book);
		rttr.addFlashAttribute("enroll_result", book.getBookName());
		return "redirect:/admin/goodsManage";
	}
```

<div style="text-align:center; font-size:0.8em;">controller 작성</div>

---

<br>
<br>

### 6. View 작성
등록 매핑을 모두 등록했으니 view를 상세히 제작

### goodsEnroll.jsp

![image]({{ site.baseurl }}/images/20230517_145333.png)

<div style="text-align:center; font-size:0.8em;">goodsEnroll.jsp
</div>

버튼이 작동하도록 스크립트 작성

```javascript
	<script>
let enrollForm = $("#enrollForm")

//취소 버튼
$("#cancelBtn").click(function(){
	location.href="/admin/goodsManage"
});

//상품 등록 버튼
$("#enrollBtn").on("click",function(e){
	e.preventDefault();
	erollForm.submit();
});
</script>
```

<div style="text-align:center; font-size:0.8em;">script 작성</div>

<br>

> .click과 on("click")의 차이점은 뭘까 의문이 들었다.<br>
정적/ 동적 차이라고 한다. .click을 사용하면 정적으로 만들어진 구성요소에 대해서는 이벤트가 작동하지만 동적으로 생성된 구성요소에는 이벤트가 작동하지 않는다고 한다. 반면 .on("click")은 둘다 작동한다고한다. 이를 "**동적바인딩**"이라고 한다는데 아직 자바스크립트를 자세히 배우지 않아서 정확하게는 잘 모르겠다.<br><br>
그리고 .on("click", function(e){}); 에서 e는 이벤트를 의미하고 그 이벤트가 "click"을 의미한다.<br>
e.preventDefault는 이벤트가 발생했을 때 혹여 입력하지 않았지만 자동으로 default값으로 설정하지 않는 메서드 이다. 


### goodsManage.jsp

먼저 상품 등록이 잘 작동되는지 확인하기위해 goodsManage.jsp에 등록결과 alert창을 띄우는 스크립트를 작성한다.

```javascript
<script>
	$(document).ready(function(){
		let eResult = '<c:out value="${enroll_result}"/>';
		checkResult(eResult);
		
		function checkResult(result){
			if(result===''){
				return;
			};
			alert("상품'"+eResult+"'을(를) 등록하였습니다.")
		}
	});
	
	
	</script>

```

<div style="text-align:center; font-size:0.8em;">script 작성</div>

![image]({{ site.baseurl }}/images/20230517_161054.png)

![image]({{ site.baseurl }}/images/20230517_161230.png)

<div style="text-align:center; font-size:0.8em;">잘 작동
</div>

---

<br>
<br>

## 위지윅 에디터 적용

예제에서는 자바스크립트 기반 위지윅 에디터를 사용한다고 한다.

무료 자바스크립트 기반 위지윅 종류
- CK Editior
- TinyMCE
- Toast Editor
- Summernote 등

예제에서는 **CK Editior 5** 사용

---

<br>

### 1. 사용 준비

[CK Editior 다운로드](https://ckeditor.com/) 

![image]({{ site.baseurl }}/images/20230517_162032.png)

<div style="text-align:center; font-size:0.8em;">CK Editior 다운로드
</div>

여기서 CDN 방식을 사용하겠다. CDN를 copy, goodsEnroll.jsp에 head 태그 안에 삽입해줍니다.

![image]({{ site.baseurl }}/images/20230517_162318.png)

<div style="text-align:center; font-size:0.8em;">링크 삽입
</div>

<br>
<br>

bookIntro와 bookContent부분의 input태그를 textarea로 바꿔준다.

```html
<div class="form_section">
	<div class="form_section_title">
		<label>책 소개</label>
	</div>
	<div class="form_section_content">
		<textarea name="bookIntro" id="bookIntro_textarea"></textarea>
	</div>
</div>
<div class="form_section">
	<div class="form_section_title">
		<label>책 목차</label>
	</div>
  <div class="form_section_content">
		<textarea name="bookContents" id="bookContents_textarea"></textarea>
	</div>
</div>
```

<div style="text-align:center; font-size:0.8em;">textarea로 변경</div>

<br>
<br>

CK Editor를 적용하는 js는 다음과 같다.

```javascript
ClassicEditor
		.create(document.querySelector('적용대상 선택자'))
		.catch(error=>{
			console.error(error);
		});
```

<div style="text-align:center; font-size:0.8em;">CK Editor js</div>

위 코드를 스크립트에 삽입합니다. '적용대상 선택자'에는 적용할 textarea의 id값을 주면 됨

```javascript
/* 책 소개 */
		ClassicEditor
			.create(document.querySelector('#bookIntro_textarea'))
			.catch(error=>{
				console.error(error);
			});
			
		/* 책 목차 */	
		ClassicEditor
		.create(document.querySelector('#bookContents_textarea'))
		.catch(error=>{
			console.error(error);
		});
```

<div style="text-align:center; font-size:0.8em;">script</div>

![image]({{ site.baseurl }}/images/20230517_163621.png)

<div style="text-align:center; font-size:0.8em;">적용된 모습
</div>

칸의 높이가 너무 작으니까 css에서 높이를 수정해 주면 된다.

```css
.ck-content {						/* ckeditor 높이 */
    height: 170px;
}

```

<div style="text-align:center; font-size:0.8em;">css</div>


> .ck-content라는 클래스를 나는 설정해 준적이 없는데 저 클래스로 높이 조절이 가능하다. 아마 CK Editor 자체 클래스인것 같다. CK Editor Document에 들어가보면 높이 외에도 다양하게 커스텀 할 수 있도록 설명해 놓았다. <br><br>
[CK Editor Document](https://ckeditor.com/docs/ckeditor5/latest/features/toolbar/toolbar.html)


![image]({{ site.baseurl }}/images/20230517_170322.png)

![image]({{ site.baseurl }}/images/20230517_170500.png)

![image]({{ site.baseurl }}/images/20230517_170537.png)

<div style="text-align:center; font-size:0.8em;">잘 작동하는지 확인
</div>

> 다만 일반 텍스트 형태로 들어가는 것이 아니라 html형식으로 입력되는것 같다.


---

<br>
<br>

## Datepicker 위젯 적용

지금까지 작성한 입력 폼에서 출판 년월일을 입력하려면 "yyyy/MM//dd" 형식의 String 값으로 입력해야 했다. 이것이 굉장히 불편했는데 Datepicker 위젯을 이용해 이를 개선한다.

[Datepicker](https://jqueryui.com/download/)

위 링크는 직접 zip파일로 다운받을수 있는 사이트

하지만 여기서는 똑같이 CDN으로 적용하겠다.
아래 링크 삽입

```javascript
<link rel="stylesheet" href="//code.jquery.com/ui/1.8.18/themes/base/jquery-ui.css" />
<script src="//ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.min.js"></script>
<script src="//code.jquery.com/ui/1.8.18/jquery-ui.min.js"></script>

```

<div style="text-align:center; font-size:0.8em;">CDN</div>

이제 출판일 input 태그에 코드를 추가해준다.

```html
<div class="form_section">
		<div class="form_section_title">
				<label>출판일</label>
		</div>
		<div class="form_section_content">
				<input name="publeYear" autocomplete="off" readonly="readonly">
		</div>
</div>

```

<div style="text-align:center; font-size:0.8em;">출판일</div>

<br>

autocomplete는 이전의 입력값이 자동으로 완성되는것을 막기위해 "off"로 해주고 readonly는 사용자가 인위적으로 입력값을 바꾸지 못하도록 하기위해 설정해줬다.

<br>

Datepicker의 js코드

```javascript
$(function() {
  $( "선택자" ).datepicker();
});

```

<div style="text-align:center; font-size:0.8em;">js code</div>

```javascript
$(function() {
  $( "input[name='publeYear']" ).datepicker();
});

```

<div style="text-align:center; font-size:0.8em;">script</div>

<br>
<br>

![image]({{ site.baseurl }}/images/20230517_173456.png)

<div style="text-align:center; font-size:0.8em;">datepicker 적용모습
</div>

![image]({{ site.baseurl }}/images/20230517_173625.png)

<div style="text-align:center; font-size:0.8em;">날짜를 클릭했을때 입력된 모습
</div>

지금은 "dd/MM/yyyy"형식이지만 "yy/MM/dd"형식으로 바꿔 보겠다.

```javascript
const config = {
				dateFormat: 'yy-mm-dd'	
			}

```

<div style="text-align:center; font-size:0.8em;">script</div>

<br>
하지만 정상적으로 작동하지 않는다. 여전히 "dd/MM/yyyy"형식으로 나와서 위 코드를 지우고 기존의 코드를

```javascript
$(function() {
			  $( "input[name='publeYear']" ).datepicker({ format: 'YYYY-MM-DD' });
			});

```

이렇게도 바꿔 보았지만 바뀌지 않는다.

```javascript
$(function() {
			  $( "input[name='publeYear']" ).datepicker({ dateFormat: 'YYYY-MM-DD' });
			});

```

이렇게 바꾸니 바뀐다!

> **문제점 발견** (5/18 추가)
<br>
<br>
처음에는 예제의 스크립트 버전이 오래되서 작동이 안되는줄로 알았지만 아니었다...

```javascript
const config = {
				dateFormat: 'yy-mm-dd'	
}

$(function() {
			$( "input[name='publeYear']" ).datepicker(config);
});

```

> 이런식으로 config 변수를 넣어줬어야했는데 그러지 않아서 변경사항이 적용되지 않았던 것이다...

<br>

하지만 

![image]({{ site.baseurl }}/images/20230517_180816.png)

<div style="text-align:center; font-size:0.8em;">이런식으로 입력이 된다.
</div>

<br>

```javascript
$(function() {
			  $( "input[name='publeYear']" ).datepicker({ dateFormat: 'yyyy-MM-dd' });
			});

```

이렇게 바꿔 보겠다.


![image]({{ site.baseurl }}/images/20230517_181225.png)

<div style="text-align:center; font-size:0.8em;">결과...
</div>


```javascript
$(function() {
			  $( "input[name='publeYear']" ).datepicker({ dateFormat: 'yy-mm-dd' });
			});

```

다시한번 포멧 수정

![image]({{ site.baseurl }}/images/20230517_181735.png)

<div style="text-align:center; font-size:0.8em;">드디어 제데로 됐다.
</div>

오늘은 여기까지(5/17)

---

5/18

지금은 input칸을 클릭하면 달력이 나타나는 형태이지만, 날짜선택 버튼을 만들어 실행해보겠습니다.

```javascript
	$(function() {
			  $( "input[name='publeYear']" ).datepicker({ 
				  dateFormat: 'yy-mm-dd',
				showOn : "button",
				buttonText:"날짜 선택"
			  });
			});

```

<div style="text-align:center; font-size:0.8em;">코드 추가
</div>

<br>
<br>

![image]({{ site.baseurl }}/images/20230518_103623.png)

![image]({{ site.baseurl }}/images/20230518_103634.png)

<div style="text-align:center; font-size:0.8em;">버튼 클릭시
</div>

<br>

그리고 달력이 현재 영어로 표시되어있는데, 이것을 한글로도 바꿀 수 있다고 한다. 한글로 바꿔 보겠다.

```javascript
$(function() {
			  $( "input[name='publeYear']" ).datepicker({ 
				  dateFormat: 'yy-mm-dd',
				showOn : "button",
				buttonText:"날짜 선택",
				 prevText: '이전 달',
				    nextText: '다음 달',
				    monthNames: ['1월','2월','3월','4월','5월','6월','7월','8월','9월','10월','11월','12월'],
				    monthNamesShort: ['1월','2월','3월','4월','5월','6월','7월','8월','9월','10월','11월','12월'],
				    dayNames: ['일','월','화','수','목','금','토'],
				    dayNamesShort: ['일','월','화','수','목','금','토'],
				    dayNamesMin: ['일','월','화','수','목','금','토'],
				    yearSuffix: '년'
			  });
			});
```

<div style="text-align:center; font-size:0.8em;">코드 추가
</div>

<br>

![image]({{ site.baseurl }}/images/20230518_104411.png)

<div style="text-align:center; font-size:0.8em;">한글로 변경됐다.
</div>

<br>

현재 달력은 월 단위로 화살표를 이용해서 움직여야하는데, 연/월을 \<select>를 이용해서 선택할 수 있게 하겠다.

```javascript
$(function() {
			  $( "input[name='publeYear']" ).datepicker({ 
				  dateFormat: 'yy-mm-dd',
				showOn : "button",
				buttonText:"날짜 선택",
				 prevText: '이전 달',
				    nextText: '다음 달',
				    monthNames: ['1월','2월','3월','4월','5월','6월','7월','8월','9월','10월','11월','12월'],
				    monthNamesShort: ['1월','2월','3월','4월','5월','6월','7월','8월','9월','10월','11월','12월'],
				    dayNames: ['일','월','화','수','목','금','토'],
				    dayNamesShort: ['일','월','화','수','목','금','토'],
				    dayNamesMin: ['일','월','화','수','목','금','토'],
				    yearSuffix: '년',
				    changeMonth: true,
				    changeYear: true
			  });
			});
```

<div style="text-align:center; font-size:0.8em;">코드 추가
</div>

![image]({{ site.baseurl }}/images/20230518_104752.png)

<div style="text-align:center; font-size:0.8em;">선택창으로 변경</div>


<br>
<br>

마지막으로 버튼과 입력창의 css를 적용해줍니다.

```css
.ui-datepicker-trigger {						/* 캘린더 css 설정 */
    margin-left: 25px;
    width: 14%;
    height: 38px;
    font-weight: 600;
    background-color: #dfe8f5;
    font-size: 15px;
    cursor:pointer;
}
input[name='publeYear'] {
    width: 80%;
    text-align: center;
}
```

<div style="text-align:center; font-size:0.8em;">css코드 추가
</div>

<br>
Datepicker도 위즈윅처럼 기본 내장된 클래스를 변경할 수 있는 것 같다.

<br>

![image]({{ site.baseurl }}/images/20230518_105120.png)

<div style="text-align:center; font-size:0.8em;">최종 모습</div>

<br>
<br>

### 테스트

테스트로 등록해보려 했으나 이게 웬걸 오류가 떠버렸다.

```
WARN : org.springframework.web.servlet.mvc.support.DefaultHandlerExceptionResolver - Resolved [org.springframework.validation.BindException: org.springframework.validation.BeanPropertyBindingResult: 1 errors
Field error in object 'bookVO' on field 'publeYear': rejected value [2023-05-25]; codes [typeMismatch.bookVO.publeYear,typeMismatch.publeYear,typeMismatch.java.util.Date,typeMismatch]; arguments [org.springframework.context.support.DefaultMessageSourceResolvable: codes [bookVO.publeYear,publeYear]; arguments []; default message [publeYear]]; default message [Failed to convert property value of type 'java.lang.String' to required type 'java.util.Date' for property 'publeYear'; nested exception is org.springframework.core.convert.ConversionFailedException: Failed to convert from type [java.lang.String] to type [java.util.Date] for value '2023-05-25'; nested exception is java.lang.IllegalArgumentException]]
```

<div style="text-align:center; font-size:0.8em;">오류메세지
</div>

<br>

아무래도 입력 데이터 형식이 맞지않아 발생한 문제 같다. 형을 임의로 "yy-mm-dd"형식으로 바꿨는데 이 형식이 삽입하는데 오류가 발생하는 것으로 보인다.

한번 "yy/mm/dd"로 바꿔서 해보겠다. 이전 String 값으로 넘길 때 저 형식으로 했을 때 잘 들어갔기 때문이다.

![image]({{ site.baseurl }}/images/20230518_105646.png)

<div style="text-align:center; font-size:0.8em;">입력폼이 바뀜</div>

![image]({{ site.baseurl }}/images/20230518_105702.png)

<div style="text-align:center; font-size:0.8em;">성공적으로 작동 완료. 역시 내 예상이 맞았다!</div>


![image]({{ site.baseurl }}/images/20230518_105832.png)

<div style="text-align:center; font-size:0.8em;">데이터 베이스에도 잘 들어간 모습</div>

<br>
<br>

---


<br>
<br>

## 내 생각

이 예제는 달력을 Datepicker를 사용해 입력했는데, 현 시점 input type="datetime-local"이나, type="date"을 사용해도 똑같이 달력으로 선택할 수 있는데  굳이 저 위젯을 사용해야하나 싶기도 하다. 
~~버전이 낮은 웹에서는 작동이 안되서 그러려나...~~
