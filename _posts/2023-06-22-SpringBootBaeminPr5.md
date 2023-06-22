---
layout: single
title: "(SpringBoot) 배민 클론 코딩4 - 매장목록"
categories:
  - SpringBoot
---

<br>

**깃허브 repository** : [SpringBoot Baemin practice](https://github.com/Molban2J/Baemin.git "github")

\* 참고한 블로그: [blog link](https://sumin2.tistory.com/10?category=900942 "github")


<br>
<br>
<br>
<br>

## 목차

* [HTML, CSS, JS파일 추가](#html-css-js파일-추가)
* [Controller, DTO, DAO, Service, Mapper 생성](#controller-dto-dao-service-mapper-생성)

<br>
<br>
<br>

## 매장 목록


<br>
<br>
<br>

## HTML, CSS, JS파일 추가

<br>
<br>

store 경로들을 만들어 준다.


![image]({{ site.baseurl }}/images/2023-06-22/20230622_175641.png)

<div style="text-align:center; font-size:0.8em;">경로 설정</div>

<br>
<br>
<br>

해당 경로에 알맞은 파일들을 생성해 준다.

<br>
<br>

<details>
<summary>store.css</summary>
<div markdown="1">

```css
.active {
	background: #ddd;
    color: #af8865; 
}

.container {
    width: 100%;
}


/* ������ */
main {
   background: #fff; 
}

/* ��� ī�װ� */
.category{
    border-bottom: 1px solid #ddd;
    white-space: nowrap;
    overflow-x: auto;
    overflow-y: hidden; 
    padding: 15px 0;
    box-shadow: 0px 2px 3px 0px rgb(0 0 0 / 25%);
    margin: 0 auto;
    max-width: 1200px;
}

.category::-webkit-scrollbar {
    background: none;
}

.category li {
    font-size: 1.6rem;
	font-weight: bold;
    padding: 0 15px;
    display:inline;
    color: #ddd;
}

.category li > span{
	padding-bottom: 5px;
}

.category li:hover {
    color: #333333;
    cursor: pointer;
    transition: 0.1s;
}

/* ��� ī�װ� */


/* ��� �ɼ� */
.option  { 
    font-size: 1.6rem;
    margin: 0 auto;
    margin-top: 10px;
    max-width: 1200px;
}
.option ul { 
    margin:20px 10px;
}

.option li { 
    display: inline;
    padding: 10px 20px;
    border-radius: 20px;
}

.option li:hover {  
   	background: #ddd;
    color: #af8865; 
    transition: 0.1s;
}

/* ��� �ɼ� */


/* ���� ��� */
/* �и� */
/* ���� ��� */


/* ������ */




@media (max-width:1024px) {
	.container {
	    width: 100%;
	}
	
	.option  { 
	    white-space: nowrap;
	    overflow-x: auto;
	    overflow-y:hidden; 
	}
	
	.option::-webkit-scrollbar  { 
	    background:none;
	}
	
}


@media (max-width:767px) {
	
	.container {
	    width: 99%;
	}
	
	.category {
		padding: 10px 0;
		height: 25px;
	}
	
	.category li {
	    font-size: 15px;
	    padding: 10px 15px;
	}
	.option  { 
	   font-size: 13px;
	}
	
	.option ul { 
	    margin:10px;
	}
	
	
}




```

<div style="text-align:center; font-size:0.8em;">store.css</div>

</div>
</details>

<br>
<br>

<details>
<summary>store-li.css</summary>
<div markdown="1">

```css

/* ���� ��� */

.box .fas {
	color: gold;
}

.box {
 	margin: 0 auto;
    max-width: 1200px;
}

.box .temp_img {
	display: block;
	margin: 0 auto;
	width:90%;
	max-width: 500px;
}


.box .store {
    display: flex;
    flex-wrap: wrap;
}

.box .store li {
	display: flex;
	border: 1px solid #ddd;
	width: 47%;
	margin: 15px;
	padding: 20px 15px;
	box-sizing: border-box;
	border-radius: 10px;
	position: relative;
	overflow: hidden;
}


.box .store li .img_box {
	margin-right: 10px;
	border: 1px solid #ddd;
	width: 130px;
	height: 130px;
	border-radius: 10px;
	overflow: hidden;
}

.box .store li .img_box img {
	width: 100%;
	height: 100%;
	object-fit: cover;
}

.box .store li .img_box img:hover {
	transform: scale(1.05);
	transition: 0.1s;
}

.box .store li .info_box {
	overflow: hidden;
	flex: 1;
}


.box .store li .info_box h2 a {
    text-overflow: ellipsis;
    overflow: hidden;
    white-space: nowrap;
}


.box .store li .info_box a > span {
	display: block;
	white-space: nowrap;
	text-overflow: ellipsis;
}

.store .is_open {
	background: rgba(0,0,0,0.7);
    width: 100%;
    height: 100%;
    position: absolute;
    top: 0;
    left: 0;
    border-radius:10px;
    font-size: 1.8rem;
}
.store .is_open a{
    color: #fff;
    display: flex;
	justify-content: center;
	align-items: center;
	height: 100%; 
}

/* ���� ��� */



@media (max-width:1024px) {
	.box .store {
		background: #eee;
	}
	.box .store li {
	    width: 100%;
	    border:none;
	    border-radius: 0;
	    border-bottom: 2px solid #ddd;
	    margin: 0 auto 10px;
	    background: #fff;
	}
	
	.store .is_open {
		border-radius: 0;
	}
}


@media (max-width:767px) {
	.box .store li .img_box {
		width: 100px;
		height: 100px;
	}
}

@media (max-width:480px) {
	.box .store li {
		padding: 20px 10px;
	}
	
	.box .store li .img_box {
		width: 80px;
		height: 80px;
	}
}

```

<div style="text-align:center; font-size:0.8em;">store-li.css</div>

</div>
</details>

<br>
<br>

<details>
<summary>store.js</summary>
<div markdown="1">

```javascript
/*
$(document).ready(function() {
	const category = $(".category").data("category");
	const address1 = $(".address1").val();
	
	let sort = "기본순";
	$(".option li[data-sort='기본순']").addClass("active");

	$("li[data-category = '" + category + "'] > span").css("border-bottom", "3px solid #333333");
	$("li[data-category = '" + category + "'] > span").css("color", "#333333");

	let winHeight = 0;
	let docHeight = 0;
	let page = 1;
	let run = false;
	
	$(window).scroll(function(){
		winHeight = $(window).height();
		docHeight = $(document).height();
		
		const top = $(window).scrollTop();
		
		if(docHeight <= winHeight + top + 10 ) {
			if(run) {
				return;
			}
			console.log("페이지 추가");
			console.log("sort= " + sort);
			
			page++;
			run = true;
			
			const data = {
				category : category,
				address1 : address1,
				sort : sort,
				page : page
			}
			
			$.ajax({
				url: "/store/storeList",
				type: "GET",
				data : data
			})
			.done(function(result){
				const storeHtml = storeList(result);
				
				$(".store").append(storeHtml);
				
				if(storeHtml != "") {
					run = false;
				}
			})
			.fail(function(data, textStatus, errorThrown){
				swal("다시 시도해주세요");
			})	
		} // if
	}) // scroll
	
	

// 가게 정렬 
$(".option li").click(function() {
	sort = $(this).data("sort");
	page = 1;
	
	$(".option li").removeClass("active");
	$(this).addClass("active");
	
	const data = {
				category : category,
				address1 : address1,
				sort : sort,
				page : page
			}
			
	$.ajax({
		url: "/store/storeList",
		type: "get",
		data: data
	})
	.done(function(result, textStatus, xhr){
		// 페이지 초기화
		run = false;
		const storeHtml = storeList(result);
		$(".box ul.store").html(storeHtml);
		
	})
	.fail(function(data, textStatus, errorThrown){
		swal("다시 시도해주세요");
	})
}); // function




function storeList(result){
	console.log("sort = " + sort);
	let html = "";
		for(var i=0;i<result.length;i++) {
			const store = result[i];
			
			const id = store.id;
			
			const storeImg = store.storeImg;
			const storeThumb = store.storeThumb;
			const storeName = store.storeName;
			const deleveryTime = store.deleveryTime;
			const minDelevery = store.minDelevery.toLocaleString();
			const deleveryTip = store.deleveryTip.toLocaleString();
			const score = store.score.toFixed(1);
			const reviewCount = store.reviewCount;
			const bossCommentCount = store.bossCommentCount;
			const openingTime = store.openingTime;
			const closingTime = store.closingTime;
			
			let scoreHtml = "";
			for(var j=0;j<5;j++) {
				if(Math.round(score)  > j) {
					scoreHtml += "<i class='fas fa-star'></i> ";
				} else {
					scoreHtml += "<i class='far fa-star'></i> ";
				}
			}
			let isOpenHtml = "";
			if(store.isOpen == "false") {
				isOpenHtml = `<div class="is_open">
								<a href="/store/detail/${id }">지금은 준비중입니다</a>
							</div>`;
			}
			
			
			html += 
			`<li >
				<div class="img_box">
					<a href="/store/detail/${id }"><img src="${storeImg }" alt="이미지"></a>
				</div>
				<div class="info_box">
					<h2><a href="/store/detail/${id }">${storeName }</a></h2>
					<a href="/store/detail/${id }">
						<span>
							<span>평점 ${score }</span>
							<span class="score_box">
								${scoreHtml}
							</span>
						</span>
						
						<span>
							<span>리뷰 ${reviewCount }</span>
							<span>사장님 댓글 ${bossCommentCount }</span>
						</span>
						
						<span>
							<span>최소주문금액 ${minDelevery }원</span>
							<span>배달팁 ${deleveryTip }원</span>
						</span>
						<span>배달시간 ${deleveryTime }분</span>
					</a>
				</div>
        	${isOpenHtml}
        </li>`;
		}
	return html;	
}



});

*/
```

<div style="text-align:center; font-size:0.8em;">store.js</div>

</div>
</details>

<br>
<br>

<details>
<summary>store.html</summary>
<div markdown="1">

```html


<th:block th:insert="/include/link.html"></th:block>

<link rel="stylesheet" href="/css/store/store.css">
<link rel="stylesheet" href="/css/store/store-li.css">

<th:block th:insert="include/header.html"></th:block>


<!-- 콘텐츠 -->
<main>
    <div class="container">
        <div class="category" data-category="${category }">
            <ul>
                <li data-category ='100' onclick="location.href='/store/100/${address1 }'"><span>피자</span></li>
                <li data-category ='101' onclick="location.href='/store/101/${address1 }'"><span>치킨</span></li>
                <li data-category ='102' onclick="location.href='/store/102/${address1 }'"><span>패스트푸드</span></li>
                <li data-category ='103' onclick="location.href='/store/103/${address1 }'"><span>분식</span></li>
                <li data-category ='104' onclick="location.href='/store/104/${address1 }'"><span>카페/디저트</span></li>
                <li data-category ='105' onclick="location.href='/store/105/${address1 }'"><span>돈까스/일식</span></li>
                <li data-category ='106' onclick="location.href='/store/106/${address1 }'"><span>중국집</span></li>
                <li data-category ='107' onclick="location.href='/store/107/${address1 }'"><span>족발/보쌈</span></li>
                <li data-category ='108' onclick="location.href='/store/108/${address1 }'"><span>야식</span></li>
                <li data-category ='109' onclick="location.href='/store/109/${address1 }'"><span>한식</span></li>
                <li data-category ='110' onclick="location.href='/store/110/${address1 }'"><span>1인분</span></li>
                <li data-category ='111' onclick="location.href='/store/111/${address1 }'"><span>도시락</span></li>
            </ul>
        </div>

        <input type="hidden" value="${address1 }" class="address1">

        <div class="option">
            <ul>
                <li data-sort="기본순">기본순</li>
                <li data-sort="배달 빠른 순">배달 빠른 순</li>
                <li data-sort="배달팁 낮은 순">배달팁 낮은 순</li>
                <li data-sort="별점 높은 순">별점 높은 순</li>
                <li data-sort="리뷰 많은 순">리뷰 많은 순</li>
                <li data-sort="최소 주문 금액 순">최소 주문 금액 순</li>
            </ul>
        </div>



        <div class="box">

            <th:block th:if="${storeList == null}">
                <img class="temp_img" alt="이미지" src="/img/temp2.png">
                <style>main .box {background: #F6F6F6; max-width: 100%; }</style>
            </th:block>


            <ul class="store">
                <th:block th:with="store_admin='/store/'"></th:block>

                    <th:block th:insert="/store/store-li.html"></th:block>
                </th:block>
            </ul>
        </div>

    </div>
</main>
<!-- 콘텐츠 -->


<!-- 하단 메뉴 -->
<th:block th:insert="/include/nav.html"></th:block>
<!-- 하단 메뉴 -->

<!-- 푸터 -->
<th:block th:insert="/include/footer.html"></th:block>
<!-- 푸터 -->

<script type="text/javascript" src="/js/store/store.js" ></script>

</body>
</html>
```

<div style="text-align:center; font-size:0.8em;">store.html</div>

</div>
</details>

<br>
<br>

<details>
<summary>store-li.html</summary>
<div markdown="1">

```html

<li th:each="storeList : ${storeList}">
  <!-- <a href="${store_admin }/detail/${storeList.id }"> -->

  <div class="img_box">
    <a th:href="${store_admin }+'/detail/'+${storeList.id }"><img th:src="${storeList.storeImg }" alt="이미지"></a>
  </div>

  <div class="info_box">
    <h2><a th:href="${store_admin}+'/detail/'+${storeList.id}" th:text="${storeList.storeName}"></a></h2>

    <a th:href="${store_admin }+'/detail/'+${storeList.id }">
      <span>
          <span th:text="평점 ${storeList.score }"></span>

          <span class="score_box">
              <th:block th:each="i: ${#numbers.sequence(0, 4)}">
                <th:block th:if="${#numbers.round(storeList.score) > i}">
                  <i class="far fas fa-star"></i>
                </th:block>
                <th:block th:if="${#numbers.round(storeList.score) <= i}">
                  <i class="far fa-star"></i>
                </th:block>
              </th:block>

          </span>
      </span>

<span>
      <span th:text="리뷰 ${storeList.reviewCount }"></span>
      <span th:text="사장님 댓글 ${storeList.bossCommentCount }"></span>
  </span>


        <span th:text="'최소주문금액 '+${#numbers.formatInteger(storeList.minDelevery, 0, 'DEFAULT')}+'원'"></span>
        <span th:text="'배달팁 '+${#numbers.formatInteger(storeList.deleveryTip, 0, 'DEFAULT')}+'원'"></span>

      <span th:text="'배달시간 '+${storeList.deleveryTime }+'분'"></span>
    </a>
  </div>


  <div th:if="${!storeList.isOpen}">
    <div class="is_open">
      <a th:href="'/store/detail/'+${storeList.id }">지금은 준비중입니다</a>
    </div>
  </div>
</li>

```

<div style="text-align:center; font-size:0.8em;">store-li.html</div>

<br>
<br>

기존 jsp의 fmt에서 format기능을 대신 하는것이 #numbers.formatInteger이다. 형식화 할 숫자를 넣고, 0은 소수점 밑으로 표현되는 자릿수(여기서는 소수점 밑으로 표현하지 않기 때문에 0이다.), 그리고 쉼표로 구분한다는 'COMMA'가 들어간다. DEFAULT가 COMMA기 때문에 나는 DEFALT로 해줬다.

</div>
</details>

<br>
<br>
<br>

## Controller, DTO, DAO, Service, Mapper 생성

<br>
<br>

먼저 StoreController 파일 생성후 코드를 작성해준다.

<br>
<br>

```java
@Controller
public class StoreController {
    @Autowired
    private StoreService storeService;

    @GetMapping("/store/{category}/{address1}")
    public String store(@PathVariable int category, @PathVariable int address1, Model model){
        List<Store> storeList = storeService.storeList(category, address1/100);
        model.addAttribute("storeList", storeList);
        return "store/store";
    }
}
```

<div style="text-align:center; font-size:0.8em;">StoreController.java</div>

<br>
<br>
<br>
<br>

StoreDTO 생성

<br>
<br>

```java
@Getter
@Setter
@ToString
public class Store {
    private long id;
    private int category;
    private String storeName;
    private int storeAddress1;
    private String storeAddress2;
    private String storeAddress3;
    private String storePhone;
    private String storeImg;
    private String storeThumb;
    private int openingTime;
    private int closingTime;
    private int minDelevery;
    private int deleveryTime;
    private int deleveryTip;
    private String storeDes;
    private boolean isOpen; //추가
}
```

<div style="text-align:center; font-size:0.8em;">Store.java</div>

<br>
<br>

블로그에는 isOpen이 없어서 boolean으로 추가해줬다.

<br>
<br>
<br>
<br>

Service와 ServiceImpl 작성해준다.

<br>
<br>

```java
public interface StoreService {
	List<Store> storeList(int category, int address);
}
```

<div style="text-align:center; font-size:0.8em;">StoreService.java</div>


<br>
<br>

```java
@Service
public class StoreServiceImp implements StoreService {
 
	@Autowired
	private StoreDAO storeDAO;
	
	@Override
	public List<Store> storeList(int category, int address) {
		Map<String, Object> map = new HashMap<>();
		map.put("category", category);
		map.put("address1", address);
		
		return storeDAO.storeList(map);
	}
 
```

<div style="text-align:center; font-size:0.8em;">StoreService.java</div>

<br>
<br>
<br>
<br>

StoreDAO와 DAOImpl파일 생성


<br>
<br>

```java
public interface StoreDAO {
 
	List<Store> storeList(Map<String, Object> map);
 
}
```

<div style="text-align:center; font-size:0.8em;">StoreDAO.java</div>


<br>
<br>

```java
@Repository
public class StoreDAOImp implements StoreDAO {
 
	@Autowired
	private SqlSession sql;
	
	@Override
	public List<Store> storeList(Map<String, Object> map) {
		return sql.selectList("store.storeList", map);
	}
}
 
```

<div style="text-align:center; font-size:0.8em;">StoreDAOImpl.java</div>

<br>
<br>
<br>
<br>

그리고 쿼리문을 실행할 Mapper도 생성해줍니다.


<br>
<br>

```java
@Mapper
public interface StoreMapper {
    @Select("SELECT * FROM BM_STORE")
    public List<Store> storeList(Map<String,Object> map);
}
```

<div style="text-align:center; font-size:0.8em;">StoreMapper.java</div>

<br>
<br>
<br>
<br>

아직 관리자 페이지가 없기 때문에 예시 데이터를 임시로 넣어줍니다.

<br>
<br>

```sql
INSERT INTO bm_store VALUES (STORE_ID_SEQ.NEXTVAL, 100, '도미노피자'
		    ,'31099','천안시 두정동', '상세주소', '01013245678', '\img\none.gif', '\img\none.gif', 10, 19, 2000
		    ,30, 2000, '가게 소개'  );
            
INSERT INTO bm_store VALUES ( STORE_ID_SEQ.NEXTVAL, 100, '빅스타피자'
		    ,'31099','천안시 두정동', '상세주소', '01013245678', '\img\none.gif', '\img\none.gif', 13, 25, 3000
		    ,40, 3000, '가게 소개'  );	
            
INSERT INTO bm_store VALUES ( STORE_ID_SEQ.NEXTVAL, 100, '피자스쿨'
		    ,'31099','천안시 두정동', '상세주소', '01013245678', '\img\none.gif', '\img\none.gif', 10, 15, 4000
		    ,50, 4000, '가게 소개'  );	
            
INSERT INTO bm_store VALUES ( STORE_ID_SEQ.NEXTVAL, 100, '피자헛'
		    ,'31099','천안시 두정동', '상세주소', '01013245678', '\img\none.gif', '\img\none.gif', 11, 13, 1000
		    ,30, 2000, '가게 소개'  );
```

<div style="text-align:center; font-size:0.8em;">DB에 추가</div>

<br>
<br>
<br>
<br>

아직 가게 평점과 리뷰가 없기 때문에 store-li.html에 해당 부분을 주석처리 하고 실행해 본다.

<br>
<br>


![image]({{ site.baseurl }}/images/2023-06-22/20230622_194929.png)

<div style="text-align:center; font-size:0.8em;">가게 리스트 화면</div>

