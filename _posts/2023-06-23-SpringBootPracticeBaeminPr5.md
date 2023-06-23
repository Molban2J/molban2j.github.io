---
layout: single
title: "(SpringBoot) 배민 클론 코딩5 - 매장상세"
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

* [Controller, DTO, DAO, Service, Mapper 생성](#controller-dto-dao-service-mapper-생성)
* [HTML, CSS, JS파일 추가](#html-css-js파일-추가))


<br>
<br>
<br>

## Controller, DTO, DAO, Service, Mapper 생성

<br>
<br>

매장 상세페이지는 가게 정보 뿐만아니라 리뷰와 메뉴들도 필요하기 때문에 StoreDetail.java를 만들어준다.

<br>
<br>


```java
@Getter
@Setter
@ToString
@NoArgsConstructor
@AllArgsConstructor
public class StoreDetail {
    private Store storeDetail;
//    private List<Food> foodList;
//    private List<Review> reviewList;
}
```

<div style="text-align:center; font-size:0.8em;">StoreDetail.java</div>

<br>
<br>

Controller에 getmapping 작성

<br>
<br>


```java
@GetMapping("/store/detail/{id}")
    public String storeDetail(@PathVariable long id, Model model){
        StoreDetail storeDetail = storeService.storeDetail(id);
        model.addAttribute("store", storeDetail);
        return "store/detail";
    }
```

<div style="text-align:center; font-size:0.8em;">StoreController.java</div>

<br>
<br>

필요한 service, dao 작성해준다.


<br>
<br>


```java
public interface StoreService {
	List<Store> storeList(int category, int address);
 
	StoreDetail storeDetail(long id);
}
```

<div style="text-align:center; font-size:0.8em;">StoreService.java</div>

<br>
<br>



```java
	@Override
	public StoreDetail storeDetail(long storeId) {
		Store storeInfo = storeDAO.storeDetail(storeId); 
		
//		List<Food> foodList = storeDAO.foodList(storeId);
//		List<Review> reviewList = storeDAO.reviewList(storeId);
		
		return new StoreDetail(storeInfo);
	}
```

<div style="text-align:center; font-size:0.8em;">StoreServiceImpl.java</div>

<br>
<br>


```java
public interface StoreDAO {
 
	List<Store> storeList(Map<String, Object> map);
 
	Store storeDetail(long storeId);
 
}
```

<div style="text-align:center; font-size:0.8em;">StoreDAO.java</div>

<br>
<br>

```java
@Repository
public class StoreDAOImpl implements StoreDAO {
 
	@Autowired
	private SqlSession sql;
	
	@Override
	public List<Store> storeList(Map<String, Object> map) {
		return sql.selectList("store.storeList", map);
	}
 
	@Override
	public Store storeDetail(long storeId) {
		return sql.selectOne("store.storeDetail", storeId);
	}
}
```

<div style="text-align:center; font-size:0.8em;">StoreDAOImpl.java</div>

<br>
<br>

```java
@Select("select * from bm_store where id = #{storeId}")
    public Store storeDetail(long storeId);
```

<div style="text-align:center; font-size:0.8em;">StoreMapper.java</div>

<br>
<br>
<br>
<br>

## HTML, CSS, JS파일 추가

이제 view와 css, js설정을 해준다.

모달 설정도 들어있기 때문에 templates -> modal 경로 생성해준다.

<br>
<br>

![image]({{ site.baseurl }}/images/2023-06-23/20230623_124101.png)

<div style="text-align:center; font-size:0.8em;">경로와 파일 위치</div>

<br>
<br>

<details>
<summary>detail.css</summary>
<div markdown="1">

```css
.fas.fa-heart {
	color: red;
}

.score_box .fas {
	color: gold;
}

.comment .fas {
	color: gold;
}

#wrap {
	width: 100%;
	max-width: 1200px;
	margin: 0 auto;
	flex-wrap: wrap;
	display: flex;
	min-height: calc(100vh - 210px);
}

/* -------------- 상단 가게 정보 -------------- */
nav {
	width: 100%;
	text-align: center;
	margin: 30px 0 30px 0;
}

nav i {
	font-size: 2rem;
}

nav div {
	font-size: 1.8rem;
}

nav .inf button {
	width: 200px;
	height: 40px;
	margin-top: 20px;
	font-size: 1.8rem;
	background: none;
}

nav .inf .store_review_count:after {
	content: '|';
}

/* -------------- 상단 가게 정보 -------------- */

/* -------------- 장바구니 -------------- */

/* 모바일 카트 */
.m_cart_img {
	position: fixed;
	bottom: 3%;
	right: 3%;
	display: none;
	z-index: 11;
	cursor: pointer;
}

.m_cart_img_box {
	width: 70px;
	height: 70px;
	border-radius: 50%;
	background: #2AC1BC;
	box-shadow: 0px 2px 3px 0px rgb(0 0 0/ 25%);
	display: flex;
	justify-content: center;
	align-items: center;
}

.fa-shopping-cart {
	color: #fff;
	font-size: 20px;
}

.m_cart_count {
	width: 15px;
	height: 15px;
	border-radius: 50%;
	position: absolute;
	top: 20%;
	right: 20%;
	font-size: 13px;
	text-align: center;
	color: #2AC1BC;
	background: #fff;
	border: 2px solid #2AC1BC;;
	vertical-align: middle;
	font-weight: 1000;
	display: none;
}
/* 모바일 카트 */

/* 카트 담았을때 메세지 */
.alarm {
	position: fixed;
	left: 50%;
	top: 50%;
	width: 30vw;
	height: 15vh;
	background: rgba(0, 0, 0, 0.7);
	color: #fff;
	text-align: center;
	transform: translate(-50%, -50%);
	border-radius: 15px;
	line-height: 15vh;
	z-index: 1000;
	min-width: 200px;
	display: none;
}

#cart {
	width: 28%;
	order: 1;
}

#cart .cart {
	position: sticky;
	top: 10%;
	font-size: 1.8rem;
	margin: 5%;
	border: 1px solid #ddd;
}

#cart .cart h2 {
	background-color: #333333;
	color: #fff;
	text-align: center;
}

#cart .cart .deleteAll {
	font-size: 2rem;
	cursor: pointer;
	color: white;
	position: absolute;
	right: 10px;
	top: 5px;
}

#cart .cart .cart_list {
	max-height: 400px;
	overflow: auto;
}

#cart .cart ul {
	width: 90%;
	margin: 0 auto;
}

#cart .cart li {
	border-bottom: 1px solid #ddd;
	padding: 10px 0 10px 0;
	position: relative;
}

#cart .cart li .cart_menu_option {
	color: #999;
}

#cart .cart button.cancle_btn {
	font-size: 1.8rem;
	width: 30px;
	height: 30px;
	position: absolute;
	top: 10px;
	right: 10px;
	padding-bottom: 30px;
	border: none;
	background: none;
	cursor: pointer;
}

#cart .cart button.order_btn {
	width: 100%;
	height: 30px;
	margin-top: 20px;
	font-size: 1.8rem;
	border: none;
	cursor: pointer;
	/* background: #30DAD9; */
	background: #ddd;
	color: #fff;
	border-radius: 0;
}

#cart .cart .total {
	text-align: center;
	margin-top: 10px;
}

/* -------------- 장바구니 -------------- */

/* -------------- 메인 -------------- */
main {
	width: 70%;
}

main ul.tab {
	display: flex;
	background: #fff;
}

main ul.tab li {
	width: 100%;
	padding: 5px 0;
	border: 1px solid #ddd;
	text-align: center;
	cursor: pointer;
	font-size: 2rem;
}

main ul.tab li:hover {
	background: #30DAD9;
	color: #fff;
	transition: 0.1s;
}

main ul.tab li.select {
	background: #30DAD9;
	color: #fff;
	/* 메뉴탭 클릭시 색 바뀜 */
}

/* 메뉴 정보 리뷰 탭 */
main ul.menu {
	padding-bottom: 50px;
	border: 2px solid #ddd;
	margin-bottom: 30px;
}

/* 메뉴 내용 */
main ul.menu li {
	border-bottom: 1px solid #ddd;
}

main ul li .menu_delete_label {
	margin-left: 15px;
	cursor: pointer;
	color: #ddd;
	/* #30DAD9 */
}

main ul li .menu_delete_checkbox {
	display: none;
}

main ul.menu li .menu_box {
	display: flex;
	justify-content: space-between;
	padding: 15px;
	cursor: pointer;
}

main ul.menu li .menu_box img {
	width: 150px;
	height: 150px;
    object-fit: cover;
}

/* -------------- 메인 -------------- */

/* ----------------- 정보 탭 ----------------- */
main ul.info {
	/* display: none; */
	/* 카카오 지도 깨져서 스크립트로 hide함 */
	
}

.info li {
	width: 100%;
	margin: 15px auto;
}

#map_box {
	position: relative;
	width: 100%;
	height: 400px;
	margin-bottom: 10px;
}

#map_box #map {
	width: 100%;
	height: 100%;
}

#map_box #position_box {
	position: absolute;
	bottom: 10px;
	right: 10px;
	z-index: 1;
}

#map_box #position_box button {
	border-radius: 50px;
	border: 1px solid #999999;
	background: #fff;
	padding: 5px 10px;
	font-size: 1.5rem;
}

#map_box #position_box .userPosition {
	display: none;
}

.info li>div {
	font-size: 1.8rem;
	border-radius: 10px;
	border: 1px solid #ddd;
	padding: 15px;
	min-height: 150px;
}

.info h2 {
	padding-bottom: 10px;
}

.info div {
	font-size: 1.6rem;
}

.info li .info_detail_title {
	display: inline-block;
	width: 200px;
}

.info li .info_detail {
	display: inline-block;
}

/* ----------------- 정보 탭 ----------------- */

/* ----------------- 리뷰 탭 ----------------- */
main .comment {
	display: none;
}

main .comment .score_info {
	border: 1px solid #ddd;
	border-radius: 10px;
	padding: 15px;
	display: flex;
	justify-content: space-around;
}

main .comment .score_info .score {
	font-size: 4.5rem;
	font-weight: bold;
	text-align: center;
}

main .comment .score_info i {
	font-size: 1.4rem;
}

main .comment .score_count {
	font-size: 1.4rem;
}

main .comment .score_count>div {
	display: flex;
	justify-content: space-between;
	width: 150px;
}

main .comment .score_info .score_count>div div:first-child {
	font-weight: bold;
}

main .comment .score_count .graph_box {
	position: relative;
	width: 100px;
}

main .comment .score_count .graph_box div {
	width: 100%;
	height: 5px;
	background: #ddd;
	border-radius: 5px;
	position: absolute;
	top: 40%;
}

main .comment .score_info .score_count .review_count {
	color: #999999;
	text-align: left;
	width: 0px;
}

main .comment .boss_comment {
	margin-bottom: 10px;
}

main .comment li {
	border-radius: 10px;
	width: 100%;
	margin: 15px auto 30px;
	font-size: 1.8rem;
}

main .comment li .nickname {
	font-weight: bold;
}

main .comment li .client {
	border: 1px solid #ddd;
	border-radius: 10px;
	padding: 15px;
}

.review_header {
	display: flex;
	justify-content: space-between;
	margin-bottom: 15px;
}

.review_header div div:last-child {
	font-size: 1.4rem;
}

main .comment li .boss_comment_box {
	background: #ddd;
	width: 90%;
	border-radius: 10px;
	padding: 15px;
	position: relative;
	left: 6%;
	margin-top: 15px;
}

.review_btn, .boss_comment_btn {
	background: #fff;
	border-radius: 5px;
	padding: 5px;
}

.boss_comment .comment_area {
	width: 90%;
	height: 100px;
	padding: 10px;
	border-radius: 5px;
	margin-top: 5px;
}

.boss.input {
	display: none;
}

.review_img {
	width: 30%;
	border-radius: 10px;
	cursor: pointer;
}
/* 리뷰탭 */



/* 메뉴 클릭 모달 */
.food_modal img {
	width: 100%;
}

.food_modal .menu_name {
	text-align: center;
	display: block;
	font-size: 20px;
}

.food_modal .modal_box>div {
	font-size: 1.6rem;
	width: 95%;
	margin: 0 auto;
	display: flex;
	justify-content: space-between;
	padding: 15px 0;
	border-bottom: 1px solid #ddd;
}

.food_modal .modal_box .menu_dec {
	justify-content: center;
	font-size: 1.8rem;
}

.food_modal .modal_box .price {
	font-weight: bold;
	font-size: 2rem;
}

.food_modal .modal_box #option {
	display: block;
}

.food_modal .modal_box #option .fas {
	color: #ddd;
}

.food_modal .amount_box {
	border: 1px solid #ddd;
	display: flex;
	width: 150px;
	border-radius: 40px;
	height: 35px;
	overflow: hidden;
}

.food_modal .amount_box button {
	border: none;
	background: #fff;
	font-size: 2.2rem;
	width: 30%;
}

.food_modal .amount_box #amount {
	text-align: center;
	border: none;
	width: 40%;
}


.food_modal .amount .amount_text {
	font-size: 18px;
	font-weight: bold;
	line-height: 43px;
}

.food_modal .modal_box {
	height: calc(100% - 130px);
}

.food_modal #btn_box {
	height: 90px;
	
}

.food_modal #btn_box>div {
	text-align: center;
	height: 45px;
}

.food_modal .min_delevery {
	font-size: 1.5rem;
	margin-top: 5px;
}

.add_cart {
	background: #30DAD9;
}

.food_modal ul {
	font-size: 1.8rem;
	width: 95%;
	margin-left: auto;
}

.food_modal ul .option_box {
	display: flex;
	justify-content: space-between;
}

.food_modal ul .option_box .menu_option {
	display: none;
}

/*-------메뉴리스트 클릭 모달 ---------*/
@media ( max-width :1023px) {
	#wrap {
		width: 95%;
	}
	#cart {
		display: none;
	}
	.m_cart_img {
		display: block;
	}
	main {
		width: 100%;
	}
	.info li .info_detail_title {
		width: 40%;
	}
	.store_reg_modal .modal_box ul {
		margin-left: 0px;
	}
	.alarm {
		font-size: 2rem;
	}
	
	main ul.menu li .menu_box img {
	 	width: 125px;
		height: 125px;
	}
}

@media ( max-width :767px) {
	html {
		font-size: 60%;
	}
	
	
	#wrap {
		width: 99%;
	}
	nav .inf button {
		width: 100px;
		font-size: 1.6rem;
		border-radius: 10px;
	}
	main ul.tab {
		position: sticky;
		top: 0;
		z-index: 10;
	}
	main ul.menu li .menu_box img {
	 	width: 100px;
		height: 100px;
	}
	#map_box {
		height: 300px;
	}
	main .comment li .boss {
		width: 86%;
	}
	.alarm {
		font-size: 1.8rem;
		height: 10vh;
		line-height: 10vh;
	}
}

```

<div style="text-align:center; font-size:0.8em;">detail.css</div>

</div>
</details>

<br>
<br>

<details>
<summary>modal.css</summary>
<div markdown="1">

```css
/*--------- 모달 공통 ---------*/
.modal {
	width: 560px;
	height: 85vh;
	max-height: 720px;
	position: fixed;
	left: 50%;
	top: 100%;
	z-index: 200;
	margin-left: -275px;
	margin-top: 3%;
	border-radius: 10px;
	overflow-x: hidden;
	overflow: hidden;
	background: #fff;
	box-shadow: 3px 3px 20px 10px rgb(0 0 0/ 25%);
}

#modal_header {
	background: #333333;
	position: relative;
	width: 100%;
	display: block;
}

.closeA {
	background: #333333;
	border: none;
	color: #fff;
	position: absolute;
	cursor: pointer;
	top: 0;
	right: 2%;
	width: 4%;
	height: 100%;
	font-size: 2.4rem;
}

.closeB {
	background: #808080;
}

.modal h1 {
	color: #fff;
	text-align: center;
	display: flex;
	align-items: center;
	justify-content: center;
	height: 40px;
}

#modal_bg {
	background: rgba(0, 0, 0, 0.3);
	width: 100vw;
	height: 100vh;
	position: fixed;
	top: 0;
	left: 0;
	display: none;
	z-index: 10;
}

.modal_box {
	position: relative;
	overflow: auto;
	overflow-x: hidden;
	/* height: 70vh; */
	height: calc(100% - 81px);
}

.modal_box::-webkit-scrollbar {
	display: none; /* Chrome, Safari, Opera*/
}

.modal #btn_box {
	/* 
	position: absolute;
	bottom: 0;
	 */
	width: 100%;
	font-size: 1.8rem;
	border-top: 1px solid #ddd;
	height: 40px;
}

.modal .btn_box input, .modal #btn_box button {
	height: 40px;
	font-size: 2rem;
	width: 50%;
	float: left;
	border: none;
	color: white;
	border-radius: unset;
}

.modal .btn_box input[type="submit"] {
	background: #30DAD9;
}



/* input file */

.modal .img_box {
	width: 80%;
	margin: 0 auto 50px;
}

.modal .img {
	display: none; 
}

.modal .img_box label {
	display: inline-block;
	width: 80px;
	height: 60px;
	border: 1px solid #ddd;
	border-radius: 10px;
	text-align: center;
	line-height: 60px;
}

.modal .img_box div {
	margin-top: 20px;
	position: relative;
	display: none;
	width: 150px;
	height: 150px;
}

.modal .img_box img {
	width: 100%;
	height: 100%;
	border-radius: 10px;
	object-fit: cover;
	border: 1px solid #ddd;
}

.modal .img_close {
	background: #ccc;
	position: absolute;
	top: -10px;
	right: -10px;
	width: 25px;
	height: 25px;
	color: #fff;
	border-radius: 50%;
	border: none;
}






@media ( max-width :1023px) {
}

@media ( max-width :767px) {
	.modal {
		width: 80%;
		margin-left: -40%;
	}
	.closeA {
		display: none;
	}
	.modal {
		width: 100%;
		margin: 0;
		/* margin-top: 1vh; */
		left: 0;
		height: 100%;
	}
	.modal .btn_box input, .modal .btn_box button {
		height: 45px;
	}
}

@media ( max-width :480px) {
}

```

<div style="text-align:center; font-size:0.8em;">modal.css</div>

</div>
</details>

<br>
<br>

<details>
<summary>storeDetail.js</summary>
<div markdown="1">

```javascript


$(document).ready(function() {
	const minDelevery = Number($("#min_delevery").data("min_delevery"));
	const deleveryTip = Number($("#delevery_tip").data("delevery_tip"));
	const storeId = $("#store_id").val();
	const storeName = $("#store_name").data("store_name");
	
	const cart = (function(){
		// 장바구니에 담긴 가게번호 (다른가게에서 담은 상품인지 확인) 
		let cartStoreId = null;
		const getCartStoreId = function(){
			return cartStoreId;
		}
		const setCartStoreId = function(set){
			cartStoreId = set;
		}
		// 장바구니에 담긴 상품 수
		let cartSize = 0;
		
		const getCartSize = function(){
			return cartSize;
		}
		
		const setCartSize = function(set){
			cartSize = set;
		}
		
		
		// 장바구니에 담은 메뉴가격 총합
		let menuTotalPrice = 0;
		
		const getMenuTotalPrice = function(){
			return menuTotalPrice;
		}
		
		const setMenuTotalPrice = function(set){
			menuTotalPrice = set;
		}

	
		
		return {
			getCartStoreId : getCartStoreId, 
			setCartStoreId : setCartStoreId,
			getCartSize : getCartSize,
			setCartSize : setCartSize,
			getMenuTotalPrice : getMenuTotalPrice,
			setMenuTotalPrice : setMenuTotalPrice,
			};
	})();
	
	
	
	
	// 가게 입장시 카트리스트 불러오기
	/*(function(){
		$.ajax({
			url: "/cartList",
			type: "get"
		})
		.done(function(result){
			if(result == "" ) {
				cartReset();
				return;
			}
			cartList(result);
		})
		.fail(function(){
			swal("장바구니 정보 에러");
		})
	})();*/
	
	



	// 찜하기
	$(".inf i").click(function(){
		let likes ="";
		
		if($(this).hasClass("far")) {
			$(this).removeClass("far").addClass("fas");
			likes = "on";
		} else {
			$(this).removeClass("fas").addClass("far");
			likes = "off";
		}
		
		const data = {
			id : $("#store_id").val(),
			likes : likes
		}
		$.ajax({
			url: "/store/likes",
			type: "POST",
			data: data
		})
		.done(function(result){
			if(result == 0) {
			} else {
				
				let likesCount = $(".likes_count").data("count");
				
				if(likes == "on") {
					$(".likes_count").text(likesCount+1);
					$(".likes_count").data("count", likesCount+1 );
				} else {
					$(".likes_count").text(likesCount-1);
					$(".likes_count").data("count", likesCount-1 );
				}
			}
		})
	}) // 찜





	// 메뉴 클릭시 모달창 
	$(".menu > li .menu_box").click(function() {
		
		const isAdmin = $("#admin_button_box").data("is_admin");
		if(isAdmin) {
			return;
		}
		/*
		const isOpen = $("#is_open").data("is_open");
		if(!isOpen) {
			swal("지금은 준비중이에요");
			return;
		}
		*/
		const foodId = $(this).find(".food_id").val();
		$.ajax({
			url: "/foodOption",
			type: "get",
			data: {foodId : foodId}
		})
		.done(function(result){
			foodModalHtml(result);
				
			if(result.length == 0) {
					$("#option").hide();
				} else {
					$("#option").show();
				}
		})
		.fail(function(){
			swal("에러가 발생했습니다");
			food.hide();
		}) // ajax
		
		
		const addCartModal = $(".food_modal");
		const foodImg = $(this).find(".food_img").val();
		const foodName = $(this).find(".food_name").val();
		let foodPrice = Number($(this).find(".food_price").val());
		const foodDec = $(this).find(".food_dec").val();
		const amount = $("#amount").val();
		const totalPrice = amount * foodPrice;

		$(".menu_img").attr("src", foodImg);
		$(".menu_name").text(foodName);
		$(".menu_dec").text(foodDec);
		$(".price").text(Number(foodPrice).toLocaleString() + '원');
		$(".total_price").text(Number(totalPrice).toLocaleString() + '원');
		
		$(".add_cart_food_name").val(foodName);
		$(".add_cart_food_price").val(foodPrice);
		$(".add_cart_food_id").val(foodId);

		openModal(addCartModal);
		




		// 수량 증가 감소
		$(".minus").off().click(function() {
			if (1 < Number($("#amount").val())) {
				$("#amount").val(Number($("#amount").val()) - 1);
			}
			const amount = Number($("#amount").val());
			const totalPrice = amount * foodPrice;
			$(".total_price").text(Number(totalPrice).toLocaleString() + '원');
		})
		
		$(".plus").off().click(function() {
			$("#amount").val(Number($("#amount").val()) + 1);
			const amount = $("#amount").val();
			const totalPrice = amount * foodPrice;
			$(".total_price").text(Number(totalPrice).toLocaleString() + '원');
		})
		
		
		// 옵션 체크박스 변경
		$(document).off().on("click" , ".option_box i", function(){
			const optionPrice = Number($(this).siblings(".option_price").val());
			
			if($(this).siblings(".menu_option").is(":checked")) {
				$(this).siblings(".menu_option").prop("checked" ,false);
				$(this).css("color" , "#ddd");
				
				foodPrice -= optionPrice; 
				
			} else {
				$(this).siblings(".menu_option").prop("checked" , true);
				$(this).css("color" , "#30DAD9");
				
				foodPrice += optionPrice; 
			}
			
			const amount = Number($("#amount").val());
			const totalPrice = amount * foodPrice;
			$(".total_price").text(Number(totalPrice).toLocaleString() + '원');
		})
		
	}) // 메뉴 클릭
	
	
	
	
	
	
	// 장바구니 담기
	$(".add_cart").click(function(){
		const cartStoreId = cart.getCartStoreId();
		if(cartStoreId != null && storeId != cartStoreId ) {
			swal({
				buttons: ["취소", "담기"],
				title: "장바구니에는 같은 가게의 메뉴만 담을 수 있습니다",
				text: "선택하신 메뉴를 장바구니에 담을 경우 이전에 담은 메뉴가 삭제됩니다"
			})
			.then((value) => {
				if (value == true) {
					deleteCartAll();
					addCart($(this));
				}
			});			
		} else {
			addCart($(this));
		}
	}) // 장바구니 담기
	
	
	
	
	// 장바구니 전부 삭제
	$("#cart i").click(function(){
		deleteCartAll();
	})	

	// 리뷰탭 그래프
	const reviewCount = $(".store_review_count").data("review_count");
	
	if(reviewCount != 0) {
		for(var i=1;i<=5;i++) {
			const target = $(".graph.score"+i)
			const score = target.data("score"+i) / reviewCount * 100;
			target.css("background","gold").css("width", score+"%");
		}
	}
	
	
	
	function foodModalHtml(result) {
		let html = "";
			
		for(var i=0;i<result.length;i++) {
			html += `<li>
                <div class="option_box">
                	<span>
            			<i class="fas fa-check-square"></i>
     	 				<input type="checkbox" class="menu_option" name="option" value="${result[i]["optionName"] }"> ${result[i]["optionName"] }
        				<input type="hidden" class="option_price" value="${result[i]["optionPrice"] }">
            			<input type="hidden" class="option_id" value="${result[i]["id"] }">
         	 		</span>
        			<span>${result[i]["optionPrice"] } 원</span>
            	</div>
          	</li>`;
		}
			
		$("#option ul").html(html);
	}
	
	
	function addCart(addCart){
		// 선택한 추가옵션 배열에 저장
		const foodOptionName = [];
		const foodOptionPrice = [];
		const foodOptionId = [];
			
		// 선택된 추가옵션 가져오기 
		$("input[name='option']:checked").each(function() {
			const optionName = $(this).val();
			const optionId = $(this).siblings(".option_id").val();
			const optionPrice = $(this).siblings(".option_price").val();  
			
			foodOptionName.push(optionName);
			foodOptionId.push(optionId);
			foodOptionPrice.push(optionPrice);
		})
		
		const data = {
			foodId : addCart.siblings(".add_cart_food_id").val(),
			foodName : addCart.siblings(".add_cart_food_name").val(),
			foodPrice : addCart.siblings(".add_cart_food_price").val(),
			amount : addCart.parent().siblings(".modal_box").find("#amount").val(),
			optionName : foodOptionName,
			optionId : foodOptionId,
			optionPrice : foodOptionPrice,
			deleveryTip : deleveryTip,
			storeId : storeId, 
			storeName : storeName
		}
		
		$.ajax({
			url: "/addCart",
			type: "post",
			data : data,
			traditional : true
		})
		.done(function(result){
			cartList(result);
			
			alarm();	
			closeModal();
			$("#amount").val(1);
			
			// 밖에 있으니 작동이 안되서 추가
			$(document).on("click", ".cancle_btn", function() {
				const index = $(this).parent().index();
				deleteCartOne(index);
			}); // 장바구니 1개 삭제
			
		}) // done
		.fail(function(){
			swal("에러가 발생했습니다");
		}) // ajax
	} // addCart
	
	
	
	function alarm(text) {
		$(".alarm").text(text);
		
		$(".alarm").show();
		setTimeout(function(){
			$(".alarm").hide();
		},1000);
	}
			
	
	// 장바구니 1개 삭제
	$(document).on("click", ".cancle_btn", function() {
		const index = $(this).parent().index();
		deleteCartOne(index);
	}); 
	
	
	// 장바구니 1개 삭제
	function deleteCartOne(index){
		$.ajax({
			url: "/cartOne",
			type: "DELETE",
			data: {index : index}
		})
		.done(function(result){
			if(result == "") {
				cartReset();
				return;
			}
			cartList(result);
			$(".m_cart_count").css("display" , "block");
			$(".m_cart_count").text(result.cart.length);
		})
		.fail(function(){
			swal("에러가 발생했습니다");
		})
	}
	

	function deleteCartAll(){
		$.ajax({
			url: "/cartAll",
			type: "DELETE"
		})
		.done(function(){
			cartReset();
		})
		.fail(function(){
			swal("에러가 발생했습니다");
		})
	}


	function cartList(result){
		const cartList = result.cart;
		const storeId = result.storeId;
		const storeName = result.storeName;
		const cartTotal = result.cartTotal;
		
		cart.setCartSize(cart.length);
		
		let html = "";
			
		for(var i=0;i<cartList.length;i++) {
			let optionHtml = "";
			if(cartList[i].optionName != null ) {
				for(var j=0;j<cartList[i].optionName.length;j++) {
					const optionName = cartList[i].optionName[j];
					const optionPrice = Number(cartList[i].optionPrice[j]).toLocaleString();
					
					optionHtml += `<div class="cart_menu_option"> ${optionName }  ${optionPrice }원</div>`;
				}
			}
			
			html += `<li> 
						<h3>${cartList[i].foodName  }</h3>
						<div>${cartList[i].foodPrice.toLocaleString()}원</div>
						<div>수량 : ${cartList[i].amount }</div>
						<div>${optionHtml} </div>
						<div>합계 : ${cartList[i].totalPrice.toLocaleString() }원</div>
						<button class="cancle_btn"> ${"ｘ"} </button>
					</li>`; 
					 // 장바구니 추가하면 장바구니 리스트 변경
			
			
		}
		cart.setMenuTotalPrice(cartTotal);
		cart.setCartStoreId(storeId );
		
		$(".cart ul").html(html);
		$(".total").html("총 합계 : " + cartTotal.toLocaleString() + "원");
		$(".m_cart_count").css("display" , "block");
		$(".m_cart_count").text(cartList.length);
		console.log(cartList);
		console.log("장바구니 가게 id " + storeId);
		
		minDeleveryCheck();
	}	



	// 주문금액이 최소주문금액 이상이어야 주문가능
	function minDeleveryCheck() {
		const menuTotalPrice = cart.getMenuTotalPrice();
		
		if(minDelevery <= menuTotalPrice) {
			$(".order_btn").attr("disabled", false); 
			$(".order_btn").css("background", "#30DAD9");
			$(".order_btn").text("주문하기");
		} else {
			$(".order_btn").css("background", "#ddd");
			$(".order_btn").attr("disabled", true); 
			$(".order_btn").text(minDelevery + "원 이상 주문할 수 있습니다");
		}
	}


	// css로 display none시 카카오 맵 깨짐
	$("main ul.info").hide();
	// 탭 눌렀을때 색변경 콘텐츠 변경
	$("ul.tab > li").click(function() {

		const index = $(this).index() + 1;

		$("ul.tab > li").removeClass("select");
		$(this).addClass("select");

		$("main  ul").eq(1).hide();
		$("main  ul").eq(2).hide();
		$("main  ul").eq(3).hide();
		$("main  ul").eq(index).show();

		const offset = $(".offset").offset();
		const scrollPosition = $(document).scrollTop();

		if (offset["top"] < scrollPosition) {
			$("html").animate({ scrollTop: offset.top }, 100);
		}

	});





	function cartReset() {
		$(".cart ul").html("");
		$(".total").html("장바구니가 비었습니다.");
		$(".order_btn").css("background", "#ddd");
		$(".order_btn").attr("disabled", true); 
		$(".order_btn").text("주문하기");
		$(".m_cart_count").css("display" , "none");
		$(".m_cart_count").text("");
		
		cart.setCartSize(0);
		cart.setMenuTotalPrice(0);
	};
	
	
	
	
	// 주문하기
	$(".order_btn").click(function() {
		location.href = "/order";
	});
	// 모바일 주문하기
	$(".m_cart_img_box").click(function() {
		
		if(cart.getCartSize() == 0 ){
			alarm("메뉴를 추가해주세요");
			return;
		}
		location.href = "/order";
	});
	
	



});

```

<div style="text-align:center; font-size:0.8em;">storeDetail.js</div>

</div>
</details>

<br>
<br>

<details>
<summary>nav.html</summary>
<div markdown="1">

```html

```

<div style="text-align:center; font-size:0.8em;">메인 화면</div>

</div>
</details>


<br>
<br>

<details>
<summary>modal.html</summary>
<div markdown="1">

```html

<div id="modal_bg"></div>

<div class="food_modal modal">

  <div id="modal_header">
    <button type="button" class="closeA"><i class="fas fa-times"></i></button>
    <h1>메뉴 상세</h1>
  </div>

  <div class="modal_box" >

    <img src="" alt="이미지" class="menu_img" >
    <h2 class="menu_name">메뉴 이름</h2>
    <div class="menu_dec"></div>
    <div class="price"><span>가격</span><span class="menu_price" >0</span></div>

    <div id="option">
      <h2>옵션 선택</h2>
      <ul>
        <li>
          <div class="option_box">
		                	<span>
	                			<i class="fas fa-check-square"></i>
               	 				
             	 				<input type="checkbox" class="menu_option" name="option" value="123">123
                				<input type="hidden" class="option_price" value="">
    	            			<input type="hidden" class="option_num" value="">
                 	 		</span>
            <span>0원</span>
          </div>
        </li>
        <li>
          <div class="option_box">
		                	<span>
	                			<i class="fas fa-check-square"></i>
             	 				<input type="checkbox" class="menu_option" name="option" value="123">123
                				<input type="hidden" class="option_price" value="">
    	            			<input type="hidden" class="option_id" value="">
                 	 		</span>
            <span>0원</span>
          </div>
        </li>


      </ul>
    </div>

    <div class="amount">
      <span class="amount_text">수량</span>

      <span class="amount_box">
                    <button class="minus">-</button>
                    <input type="number" id="amount" min="1" value="1" readonly >
                    <button class="plus">+</button>
                </span>

    </div>
  </div>

  <div id="btn_box">

    <input type="hidden" class="add_cart_food_name" >
    <input type="hidden" class="add_cart_food_price" >
    <input type="hidden" class="add_cart_food_id" >
    <div>
      <div class="min_delevery"><span th:text="'배달최소주문금액'+${#numbers.formatInteger(store.storeInfo.minDelevery,0,'COMMA')}+'원'"></span></div>
      <div class="sum"><span>총 주문금액</span><span class="total_price">0</span></div>
    </div>

    <button class=closeB type="button">취소</button>
    <button class="add_cart" type="button">장바구니에 담기 </button>
  </div>
</div>

```

<div style="text-align:center; font-size:0.8em;">modal.html</div>

</div>
</details>

<br>
<br>

<details>
<summary>detail.html</summary>
<div markdown="1">

```html

<th:block th:insert="include/link.html"></th:block>

<link rel="stylesheet" href="/css/modal.css">
<link rel="stylesheet" href="/css/store/detail.css">


<th:block th:insert="include/header.html"></th:block>


<!-- 메인 -->
<th:block th:insert="store/storeDetail.html"></th:block>
<!-- 메인 -->

<!-- 푸터 -->
<th:block th:insert="include/footer.html"></th:block>
<!-- 푸터 -->

<!-- 메뉴 모달 -->
<th:block th:insert="modal/modal_food.html"></th:block>
<!-- 메뉴 모달 -->




<script type="text/javascript" src="/js/store/storeDetail.js"></script>
</body>
</html>
```

<div style="text-align:center; font-size:0.8em;">detail.html</div>

</div>
</details>

<br>
<br>

<details>
<summary>storeDetail.html</summary>
<div markdown="1">

```html

<div id="wrap" th:with="info=${store.storeInfo}">
    <nav>
        <h1 id="store_name" data-store_name="${info.storeName }" th:text="${info.storeName }"></h1>
        <!-- <div id="is_open" data-is_open="${store.storeInfo.isOpen }"></div> -->
        <div class="inf">
            <div>
                <!--
                <span class="score_box">
             		<th:block th:each="i: ${#numbers.sequence(0, 4)}">
             			<i th:class="${#numbers.round(info.score) > i ? 'far fas fa-star' : 'far fa-star'}"></i>
             		</th:block>

                  	<span class="store_score" data-score="${info.score}" th:text="${info.score}"></span>
				</span><br>
                -->

                <span><i class="fas fa-heart" ></i> 찜 </span>


                <span class="likes_count" data-count=0 >0</span>

            </div>
            <div>
                <span class="store_review_count" data-review_count="0"> 리뷰 0</span>
                <span>사장님 댓글 0</span>
            </div>

            <div id="min_delevery" data-min_delevery="${info.minDelevery }"><span th:text="'최소주문금액'+${#numbers.formatInteger(info.minDelevery,0,'COMMA')}+'원'"></span></div>
            <div><span th:text="'예상 배달시간 '+${info.deleveryTime  }+'분'"></span></div>
            <div id="delevery_tip" data-delevery_tip="${info.deleveryTip }"><span th:text="'배달팁'+${#numbers.formatInteger(info.deleveryTip,0,'COMMA')}+'원'"></span></div>
        </div>
    </nav>


    <!-- 모바일 카트 -->
    <div class="m_cart_img">
        <div class="m_cart_img_box">
            <i class="fas fa-shopping-cart"></i>
            <span class="m_cart_count"></span>
        </div>
    </div>
    <!-- 모바일 카트 -->



    <!-- 장바구니 -->
    <aside id="cart">
        <div class="cart">
            <h2>장바구니</h2>
            <i class="far fa-trash-alt deleteAll" ></i>

            <div class="cart_list">
                <ul>
                    <!--
                    <li>
                        <h3>메뉴</h3>
                              <div>가격</div>
                              <div>수량 : 0 </div>
                              <div> 옵션  </div>
                              <div>합계 : 0원</div>
                              <button class="cancle_btn"> ｘ </button>
                    </li>
                              -->
                </ul>
            </div>

            <div class="order_btn_box">
                <div class="total">장바구니가 비었습니다.</div>
                <button class="order_btn" disabled>주문하기</button>
            </div>
        </div>

    </aside>
    <div class="alarm">장바구니에 담았습니다</div>
    <!-- 장바구니 -->


    <main>
        <div class="offset"></div>
        <ul class="tab ">
            <li class="select">메뉴</li>
            <li>정보</li>
            <li>리뷰</li>
        </ul>


        <!-- 메뉴 탭 -->
        <ul class="menu">



        </ul>
        <!-- 메뉴 탭 -->


        <!-- 정보 탭 -->
        <ul class="info" >




        </ul>
        <!-- 메뉴 탭 -->



        <!-- 리뷰 탭 -->
        <ul class="comment" >




        </ul>
    </main>
</div>


<input type="hidden" value="${info.id }" id="store_id">
<input type="hidden" value="${info.category }" id="store_category">
<input type="hidden" value="${info.openingTime }" id="store_opening_time">
<input type="hidden" value="${info.closingTime }" id="store_closing_time">

<input type="hidden" value="${BMaddress.address2 }" id="delevery_address">
```

<div style="text-align:center; font-size:0.8em;">storeDetail.html</div>

<br>

#numbers.formatInteger로 숫자를 형식화 해줬고 윗부분 주석부분을 보면 조건에따라 class가 바뀌어야하므로 **th:class**를 사용해줬다. 

> ~~기존 코드는~~

```
  <div id="min_delevery" data-min_delevery="${info.minDelevery }"><span th:text="'최소주문금액'+${#numbers.formatInteger(info.minDelevery,0,'COMMA')}+'원'"></span></div>
            <div><span th:text="'예상 배달시간 '+${info.deleveryTime  }+'분'"></span></div>
            <div id="delevery_tip" data-delevery_tip="${info.deleveryTip }"><span th:text="'배달팁'+${#numbers.formatInteger(info.deleveryTip,0,'COMMA')}+'원'"></span></div>
```

<div style="text-align:center; font-size:0.8em;">기존코드</div>

<br>

~~\${info.deleveryTime}인데, info값을 전달해주고있지 않아서 에러가난다.
\${store.storeInfo.deleveryTime}으로 바꿔준다~~

<br>

```
  <div id="min_delevery" data-min_delevery="${info.minDelevery }"><span th:text="'최소주문금액'+${#numbers.formatInteger(store.storeInfo?.minDelevery,0,'COMMA')}+'원'"></span></div>
            <div><span th:text="'예상 배달시간 '+${store.storeInfo?.deleveryTime  }+'분'"></span></div>
            <div id="delevery_tip" data-delevery_tip="${info.deleveryTip }"><span th:text="'배달팁'+${#numbers.formatInteger(store.storeInfo?.deleveryTip,0,'COMMA')}+'원'"></span></div>
```

<div style="text-align:center; font-size:0.8em;">수정코드</div>

<br>

<br>

> **수정**<br> 내가 착각했었다. 위 방식으로 해도 되지만, 코드의 제일 윗부분을 보면 **th:with**를 사용해서 info변수를 정의해주고 있다. 다만 저 변수는 해당 태그 범위 내에서만 유효하기 때문에 전체를 감싸주는 첫 div태그 안에 선언해준다. 그리고 store.storeInfo부분을 다시 info로 바꿔준다.

<br>

```html
<div id="wrap">
    <nav>
        <th:block th:with="info=${store.storeInfo}"></th:block>
```

<div style="text-align:center; font-size:0.8em;">기존코드</div>


<br>
<br>


```html
<div id="wrap" th:with="info=${store.storeInfo}">
    <nav>
        <h1 id="store_name" data-store_name="${info.storeName }" th:text="${info.storeName }"></h1>
```

<div style="text-align:center; font-size:0.8em;">수정코드</div>


</div>
</details>

<br>
<br>
<br>

![image]({{ site.baseurl }}/images/2023-06-23/20230623_131538.png)

<div style="text-align:center; font-size:0.8em;">상세화면(음식점이름이 이상하다)</div>



<br>
<br>
<br>

![image]({{ site.baseurl }}/images/2023-06-23/20230623_144211.png)

<div style="text-align:center; font-size:0.8em;">상세화면(수정완료)</div>
