---
layout: single
title:  "쇼핑 페이지 예제 클론코딩3"
---

## 작가 상세보기

### 1. 작가 상세 보기 Mapper, Service 만들기

```java
//작가 상세 보기
	public AuthorVO authorDetail(int authorId);
```

#### <p style="text-align: center">AuthorMapper.java 코드, xml과 service에도 추가해주도록 하자</p>



  ```xml
<select id="authorDetail" resultType="com.shop.model.AuthorVO">
		select * from jm_author where authorId = #{authorId}
	</select>
```

#### <p style="text-align: center">AuthorMapper.xml 코드</p>

![image]({{ site.baseurl }}/images/20230516_155532.png)

##### <p style="text-align:center;">test 성공<p>


 ```java
@GetMapping("/authorDetail")
    public void authorDetailGET(int authorId, Model model, Criteria cri) throws Exception {
    	logger.info("authorDetail페이지 접속");
    	logger.info("authorId: "+authorId);
    	
    	
    	AuthorVO author = authorService.authorDetail(authorId);
    	model.addAttribute("cri",cri);
    	model.addAttribute("authorInfo", author);
    }
```

##### <p style="text-align:center;">mapper 작성<p>

<br>

authorManage.jsp에서 authorDetail.jsp로 넘어갈수 있도록 \<a> 태그를 달아줍니다.

```html
<td>
	<a class="move" href='<c:out value="${list.authorName }"/>'>
			<c:out value="${list.authorName }" />
	</a>
</td>
```

그리고 눌렀을때 값을 갖고 넘어갈수 있도록 스크립트문도 작성해 줍니다.

```javascript
	/* 작가 상세 페이지 이동 */
		$(".move").on("click", function(e){
			
			e.preventDefault();
			
			moveForm.append("<input type='hidden' name='authorId' value='"+ $(this).attr("href") + "'>");
			moveForm.attr("action", "/admin/authorDetail");
			moveForm.submit();
			
		});
		
```

##### <p style="text-align:center;">script 작성<p>

> 여기 예제에서는 값을 넘기기 위해서 일일히 스크립트문을 작성해준다.<br> 하지만 오히려 나는 
```html
<td>
	<a class="move" href='/admin/authorDetail?authorId=<c:out value="${list.authorId}"/>'>
			<c:out value="${list.authorName }" />
	</a>
</td>
```
이게 더 간편하지 않나 생각이 든다. 전달해줘야하는 값이 여러개라면 스크립트문에 더 깔끔하겠지만 값이 적다면 그냥 저렇게 작성하는게 더 나을거같다.


![image]({{ site.baseurl }}/images/20230516_161211.png)

##### <p style="text-align:center;">\<a>태그 작성까지 완성<p>


<br>

<br>

### 2. authorDetail.jsp, authorDetail.css생성

![image]({{ site.baseurl }}/images/20230516_165338.png)

##### <p style="text-align:center;">\<a>jsp, css 생성<p>

그리고 작가 목록 버튼과 수정 버튼이 작동할 수 있도록 스크립트 작성


```javascript
<script>

let moveForm = $("#moveForm");

/* 작가 관리 페이지 이동 버튼 */
$("#cancelBtn").on("click", function(e){
	
	e.preventDefault();
	
	$("input[name=authorId]").remove();
	moveForm.attr("action", "/admin/authorManage")
	moveForm.submit();
	
});

/* 작가 수정 페이지 이동 버튼 */
$("#modifyBtn").on("click", function(e){
	
	e.preventDefault();
	
	moveForm.attr("action", "/admin/authorModify");
	moveForm.submit();
	
});
</script>
```

##### <p style="text-align:center;">script 작성<p>


![image]({{ site.baseurl }}/images/20230516_171404.png)

##### <p style="text-align:center;">작가 상세페이지 모습<p>





