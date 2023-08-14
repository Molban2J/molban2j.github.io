---
layout: single
title: "(Spring) 스프링 예제 1"
categories:
  - Spring
---

## 스프링 예제 1

<br>
<br>

- 출처: [인프런] 스프링 핵심원리 - 기본편 (김영한)

### 1. 프로젝트 생성

<br>

![image]({{ site.baseurl }}/images/2023-08-14/20230814_131011.png)


<div style="text-align:center; font-size:0.8em;">Spring starter *주의: 프로젝트 생성시 버전은 2.xx대로 해준다.</div>

<br>

이번 예제는 스프링이 아닌 순수 자바위주의 예제이다.

그렇기 때문에 따로 의존성 주입을 해주지 않겠다. 

<br>
<br>
<br>
<br>

### 2. 비즈니스 요구사항과 설계

- **회원**
    - 회원은 가입하고 조회할 수 있다.
    - 회원은 일반과 VIP 두 가지 등급이 있다.
    - 회원 데이터는 자체 DB를 구축할 수 있고, 외부시스템과 연동할 수 있다.(미확정)

- **주문과 할인정책**
    - 회원은 상품을 주문할 수 있다.
    - 회원 등급에 따라 할인 정책을 적용할 수 있다.
    - 할인 정책은 모든 VIP는 1000원을 할인해주는 고정금액 할인을 적용해달라.(나중에 변경 될 수 있다.)
    - 할인 정책은 변경 가능성이 높다. 회사의 기본 할인 정책을 아직 정하지 못했고, 오픈 직전까지 고민을 미루고싶다. 최악의 경우 할인을 적용하지 않을 수도 있다.(미확정)

요구 사항을 보면 미확정인 부분, 또는 변경 가능성이 높은 요소들이 있다. 이런 요소들은 객체지향 설계 방법을 활용해 인터페이스를 만들고 언제든 구현체를 교체할수 있도록 설계한다.


<br>
<br>
<br>
<br>

### 3. 회원 도메인 개발

<br>
<br>



![image]({{ site.baseurl }}/images/2023-08-14/20230814_140920.png)


<div style="text-align:center; font-size:0.8em;">위 폴더 구조 안에 파일들을 생성해준다.</div>

<br>
<br>

```java
package hello.core.member;

public class Member {
    private Long id;
    private String name;
    private Grade grade;

    public Member(Long id, String name, Grade grade) {
        this.id = id;
        this.name = name;
        this.grade = grade;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Grade getGrade() {
        return grade;
    }

    public void setGrade(Grade grade) {
        this.grade = grade;
    }
}


```

<div style="text-align:center; font-size:0.8em;">Member.java</div>

<br>
<br>


```java
public interface MemberRepository {
    void save(Member member);

    Member findById(Long memberId);
}


```

<div style="text-align:center; font-size:0.8em;">MemberRepository.java</div>

<br>
<br>

```java
public class MemoryMemberRepository implements MemberRepository{

    private static Map<Long, Member> store = new HashMap<>();

    @Override
    public void save(Member member) {
        store.put(member.getId(), member);
    }

    @Override
    public Member findById(Long memberId) {
        return store.get(memberId);
    }
}


```

<div style="text-align:center; font-size:0.8em;">MemoryMemberRepository.java</div>

아직 DB연결을 안했으므로 로컬 메모리를 사용

> **HashMap과 ConcurrentHashMap의 차이** <br> 강의 들으면서 HashMap 부분에 원래는 동시성 문제때문에 ConcurrentHashMap을 써줘야 한다는 얘기를 들었다. HashMap과 ConcurrentHashMap의 차이점을 짚고 넘어가자.

<br>
<br>

```java
public interface MemberService {
    void join(Member member);

    Member findMember(Long id);
}


```

<div style="text-align:center; font-size:0.8em;">MemberService.java</div>

<br>
<br>

```java
public class MemberServiceImpl implements MemberService{

    private final MemberRepository memberRepository = new MemoryMemberRepository();
    @Override
    public void join(Member member) {
        memberRepository.save(member);
    }

    @Override
    public Member findMember(Long id) {
        return memberRepository.findById(id);
    }
}
```

<div style="text-align:center; font-size:0.8em;">MemberServiceImpl.java</div>

<br>
<br>


### 3-1 Test

<br>
<br>

직접 메인 클래스를 만들어서 실행하는 방법과 JUnit 테스트 두가지로 해본다.

<br>
<br>

```java
public class MemberApp {
    public static void main(String[] args) {
        MemberService memberService = new MemberServiceImpl();
        Member member = new Member(1L, "memberA", Grade.VIP);
        memberService.join(member);

        Member findMember = memberService.findMember(1L);
        System.out.println("new member = "+member.getName());
        System.out.println("find Member = "+findMember.getName());
    }
}
```

<div style="text-align:center; font-size:0.8em;">MemberApp.java</div>

<br>
<br>


![image]({{ site.baseurl }}/images/2023-08-14/20230814_141802.png)


<div style="text-align:center; font-size:0.8em;">실행화면(성공)</div>


<br>
<br>

```java
public class MemberServiceTest {

    MemberService memberService = new MemberServiceImpl();

    @Test
    void join(){
        //given
        Member member = new Member(1L, "memberA", Grade.VIP);

        //when
        memberService.join(member);
        Member findMember = memberService.findMember(1L);

        //then
        Assertions.assertThat(member).isEqualTo(findMember);
    }
}
```

<div style="text-align:center; font-size:0.8em;">MemberServiceTest.java</div>

<br>
<br>


![image]({{ site.baseurl }}/images/2023-08-14/20230814_142354.png)


<div style="text-align:center; font-size:0.8em;">실행화면(성공)</div>

<br>
<br>


![image]({{ site.baseurl }}/images/2023-08-14/20230814_143457.png)


<div style="text-align:center; font-size:0.8em;">실행화면(실패)</div>
