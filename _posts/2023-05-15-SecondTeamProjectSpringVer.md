---
layout: single
title: "(Project)DiamondBlack-두번째 팀 프로젝트 Spring ver"
categories:
  - Project
  - Spring
---


### 두번째 팀 프로젝트 프레임 워크 eclipse -> Spring

<br>

**깃허브 repository** : [second project spring .ver](https://github.com/Molban2J/SecondProjectSpringVer.git "github")

<br><br><br>

개요: 기존에 만들었던 의류 쇼핑몰 팀 프로젝트의 프레임 워크를 **spring(sts4)**으로 옮기는 작업입니다. 그리고 이전에 부족했던 부분들을 보완하고 새로 배운 **ajax**를 이용해 기존 javaspcript부분들도 보완, 수정할 계획입니다.

<br><br>
팀원: **임종민(본인)**, 박권능, 이규동, 정자윤

<br><br>

기간: 2023-05-15~

<br><br><br>

## 목차

* [주제선정](#주제선정)
* [개발 환경 및 스케줄](#개발-환경-및-개발-스케줄)
* [팀원 역할](#팀원-역할)
* [DB명세서](#db-명세서)
* [수정사항](#수정-사항)
* [종민 - 구현/오류/수정 사항](#추가-구현)
* [기능 구현 모습](#기능-구현-모습)
	* [회원가입](#회원-가입)
	* [로그인/로그아웃](#로그인--로그아웃)
	* [회원 정보수정 / 탈퇴](#회원-정보수정--탈퇴)
	* [상품 목록](#상품-목록)
	* [장바구니](#장바구니)
	* [결제](#결제)
	* [옥션](#옥션)
	* [쿠폰발급](#쿠폰-발급)
	* [게시글 등록](#게시글-등록)
	* [게시글 댓글](#게시글-댓글)
	* [게시글 수정](#게시글-수정)
	* [내 게시글](#내-게시글)
	* [게시판 검색](#게시판-검색)
	* [(관리자) 회원관리](#관리자-회원관리)
	* [(관리자) 브랜드 관리](#관리자-브랜드-관리)
	* [(관리자) 판매 관리](#관리자-판매-관리)
	* [(관리자) 상품관리](#관리자-상품관리)
	* [구매목록](#구매목록)
	* [로그인이 필요한 기능](#로그인이-필요한-기능)
*	[기능 / 코드리뷰](#코드--기능-리뷰)
* [느낀점과 개선사항](#느낀점과-개선사항)

<br><br><br>

## 주제선정

![_config.yml]({{ site.baseurl }}/images/graph.png)

현재 대한민국의 주 소비층은 MZ 세대로 대부분의 기업들은 이미 MZ세대를 타겟으로 하여 마케팅을 하고 있습니다.
현재 명품시장은 세계적으로 매해 조금씩 성장하고 있는 추세며 2021년 기준으로  세계 성장률 13%, 우리나라는 5% 성장함을 그래프로 볼 수 있습니다.

![_config.yml]({{ site.baseurl }}/images/graph2.png)

다음 그래프를 보시더라도 명품 매출은 매년 증가하고 있으며 그 중의 2030세대(이하MZ)의 비중은 나날이 증가하고 있습니다.
이러한 시장의 흐름에 발 맞추어 사용량이 많은 명품 쇼핑몰을 주제로 JSP 쇼핑몰을 만들었습니다.
<br>
추가로 다른 쇼핑몰과의 차별점을 주고자 좀 더 젊은 세대에게 흥미를 유발하고 접근성을 향상시키기 위해 hotdeal과 auction기능 을 추가했습니다.

---
<br>

## 개발 환경 및 개발 스케줄
<br>

|개발환경|상세|
|:--:|:--|
|언어| - Java<br> - Javascript<br> - SQL<br> - HTML<br> - CSS<br>|
|Front-end|- Bootstrap <br> - jQuery|
|Back-end| - Spring<br> - Mybatis<br> - Apache Tomcat |
|DB| - Oracle|
|Tool| - Sts4|
|Open API| - KakoMap API<br> - CK Editor<br> - i'mport<br> -Disqus


  <br>

**개발 스케줄**
<br>

|날짜|일정|
|:--:|:--|
|2023.04.10 ~ 2023.04.11|- 주제 선정 <br> -구체적인 개발 방향 탐색(페이지 및 기능 구성 등) <br> - 데이터베이스 설계<br> - 역할 분담|
|2023.04.12 ~ 2023.04.14|- 데이터 베이스 구축<br> - 각자 맡은 페이지의 대략적 구조 제작 <br> - 각자 브랜드 정해서 데이터 수집<br> - 페이지의 전체적 테마 설정/ 부트스트랩 지정|
|2023.04.15 ~ 2023.04.19|- 각자 맡은 페이지 상세 제작/ 부트스트랩, CSS 적용<br> - 각 페이지 기능 구현(회원가입, 주문, 장바구니, 핫딜, 옥션 등)<br> - 추가 기능 구상 및 오류 수정<br> - 틈틈히 팀원들 간 코드 병합|
|2023.04.19 ~ 2023.04.21|- 코드 전체 병합<br> -기능 테스트<br> - 디테일 수정 및 확립<br> - 오류 수정 <br> - 발표|
|2023.05.15~| -기능 및 취약점 보완<br> - 디테일 수정<br> -미구현된 기능들 추가|

---
<br>
<br>

## 팀원 역할

|이름|역할|
|:--:|:--|
|**임종민(본인)**| - 관리자 페이지 제작<br> - 브랜드, 상품관리<br> - 회원관리<br> - 옥션 등록<br> - 자유게시판 관리 <br> - 경매 페이지 제작, 기능 구현<br> - 파일 통합 및 배포|
|박권능|- 로그인, 회원가입 페이지 제작, 기능구현<br> - 유효성 검사 스크립트 삽입<br> - KakaoMap API<br> - 결제 API 삽입<br> - 게시판 페이징 처리 <br> - Contact페이지<br>|
|이규동|-핫 딜 제작, 기능구현<br> - 게시판, 상품 페이지 제작<br> -상품(로고, 카테고리, 검색)리스트 구현 <br> -관리자 페이지의 QnA게시판 관리, 매출 관리 구현 <br>- 파일 통합 및 배포<br> - ppt발표 <br>|
|정자윤|-데이터 베이스 제작, 수정, 업데이트<br> - 장바구니 제작, 구현 <br> - ppt작성 <br> - 마이페이지 주문내역 구현|

---

<br>

## DB 명세서

![db명세서 이미지]({{ site.baseurl }}/images/20230706_115246.png)

![db명세서 이미지]({{ site.baseurl }}/images/user.png)

![db명세서 이미지]({{ site.baseurl }}/images/product.png)

![db명세서 이미지]({{ site.baseurl }}/images/cart.png)

![db명세서 이미지]({{ site.baseurl }}/images/qna.png)

![db명세서 이미지]({{ site.baseurl }}/images/free.png)

![db명세서 이미지]({{ site.baseurl }}/images/auction.png)

![db명세서 이미지]({{ site.baseurl }}/images/20230706_115348.png)

---

### **수정 사항**
- 상품 페이지
    - 상품 사진 여러장 첨부	
    - 상품상세보기 페이지
    - 상품페이지 페이징처리 ----- 구현완료

- 어드민 관리자 페이지 수정      
    - 브랜드 등록시 상품등록 페이지에도 반영	----- 구현완료
    - 회원관리 페이지, 브랜드 관리 페이지 디자인 수정  ----- 구현완료

- 경매 타임세일 수정
    - 경매
      - 입찰자에게만 구매 버튼이 보이도록 설정 ----- 구현완료
      - 구매시 구매버튼 사라지게 ----- 구현완료
      - 실시간 경매 참여자 볼 수 있게
      - 실시간 입찰가 업데이트 되도록 설정 ----- 구현완료
    - 타임세일 
      - 관리자가 할인율과 시간을 정할수있게 ----- 구현완료
      - ajax로 구현


- 브랜드 추가시 리스트에 추가
- (카테고리, 메인로고) 이에 따른 상품 등록시 상품 브랜드에 자동추가  ----- 구현완료

- 장바구니 뱃지  ----- 구현완료

- 장바구니 삭제버튼 & 수량 변동 ----- 구현완료

- 상품 구매시 가격에 따른 회원포인트 지급 ----- 구현완료
- 누적 포인트에 따른 회원등급 조정 ----- 구현완료
- 회원등급에 따른 할인쿠폰 지급 ----- 구현완료
- 할인쿠폰 적용 ----- 구현완료
- user table 과 coupon table 연결 ----- 구현완료
- coupon table 생성 ----- 구현완료
- 쿠폰명 , 유저아이디, 갯수, 할인율(int 로 예)1 = 5% 2= 10% 3= 15% 4= 20%) ----- 구현완료

- 쿠폰받는 페이지 생성 ----- 구현완료
- 받은 쿠폰은 다시받을 수 없음 ----- 구현완료

- 구매 취소 및 반품 ----- 구현완료
- 마우스 커서 디자인 적용 ----- 구현완료

- 자유게시판 대댓글 기능  ----- 구현완료

---

5/15

**종민 - 구현/오류/수정 사항**

### 추가 구현

- 옥션
  - 실시간 가격 변동 구현(Ajax)
  - 입찰자에게만 구매버튼 보이도록 구현
  - 구매가 완료되면 '이미 구매한 상품입니다'라고 띄우고 버튼 disabled
  - 구매시 바로 구매페이지로 이동
- 회원관리
  - 회원 검색기능 추가
  - 회원 리스트 페이징 구현
- 브랜드 등록
  - Spring framework에서의 파일 업로드 구현
  - 브랜드 이미지 파일 등록할 때 미리보기 구현(수정 완료)
  - 브랜드 이미지 파일 등록 버튼이 두개인데, 한쪽에서 이미지 파일을 선택하면 다른쪽에서도 동일한 값을 집어넣도록 구현

	<br>

### 수정

- 옥션

- 회원 관리
  - 회원관리에서 회원 리스트 창 구현(수정 완료)
  - 회원 관리 ux/ui 개선

- 브랜드 등록
  - 브랜드 삭제 창 ux/ui 개선
  - 브랜드 관리창 ux/ui 개선

### 오류

- 옥션
  - 매핑을 할때 datetime-local형과 timestamp형 변환 문제로 옥션 등록까지는 구현 안됨(수정 완료)
  - AuctionVO에서 \@data 어노테이션을 쓰면 pName을 인식 못하는 오류 발생
  - ProductController의 auctionPurchased 매핑에서 pname과 endPrice를 인식하지못해 바인딩 실패오류가 남(수정 완료)
- 회원 관리
  - open_window로 새로운 창 띄워서 userEdit창 매핑 인식 오류(수정 완료)
  - 삭제 매핑 오류(수정 완료)

- 브랜드 등록
  - 브랜드 등록시 이미지 파일이 액박이 뜸. 서버를 종료하고 다시 실행하면 보임

---

## 기능 구현 모습

<br>
<br>

### 회원 가입

<iframe width="560" height="315" src="https://www.youtube.com/embed/fM50eE31070" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

- E-mail sender를 통해 이메일 인증을 받아야 회원가입 가능

<br><br>

### 로그인 / 로그아웃

<iframe width="560" height="315" src="https://www.youtube.com/embed/0S9vZJzjBls" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

<br><br>

### 회원 정보수정 / 탈퇴

<iframe width="560" height="315" src="https://www.youtube.com/embed/TrFCOeXVxmk" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

- 비밀번호 재 확인 후 유저 정보 변경가능

<br><br>

### 상품 목록

<iframe width="560" height="315" src="https://www.youtube.com/embed/YI7knADA_yY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

<br><br>

### 장바구니

<iframe width="560" height="315" src="https://www.youtube.com/embed/DyjE964-CpA" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

- 수량이 1 밑으로 내려가면 알림창이 뜨면서 삭제 되도록 설정

<br><br>

### 결제

<iframe width="560" height="315" src="https://www.youtube.com/embed/1wpDJ4CL9ls" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

- 아임포트 스크립트를 가져와 사용. 실제 결제는 안됨. 취소를 누르면 결제 완료 창으로 이동.

<br><br>

### 옥션

<iframe width="560" height="315" src="https://www.youtube.com/embed/BemOwc-uNa4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

-마지막 입찰한 사람이 낙찰자가 되는 시스템. 현재는 현재 입찰가보다 낮은 가격으로는 입찰하지 못하도록 스크립트 설정을 해둠.

<br><br>

### 쿠폰 발급

<iframe width="560" height="315" src="https://www.youtube.com/embed/Si61CZ2l4ec" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

- 등급에 따른 쿠폰 발급. 이미 발급받거나 등급이 맞지 않으면 발급이 제한됨.

<br><br>

### 게시글 등록

<iframe width="560" height="315" src="https://www.youtube.com/embed/mg-1-IdUmzE" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

<br><br>

### 게시글 댓글

<iframe width="560" height="315" src="https://www.youtube.com/embed/zH6nbtQrMZY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

<br><br>

### 게시글 수정

<iframe width="560" height="315" src="https://www.youtube.com/embed/yQFa3W8dONA" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

<br><br>

### 내 게시글

<iframe width="560" height="315" src="https://www.youtube.com/embed/ULISfiHhWfo" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

<br><br>

### 게시판 검색

<iframe width="560" height="315" src="https://www.youtube.com/embed/ddMjSoJ92GE" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

<br><br>

### 관리자: 회원관리

<iframe width="560" height="315" src="https://www.youtube.com/embed/56PG2QZNfTE" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

<br><br>

### 관리자: 브랜드 관리

<iframe width="560" height="315" src="https://www.youtube.com/embed/0MrN70L04RU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

- 브랜드 등록을 하면 이미지가 제데로 보이지 않는 문제가 발생. 비동기 통신으로 이미지를 불러오는 방식으로 해결.

<br><br>

### 관리자: 판매 관리

<iframe width="560" height="315" src="https://www.youtube.com/embed/vlL-sx4mSVw" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

- 주문 목록이 뜨고, 주문 상태를 배송 중, 취소 중 등으로 바꿀 수 있음.

<br><br>

### 관리자: 상품관리

<iframe width="560" height="315" src="https://www.youtube.com/embed/goGi_zaSpc4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

<br><br>

### 구매목록

<iframe width="560" height="315" src="https://www.youtube.com/embed/cUDvLWrAwe0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

<br><br>

### 로그인이 필요한 기능

<iframe width="560" height="315" src="https://www.youtube.com/embed/Ts8fyORtW28" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

- 로그인이 필요한 기능에 로그인 하지않은 유저가 접근할 때 차단

<br><br><br>

## 코드 / 기능 리뷰

<details>
<summary class="summary-text">브랜드 등록 ---->접기/펼치기<</summary>

### 브랜드 등록

![이미지]({{ site.baseurl }}/images/20230528_175653.png)

<div style="text-align:center; font-size:0.8em;">브랜드 관리 창 ux/ui 개선</div>

<br><br>

![이미지]({{ site.baseurl }}/images/20230528_175836.png)

<div style="text-align:center; font-size:0.8em;">브랜드 등록 창 ux/ui 개선</div>

<br><br>

![이미지]({{ site.baseurl }}/images/20230528_175940.png)

<div style="text-align:center; font-size:0.8em;">이미지를 선택하면 미리보기 적용. 이미지 창을 클릭해서 등록할 수도 있고, 오른쪽 밑에 버튼으로 등록할 수도 있음. 어느 것으로 하든 서로 같은 이미지 파일을 선택한 것으로 보이게 설정</div>

<br><br>

![이미지]({{ site.baseurl }}/images/20230528_180302.png)

<div style="text-align:center; font-size:0.8em;">다만 등록을 완료하면 저렇게 이미지가 나오지 않는데, 서버를 껐다가 업데이트하고 다시 키면 보인다. 원인을 모르겠다...</div>

<br><br>

![이미지]({{ site.baseurl }}/images/20230528_180614.png)

<div style="text-align:center; font-size:0.8em;">브랜드 삭제 창 ux/ui 개선. 마우스 호버시 버튼이 보임</div>

<br><br>

![이미지]({{ site.baseurl }}/images/20230528_180712.png)

<div style="text-align:center; font-size:0.8em;">회원 관리 창 ux/ui 개선. 페이징 적용과 검색기능 추가</div>

</details>

---

<details>
<summary class="summary-text">옥션 ---->접기/펼치기<</summary>

![이미지]({{ site.baseurl }}/images/20230528_180930.png)

<div style="text-align:center; font-size:0.8em;">옥션창 - 실시간으로 입찰가가 업데이트 된다. 그리고 상세 이미지가 자동으로 넘어가도록 설정</div>

<br><br>

>입찰되는 가격을 실시간으로 업데이트 하는 방법을 계속 찾아봤는데, 결론은 두가지였다. Node.js의 socket.io를 이용하여 서버와 실시간 데이터 통신을 하는 방법이 있고, Javascript의 setInterval을 통해 지정된 시간마다 ajax매서드를 실행해 지속적으로 가격을 업데이트 해주는 방법이다. <br> 전자의 방법은 아직 배우지 않아 구현에 무리가 있기에 후자의 방법을 선택했다. <br> 또한 시간이 만료되면 옥션을 종료하도록 하는 ajax매서드도 선언해줬다.

```javascript
 function sendOnOffToServer() {
		  // num과 onOff 값을 서버로 전송
		  var numValue = ${auction.num};
		  
		  // 서버로 전송할 데이터를 FormData 객체에 추가
		  var formData = new FormData();
		  formData.append('num', numValue);

		  // AJAX 요청
		  var xhr = new XMLHttpRequest();
		  xhr.open('POST', '/product/expiredAuction.do', true);
		  xhr.onreadystatechange = function() {
		    if (xhr.readyState === XMLHttpRequest.DONE) {
		      if (xhr.status === 200) {
		        // 서버 응답 성공
		        console.log('서버 응답:', xhr.responseText);
		        document.getElementById('message').innerHTML = '기간이 만료된 경매입니다.';
		        
		        // 페이지 새로고침
		        location.reload();
		      } else {
		        // 서버 응답 실패
		        console.error('서버 응답 오류:', xhr.status);
		     // 페이지 새로고침
		        location.reload();
		      }
		    }
		  };
		  xhr.send(formData);
		}
```

<div style="text-align:center; font-size:0.8em;">옥션 시간이 만료되면 자동으로 옥션의 onoff값을 0(꺼짐)으로 바꿔주는 매서드</div>

<br><br>

```javascript
  if(document.frm.onOff.value != 0){
	 $(document).ready(function() {
		  fetchPrice(); // 페이지 로드 시 가격 가져오기

		  // 일정 간격마다 가격을 업데이트하기 위해 fetchPrice() 함수를 호출합니다.
		  setInterval(fetchPrice, 1000); // 1초마다 업데이트
		});
	 }

		function fetchPrice() {
			var numValue = ${auction.num};
			
		  $.ajax({
		    url: '/product/getPrice',
		    type: 'post', // POST 방식으로 변경
		    data: { num: numValue }, // num 값을 매개변수로 전달
		    success: function(price) {
		    	console.log("가격:", price);
		      updatePrice(price);
		    },
		    error: function(xhr, status, error) {
		      console.error('가격을 가져오는 동안 오류가 발생했습니다:', error);
		    }
		  });
		}
    //가격의 포멧을 ###,####으로 바꿔주는 매서드
		function formatPrice(price) {
		    return price.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
		}

		function updatePrice(price) {
		    const priceEl = document.getElementById('cPrice');
		    priceEl.innerText = formatPrice(price);

		    const currentPrice = document.getElementById('currentPrice');
		    currentPrice.value = formatPrice(price);
		}
```

<div style="text-align:center; font-size:0.8em;">옥션 가격을 1초마다 업데이트 해주는 매서드. ajax를 통해 서버의 가격을 가져와 현재가격에 value값을 넣어준다.</div>

<br>
<br>

![이미지]({{ site.baseurl }}/images/20230528_182145.png)

<div style="text-align:center; font-size:0.8em;">옥션창 - 기간이 만료되면 자동으로 입력창이 없어진다. 그리고 마지막의 입찰한 사람이 최종 입찰자가 된다.</div>

<br><br>


![이미지]({{ site.baseurl }}/images/20230528_182313.png)

<div style="text-align:center; font-size:0.8em;">옥션 목록에 가보면 입찰자 아이디와 현재 아이디가 같으면 구매버튼이 보이고 구매 완료하면 구매완료 버튼이 보인다.</div>

<br><br>

> 구매 완료 기능은 auction table에 보면 endPrice가 있다. 원래는 입찰이 되면 바로 endPrice에 값을 삽입해야하지만, 구매 전까지는 삽입 하지않고 price값이 최종 가격으로 설정해놓는다. 그리고 구매를 완료하면 endPrice를 삽입하는 방식인데, 만약 endPrice == null이라면 아직 구매하지 않은 것이므로 구매버튼이 보이게 해놓고 endPrice != null이 아니라면 구매 완료버튼이 보이게  해놨다. <br><br> endPrice != null의 조건문이 어째서인지 잘 먹히지 않아서 endPrice > 0 으로 대체, 그리고 그 외 조건은 c:otherwise를 사용했다.


<br>
<br>

![이미지]({{ site.baseurl }}/images/20230528_182837.png)

<div style="text-align:center; font-size:0.8em;">옥션 페이지에서 구매 버튼을 누르면 바로 결제창으로 이동한다.</div>

> 옥션 결제는 일반 결제와는 다르게 따로 페이지를 만들었다. 일반 결제는 쿠폰 적용, 포인트 적립등이 적용되고, 카트에 담긴채로 결제페이지로 이동하지만, 옥션결제는 쿠폰 적용 불가, 포인트 적립 불가, 장바구니 추가를 하지 않기 때문에 따로 설정해 줘야해서 페이지를 따로 만들어줬다.

<br>
<br>

![이미지]({{ site.baseurl }}/images/20230528_183209.png)

<div style="text-align:center; font-size:0.8em;">최종 결제 완료 창. Status 관리자가 매출/주문관리 페이지에서 최종 결제/ 반품/ 환불 처리를 해줘야 한다.</div>

<br><br>


![이미지]({{ site.baseurl }}/images/20230528_183344.png)

<div style="text-align:center; font-size:0.8em;">관리자 매출/주문관리 페이지.</div>

![이미지]({{ site.baseurl }}/images/20230528_183452.png)

<div style="text-align:center; font-size:0.8em;">New order list의 order number를 누르면 결제 승인 페이지가 나온다. 결제 승인을 누르면 shipment list(배송 승인)에 주문이 추가되고 shipment list페이지에서 최종 승인을 해준다.</div>

<br><br>

![이미지]({{ site.baseurl }}/images/20230528_183756.png)

<div style="text-align:center; font-size:0.8em;">최종 승인 모습.</div>

<br><br>

</details>

---

## 느낀점과 개선사항

### 개선사항

- 구현해야할 기능들이 아직 남아있어 지속적으로 기능을 추가해줘야한다. 
- 실시간 서버 업데이트는 좀더 공부가 필요한 부분이다.
- 상용화가 가능한 수준까지 디테일을 계속 업데이트 해주고 싶다.

<br>
<br>

### 느낀점
기존 mvc2 패턴으로 eclipse에서 할때도 많이 편했지만 Spring framework에서는 더 편했다. 수많은 servlet들을 만들지 않아도 됐고 controller에서 기능을 다 수행해 주니 너무나 편했다. 

<br>
다만 작업 초기 설정이 오래걸리고 beans와 각종 라이브러리들이 충돌을 일으켜 원인 모를 오류들이 뜰 때가 있어서 답답한 점도 있었다. 그리고 lombok의 \@data 어노테이션은 굉장히 편했지만 가끔 제데로 객체를 인식 못하는 오류도 발생하기도 했고, mapping이나 binding 오류도 빈번하게 발생해, 오류를 해결하는데 이전 프로젝트 보다 훨씬 더 많은 시간을 투자한것 같다. 

<br>
그래도 이전보다도 더 모듈화가 된 것 같아서 팀프로젝트에는 훨씬 더 편리했던 것 같다. 그리고 수많은 servlet들을 찾고 관리하느라 진땀 빼지 않아서 좋았다. 이제 점점 더 완성도가 높아지니까 굉장히 뿌듯했다.




