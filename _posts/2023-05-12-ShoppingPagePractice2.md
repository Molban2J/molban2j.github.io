---
layout: single
title:  "쇼핑 페이지 예제 클론코딩2"
---

5/12

-현재까지 구현된 기능: 메인페이지, 로그인, 회원가입,관리자페이지에서 작가 등록


AuthorManage.jsp페이지에서 작가들 목록을 구현합니다. 기존 목록과 다른점은 페이징 기능이 들어간것인데, 페이징에 쓰이는 sql문과 알고리즘을 좀더 자세히 이해하고 넘어가는 것이 목표입니다.

---

<br>
<br>

먼저 기존 파일을 footer와 header를 분리해 \<include> 태그를 써줍니다.

- footer.jsp
- header.jsp

생성


![image]({{ site.baseurl }}/images/20230512_101720.png)

##### **<p style="text-align:center;">header.jsp<p>**

![image]({{ site.baseurl }}/images/20230512_101854.png)

##### **<p style="text-align:center;">footer.jsp<p>**

<br>
그리고 기존 파일의 헤더파일과 푸터파일의 코드가 중복되는 부분을 지워주고 

```jsp
<%@ include file="header.jsp"%>

<%@ include file="footer.jsp"%>
```

include 해줍니다.



![image]({{ site.baseurl }}/images/20230512_104434.png)

##### <p style="text-align:center;">authorManage.jsp<p>

<br>

기존 admin폴더 안에있는 jsp파일들에 동일하게 적용

*기존 예제는 css는 수정없이 그대로 진행한다. 하지만 footer와 header를 만들어 불필요한 중복을 피한만큼 footer.css와 header.css를 만들어 css파일 내에서도 중복을 피하고자 한다.


![image]({{ site.baseurl }}/images/20230512_105415.png)

##### <p style="text-align:center;">css파일<p>

중복코드 삭제 & footer.jsp, header.jsp에 link로 css적용

---

## 작가목록 구현

### 1. Criteria 생성

<br> 
com.shop.model 밑에 Criteria.java 생성

#### *criteria는 기준이라는 의미이다.

<br>

**Criteria 코드 작성**

```java
package com.shop.model;

import lombok.Data;

@Data
public class Criteria {
	//현재 페이지 번호
	private int pageNum;
	
	//페이지 표시 개수
	private int amount;
	
	//검색 타입
	private String type;
	
	//검색 키워드
	private String keyword;

	//Criteria 생성자
	public Criteria(int pageNum, int amount) {
		this.pageNum = pageNum;
		this.amount = amount;
	}
	
	//Criteria 기본 생성자
	public Criteria() {
		this(1,10);
	}
	
	//검색타입의 배열 변환
	public String[] getTypeArr() {
		return type==null ? new String[] {}:type.split("");
	}
}

```

검색타입은 "제목", "작가", "내용"으로 나누어 분류하기위한 변수
<br>
검색 타입의 배열 변환은 "제목+작가","제목+내용" 이런식으로 타입을 복합적으로 적용하기 위한 메소드다.

```java
//검색타입의 배열 변환
	public String[] getTypeArr() {
		return type==null ? new String[] {}:type.split("");
	}
```
첨엔 이 코드가 뭔지 이해가 잘 안갔는데 자세히 한번 뜯어보면..
<br>
>삼항 연산자로 type == null이라면 String[]의 배열에 아무것도 넣지 않은 비어있는상태로 반환 / type != null이라면 type을 문자 단위로 잘라서(split) String[]에 삽입 후 반환 한다.

라는 의미이다.

<br>

삼항연산자가 익숙하지 않아  {}:type.split("") 이부분을 붙여 하나의 변수명으로 인식 해서 처음에 이해가 안갔던것 같다.

<br><br><br>

---
### 2. Mapper 생성

AuthorMapper.java 인터페이스에 클래스 선언

```java
//작가 목록
	public ArrayList<AuthorVO> authorList(Criteria cri);
```

AuthorMapper.xml과 AuthorService.java, AuthorServiceImpl.java에도 코드를 추가

```xml
<select id="authorList" resultType="com.shop.model.AuthorVO">
	<![CDATA[
		select * from(
			select /*+INDEX_DESC(jm_author 인덱스명)*/
				rownum as rn, authorId, authorName, nationId, regDate, updateDate 
			from jm_author
			where
	]]>
	
		<if test="keyword != null">
			authorName like '%'||#{keyword}||'%' and
		</if>
	<![CDATA[
		rownum<= #{pageNum} * #{amount}
		)
		where rn > (#{pageNum} -1)* #{amount}
	]]>
		
	</select>
```

#### <p style="text-align: center">AuthorMapper.xml 코드</p>

위 코드는 잘 이해가 가지 않으니 하나씩 뜯어서 이해해보려 한다.

---

>뜯어보기 전에 먼저 현재 데이터의 양이 너무 적어 페이징 적용을 알기 어려우니 *재귀 복사*를 통해 자료의 양을 늘려놓겠다.

```sql
insert into jm_author (authorId, authorName, nationId)(select author_seq.nextval, authorName, nationId from jm_author);
```

![image]({{ site.baseurl }}/images/20230512_121257.png)

##### <p style="text-align:center;">sql실행<p>

>약 25000개의 자료를 넣었다. <br>sql문을 작성하면서도 오류를 발견했는데, jm_author 테이블에는 authorIntro이라는 long타입의 칼럼이 존재하는데, 이 칼럼명을 포함에서 재귀 복사를 하려면 오류가 발생한다.

```sql
insert into jm_author (authorId, authorName, nationId, authorIntro)(select author_seq.nextval, authorName, nationId, authorIntro from jm_author);
```
##### <p style="text-align:center;">이 sql문을 실행하면<p>

```sql
명령의 30 행에서 시작하는 중 오류 발생 -
insert into jm_author (authorId, authorName, nationId, authorIntro)(select authorId, authorName, nationId, authorIntro from jm_author)
오류 발생 명령행: 30 열: 108
오류 보고 -
SQL 오류: ORA-00997: illegal use of LONG datatype
00997. 00000 -  "illegal use of LONG datatype"
*Cause:    
*Action:
```

##### <p style="text-align:center;">이러한 오류가 뜬다..<p>

>long 타입의 잘못된 사용이라는 건데.. 찾아보니 long타입에는 제약 조건이 몇가지 있다.
* 집계함수나 그룹화에서 사용할 수 없음
* 인덱스를 만들거나 UNIQUE 제약조건을 적용할 수 없음.
* PL/SQL 변수, 파라미터 또는 함수의 매개변수로 사용 불가.
* SQL * Loader에서 사용할 수 없음

>그리고 이제는 Long타입을 잘 안쓴다고 한다. 예제가 오래전꺼다 보니 자료형이나 문법이 오래된것들이 있는데 이제는 Long타입 대신 문자열은 CLOB 형을 사용할 수 있다고 한다. <br>
알게 된 이상 Long 자료형을 CLOB으로 바꿔주도록 하겠다.

```sql
ALTER TABLE jm_author MODIFY authorIntro CLOB;
```
##### <p style="text-align:center;">Long 형을 CLOB 형으로<p>


> 하지만... 될 줄 알았지만 다시 오류가 떴다.....


```sql
명령의 31 행에서 시작하는 중 오류 발생 -
insert into jm_author (authorId, authorName, nationId,authorIntro)(select author_seq.nextval, authorName, nationId,authorIntro from jm_author)
오류 보고 -
ORA-01502: index 'SCOTT.SYS_C008154' or partition of such index is in unusable state
```

##### <p style="text-align:center;">다시 오류발생..<p>

> 이 오류는 인덱스가 손상되거나 잘못된 방법으로 수정되었을때 뜨는 오류라고 한다. 아마 alter table로 자료형을 바꾸다 보니 인덱스가 손상된거 같다.<br> 그냥 Drop table로 테이블을 삭제하고 다시 만들었다. 다시 만들었더니 재귀복사가 잘 된다.

---

생각보다 위 오류 잡는데 시간이 걸렸다.

이제 제데로 페이징 sql문을 보도록 하겠다.

<br>

sql문을 보면 중간에

```sql
/*+INDEX_DESC(jm_author 인덱스명)*/
```
이러한 코드를 볼 수 있는데 이거는 주석이 아니라 **hint**라고 한다. 힌트는 데이터베이스가 명령문을 수행할 때 "~~~식으로 수행해"라고 알려주는 것이다. <br>
DBMS가 작업을 수행하는 과정은 **1. SQL파싱 -> 2. SQL최적화 -> 3. SQL 실행**의 과정을 거친다. 이 중 *SQL 실행* 과정에서는 명령문을 어떻게 수행할지 **실행 계획**을 세우는데, **이 실행계획을 지시해 주는 것이 힌트이다.**
<br>
DBMS에서 실행계획을 세울 때는 이전 단계에서 계산한 비용(cost)값을 참고해 가장 비용이 적게 드는 방향으로 실행계획을 세운다고 한다. 하지만 자료의 양이 방대해지면 이러한 최적의 실행계획이 아니라 다른 실행계획을 세워 비교적 비효율적으로 수행 한다고 하는데 이를 방지하기 위해 **힌트**를 사용한다.

<br>
<br>

**하지만 order by 명령문을 쓰면 되지 않을까?**<br>
order by 명령문을 사용해도 똑같은 결과를 얻지만, order by의 수행 방식은 전체의 데이터를 스캔한 후 다시 정렬을 하기 때문에 자료의 양이 많을 수록 시간이 많이든다. 
<br>
하지만 힌트를 사용하면 **(첫 스캔에 정렬되어있는)인덱스**를 참조하기 때문에 더 효율적이라고 볼 수 있다.
<br>
힌트는 대용량 자료의 검색과 정렬 기능을 향상 시켜주지만 힌트 사용에 확신이 없는 어중간한 상태라면 오히려 비효율적이니 신중하게 사용하는 것이 좋다고 한다.

<br>
<br>
그리고 "jm_author 인덱스명"에 인덱스명은 업데이트 날짜 기준으로 정렬 하고싶으니까 updateDate으로 써줍니다..

```sql
/*+INDEX_DESC(jm_author updateDate)*/
```
<br>
<br>

```xml
<![CDATA[]]]>
```

<p style="text-align:center; font-size:0.8em;">이 태그는 부등호 등 수학기호를 쓰기 위해서 사용한다.</p>

<br>
<br>

```xml
<if test="keyword != null">
	authorName like '%'||#{keyword}||'%' and
</if>
```
<p style="text-align:center; font-size:0.8em;">keyword가 존재한다면 keyword 검색</p>


<br>
<br>
<br>
<br>

---

이제 마저 코딩 해보겠다.

mapper도 만들었으니 JUnitTest 진행

![image]({{ site.baseurl }}/images/20230512_144058.png)

<p style="text-align:center; font-size:0.8em;">test가 잘 작동한다.</p>

<br>
<br>

목록을 잘 불러 오니까 키워드를 세팅해주고서 다시한번 테스트

![image]({{ site.baseurl }}/images/20230512_144602.png)

<p style="text-align:center; font-size:0.8em;">어째서인지 keyword로 검색한 결과로 나오는게 아니라 이전과 똑같이 나온다.</p>

<br>

찾았다....

```java
Criteria cri = new Criteria(3,10);
List<AuthorVO> list = mapper.authorList(cri);
cri.setKeyword("홍준");
```
<p style="text-align:center; font-size:0.8em;">setKeyword의 순서가 잘못 됐다.</p>


```java
Criteria cri = new Criteria(3,10);
cri.setKeyword("홍준");
List<AuthorVO> list = mapper.authorList(cri);
```

<p style="text-align:center; font-size:0.8em;">변경 후</p>

<br>

![image]({{ site.baseurl }}/images/20230512_145455.png)

<p style="text-align:center; font-size:0.8em;">잘 작동되는 모습</p>

---
### 3. Service 생성

![image]({{ site.baseurl }}/images/20230512_145811.png)

<p style="text-align:center; font-size:0.8em;">service 생성</p>

![image]({{ site.baseurl }}/images/20230512_145919.png)

<p style="text-align:center; font-size:0.8em;">serviceImpl 생성</p>

<br>
<br>

### 4. Controller 생성

```java
 /* 작가 관리 페이지 접속 */
    @GetMapping("/authorManage")
    public void authorManageGET() throws Exception{
        logger.info("작가 관리 페이지 접속");
    }   
```
<p style="text-align:center; font-size:0.8em;">기존의 controller</p>

```java
  /* 작가 관리 페이지 접속 */
    @GetMapping("/authorManage")
    public void authorManageGET(Criteria cri, Model model) throws Exception{
        logger.info("작가 관리 페이지 접속");
        
        List<AuthorVO> list = authorService.authorList(cri);
        model.addAttribute("list", list);
    }  
```

<p style="text-align:center; font-size:0.8em;">변경 후</p>


> 하지만 여기서 의문점이 생겼다.

```java
  /* 작가 관리 페이지 접속 */
    @GetMapping("/authorManage")
    public void authorManageGET(Criteria cri, HttpServletRequest request) throws Exception{
        logger.info("작가 관리 페이지 접속");
        
        List<AuthorVO> list = authorService.authorList(cri);
        request.setParameter("list", list);
    }  
```

>이런식으로 HttpServletRequest 클래스를 사용하면 안될까? 사실 위 코드에서 실행될때 차이점은 없다고 한다.<br>
정보를 view로 전송할 때는 큰 차이가 없지만 정보를 받아올 때는 
HttpServletRequest의 경우는 getParameter로 일일히 정보를 받아서 다시 객체에 담아줘야하지만, Model은 정보를 받아올때 자동으로 매핑이 되서 객체에 담아진다고 한다. 그리고 나중에 model 객체에서 바로 view로 연결 할 수 있다고 하는데 이거는 잘 감이 안온다... <br>
아무튼 이러한 차이점이 있고 스프링에서는 되도록이면 http클래스를 안쓰려한다고 한다.(~~진짜이려나..선생님 말이지만 100퍼센트 신뢰는 못하겟다~~)

<br>
<br>

### AuthorVO 수정


```java
package com.shop.model;

import java.util.Date;

import lombok.Data;

@Data
public class AuthorVO {
	private String authorName, nationId, authorIntro, nationName;
	private int authorId;
	private Date regDate, updateDate;

	public void setNationId(String nationId) {
		this.nationId = nationId;
		
		if(nationId.equals("01")) {
			this.nationId = "국내";
		} else {
			this.nationId = "국외";
	  }
		
	}
		
}
```
<p style="text-align:center; font-size:0.8em;">변경 후<br>lombok의 data어노테이션이 있어도 getter/setter를 정의해주면 나중에 따로 정의한 getter/setter를 기준으로 데이터를 매핑한다.</p>


<br>
<br>
<br>

---

### 5. View 수정


```html
<div class="author_table_wrap">
	  <table class="author_table">
				<thead>
					<tr>
						<td class="th_column_1">작가 번호</td>
						<td class="th_column_2">작가 이름</td>
						<td class="th_column_3">작가 국가</td>
						<td class="th_column_4">등록 날짜</td>
						<td class="th_column_5">수정 날짜</td>
					</tr>
				</thead>
				<c:forEach items="${list }" var="list">
					<tr>
						<td><c:out value="${list.authorId }"/></td>
						<td><c:out value="${list.authorName }"/></td>	
                        <td><c:out value="${list.nationName }"/></td>
						<td><fmt:formatDate value="${list.regDate }" pattern="yyyy-MM-dd"/></td>
						<td><fmt:formatDate value="${list.updateDate }" pattern="yyyy-MM-dd"/></td>
					</tr>
				</c:forEach>
			</table>
		</div>
```
<p style="text-align:center; font-size:0.8em;">authorManage.jsp</p>


![image]({{ site.baseurl }}/images/20230512_154037.png)

<p style="text-align:center; font-size:0.8em;">실행 결과 <br> 매우 이상하긴한데 나오긴 나온다. 일단 css적용해보기로</p>


![image]({{ site.baseurl }}/images/20230512_154605.png)

<p style="text-align:center; font-size:0.8em;">쨘</p>

---

<br>

## 페이지 버튼 만들기

### 6. PageVO 만들기

com.shop.model에 PageVO생성

```java
package com.shop.model;

public class PageVO {
	//페이지 시작 번호
	private int pageStart;
	
	//페이지 끝 번호
	private int pageEnd;
	
	//이전, 다음 버튼의 존재 유무
	private boolean next, prev;
	
	//행 전체 개수
	private int total;
	
	//현재 페이지 번호(pageNum), 행 표시 수(amount), 검색 키워드(keyword), 검색 종류(type)
	private Criteria cri;
	
	//생성자(클래스 호출시 각 변수 값 초기화
	public PageVO(Criteria cri, int total) {
		//초기화
		this.total = total;
		this.cri = cri;
		
		//페이지 끝 번호
		this.pageEnd = (int)(Math.ceil(cri.getPageNum()/10.0))*10;
		
		//페이지 시작 번호
		this.pageStart = this.pageEnd - 9;
		
		//전체 마지막 페이지 번호
		int realEnd = (int)(Math.ceil(total*1.0/cri.getAmount()));
		
		//페이지 끝 번호 유효성 체크
		if(realEnd < pageEnd) {
			this.pageEnd = realEnd;
		}
		
		//이전 버튼 값 초기화
		this.prev = this.pageStart>1;
		
		//다음 버튼 값 초기화
		this.next = this.pageEnd<realEnd;
	}

}

```

 <p style="text-align:center; font-size:0.8em;">PageVO</p>

<br>

>페이지 끝번호 구하는 코드를 보자.
<br>
ceil함수는 소수점을 올림하는 함수이다. <br>
만약 현재 페이지가 12페이라고 가정하면, 12/10 = 1.2 -> 올림하면 2 -> 2*10 = 20 => pageEnd = 20 현재 페이지의 10의 자리가 바뀌지 않으면 계속해서 고정된 값이 나오는 것이다.<br>
10.0으로 나눠준 이유는 pageNum은 int형이라 계산할때는 소수점 계산을 위해서 10.0 소수점을 표시해 계산하고 다시 결과 값은 (int)로 형변환 해준다.<br><br>
전체 마지막 페이지 구하는 코드는 전체 자료 개수를 한 페이지에 표시할 개수로 나누고 소수점을 올림하는것이다. 이로 인해 남은 데이터들이 누락되지 않고 다음 페이지에 표시 될 수 있다. <br><br>
이전버튼이 1보다 크다면(=>11) 이전버튼이 생김(true)<br>
다음버튼이 제일 마지막 페이지 보다 작다면 다음버튼이 생김(true)

---

### 7. 전체 데이터 개수


```java
//전체 데이터 개수
	public int authorTotal(Criteria cri);
```
<p style="text-align:center; font-size:0.8em;">AuthorMapper.java에 추가</p>

```xml
<select id="authorTotal" resultType="int">
		select count(*) from jm_board
		<if test="keyword != null">
			where authorName like '%'||#{keyword}||'%'
		</if>
	</select>
```
<p style="text-align:center; font-size:0.8em;">AuthorMapper.xml에 추가</p>

![image]({{ site.baseurl }}/images/20230512_162114.png)

<p style="text-align:center; font-size:0.8em;">JUnitTest성공</p>

+AuthorService.java, AuthorServiceImpl.java에도 코드 추가
<br>
<br>

```java
/* 페이지 이동 인터페이스 데이터 */
        int total = authorService.authorTotal(cri);
        
        PageVO pageMaker = new PageVO(cri, total);
        
        model.addAttribute("pageMaker", pageMaker);
```
<p style="text-align:center; font-size:0.8em;">AdminController에 authorManage매퍼에 삽입</p>

---

### 8. View처리


```html

<!-- 페이지 이동 인터페이스 영역 -->
				<div class="pageMaker_wrap">

					<ul class="pageMaker">

						<!-- 이전 버튼 -->
						<c:if test="${pageMaker.prev}">
							<li class="pageMaker_btn prev"><a href="${pageMaker.pageStart - 1}">이전</a></li>
						</c:if>

						<!-- 페이지 번호 -->
						<c:forEach begin="${pageMaker.pageStart}" end="${pageMaker.pageEnd}" var="num">
							<li class='pageMaker_btn ${pageMaker.cri.pageNum == num ? "active":""}'><a href="${num}">${num}</a></li>
						</c:forEach>

						<!-- 다음 버튼 -->
						<c:if test="${pageMaker.next}">
							<li class="pageMaker_btn next"><a href="${pageMaker.pageEnd + 1 }">다음</a></li>
						</c:if>

					</ul>

				</div>

```

<p style="text-align:center; font-size:0.8em;">authorMange.jsp에 페이지 버튼 삽입</p>

\<li class='pageMaker_btn ${pageMaker.cri.pageNum == num ? "active":""}'> 이 코드는 현재 페이지면 버튼을 비활성화 하고 아니라면 활성화하여 이동할수 있게 하는 코드


```html
<form id="moveForm" action="/admin/authorManage" method="get">
		<input type="hidden" name="pageNum" value="${pageMaker.cri.pageNum}">
		<input type="hidden" name="amount" value="${pageMaker.cri.amount}">
		<input type="hidden" name="keyword" value="${pageMaker.cri.keyword}">
	</form>
```

<p style="text-align:center; font-size:0.8em;">버튼을 누르면 페이지 값들이 넘어가도록 폼태그 작성</p>

그리고 버튼을 눌렀을때 submit이 되도록 스크립트문 작성


```javascript
let moveForm = $('#moveForm');

/* 페이지 이동 버튼 */
$(".pageMaker_btn a").on("click", function(e){
    
    e.preventDefault();
    
    moveForm.find("input[name='pageNum']").val($(this).attr("href"));
    
    moveForm.submit();
```

<p style="text-align:center; font-size:0.8em;">script문</p>



![image]({{ site.baseurl }}/images/20230512_164043.png)


<p style="text-align:center; font-size:0.8em;">실행했을때 모습. css적용하기 전 모습이다.</p>

<br>

그리고 페이지 버튼을 누르면 주소가 /admin/pageNum이렇게 떠서 매핑이 되질 않는다. 이 오류는 차차 해결해 보는거로..














