---
layout: single
title:  "스프링 부트 JPA 예제"
categories:
  - springboot
---

<br>

**깃허브 repository** : [SpringBoot class practice](https://github.com/Molban2J/SpringBootPractice.git "github")

<br>

## 스프링 부트 예제

스프링 부트 프레임 워크로 넘어와서의 JPA 예제입니다.

---

![image]({{ site.baseurl }}/images/2023-05-30/20230531_143219.png)

<div style="text-align:center; font-size:0.8em;">프로젝트 의존성 생성. 이전 예제와 달라진건 JPA추가 했다는 점입니다.</div>

<br>

이전 프로젝트의 application.yml파일도 복사해서 같은 위치에 넣어줍니다.

<br>
<br>

---

domain 패키지 생성후 Member.java를 생성해줍니다.

<br>

![image]({{ site.baseurl }}/images/2023-05-30/20230531_170439.png)

<div style="text-align:center; font-size:0.8em;">Member.java</div>

<br>

```java
@Getter
@Setter
@Entity
public class Member {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;

    private String name;
}

```

<div style="text-align:center; font-size:0.8em;">Member.java 코드</div>

<br>

**> 리뷰**

Getter/Setter 어노테이션을 사용해 주고 **Entity 어노테이션**도 추가해줍니다.<br>

**@Entity** : 이 어노테이션은 클래스 이름의 테이블이 존재하지 않을 경우 밑의 객체들을 가지고 새로운 테이블을 만들어주는 어노테이션입니다. 그리고 application.yml에 보면 jpa에 관한 설정이 있는데 

```yml
    hibernate:
      ddl-auto: create
```

create 설정이 되어있으면 기존의 동일한 이름의 테이블은 drop되고 새롭게 create 해줍니다. 

또한 어노테이션 옆에 이름 속성을 지정해주면 그 이름으로 테이블이 생성되고 지정해주지 않으면 클래스 이름으로 테이블을 생성해줍니다.

<br>

**@Id** : 클래스 안의 객체 위에 삽입해준다. 이 어노테이션을 달아주면 테이블을 생성할 때 해당 객체는 **NOT NULL** , 그리고 **PRIMARY KEY** 속성을 가진다.

<br>

**@GeneratedValue** : 값을 생성해주는 어노테이션이다. strategy는 전략이고, **GenerationType.IDENTITY**는 **auto increment**를 의미한다.

<br>

![image]({{ site.baseurl }}/images/2023-05-30/20230531_145944.png)

<div style="text-align:center; font-size:0.8em;">실행했을 때 콘솔창</div>

<br>

![image]({{ site.baseurl }}/images/2023-05-30/20230531_172158.png)

<div style="text-align:center; font-size:0.8em;">생성된 테이블</div>

<br>

---

<br>

**repository** 패키지를 만들고 안에 **MemberRepository.java**(interface)를 생성해줍니다. 

<br>

![image]({{ site.baseurl }}/images/2023-05-30/20230531_172623.png)

<div style="text-align:center; font-size:0.8em;">MemberRepository.java</div>

>**리뷰**<br><br>
**Optional** 클래스는 처음보는데 이는 검색해서 결과 값이 없을 수도 있어서 Optional클래스를 사용해 줬습니다. 코틀린에서 ?와 같은 기능이라고 합니다.

<br>

**repository** 패키지를 만들고 안에 **MemoryMemberRepository.java**를 생성해줍니다. 그리고 MemberRepository를 implements 해줍니다. 

매서드들을 오버라이드 해줍니다.

```java
   public class MemoryMemberRepository implements MemberRepository{
    private static HashMap<Integer, Member> store = new HashMap<>();
    private static int sequence = 0;
    @Override
    public Member save(Member member) {
        member.setId(++sequence);
        store.put(member.getId(), member);
        return member;
    }

    @Override
    public Optional<Member> findById(int id) {
        return Optional.ofNullable(store.get(id));
    }

    @Override
    public Optional<Member> findByName(String name) {
        return store.values().stream().filter(member -> member.getName().equals(name)).findAny();
    }

    @Override
    public List<Member> findAll() {
        return new ArrayList<>(store.values());
    }
}

```

<div style="text-align:center; font-size:0.8em;">MemoryMemberRepository.java</div>

<br>

**findById** 매서드를 보면 Optional.**ofNullable**이 있다. 이는 null값을 허용하겠다는 의미로, null값을 허용하지 않으려면 Optional.**of**로 하면 된다.

*다만 드는생각은 List로 선언하고 List의 길이가 0일때로 결과 값이 존재 여부를 판단하면 될것 같은데 Optional에 더 편한 기능이 있을지가 궁금하다.*

>추가 <br> 
**Optional**클래스는 코틀린과 연동해서 쓰기위해 쓴다고 들었다. 정확하게는 모르겠다.

**findMyName**을 보면 stream()매서드를 사용하는데, 이는 지금 하는 예제가 데이터 베이스에 직접 삽입하지 않고, 파일에 저장하는 방식을 사용할 것이기 때문에 파일을 읽기 위해 stream()매서드를 사용한다.

그리고 filter(member->memeber~~)는 람다식 표현이다.

<br>

>**리뷰**<br><br>
JPA를 이용하여 프로젝트를 만들 때에는 기존 마이바티스에서 사용했던 **Mapper.xml** 파일을 사용하지 않고 **repository**안의 파일들이 이를 대신한다고 한다.

---

<br>

service 패키지를 만들고 service파일을 생성해줍니다.


```java
 public class MemberService {
    private final MemberRepository memberRepository;
    @Autowired
    public MemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }


    public int join(Member member){
        validateDuplicateMember(member);
        memberRepository.save(member);
        return member.getId();
    }

    private void validateDuplicateMember(Member member){
        memberRepository.findByName(member.getName()).ifPresent( m -> {
            throw new IllegalStateException("이미 존재하는 파일입니다.");
        });
    }

    public List<Member> findMembers(){
        return memberRepository.findAll();
    }

    public Optional<Member> findOne(int memberId){
        return memberRepository.findById(memberId);
    }
}
```

<div style="text-align:center; font-size:0.8em;">MemberService.java</div>

<br>

validateDuplicateMember는 아이디 중복을 검사하는 매서드로, join에서 아이디가 중복이 되면 throw exception을 통해 매서드를 이스케이프 한다.

---

<br>

controller 패키지 생성후 controller 클래스 생성.

service와 controller를 연결해 주기위해 의존관계를 주입해줘야 한다고 한다.

<br>


```java
 @Controller
public class MemberController {
    private final MemberService memberService;

    @Autowired
    public MemberController(MemberService memberService){
        this.memberService = memberService;
    }

    @GetMapping("/new")
    public String createForm(){
        return "member/createMemberForm";
    }

    @PostMapping("/new")
    public String create(MemberForm form){
        Member member = new Member();
        member.setName(form.getName());
        memberService.join(member);
        return "redirect:/";
    }

    @GetMapping("/list")
    public String list(Model model){
        List<Member> members = memberService.findMembers();
        model.addAttribute("members",members);
        return "member/memberList";

    }
}
```

<div style="text-align:center; font-size:0.8em;">MemberController.java</div>

MemberService를 쓰기위해 객체 선언해주고 Autowired 어노테이션으로 생성자도 선언해준다.


MemberService에도 service와 repository를 연결해주기위해 의존성 주입해준다. @Service 어노테이션을 넣어준다.

![image]({{ site.baseurl }}/images/2023-05-30/20230531_182254.png)

<div style="text-align:center; font-size:0.8em;">MemberService.java</div>

<br>

MemberRepository에 @Repository 어노테이션 추가해준다.

![image]({{ site.baseurl }}/images/2023-05-30/20230531_182505.png)

<div style="text-align:center; font-size:0.8em;">MemberRepository.java</div>

<br>

---

기본 홈 화면에 띄울 페이지(html)과 cotroller를 생성해줍니다.

<br>

```java
@Controller
public class HomeController {
    @GetMapping("/")
    public String home(){
        return "home";
    }
}
```

<div style="text-align:center; font-size:0.8em;">HomeController.java</div>

<br>

<br>

home.html생성

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <div class="container">
        <div>
            <h1>Hello Spring</h1>
            <p>회원</p>
            <p>
                <a href="/new">회원가입</a>
                <a href="/list">회원목록</a>
            </p>
        </div>

    </div>
</body>
</html>
```

<div style="text-align:center; font-size:0.8em;">home.html</div>

---

<br>

<br>

이제 회원 가입과 회원 목록 창을 구현 해보겠다.

controller에 new매핑이 돼 있으니, createMemberForm.html을 작성한다.

<br>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div class="container">
    <form action="/new" method="post">
        <div class = "form-group">
            <label for="name">이 름</label>
            <input type="text" id="name" name="name" placeholder="이름을 입력하세요">
        </div>
        <button type="submit">가입</button>
    </form>
</div>

</body>
</html>
```

<div style="text-align:center; font-size:0.8em;">createMemberForm.html</div>

아직은 직접 데이터 베이스에 삽입 하지 않을 것이기 때문에 이름을 등록하고 파라미터로 넘겨주기 위한 MemberForm.java파일을 controller 패키지 밑에 만들어줍니다.
<br>

```java
package com.example.member.controller;

import lombok.Getter;
import lombok.Setter;

@Getter
@Setter
public class MemberForm {
    private String name;
}

```

<div style="text-align:center; font-size:0.8em;">memberForm.java</div>



<br>

참고로 회원가입 페이지에서 중복된 이름으로 가입을 하면 예외처리가 되기 때문에 오류페이지가 뜬다.

![image]({{ site.baseurl }}/images/2023-05-30/20230531_183555.png)

<div style="text-align:center; font-size:0.8em;">중복시 오류 메세지</div>


---

<br>

마지막으로 회원 목록 리스트 페이지를 만들어줍니다.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div class="container">
  <table>
    <thead>
      <tr>
        <th>#</th>
        <th>이름</th>
      </tr>
    </thead>
    <tbody>
      <tr th:each ="member: ${members}">
        <td th:text="${member.id}"></td>
        <td th:text="${member.name}"></td>
      </tr>
    </tbody>
  </table>
</div>
</body>
</html>

```

<div style="text-align:center; font-size:0.8em;">memberList.html</div>


![image]({{ site.baseurl }}/images/2023-05-30/20230531_183914.png)

<div style="text-align:center; font-size:0.8em;">회원 목록 구현화면</div>










