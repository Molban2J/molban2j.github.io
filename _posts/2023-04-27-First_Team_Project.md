---
layout: single
title:  "첫번째 팀 프로젝트"
categories:
  - JavaPr
  - Project
---


# 첫번째 팀 프로젝트
java와 gui를 이용한 첫번째 팀 프로젝트입니다.<br>

**팀원**: **임종민(팀장,본인)** , 이준환, 오현서, 김성준, 이정효<br>

**깃허브 repository** : [github.com/Molban2J/ClothSizeRecommandProject.git](https://github.com/Molban2J/ClothSizeRecommandProject.git "github")

**프로젝트 진행 기간**: 2023.03.1 5~ 2023.03.22<br>

###### ~~깃 블로그 만드는데 오래걸려 이제야 포스팅합니다..~~ 



---
<br>

## 주제
<br>

**주제선정**: 자신에게 적합한 의류 사이즈 추천 페이지<br><br>
**주제 선정 이유**: 
>의류회사마다 같은 사이즈 표기라도 실제 사이즈가 다른 경우가 많고
사람들마다 선호하는 사이즈가 다르기 때문에 온라인에서 의류를 
선택하기에 어려움이 있다.<br>
![_config.yml]({{ site.baseurl }}/images/sizecompareEx.png)
<br>
이러한 문제점을 쉽게 해결하고자
의류 사이즈 추천 페이지를 주제로 선정했다.

---
## 개발 환경 및 개발 스케줄
<br>

**개발 환경**
  * eclipe
  * MySQL
  * Illustrator

  <br>

**개발 스케줄**
<br>

|날짜|일정|
|:--:|:--|
|2023.03.15 ~ 2023.03.17|각자 맡은 페이지 프레임 및 레이아웃 제작,<br> 데이터 베이스 구축 및 연동|
|2023.03.17 ~ 2023.03.20|페이지 디자인 구상 및 제작,<br> 보완점 서칭|
|2023.03.15 ~ 2023.03.17|페이지 컴포넌트 디테일 수정,<br> thread기능 추가, 자료 및 코드 통합|

---
---
<br>
<br>

## 팀원 역할

|이름|역할|
|:--:|:--|
|**임종민(본인)**| - 프로젝트 구상<br> - 역할 분배 및 스케줄 조정<br> - 프로젝트 구조 및 페이지 흐름 설정<br>- 각 페이지 디자인<br> - 회원가입 페이지에서 DB로 데이터 전송<br> - 의류 선택창 제작<br> - ppt제작 및 발표|
|이준환|- 브랜드 선택창 제작<br> - 각 페이지 간의 코드 연결 및 연계<br> - DB설정 <br>- 로그인 페이지, 사이즈 추천 창 페이지 제작 어시스트<br> - 각종 thread, listener클래스 정리|
|오현서|-회원 가입 페이지 제작<br> - 관리자 페이지 제작<br> - profile DB 설정 <br> - 자료조사 <br> - 로그인 페이지와 회원관리 페이지, 관리자 페이지 연계 및 연결|
|김성준|-의류 사이즈 추천 페이지 제작<br> - 자료조사<br>- 각종 기능들 구상|
|이정효| - 로그인 페이지 제작<br> - 자료조사|

---
---
<br>

## DB 명세서

![db명세서 이미지]({{ site.baseurl }}/images/product2.png)

---
<br>

## 페이지 흐름도
<br>

![페이지 흐름도1 이미지]({{ site.baseurl }}/images/%EB%8C%80%EC%A7%80%201.png)
<br>

![페이지 흐름도2 이미지]({{ site.baseurl }}/images/%EB%8C%80%EC%A7%80%202.png)<br>

![페이지 흐름도3 이미지]({{ site.baseurl }}/images/%EB%8C%80%EC%A7%80%203.png)
<br>
예시 2번으로 결정

---
<br>

## Concept

![concept 이미지]({{ site.baseurl }}/images/%EB%8C%80%EC%A7%80%204.png)
---

<br>

---
<br>

## 페이지 상세
![login page 이미지]({{ site.baseurl }}/images/%EB%8C%80%EC%A7%80%205.png)
<br>

![sign in page 이미지]({{ site.baseurl }}/images/%EB%8C%80%EC%A7%80%206.png)
<br>

![brand page 이미지]({{ site.baseurl }}/images/%EB%8C%80%EC%A7%80%207.png)
<br>

![item page 이미지]({{ site.baseurl }}/images/%EB%8C%80%EC%A7%80%208.png)
<br>

![item detail page 이미지]({{ site.baseurl }}/images/%EB%8C%80%EC%A7%80%209.png)
<br>

---
<br>

## 구현 및 기능설명

![login page 이미지]({{ site.baseurl }}/images/%EB%8C%80%EC%A7%80%2010.png)<br>

![관리자 page 이미지]({{ site.baseurl }}/images/%EB%8C%80%EC%A7%80%2018.png)<br>

![sign in page 이미지]({{ site.baseurl }}/images/%EB%8C%80%EC%A7%80%2011.png)<br>

![brand page 이미지]({{ site.baseurl }}/images/%EB%8C%80%EC%A7%80%2012.png)<br>

![item page 이미지]({{ site.baseurl }}/images/%EB%8C%80%EC%A7%80%2017.png)<br>

![item detail page 이미지]({{ site.baseurl }}/images/%EB%8C%80%EC%A7%80%2019.png)<br>

---
---
<br>

## 개선사항 및 느낀점
<br>

* 개선사항
  * 중복 코드가 너무 많아 **중복 코드 제거** 절실
  * 코드의 **가독성이 떨어져** 정리가 필요함
  * 검색 기능 구현
  * 마우스 롤 오버시 이미지 확대 기능 추가 하고자 함
  * 옷 사이즈 추천 알고리즘에 대해 좀 더 연구해 봐야..
  * 핏 모드를 추가해 다양한 추천 방식 구현
  * 웹 크롤링을 통해 데이터 베이스에 데이터 삽입하는 방식을 <br>해보면 어떨까
  * 테이블 구성을 좀 더 신경써서 짜야할듯(중분류, 소분류 등)

---
<br>

### 느낀점
<br>
 첫 팀 프로젝트인 만큼 부족한 점이 두드러지게 보여진 거 같다.
개발을 하는데 있어서 코딩의 능력적인 부분도 부족함을 많이 느꼈고, 무엇보다도 팀장을 맡고 팀 프로젝트를 진행하면서 팀원간 능력, 성향 파악 및 리더십 발휘에서 부족함을 많이 느낀 프로젝트였다.<br>

의욕이 없는 팀원들은 어느 정도 선을 정해놓고 그 선까지 이끌어보고 안된다 싶으면 포기할 줄도 알아야 한다고 느꼈다. 열심히 챙기려고 했으나 소수의 팀원이 안 따라주니 나머지 팀원들이 뒤늦게 빈 공백을 채우느라 더 고생했던 것 같다.

그러면서 일정에 차질이 생기고 점점 일이 몰리자 코드를 병합하는데 있어서 문제가 많았는데 그 부분이 제일 아쉽다. 이 프로 젝트의 코드를 살펴보면 굉장히 이상한 방식으로 짜여져 있는데, 병합을 담당했던 사람이 자신의 방식대로 짜다보니 그렇게 된것이었다. 

ex)
//의류 상세페이지 창

```java
public Table getProfile(String id) {
	System.out.println("getProfile(String id)");
	Table profile=sql(7,"select", "name","sex","height","shoulder","chest","waist","leg", "from profile where id='"+id+"';");
	return profile;
}
```

##### * 위 코드를 보면 sql문을 만드는 매서드를 또 호출해서 데이터를 불러오고 있다.

<br>

이를 보고 프로젝트를 진행함에 있어서 작업 룰과 패턴을 정하는 것이 굉장히 중요하다는 것을 느꼈고, 이를 좀더 집요하게, 강하게 주장을 했어야하는 후회가 남는다.

비록 겉에서 보기는 멀끔하고 퀄리티도 좋아보이지만 정작 속은 곪아있는 것이다. 디자이너라면 만족해도 상관 없다고 생각하지만 우리는 개발자를 꿈꾸는 사람들이니 코드가 만족스럽지 않아 아쉬움이 남는다. 


