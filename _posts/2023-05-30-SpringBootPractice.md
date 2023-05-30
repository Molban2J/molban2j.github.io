---
layout: single
title:  "스프링 부트 쇼핑몰 예제"
---

<br>

**깃허브 repository** : [SpringBoot class practice](https://github.com/Molban2J/SpringBootPractice.git "github")

<br>

## 스프링 부트 예제

스프링 부트 프레임 워크로 넘어와서의 쇼핑몰 예제입니다.

---

<br>

<br>

## 1. 프로젝트 생성

![image]({{ site.baseurl }}/images/2023-05-30/20230530_143656.png)

<div style="text-align:center; font-size:0.8em;">프로젝트 의존성 생성</div>

> **수정**: mysql과 mybatis도 추가해 줘야합니다

<br>

![image]({{ site.baseurl }}/images/2023-05-30/20230530_143804.png)

<div style="text-align:center; font-size:0.8em;">application.yml추가</div>

<br>

![image]({{ site.baseurl }}/images/2023-05-30/20230530_144109.png)

![image]({{ site.baseurl }}/images/2023-05-30/20230530_144123.png)

<div style="text-align:center; font-size:0.8em;">controller추가, hello.html 추가</div>

<br>

<br>

---

<br>

## 2. 테이블 생성

```sql
use mydb;

create table tbl_board(
	boardId int auto_increment,
    title varchar (30) not null,
    content varchar(300) not null,
    name varchar(30) not null,
    primary key(boardId)
);
```

<div style="text-align:center; font-size:0.8em;">MySql 워크벤치에 테이블 생성</div>

---

<br>

## 3. 인터페이스와 매퍼 생성

<br>
<br>

my batis 의존성 주입을 깜빡했다. build.gradle에 새로 코드를 추가해 주겠다.

![image]({{ site.baseurl }}/images/2023-05-30/20230530_145136.png)

<div style="text-align:center; font-size:0.8em;">의존성 추가</div>


<br>

![image]({{ site.baseurl }}/images/2023-05-30/20230530_145247.png)

<div style="text-align:center; font-size:0.8em;">mapper scan과 bean을 등록해줍니다.</div>

> **수정**: controller에 "MapperScan"의 "basePackage"값을 "com.example.springbootpractice로 바꿔줍니다.

<br>

![image]({{ site.baseurl }}/images/2023-05-30/20230530_150109.png)

<div style="text-align:center; font-size:0.8em;">mapper 경로를 만들고, interface 파일을 생성해줍니다. 아직 Board DTO파일을 만들지 않아서 오류가 난다.</div>

<br>

![image]({{ site.baseurl }}/images/2023-05-30/20230530_150303.png)

<div style="text-align:center; font-size:0.8em;">resources 파일 밑에 BoardMapper Interface 경로와 똑같이 경로파일을 생성해주고 BoardMapper.xml 생성.</div>

> **수정**: 사진을 보면 com.example.springbootpractice.mapper경로를 한번에 만들었는데, 파일을 하나하나 만들어 줘야한다.

![image]({{ site.baseurl }}/images/2023-05-30/20230530_161833.png)

<div style="text-align:center; font-size:0.8em;">수정된 경로</div>

<br>

![image]({{ site.baseurl }}/images/2023-05-30/20230530_151201.png)

<div style="text-align:center; font-size:0.8em;">BoardMapper.xml에 코드 추가. 노란줄은 'SQL 파생언어가 구성되지 않았습니다'라는 메세지인데 무슨 메세지인지 모르겠다. 그리고 반환 타입에 보면 경로가 domain.Board인데 경로를 domain을 만들고 Board.java를 만들어 DTO파일을 만들어준다(제일 먼저 해줬어야..)</div>


> **주의** <br> 기존 Spring framework에서는 sql문을 작성할때 ';'을 붙이지 않았지만 SpringBoot에서는 붙여줘야한다.

<br>
<br>

![image]({{ site.baseurl }}/images/2023-05-30/20230530_151443.png)

<div style="text-align:center; font-size:0.8em;">domain > Board.java</div>

> 보면 기존에 쓰던 @Data 대신 @Getter/@Setter를 썼는데, 이것이 충돌이나 오류를 줄일 수 있는 방법이라 했다. 이전 스프링 프로젝트에서는 가끔 원인을 모르게 프로그램이 vo객체를 잘 인식하지 못하는 경우가 발생하기도 했는데 직접 Getter와 Setter를 지정해 주는 것이 이러한 오류를 줄일 수 있다고 한다. 그래도 제일 확실한 방법으로는 직접 코드로 Getter/Setter를 쳐주는 것이 제일 확실하다고 한다.

<br>

<br>

---

<br>

## 4. 서비스 생성

<br>
<br>

![image]({{ site.baseurl }}/images/2023-05-30/20230530_153026.png)

<div style="text-align:center; font-size:0.8em;">service 생성</div>

> **Transactional** 어노테이션: CRUD 기능을 테스트할때 기능 확인만 하고 commit하지 않고 rollback해주는 어노테이션 <br> **주의**: 클래스 임포트를 할때 

```java
import org.springframework.transaction.annotation.Transactional;
```

> 로 올바르게 임포트 해주자.


---

## 5. 테스트 메퍼 작성

테스트용으로 테이블에 데이터를 하나 삽입해줍니다.

```sql
insert into tbl_board(title, content, name) values('test title', 'test content', 'test');
```

<br>

Controller에서 test용 매퍼를 작성해줍니다.

```java
    @GetMapping("/test")
    public String test(Model model){
        model.addAttribute( "cnt", service.boardCount());
        model.addAttribute("test", service.boardList());
        return "/board/hello";
    }
```

<div style="text-align:center; font-size:0.8em;">test매퍼 생성</div>


<br>

![image]({{ site.baseurl }}/images/2023-05-30/20230530_160316.png)

<div style="text-align:center; font-size:0.8em;">hello.html</div>


```html
<hr>

<h1>[[${cnt}]]</h1>

<hr>

<h1>[[${test[0].title}]]</h1>
```

> 여기 보면 처음 보는 형태의 코드가 있다. 기존 Spring framework의 jsp에서는 jstl의 형식을 사용했지만 저 형식은 Spring Boot의 Thymeleaf 형식이라고 한다. * Thymeleaf는 SpringBoot의 View를 담당하는 부분이다.

<br>
<br>

![image]({{ site.baseurl }}/images/2023-05-30/20230530_160714.png)

<div style="text-align:center; font-size:0.8em;">localhost:8080/board/test 접속화면. 데이터가 잘 나오는걸 확인할 수 있다.</div>

<br>

*오늘은 여기까지(2023-05-30)*



