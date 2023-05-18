---
layout: single
title:  "쇼핑 페이지 예제 클론코딩5(상품등록-작가선택, 카테고리 리스트)"
---

## 상품등록 - 작가선택 팝업 창 구현

### 1. 작가 선택 버튼 추가

기존 작가 input태그를 수정해 줍니다.

```html
<div class="form_section_content">
		<input id="authorName_input" readonly="readonly">
		<input id="authorId_input" name="authorId" type="hidden">
		<button class="authorId btn">작가 선택</button>
</div>
```

<div style="text-align:center; font-size:0.8em;">input과 button 추가</div>

<br>

css도 적용해줍니다.

<br>
<br>

![image]({{ site.baseurl }}/images/20230518_111929.png)

<div style="text-align:center; font-size:0.8em;">적용 완료</div>

<br>
<br>

버튼 작동 스크립트 작성, 클릭했을때 새로운 창이 열리도록 open.window 매서드 사용

 ```javascript
$('.authorId_btn').on("click",function(e){
			
			e.preventDefault();
			
			let popUrl = "/admin/authorPop";
			let popOption = "width = 650px, height=550px, top=300px, left=300px, scrollbars=yes";
			
			window.open(popUrl,"작가 찾기",popOption);
			
		});
```

<div style="text-align:center; font-size:0.8em;">script 추가</div>

<br>

### 2. mapper와 view 생성

저 url에 대한 controller mapper 작성, view authorPop.jsp도 생성. authorPop.jsp와 mapper는 기존 authorManage.jsp와 mapper의 코드를 수정해서 사용한다.


```java
@GetMapping("/authorPop")
	public void authorPopGET(Criteria cri, Model model) throws Exception{
		logger.info("authorPop 접속");
    	cri.setAmount(5);
		
		List<AuthorVO> list = authorService.authorList(cri);
		if (list.isEmpty()) { // 비어있으면
			model.addAttribute("listCheck", "empty");
		} else { // 검색 결과가 존재한다면
			model.addAttribute("list", list);
    }

		/* 페이지 이동 인터페이스 데이터 */
		int total = authorService.authorTotal(cri);

		PageVO pageMaker = new PageVO(cri, total);

		model.addAttribute("pageMaker", pageMaker);
	}
```

<div style="text-align:center; font-size:0.8em;">authorPop mapping. authorManage 매핑과 동일하지만 amount 값만 바꿔줬다.</div>

<br>
<br>
authorPop.jsp도 authorManage.jsp와 거의 동일하다. header와 footer는 제외해준다.

<br>

![image]({{ site.baseurl }}/images/20230518_121102.png)

<div style="text-align:center; font-size:0.8em;">팝업 창 모습</div>

<br>
<br>

작가 페이지는 아직 jquery스크립트를 추가하지 않았기때문에 작동하지 않는다. 스크립트를 추가해 준다.

<br>

---

<br>
<br>

*JQuery에 대해서는 자세히 모르겠으니 나중에 다시 한번 공부해서 집고 넘어가도록 하자*

> 예제를 공부하면서 Jquery를 쓴다고 그냥 생각 없이 따라 썼지만 그래도 정확히 Jquery가 뭔지 궁금해 선생님께 질문하고, 인터넷으로 찾아봤다.<br>
**JQuery**는 2009년 즈음 많은 브라우저들이 생겨나면서 생겼다고 한다. 당시에는 각 브라우저 별로 지원하는 태그들이 달랐는데, 크로스 브라우징을 위해 JQuery가 생겼다는 것이다. 하지만 현재 웹 표준이 html로 통합되면서 다시 javascript로 돌아간다고 하지만 기존 Jquery자료들이 많아 아직도 많이 쓰인다고한다.


<br>
<br>

---

<br>
<br>

```javascript
let searchForm = $('#searchForm');
		let moveForm = $('#moveForm');
		
		/* 작거 검색 버튼 동작 */
		$("#searchForm button").on("click", function(e){
			
			e.preventDefault();
			
			/* 검색 키워드 유효성 검사 */
			if(!searchForm.find("input[name='keyword']").val()){
				alert("키워드를 입력하십시오");
				return false;
			}
			
			searchForm.find("input[name='pageNum']").val("1");
			
			searchForm.submit();
			
		});
		
		
		/* 페이지 이동 버튼 */
		$(".pageMaker_btn a").on("click", function(e){
			
			e.preventDefault();
			
			console.log($(this).attr("href"));
			
			moveForm.find("input[name='pageNum']").val($(this).attr("href"));
			
			moveForm.submit();
			
		});	

```

<div style="text-align:center; font-size:0.8em;">script 작성. 페이지 이동버튼과 검색 기능 구현</div>

<br>
<br>

![image]({{ site.baseurl }}/images/20230518_121849.png)

<div style="text-align:center; font-size:0.8em;">팝업 창 페이징 작동 모습</div>

<br>

이제 팝업창에서 선택한 값을 부모페이지에 보내주도록 하자.
팝업창 작가 이름에 a태그를 달아주고, 작가 아이디값을 넘겨주도록 한다.


```html
<td>
		<a class="move" href='<c:out value="${list.authorId}"/>' data-name='<c:out value="${list.authorName}"/>'>
									<c:out value="${list.authorName}"/></a>

</td>

```

<div style="text-align:center; font-size:0.8em;">a태그</div>

```javascript
/* 작가 선택 및 팝업창 닫기 */
		$(".move").on("click", function(e){
			
			e.preventDefault();
			
			let authorId = $(this).attr("href");
			let authorName= $(this).data("name");
			$(opener.document).find("#authorId_input").val(authorId);
			$(opener.document).find("#authorName_input").val(authorName);
			
			window.close();

		});	

```

<div style="text-align:center; font-size:0.8em;">script</div>

<br>

> 먼저 a태그를 좀 살펴본다. 작가 이름란이라 작가 이름이 나와있지만 그래도 정확한 식별을 위해서는 작가 아이디 값으로 넘겨주는게 좋다. 그렇기 때문에 링크를 작가 아이디로 걸어준다.<br>
그리고 data-name은 사용자가 임의로 지정해준 data를 javascript에서 불러올 수 있게하는 속성이다.~~(자세히는 모르겠다..)~~
<br><br>
해서 밑에 스크립트를 보면 authorId 와 authorName 변수를 생성해주고 이 값들을 부모 창( **opener.document** )에서 알맞은 input태그를 찾아(**.find**) 삽입한다.

<br>
<br>
<br>

![image]({{ site.baseurl }}/images/20230518_124438.png)

<div style="text-align:center; font-size:0.8em;">잘 입력된다.</div>

<br>
<br>
<br>
<br>

---

## 카테고리 리스트

<br>

카테고리를 그냥 단순히 \<select>와 \<option>으로 제작할 수도 있지만, 이 예제는 카테고리 양도 많고 대분류나 중분류에 따른 소분류들이 매번 달라져야하기 때문에 데이터를 view로 전송해 이것을 추가로 가공하여 사용해야 한다고 합니다.

이러한 이슈로 데이터를 JSON타입으로 변환하여 처리한다.

---

### 1. 카테고리 VO 생성

```java
@Data
public class CateVo {
	private int tier;
	private String cateName, cateCode, cateParent;
}

```

<div style="text-align:center; font-size:0.8em;">CateVO</div>

<br>

### 2. Mapper 생성

전체 카테고리 객체 불러오는 매퍼

```java
//카테고리 목록
	public List<CateVO> cateList();
```

<div style="text-align:center; font-size:0.8em;">AdminMapper.java</div>

```xml
<select id="cateList" resultType="com.shop.model.CateVO">
		select * from jm_bcate order by catecode
	</select>

```

<div style="text-align:center; font-size:0.8em;">AdminMapper.xml</div>

<br>


![image]({{ site.baseurl }}/images/20230518_130443.png)

<div style="text-align:center; font-size:0.8em;">JUnit Test</div>

<br>
Service 등록

### 3. Controller 작성

기존 "/goodsEnroll"에 코드를 추가해줍니다.

```java
List<CateVO> list = adminService.cateList();

```

<div style="text-align:center; font-size:0.8em;">AdminController.java 에 @GetMapping("/goodsEnroll")</div>

<br>

**Json 라이브러리 추가**

![image]({{ site.baseurl }}/images/20230518_131145.png)

<div style="text-align:center; font-size:0.8em;">Json 라이브러리 추가</div>

<br>
pom.xml에 라이브러리 추가후 Maven Update 실행.

```java
@GetMapping("/goodsEnroll")
	public void goodsEnrollGET(Model model) throws Exception {
		logger.info("상품 등록 페이지 접속");
		
		ObjectMapper objm = new ObjectMapper();	//
		
		List<CateVO> list = adminService.cateList();
		
		String cateList = objm.writeValueAsString(list);
		model.addAttribute("cateList",cateList);
    logger.info("변경전: "+list);
		logger.info("변경 후: "+cateList);
	}

```

<div style="text-align:center; font-size:0.8em;">코드 수정, 추가</div>

<br>

아까 추가한 JSON라이브러리의 클래스는 static이 아니라 ObjectMapper 인스턴스를 생성해서 사용해야한다. 그리고 writeValueAsString은 java 객체를 String 타입의 JSON 데이터로 변환해주는 매서드이다.
<br>
그리고 logger.info를 통해 어떻게 자료가 변경되는지 확인한다.

![image]({{ site.baseurl }}/images/20230518_144628.png)

<div style="text-align:center; font-size:0.8em;">콘솔 로그 확인</div>

<br>
<br>

---

<br>

### 3. View에 \<select>/\<option> 추가

<br>

기존 카테고리 input 자리에 삽입해줍니다.

![image]({{ site.baseurl }}/images/20230518_145453.png)

<div style="text-align:center; font-size:0.8em;">goodsEnroll.jsp</div>

<br>

css도 적용해줍니다.

![image]({{ site.baseurl }}/images/20230518_145653.png)

<div style="text-align:center; font-size:0.8em;">적용모습</div>

<br>
<br>

### 4. 스크립트 작성

그리고 전달 받은 JSON 데이터를 쓰기 위해 스크립트 작성.

```java
/* 카테고리 */
		let cateList = JSON.parse('${cateList}');
```

<div style="text-align:center; font-size:0.8em;">goodsEroll.jsp script</div>

<br>

JSON형태로 전달 받은 데이터를 javascript에서 바로 사용이 안된다고 한다. 그래서 위 코드는 JSON 데이터를 javascript에서 사용할 수 있도록 변환해 주는 매서드이다.

cateList 데이터는 분류가 안된채로 넘어오는데 이것을 분류해서 변수로 초기화 해줄것이다.

```javascript
let cate1Array = new Array();
		let cate2Array = new Array();
		let cate3Array = new Array();
		let cate1Obj = new Object();
		let cate2Obj = new Object();
		let cate3Obj = new Object();
		
		let cateSelect1 = $(".cate1");		
		let cateSelect2 = $(".cate2");
		let cateSelect3 = $(".cate3");
```

<div style="text-align:center; font-size:0.8em;">변수 선언</div>

<br>

cate1Array, cate2Array, cate3Array는 tier에 따라 분류 된 배열이다. 데이터 tier는 1,2,3이 있는데 이에 따라 분류한다.

cate1Obj, cate2Obj, cate3Obj는 cateCode, cateName, cateParent를 담기 위한 객체이다. 

cateSelect1, cateSelect2, cateSelect3은 \<select>의 \<option>에 값을 집어넣기 위한 변수이다.

<br>
<br>

```javascript
for(let i = 0; i < cateList.length; i++){
				if(cateList[i].tier === tier){
					obj = new Object();
					
					obj.cateName = cateList[i].cateName;
					obj.cateCode = cateList[i].cateCode;
					obj.cateParent = cateList[i].cateParent;
					
					array.push(obj);				
					
				}
			}
```

<div style="text-align:center; font-size:0.8em;">변수 값 초기화 반복문</div>

<br>
tier에 따른 배열에 값을 집어넣는 반복문. tier에 따라 반복해 줘야하므로 매서드로 만들겠다.

```javascript
function makeCateArray(obj,array,cateList, tier){
			for(let i = 0; i < cateList.length; i++){
				if(cateList[i].tier === tier){
					obj = new Object();
					
					obj.cateName = cateList[i].cateName;
					obj.cateCode = cateList[i].cateCode;
					obj.cateParent = cateList[i].cateParent;
					
					array.push(obj);				
					
				}
			}
		}
```

<div style="text-align:center; font-size:0.8em;">변수 값 초기화 매서드</div>

```javascript
makeCateArray(cate1Obj,cate1Array,cateList,1);
makeCateArray(cate2Obj,cate2Array,cateList,2);
makeCateArray(cate3Obj,cate3Array,cateList,3);
```

<div style="text-align:center; font-size:0.8em;">배열 초기화</div>

<br>

배열이 초기화 됐으니 \<option>값을 넣어주자

```javascript
for(let i = 0; i < cate1Array.length; i++){
			cateSelect1.append("<option value='"+cate1Array[i].cateCode+"'>" + cate1Array[i].cateName + "</option>");
		}
```

<div style="text-align:center; font-size:0.8em;">대분류</div>

```javascript
$(cateSelect1).on("change",function(){
			
			let selectVal1 = $(this).find("option:selected").val();	
			
			cateSelect2.children().remove();
			
			cateSelect2.append("<option value='none'>선택</option>");
			
			for(let i = 0; i < cate2Array.length; i++){
				if(selectVal1 === cate2Array[i].cateParent){
					cateSelect2.append("<option value='"+cate2Array[i].cateCode+"'>" + cate2Array[i].cateName + "</option>");	
				}
			}
			
		});
```

<div style="text-align:center; font-size:0.8em;">대분류</div>

> 찬찬히 보면<br>
1.  처음 매서드가 실행 되는 조건: 대분류가 변경되면("**change**") 매서드를 실행한다.
2. 선택된 옵션 값을("**option:selected**") 찾아서(**find**) selectVal1에 초기화한다.
3. 중분류(cateSelect2)의 옵션(자식=**children**)을 지우고(**remove**), 
4. 중분류(cateSelect2)에 옵션(**\<option value='none'>선택\</option>**)을 삽입한다.
5. selectVal1(**선택된 대분류**) 값과 cate2Array.cateParent(**기존 배열의 대분류**) 값이 같은 것들 중에서 cate2Array.cateCode(**중분류**)값을 \<option>에 추가한다.
6. 이것을 i(배열의 길이)만큼 반복한다(**for**)

<br>
<br>

소분류도 같은 원리로 작성해준다.


![image]({{ site.baseurl }}/images/20230518_160411.png)

![image]({{ site.baseurl }}/images/20230518_160426.png)

<div style="text-align:center; font-size:0.8em;">상위 옵션값에따라 하위 옵션도 바뀌는 모습</div>

<br>
<br>

![image]({{ site.baseurl }}/images/20230518_160657.png)

![image]({{ site.baseurl }}/images/20230518_160708.png)

<div style="text-align:center; font-size:0.8em;">다만, 대분류가 바뀌면 중분류는 이전 값이 지워지지만 소분류는 그대로다.</div>

<br>

대분류가 바뀌면 중분류만 초기화되게 해놨기 때문인데 소분류도 초기화 하도록 해준다.

```javascript
$(cateSelect1).on("change",function(){
			
			let selectVal1 = $(this).find("option:selected").val();	
			
			cateSelect2.children().remove();
			cateSelect3.childern().remove();
			
			cateSelect2.append("<option value='none'>선택</option>");
			cateSelect3.append("<option value='none'>선택</option>");
			
			for(let i = 0; i < cate2Array.length; i++){
				if(selectVal1 === cate2Array[i].cateParent){
					cateSelect2.append("<option value='"+cate2Array[i].cateCode+"'>" + cate2Array[i].cateName + "</option>");	
				}
			}
			
		});
```

<div style="text-align:center; font-size:0.8em;">코드 수정</div>

![image]({{ site.baseurl }}/images/20230518_161137.png)

![image]({{ site.baseurl }}/images/20230518_161215.png)

<div style="text-align:center; font-size:0.8em;">test 데이터가 잘 삽입됨.</div>

<br>
<br>
<br>

---

<br>

## 상품등록 유효성검사

<br>

마지막으로 상품을 등록할 때 입력 안한 곳은 없는지, 이상한 값을 입력했는지 확인하는 유효성 검사이다.

### 1. View수정

각 \<input>태그 밑에 \<span> 태그를 작성해 준다.

![image]({{ site.baseurl }}/images/20230518_162119.png)

<div style="text-align:center; font-size:0.8em;">span태그 작성.</div>

css도 작성해준다.

### 2. Script작성

각각의 입력값 확인을 위한 변수 선언

```javascript
let bookNameCk = false; //책이름확인
let authorIdCk = false; //작가 확인
let publeYearCk = false;  //출판일 확인
let publisherCk = false;  //출판사 확인
let cateCodeCk = false; //카테고리 확인
let priceCk = false;  //가격 확인
let stockCk = false;  //재고확인
let discountCk = false; //할인률 확인
let introCk = false;  //책 소개 확인
let contentsCk = false; //목차 확인
```

<div style="text-align:center; font-size:0.8em;">변수 선언</div>

각각 입력값에 해당하는 변수 초기화

```javascript
let bookName = $("input[name='bookName']").val();
let authorId = $("input[name='authorId']").val();
let publeYear = $("input[name='publeYear']").val();
let publisher = $("input[name='publisher']").val();
let cateCode = $("select[name='cateCode']").val();
let bookPrice = $("input[name='bookPrice']").val();
let bookStock = $("input[name='bookStock']").val();
let bookDiscount = $("input[name='bookDiscount']").val();
let bookIntro = $(".bit p").html();
let bookContent = $(".bct p").html();
```

<div style="text-align:center; font-size:0.8em;">변수 선언</div>

> 위 변수에서 bookIntro나 bookContent는 .val()이 아니라 .html()인데 이는 위즈윅 위젯이 html형식으로 입력되기 때문이다. 그리고 평문은 html에서 p태그로 작성되기 때문에
해당 클래스에 p태그로 작성되어 있는 값으로 초기화한다.
<br>
*하지만 그럴일은 별로 없겠지만 h1이나 p태그가 아닌 다른 태그로 작성되는 경우의 수도 고려해서 작성하는게 좋지 않을까 하는 생각이 든다.*

```javascript
if(bookName){
			$(".bookName_warn").css('display','none');
			bookNameCk = true;
		} else {
			$(".bookName_warn").css('display','block');
			bookNameCk = false;
		}
		
		if(authorId){
			$(".authorId_warn").css('display','none');
			authorIdCk = true;
		} else {
			$(".authorId_warn").css('display','block');
			authorIdCk = false;
		}
		
		if(publeYear){
			$(".publeYear_warn").css('display','none');
			publeYearCk = true;
		} else {
			$(".publeYear_warn").css('display','block');
			publeYearCk = false;
		}	
		
		if(publisher){
			$(".publisher_warn").css('display','none');
			publisherCk = true;
		} else {
			$(".publisher_warn").css('display','block');
			publisherCk = false;
		}
		
		if(cateCode != 'none'){
			$(".cateCode_warn").css('display','none');
			cateCodeCk = true;
		} else {
			$(".cateCode_warn").css('display','block');
			cateCodeCk = false;
		}	
		
		if(bookPrice != 0){
			$(".bookPrice_warn").css('display','none');
			priceCk = true;
		} else {
			$(".bookPrice_warn").css('display','block');
			priceCk = false;
		}	
		
		if(bookStock != 0){
			$(".bookStock_warn").css('display','none');
			stockCk = true;
		} else {
			$(".bookStock_warn").css('display','block');
			stockCk = false;
		}		
		
		if(bookDiscount < 1 && bookDiscount != ''){
			$(".bookDiscount_warn").css('display','none');
			discountCk = true;
		} else {
			$(".bookDiscount_warn").css('display','block');
			discountCk = false;
		}	
		
		if(bookIntro != '<br data-cke-filler="true">'){
			$(".bookIntro_warn").css('display','none');
			introCk = true;
		} else {
			$(".bookIntro_warn").css('display','block');
			introCk = false;
		}	
		
		if(bookContent != '<br data-cke-filler="true">'){
			$(".bookContents_warn").css('display','none');
			contentCk = true;
		} else {
			$(".bookContents_warn").css('display','block');
			contentCk = false;
		}

    if(bookNameCk && authorIdCk && publeYearCk && publisherCk && cateCodeCk && priceCk && stockCk && discountCk && introCk && contentCk ){
		//alert('통과');
		enrollForm.submit();
	} else {
		return false;
	}
```

<div style="text-align:center; font-size:0.8em;">각 항목들을 체크해준다. 최종 체크까지 작성</div>

<br>

실행을 했지만 

![image]({{ site.baseurl }}/images/20230518_171740.png)

<div style="text-align:center; font-size:0.8em;">실행화면</div>

<br>
웹에서 정보도 못 불러오고  위즈윅 에디터도 작동이 되지 않는다. 또한 위즈윅 에디터 밑에도 경고span도 안뜨고 그외 버튼들도 모두 오류가 뜬다.

이유는 위에서 작성한 변수명과 유효성 검사 조건문들이 모두 아무 메서드에도 속하지 않았기 때문이다.

등록 버튼을 눌렀을때 유효성 검사를 해야하니까 등록 버튼 클릭시 실행되는 매서드 안에 집어 넣어야한다.

![image]({{ site.baseurl }}/images/20230518_172109.png)

<div style="text-align:center; font-size:0.8em;">등록 버튼 매서드에 넣어준다.</div>

<br>
<br>

![image]({{ site.baseurl }}/images/20230518_172208.png)

<div style="text-align:center; font-size:0.8em;">역시나 잘 된다.</div>

오늘은 여기까지

<br>
<br>

>추가로 위에 **bookContent != '\<br data-cke-filler="true">'** 이 조건문이 등장하는데, 위즈윅 에디터에서 textarea 공란이 아니면 \<p>\<br>\<p>이런 형태이고, 공란이면 \<p>\<br data-cke-filler="true">\<p> 형태를 띈다고 한다. 그래서 위 조건문은 위즈윅 에디터가 공란이 아닐때를 의미한다.






