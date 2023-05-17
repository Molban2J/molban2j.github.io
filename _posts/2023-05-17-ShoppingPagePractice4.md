---
layout: single
title:  "쇼핑 페이지 예제 클론코딩4"
---

개요: 상품등록 창과 상품 관리 창 제작, 그리고 상품 등록창에서 위지윅 에디터 적용

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