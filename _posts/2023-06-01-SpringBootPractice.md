---
layout: single
title:  "스프링 부트 JPA 예제2"
categories:
  - springboot
---

<br>

**깃허브 repository** : [SpringBoot class practice](https://github.com/Molban2J/SpringBootPractice.git "github")

<br>

## 스프링 부트 예제

스프링 부트 프레임 워크로 넘어와서의 JPA 예제입니다.

---

<br>

오늘은 JPA를 더 자세히 배워보도록 하는 것 같다.

프로젝트 생성 후 의존성 주입 해준다.

![image]({{ site.baseurl }}/images/2023-06-01/20230601_095442.png)

<div style="text-align:center; font-size:0.8em;">의존성 주입.</div>

<br>

entity 패키지 생성 후 CrudEntity.java파일을 만들어줍니다.

![image]({{ site.baseurl }}/images/2023-06-01/20230601_100322.png)

<div style="text-align:center; font-size:0.8em;">CrudEntity.java.</div>

<br>

어제와 비슷한데,

 **@Entity**어노테이션은 application.yml 파일에서 jpa 설정에서 **ddl: create**라면 기존의 같은 이름의 테이블을 drop하고 새로 create해준다. 다만 여기서는 직접 entity이름을 지정해 줬으므로 그 이름으로 테이블이 생성된다.

**@Build** 어노테이션은 sql 사용시 파라미터 값을 쉽게 삽입해주기 위해 사용

**@NoArgsConstructor** / **@AllArgsConstructor** 는 묵시적 / 명시적 생성자를 생성해주는 어노테이션이다.

**@Id** 어제 했듯이 NOT NULL과 PRIMARY KEY 속성을 부여해 주는 어노테이션

**@Column** 은 추가로 칼럼 속성을 추가해주는 어노테이션이다. 위에서는 NOT NULL 속성을 부여해줬다.

<br>

![image]({{ site.baseurl }}/images/2023-06-01/20230601_100338.png)

<div style="text-align:center; font-size:0.8em;">테이블이 drop되고 create됨.</div>

---

<br>

Repostitory 생성


<br>

![image]({{ site.baseurl }}/images/2023-06-01/20230601_101448.png)

<div style="text-align:center; font-size:0.8em;">CrudRepository.java</div>

<br>

![image]({{ site.baseurl }}/images/2023-06-01/20230601_101503.png)

<div style="text-align:center; font-size:0.8em;">어째서인지 오류가 뜨는 모습</div>

<br>

알고봤더니 클래스로 생성이 되어서 그랬던 것이다. Interface로 바꿔준다.


<br>

![image]({{ site.baseurl }}/images/2023-06-01/20230601_102702.png)

<div style="text-align:center; font-size:0.8em;">해결</div>

<br>

> **@Query** : JPA에서 제공하는 기본 Repository 안에는 다양한 sql들이 만들어져 있다. 평소에는 이러한 sql문들을 쓰면 되지만 복잡하거나 직접 sql문을 작성할때 이 애노테이션을 선언하고 안에 sql문을 지정해주면 된다.

---

<br>

![image]({{ site.baseurl }}/images/2023-06-01/20230601_102851.png)

<div style="text-align:center; font-size:0.8em;">DB에 테스트로 몇개를 삽입했다.</div>

<br>

![image]({{ site.baseurl }}/images/2023-06-01/20230601_102902.png)

<div style="text-align:center; font-size:0.8em;">실행 화면</div>

<br>

 application.yml에 JPA ddl설정이 create라 서버를 돌리면 테이블이 drop되서 결과 값이 나오질 않는다. 이부분을 **update**로 바꿔준다.

 다시 값들을 삽입하고 돌린다.

 <br>

![image]({{ site.baseurl }}/images/2023-06-01/20230601_102917.png)

<div style="text-align:center; font-size:0.8em;">잘 나오는 모습</div>

<br>

---

정보 검색을 넣어보겠다.

매핑은 /searchParamRepo?name=값 이런 형식으로 한다.

```java
 @GetMapping("/searchParamRepo")
    public String searchParamRepoMember(@RequestParam(value = "name") String name){
        return crudEntityRepository.searchParamRepo(name).toString();
    }
```

<div style="text-align:center; font-size:0.8em;">controller</div>

<br>

searchParamRepo 매서드는 이미 repository에서 선언해 줬으므로 실행해 본다..


---

<br>

정보 삽입 구현

 http://localhost:8080/test/insert?name=값&age=숫자 형식으로 구현

```java
 @GetMapping("/insert")
    public String insertMember(@RequestParam(value = "name") String name, @RequestParam(value = "age") int age){
        if(crudEntityRepository.findById(name).isPresent()){
            return "동일한 이름이 있습니다.";
        } else {
            CrudEntity entity = CrudEntity.builder().name(name).age(age).build();
            crudEntityRepository.save(entity);
            return "이름 : "+name+"나이 : "+age+"으로 추가되었습니다.";
        }
    }
```

<div style="text-align:center; font-size:0.8em;">controller</div>

<br>

name과 age 값을 입력하면 값이 삽입된다. 중복된 이름이면 동일한 이름이 있다고 뜬다.

<br>

![image]({{ site.baseurl }}/images/2023-06-01/20230601_104855.png)

<div style="text-align:center; font-size:0.8em;">등록 모습</div>

<br>

![image]({{ site.baseurl }}/images/2023-06-01/20230601_104908.png)

<div style="text-align:center; font-size:0.8em;">동일한 페이지를 다시한번 로딩하면 중복된 이름이 있다고 뜬다.</div>

<br>

---

<br>

정보 업데이트 구현

 http://localhost:8080/test/update?name=춘향이&age=40 형식

 <br>

```java
 @GetMapping("/update")
    public String updateMember(@RequestParam(value = "name") String name, @RequestParam(value = "age") int age){
        if(crudEntityRepository.findById(name).isEmpty()){
            return "입력한 "+ name+ "이 존재하지 않습니다.";
        } else {
            crudEntityRepository.save(CrudEntity.builder().name(name).age(age).build());
            return name + "의 나이를 "+age+ "로 변경했습니다.";
        }
    }
```

<div style="text-align:center; font-size:0.8em;">controller</div>

<br>

이전들과 비슷하지만 다만, update에서도 insert와 똑같이 **save** 매서드를 쓴다는 것이다.

<br>

![image]({{ site.baseurl }}/images/2023-06-01/20230601_105406.png)

<div style="text-align:center; font-size:0.8em;">변경 모습</div>

<br>

![image]({{ site.baseurl }}/images/2023-06-01/20230601_105423.png)

<div style="text-align:center; font-size:0.8em;">변경할 정보의 아이디(이름)값이 존재하지 않을때</div>

<br>

---

<br>

마지막으로 삭제 기능 구현

http://localhost:8080/test/delete?name=값 형식

<br>

```java
 @GetMapping("delete")
    public String deleteMember(@RequestParam(value = "name") String name){
        if(crudEntityRepository.findById(name).isEmpty()){
            return "입력한 "+ name+ "이 존재하지 않습니다.";
        } else {
            crudEntityRepository.delete(CrudEntity.builder().name(name).build());
            return name + "삭제 완료";
        }
    }
```

<div style="text-align:center; font-size:0.8em;">controller</div>

<br>

![image]({{ site.baseurl }}/images/2023-06-01/20230601_105753.png)

<div style="text-align:center; font-size:0.8em;">삭제 모습</div>

<br>