---
layout: single
title:  "(Project)두번째 팀 프로젝트"
categories:
  - Java
  - Project
---

# 두번째 팀 프로젝트

mvc2 패턴을 배우고, 두번째 팀 프로젝트를 진행했습니다.
<br><br>

**프로젝트 명**: Diamond Black

**팀원**: **임종민(본인)** , 박권능, 이규동, 정자윤<br>

**GitHub Repository** : [github.com/Molban2J/SecondTeamProject.git](https://github.com/Molban2J/SecondTeamProject.git "github")

**프로젝트 진행 기간**: 2023.04.10 ~ 2023.04.21<br>
<br><br>

---


<br><br><br>

## 목차

* [주제선정](#주제선정)
* [개발 환경 및 개발 스케줄](#개발-환경-및-개발-스케줄)
* [팀원 역할](#팀원-역할)
* [DB 명세서](#db-명세서)
* [프로젝트 파일](#프로젝트-파일)
* [기능 개요](#기능-개요)
* [기능별 요구사항](#기능별-요구사항)
* [기능 구현](#기능-구현)
* [개선사항과 느낀점](#개선사항과-느낀점)

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

**개발 환경**
  * Eclipe
  * MySQL
  * Javascript
  * HTML
  * CSS
  * Bootstrap
  * KakaoMap API


  <br>

**개발 스케줄**
<br>

|날짜|일정|
|:--:|:--|
|2023.04.10 ~ 2023.04.11|- 주제 선정 <br> -구체적인 개발 방향 탐색(페이지 및 기능 구성 등) <br> - 데이터베이스 설계<br> - 역할 분담|
|2023.04.12 ~ 2023.04.14|- 데이터 베이스 구축<br> - 각자 맡은 페이지의 대략적 구조 제작 <br> - 각자 브랜드 정해서 데이터 수집<br> - 페이지의 전체적 테마 설정/ 부트스트랩 지정|
|2023.04.15 ~ 2023.04.19|- 각자 맡은 페이지 상세 제작/ 부트스트랩, CSS 적용<br> - 각 페이지 기능 구현(회원가입, 주문, 장바구니, 핫딜, 옥션 등)<br> - 추가 기능 구상 및 오류 수정<br> - 틈틈히 팀원들 간 코드 병합|
|2023.04.19 ~ 2023.04.21|- 코드 전체 병합<br> -기능 테스트<br> - 디테일 수정 및 확립<br> - 오류 수정 <br> - 발표|

---
<br>
<br>

## 팀원 역할

|이름|역할|
|:--:|:--|
|**임종민(본인)**| - 관리자 페이지 제작<br> - 브랜드, 상품관리<br> - 회원관리<br> - 옥션 등록<br> - 자유게시판 관리 <br> - 경매 페이지, 기능 구현<br>|
|박권능|- 로그인, 회원가입 페이지 제작, 기능구현<br> - 유효성 검사 스크립트 삽입<br> - 패스워드 암호화 처리<br> - KakaoMap API<br> - 결제 API 삽입<br> - 게시판 페이징 처리 <br> - Contact페이지<br>|
|이규동|-핫 딜 제작, 기능구현<br> - 게시판, 상품 페이지 제작<br> -상품(로고, 카테고리, 검색)리스트 구현 <br> -관리자 페이지의 QnA게시판 관리, 매출 관리 구현 <br>- 파일 통합<br> - ppt발표 <br>|
|정자윤|-데이터 베이스 제작, 수정, 업데이트<br> - 장바구니 제작, 구현 <br> - ppt작성 <br> - 마이페이지 주문내역 구현|

---

<br>

## DB 명세서

![db명세서 이미지]({{ site.baseurl }}/images/dbtable.png)

![db명세서 이미지]({{ site.baseurl }}/images/user.png)

![db명세서 이미지]({{ site.baseurl }}/images/product.png)

![db명세서 이미지]({{ site.baseurl }}/images/cart.png)

![db명세서 이미지]({{ site.baseurl }}/images/qna.png)

![db명세서 이미지]({{ site.baseurl }}/images/free.png)

![db명세서 이미지]({{ site.baseurl }}/images/auction.png)

---

<br>

## 프로젝트 파일

![프로젝트 파일 이미지]({{ site.baseurl }}/images/console.png)

<br>

---

<br>

## 기능 개요

### **관리자**

* 회원관리
  * 회원 정보 수정
  * 회원 탈퇴

* 상품 관리
  * 상품 등록
  * 상품 삭제

* 브랜드 관리
  * 브랜드 추가
  * 브랜드 삭제

* 게시판 관리
  * 게시판 등록
  * 게시판 수정
  * 게시판 삭제

* 경매 관리
  * 경매 등록(경매 기간, 시작가 설정)

### **회원**

* 회원 가입
  * 회원 가입 및 탈퇴

* 로그인

* 마이페이지
  * 주문 내역 
  * 내가 쓴 글 확인
  * 회원 정보 수정 및 탈퇴

* 게시글
  * 게시글 등록
  * 게시글 수정
  * 게시글 삭제

* 상품 구매
  * 경매 및 핫딜 등 상품 구매
  * 장바구니 및 결제

### **게시판**
* 자유 게시판
  * CRUD 기능, 조회수, 페이징 처리, 댓글, 이미지 첨부
* QnA 게시판 
  * CRUD 기능, 조회수, 페이징 처리, 이미지 첨부

### **상품**
* 상품등록
  * 가격, 이미지, 상품 설명, 사이즈, 브랜드
* 장바구니
  * 장바구니 담긴 상품 삭제
  * 총 상품 가격 확인

### **결제**
* 구매자 정보 확인
* 총 결제금액 확인
* 결제 API를 이용하여 결제
### **Contact**
* 지도 API를 이용하여 회사 위치 확인
* 회사 정보 확인

### **옥션(경매)**
* 어드민이 설정한 상품, 시간, 시작가 확인
* 입찰이 끝나면 낙찰자에게 구매 권한 부여

### **핫딜(타임세일)**
* 어드민이 설정한 상품 갯수, 시간, 할인율 확인
* 해당 시간안에 할인된 임의의 상품을 구매 가능

---

## 기능별 요구사항

* **회원가입**
  * 유효성 검사
    * 이름은 한글(자음+모음)의 조합 2글자 이상
    * 아이디는 영문 대소문자와 숫자 4~12 자리의 조합으로 입력
    * 아이디 중복 체크 후 사용 가능
      * 아이디 중복 시 “이미 사용 중인 아이디입니다.” 메시지 출력
    * 이메일 형식은 Emailid 뒤에 @ 과 ###.### 의 형식으로 입력
    * 우편번호는 주소API를 이용하여 자신의 집 검색 후 자동 입력
    * 비밀번호는 영문과 숫자 조합으로 입력
    * 비밀번호는 sha256 알고리즘으로 암호화 되어 DataBase에 저장

    <br><br>
  
* **로그인**
  * 로그인을 하지 않은 경우 아래 페이지만 이용 가능
    * 회원 가입 페이지
    * 로그인 페이지
    * 게시글 목록 조회 페이지
    * 게시글 상세 보기 페이지
    * 상품 페이지
    * 옥션 페이지
    * 핫딜 페이지
    * 로그인을 하지 않고 위에 페이지를 제외한 다른 페이지를 이용하려 할 시 <br>
“로그인 후 사용 가능 합니다.” 메시지 출력 후 기능 이용 제한
  * 로그인 검사
    * 아이디와 비밀번호가 일치하지 않을 경우 “비밀 번호가 맞지 않습니다.” 메시지 출력
    * 아이디가 존재하지 않을 경우 “존재하지 않는 아이디 입니다.” 메시지 출력
    * 아이디와 비밀번호가 일치할 시 메인 페이지로 이동
    * 어드민 계정으로 로그인할 시 관리자 페이지 추가

<br><br>

* **유저**
  * 상품 장바구니 담기
  * 장바구니 상품 구매
    * 구매 시 이용 약관에 동의 하지 않았거나, 결제 방식을 선택 하지 않았으면
각각의 선택 메시지 출력
    * 위의 이용 약관과 결제 방식을 선택 후 구매 버튼을 눌렀다면, 결제API 모듈 실행
  * 옥션 상품 입찰, 구매
    * 낙찰된 최종 입찰자만 구매 가능
  * 핫딜 상품 구매
  * 자유 게시판 이용
    * 등록, 수정, 삭제 가능(수정 과 삭제는 작성한 본인 만 가능)
  * QnA 게시판 이용
    * 등록, 수정, 삭제는 어드민만 가능

    <br><br>

* **마이페이지**
  * 내 정보
    * 비밀번호 입력 후 수정 페이지로 이동
    * 아이디와 이름을 제외한 정보들을 수정 가능
    * 수정란에 NOT NULL인 비밀번호를 입력하지 않을 시 <br>
“비밀번호를 입력해주세요” 메시지 출력
    * 비밀번호를 재차 확인하여 비밀번호 일치 여부 확인
    * 회원 탈퇴 가능
  * 주문 내역
    * 자신이 구매한 주문 번호, 주문 날짜, 상태 목록 출력
    * 주문 번호 클릭 시 구매한 해당 주문 번호의 상품과 가격 등의 상세 내역을 확인
  * 내가 쓴 글
    * 자유 게시판에 작성한 나의 글 확인 및 수정, 삭제 가능

    <br><br>
  
* **관리자(어드민)**
  * 상품관리
    * 브랜드 관리
      * 브랜드 로고 이미지와 브랜드 명을 추가, 삭제 가능
    * 상품 관리
      * 브랜드, 상품 카테고리, 상품 이름, 사이즈, 가격, 성별, 이미지, 상품 설명, 재고량 등을 설정하여 추가, 삭제 가능

  * 회원 관리
    * 가입일자를 제외한 모든 회원 정보 수정 가능
    * 회원 삭제
  * 게시판 관리
      * 게시판 등록, 수정, 삭제 가능
  * 옥션
    * 옥션 상품 등록
      * 등록할 상품을 등록, 사이즈와 제한 시간(endtime), 시작가를 설정하여 등록함.

      <br><br>

* **상품**
  * 브랜드 로고 클릭 시 해당 브랜드의 모든 상품 리스트 출력
  * 카테고리 클릭 시 해당 브랜드의 카테고리가 설정된 상품 리스트 출력
  * 상품 검색 시 해당 검색어가 들어가는 상품 리스트 출력
  * 검색한 상품이 존재 하지 않을 시 “검색 결과 없음” 페이지 출력

  <br><br>

* **Contact**
  * Kakao지도API를 활용하여 설정한 임의의 회사 위치를 지도로 확인 가능
  * 회사의 상세 정보를 확인 가능

  <br><br>

* **디자인**
  * Bootstrap을 이용하여 모바일과 pc에 따른 반응형 웹으로 제작



---

<br><br>

## 기능 구현

<br><br>

* 상품

![image]({{ site.baseurl }}/images/2023-06-19/20230619_160737.png)

<div style="text-align:center; font-size:0.8em;">브랜드 클릭시 모든 상품들이 정렬</div>

<br><br>

![image]({{ site.baseurl }}/images/2023-06-19/20230619_161004.png)

<div style="text-align:center; font-size:0.8em;">드롭다운 메뉴는 옷 카테고리에 따라 분류</div>

<br><br>

![image]({{ site.baseurl }}/images/2023-06-19/20230619_160826.png)

![image]({{ site.baseurl }}/images/2023-06-19/20230619_160908.png)

<div style="text-align:center; font-size:0.8em;">검색 결과/ 결과 없을 시</div>

<br><br>

* 회원가입

![image]({{ site.baseurl }}/images/2023-06-19/20230619_153033.png)

<div style="text-align:center; font-size:0.8em;">회원가입 > 아이디 중복검사</div>

<br><br>

![image]({{ site.baseurl }}/images/2023-06-19/20230619_145036.png)

<div style="text-align:center; font-size:0.8em;">회원가입 > 주소록 api를 통해 주소 입력</div>


<br><br>

![image]({{ site.baseurl }}/images/2023-06-19/20230619_145453.png)

<div style="text-align:center; font-size:0.8em;">회원가입 > SHA256을 통해 비밀번호를 암호화하여 저장</div>


<br><br>

* 로그인

<br><br>

![image]({{ site.baseurl }}/images/2023-06-19/20230619_153016.png)

![image]({{ site.baseurl }}/images/2023-06-19/20230619_153001.png)

<div style="text-align:center; font-size:0.8em;">계정 정보가 올바르게 입력되지 않았을 때</div>

<br><br>

![image]({{ site.baseurl }}/images/2023-06-19/20230619_153231.png)

<div style="text-align:center; font-size:0.8em;">로그인 후 접근이 필요한 페이지면 경고창을 띄우고 창이 넘어가지 않음</div>


<br><br>

![image]({{ site.baseurl }}/images/2023-06-19/20230619_152910.png)

<div style="text-align:center; font-size:0.8em;">일반 회원으로 로그인 시</div>

<br><br>

![image]({{ site.baseurl }}/images/2023-06-19/20230619_153509.png)

<div style="text-align:center; font-size:0.8em;">관리자로 로그인 시</div>

<br><br>

* 유저

<br><br>

![image]({{ site.baseurl }}/images/2023-06-19/20230619_153404.png)

<div style="text-align:center; font-size:0.8em;">유저로 내 정보수정 페이지 접근시 2차 확인</div>

<br><br>

![image]({{ site.baseurl }}/images/2023-06-19/20230619_150443.png)

<div style="text-align:center; font-size:0.8em;">유저 > 내 정보수정 페이지</div>

<br><br>

![image]({{ site.baseurl }}/images/2023-06-19/20230619_154511.png)

<div style="text-align:center; font-size:0.8em;">장바구니</div>

<br><br>

![image]({{ site.baseurl }}/images/2023-06-19/20230619_155200.png)

<div style="text-align:center; font-size:0.8em;">결제화면(50% 축소 화면) / 약관 동의 후 결제 api로 이동</div>

<br><br>

![image]({{ site.baseurl }}/images/2023-06-19/20230619_155221.png)

<div style="text-align:center; font-size:0.8em;">결제 api. 아직 제데로된 연동이 안되었기 때문에 결제 취소후 결제 실패 메세지가 뜨면 결제 완료 창으로 넘어가도록 설계</div>

<br><br>

![image]({{ site.baseurl }}/images/2023-06-19/20230619_155601.png)

<div style="text-align:center; font-size:0.8em;">결제 완료 화면</div>

<br><br>

![image]({{ site.baseurl }}/images/2023-06-19/20230619_155641.png)

<div style="text-align:center; font-size:0.8em;">유저 > MyPage > 주문내역. status는 어드민이 주문관리에서 주문 처리에 따라 바뀜</div>


<br><br>

![image]({{ site.baseurl }}/images/2023-06-19/20230619_155820.png)

<div style="text-align:center; font-size:0.8em;">게시판 등록</div>

<br><br>

![image]({{ site.baseurl }}/images/2023-06-19/20230619_155904.png)

<div style="text-align:center; font-size:0.8em;">유저 > MyPage > 내가 쓴 글</div>

<br><br>

* 관리자

<br><br>

![image]({{ site.baseurl }}/images/2023-06-19/20230619_160036.png)

<div style="text-align:center; font-size:0.8em;">관리자 > 상품관리</div>

<br><br>

![image]({{ site.baseurl }}/images/2023-06-19/20230619_160117.png)

![image]({{ site.baseurl }}/images/2023-06-19/20230619_160139.png)

<div style="text-align:center; font-size:0.8em;">관리자 > 브랜드 관리/ 등록</div>

<br><br>

![image]({{ site.baseurl }}/images/2023-06-19/20230619_160224.png)

![image]({{ site.baseurl }}/images/2023-06-19/20230619_160304.png)

<div style="text-align:center; font-size:0.8em;">관리자 >상품 관리/ 등록</div>

<br><br>

![image]({{ site.baseurl }}/images/2023-06-19/20230619_160345.png)

![image]({{ site.baseurl }}/images/2023-06-19/20230619_160430.png)

<div style="text-align:center; font-size:0.8em;">관리자 >회원 관리/ 수정</div>

<br><br>

![image]({{ site.baseurl }}/images/2023-06-19/20230619_160507.png)

![image]({{ site.baseurl }}/images/2023-06-19/20230619_160554.png)

<div style="text-align:center; font-size:0.8em;">관리자 >옥션 등록 / 사이즈, 시작가격, 경매 기간 설정</div>

<br><br>

* Contact

![image]({{ site.baseurl }}/images/2023-06-19/20230619_161109.png)

<div style="text-align:center; font-size:0.8em;">카카오 지도 api 사용</div>


<br><br><br><br>

## 개선사항과 느낀점 

<br>

### 개선 사항

1. 로그인
    1. 카카오톡/ 구글 API를 이용하여 로그인 가능하게 구현
    2. 이메일 인증 구현
2. 유저
    1. 찜하기 기능
    2. 장바구니에 담은 상품 수를 장바구니 뱃지에 표현
    3. 결제 시 결제한 금액에 따른 포인트 지급 및 포인트에 따른 회원 등급 조정
    4. 회원 등급에 따른 상품 할인 쿠폰 지급
3. 상품
    1. 상품 구매 할 시 재고량의 변동을 구현
    2. 할인 쿠폰 구현
    3. 취소 및 반품 구현
    4. 옥션의 낙찰자가 상품 구매 후 해당 상품의 구매하기 버튼을 사라지게 구현
    5. 상품 페이지의 페이징 처리
    6. 핫딜 기능을 코드로 직접 고쳐서 반영 시키는게 아닌 어드민 페이지에서 조정 할 수 있게 구현
    7. 실시간 경매 참여자 리스트 목록과 실시간 입찰가 확인 구현
4. 게시판의 답글 달기 및 API를 이용한 댓글이 아닌 웹페이지 자체의 댓글 기능 구현

<br>

### 느낀점

<br>

확실히 날이 갈 수록 배우는 것들이 프로젝트를 하기에 점점 편리해 지는 것을 느낀다. 이전에는 코드들을 일일이 확인하고 복사해서 붙여넣는 것이었다면, mvc2패턴을 배우면서 서로 작업한 부분을 모듈화 해서 기존 뼈대에 살을 붙여 나가듯이 붙이면 됐다. 덕분에 오류도 적어지고 코드를 수정하고 기능을 확장하기도 훨씬 수월했다. 

<br>

앞으로 개선해야 할점으로는 아직 배우지 않은 Ajax의 비동기 방식이나 스크립트 부분, 그리고 기존 코드의 간결화에 초점을 두어 지금까지 만들어 놓은 프로젝트를 계속 다듬고 디테일을 잡는 것이 좋을것 같다. 그리고 사용자 측면에서의 ui나 기능성도 계속해서 보완하고 발전시켜 나가야하겠다.

<br>
다음 프로젝트는 이 프로젝트를 Spring framework로 옮겨서 개선하는 방향으로 진행할 예정이다.