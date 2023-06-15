---
layout: single
title: "(SpringBoot) 배민 클론 코딩3"
categories:
  - SpringBoot
---

<br>

**깃허브 repository** : [SpringBoot Baemin practice](https://github.com/Molban2J/Baemin.git "github")

\* 참고한 블로그: [blog link](https://sumin2.tistory.com/10?category=900942 "github")

<br>
<br>

이전 포스팅에서 이어진다.

<br>
<br>
<br>
<br>

## Error 해결하기

---

<br>

회원가입을 시도 하는데 자꾸 에러창이 뜬다. 

![image]({{ site.baseurl }}/images/2023-06-15/20230615_110145.png)

<div style="text-align:center; font-size:0.8em;">아이디 중복과 함께 에러가 뜸</div>

<br>
<br>

![image]({{ site.baseurl }}/images/2023-06-15/20230615_110307.png)

<div style="text-align:center; font-size:0.8em;">콘솔창 에러</div>

<br>
<br>

콘솔창에 뜬 에러 내용을 보니, UserMapper에서의 overlapCheck 매서드를 찾지 못하는 것 같다. 

<br>
<br>

### <해결>

UserDAOImpl.java 코드를 수정해줘야한다. 기존 코드는 SqlSession을 Autowired해서 사용했지만 나는 UserMapper를 Autowired해서 사용하도록 하겠다.

<br>

```java
@Repository
public class UserDAOImpl implements UserDAO{

    @Autowired
    private UserMapper userMapper;  //수정
    @Override
    public void join(Join join) {
        userMapper.join(join);  //수정
    }

    @Override
    public int overlapCheck(String value, String valueType) {
        Map<String, String> map = new HashMap<>();
        map.put("value", value);
        map.put("valueType", valueType);
        return userMapper.overlapCheck(map);  //수정
    }
}
```

<div style="text-align:center; font-size:0.8em;">UserDAOImpl.java</div>

<br>
<br>
<br>

이에 맞게 UserMapper.java도 수정해준다.

```java
@Mapper
public interface UserMapper {
    @Insert("insert into BM_USER(ID, USERNAME, PASSWORD, EMAIL, NICKNAME, PHONE VALUES USER_ID_SEQ.NEXTVAL, #{username}, #{password}, #{email}, #{nickname}, #{phone}")
    public void join(Join join);  //수정

    @Select("SELECT COUNT(*) FROM BM_USER WHERE ${valueType} = #{value}")
    int overlapCheck(Map<String, String> map);  //수정
}

```

<div style="text-align:center; font-size:0.8em;">UserMapper.java</div>

<br>
<br>
<br>

![image]({{ site.baseurl }}/images/2023-06-15/20230615_122624.png)

<div style="text-align:center; font-size:0.8em;">ajax와 다른 유효성 스크립트가 잘 작동하는 모습</div>

<br>
<br>
<br>

### 하지만...
회원가입 버튼을 누르자 또 에러가 난다.

문제는 두가지인데, 하나는 올바른 이메일 형식이 아니라는 메시지가 뜬다는 것이고, 하나는 Controller에서 errorMsg를 보낼때 HashMap에서 username을 매칭을 못하는 것 같다.

![image]({{ site.baseurl }}/images/2023-06-15/20230615_124210.png)

![image]({{ site.baseurl }}/images/2023-06-15/20230615_124225.png)

<div style="text-align:center; font-size:0.8em;">에러메세지들...</div>

<br>
<br>

아무래도 예상으로는 이메일 부분에서 에러가 났지만 username에서는 에러가 없어서 field = username이 존재하지 않아서 이런 오류가 뜨는 것 같다. null일때의 처리 방법을 찾아봐야할 것 같다.

<br>
<br>

정말 오랜시간동안 에러가 발생하는 부분을 찾았다. 그리고 구조적으로 중복되는 부분도 찾았는데, 먼저 에러가 발생한 부분부터 말하자면 Controller의 "/join" PostMapping에서 오류가 발생했다.

 그 안에서도 **@Valid** 어노테이션에서 에러가 발생하는 거였는데, 이 어노테이션이 이메일 형식을 제데로 이메일 형식이라고 인식하지 못해서 오류 메세지를 띄우는 것이었다...

처음에는 자바스크립트 파일에서의 유효성 검사에서 문제가 발생하나 했는데 애초에 자바스크립트에서 유효성 검사가 통과 됐기에 회원가입 버튼이 활성화 되서 서버로 데이터를 전송하고 처리를 하는 것이기에 스크립트는 아니었다. 그렇다면 Controller에서 유효성 검사를 하고 에러가 뜨면 에러메세지를 보내는 과정에서 오류가 발생했다는 것인데, 처음에는 BindingResult 클래스에서 오류가 발생하는 것인가 했다. 그래서 BindingResult 매개변수를 지워서 실행했더니 springframework.validation 뭐시기가 뜨길래 @Valid 어노테이션이 문제라는 것을 알아차렸다. 프레임웍 안의 정규식이 잘못된건지 왜그러는지는 모르겠으나 어노테이션 자체의 오류라니... 찾기가 너무 힘들었다..

그리고 view에서 유효성 검사를 하는데 굳이 서버에서도 할 필요가 있을까 싶어 Controller에서의 유효성 검사는 지웠다. 게다가 HashMap을 사용하니 오류가 발생해도 발생하지 않은 부분에 대해서는 처음이랑 똑같이 매핑할수 없는 오류가 뜰테니까 지워버리도록 하자.

아 그리고 Mapper의 sql문에서의 오류도 발견하여 고쳐줬다.

```java
    @PostMapping("/join")
    public String joinProc(Join join,Model model){
        System.out.println(join);
        userService.join(join);
        return "redirect:/login";
    }
```

<div style="text-align:center; font-size:0.8em;">UserController.java</div>

<br>
<br>

```java
     @Insert("insert into BM_USER(ID, USERNAME, PASSWORD, EMAIL, NICKNAME, PHONE) VALUES (USER_ID_SEQ.NEXTVAL, #{username}, #{password}, #{email}, #{nickname}, #{phone})")
    public void join(Join join);
```

<div style="text-align:center; font-size:0.8em;">UserMapper.java (괄호를 안닫아줬다.)</div>

<br>
<br>

![image]({{ site.baseurl }}/images/2023-06-15/20230615_151527.png)

<div style="text-align:center; font-size:0.8em;">데이터베이스에 잘 들어간모습</div>

<br>
<br>
<br>
<br>

## 테이블 생성하기

---

로그인을 하기 위해 미리 테이블을 내 임의로 만들었는데 다시 drop table 하고 새로 만들도록 하겠다. 그리고 프로젝트에 필요한 모든 테이블들을 만들어 놓는다.


<br>
<br>

<details>
<summary>Table Sql</summary>
<div markdown="1">

```sql
  CREATE TABLE BM_USER (
    ID NUMBER PRIMARY KEY,
    USERNAME VARCHAR2(100) NOT NULL,
    PASSWORD VARCHAR2(100) NOT NULL,
    EMAIL VARCHAR2(50) ,
    NICKNAME VARCHAR2(50),
    POINT NUMBER DEFAULT 0,
    PHONE VARCHAR2(20) ,
    RATING VARCHAR2(50) DEFAULT 0,
    ROLE VARCHAR2(20) DEFAULT 'ROLE_USER'
); 
    
    -- 유저 번호 자동증가    
    CREATE SEQUENCE USER_ID_SEQ
       INCREMENT BY 1
       START WITH 1
       MINVALUE 1
       MAXVALUE 99999999999
       NOCYCLE
       NOCACHE
       NOORDER;
 
 
create table bm_store (
    id NUMBER primary key,
    category number NOT NULL,
    store_name varchar2(100) NOT NULL,
    store_address1 varchar2(200) NOT NULL,
    store_address2 varchar2(200) NOT NULL,
    store_address3 varchar2(200),
    store_phone varchar2(20) NOT NULL,
    store_img varchar2(200),
    store_thumb varchar2(200),
    opening_time number DEFAULT 0,
    closing_time number DEFAULT 0,
    min_delevery number DEFAULT 0,
    delevery_time number DEFAULT 0,
    delevery_tip number  DEFAULT 0,
    store_des varchar2(1000) DEFAULT '가게소개가 없습니다'
);
 
    CREATE SEQUENCE STORE_ID_SEQ
       INCREMENT BY 1
       START WITH 1
       MINVALUE 1
       MAXVALUE 99999999999
       NOCYCLE
       NOCACHE
       NOORDER;
 
create table bm_food (
    id number primary key,
    store_id number NOT NULL,
    food_name varchar2(100) NOT NULL,
    food_price number NOT NULL,
    food_dec varchar2(200), 
    food_img varchar2(200),
    food_thumb varchar2(200)
);
 
    CREATE SEQUENCE FOOD_ID_SEQ
       INCREMENT BY 1
       START WITH 1
       MINVALUE 1
       MAXVALUE 99999999999
       NOCYCLE
       NOCACHE
       NOORDER;
       
ALTER TABLE BM_FOOD
ADD CONSTRAINT FOOD
FOREIGN KEY (STORE_ID)
REFERENCES BM_STORE(ID)
on delete cascade;
 
 
-- 음식 추가옵션
create table bm_food_option (
    id number PRIMARY KEY, 
    food_id number not null,
    option_name varchar2(100) not null,
    option_price number not null
);
 
    CREATE SEQUENCE OPTION_ID_SEQ
       INCREMENT BY 1
       START WITH 1
       MINVALUE 1
       MAXVALUE 99999999999
       NOCYCLE
       NOCACHE
       NOORDER;    
    
    
ALTER TABLE BM_FOOD_OPTION
ADD CONSTRAINT FOOD_OPTION
FOREIGN KEY (FOOD_ID)
REFERENCES BM_FOOD(ID)
on delete cascade;
 
 
 
-- 회원 주문정보 테이블
CREATE TABLE BM_ORDER_USER (
    ORDER_NUM NUMBER PRIMARY KEY,
    STORE_ID NUMBER NOT NULL,
    USER_ID NUMBER NOT NULL,
    ORDER_DATE TIMESTAMP DEFAULT SYSDATE,
    PAY_METHOD VARCHAR2(30),
    DELEVERY_STATUS VARCHAR2(50) DEFAULT '주문접수 대기 중',
    PHONE VARCHAR2(20) NOT NULL,
    DELEVERY_ADDRESS1 NUMBER NOT NULL,
    DELEVERY_ADDRESS2 VARCHAR2(200) NOT NULL,
    DELEVERY_ADDRESS3 VARCHAR2(200),
    TOTAL_PRICE NUMBER NOT NULL,
    USED_POINT NUMBER DEFAULT 0,
    REQUEST VARCHAR2(2000),
    IMP_UID VARCHAR2(30) -- 아임포트 결제번호
);
 
 
CREATE TABLE BM_ORDER_DETAIL_USER (
    ORDER_NUM NUMBER,
    FOOD_INFO VARCHAR2(2000)
);
 
 
ALTER TABLE BM_ORDER_DETAIL_USER
ADD CONSTRAINT ORDER_DETAIL_USER
FOREIGN KEY (ORDER_NUM)
REFERENCES BM_ORDER_USER(ORDER_NUM)
on delete cascade;
 
 
 
 
 
 
-- 비회원 테이블
-- 회원 비회원 union all 하기위해 user_id 컬럼 추가
CREATE TABLE BM_ORDER_NON_USER (
    ORDER_NUM NUMBER PRIMARY KEY,
    STORE_ID NUMBER NOT NULL,
    USER_ID NUMBER NOT NULL,
    ORDER_DATE TIMESTAMP DEFAULT SYSDATE,
    PAY_METHOD VARCHAR2(30),
    DELEVERY_STATUS VARCHAR2(50) DEFAULT '주문접수 대기 중',
    PHONE VARCHAR2(20) NOT NULL,
    DELEVERY_ADDRESS1 NUMBER NOT NULL,
    DELEVERY_ADDRESS2 VARCHAR2(200) NOT NULL,
    DELEVERY_ADDRESS3 VARCHAR2(200),
    TOTAL_PRICE NUMBER NOT NULL,
    USED_POINT NUMBER DEFAULT 0,
    REQUEST VARCHAR2(2000),
    IMP_UID VARCHAR2(30) -- 아임포트 결제번호
);
 
 
 
CREATE TABLE BM_ORDER_DETAIL_NON_USER (
    ORDER_NUM NUMBER,
    FOOD_INFO VARCHAR2(2000)
);
 
ALTER TABLE BM_ORDER_DETAIL_NON_USER
ADD CONSTRAINT ORDER_DETAIL_NON_USER
FOREIGN KEY (ORDER_NUM)
REFERENCES BM_ORDER_NON_USER(ORDER_NUM)
on delete cascade;
 
 
 
 
-- 포인트 테이블
CREATE TABLE BM_POINT (
    USER_ID NUMBER,
    USED_DATE TIMESTAMP DEFAULT SYSDATE,
    INFO VARCHAR2(100) NOT NULL,
    POINT NUMBER NOT NULL
);
 
ALTER TABLE BM_POINT
ADD CONSTRAINT POINT
FOREIGN KEY (USER_ID)
REFERENCES BM_USER(ID)
on delete cascade;
 
 
 
 
 
CREATE TABLE BM_REVIEW (
    ORDER_NUM NUMBER PRIMARY KEY,
    STORE_ID NUMBER NOT NULL,
    REVIEW_CONTENT VARCHAR2(3000) NOT NULL,
    BOSS_COMMENT VARCHAR2(3000),
    REGI_DATE TIMESTAMP DEFAULT SYSDATE,
    USER_ID NUMBER NOT NULL,
    SCORE NUMBER NOT NULL,
    REVIEW_IMG VARCHAR2(200) 
);
 
ALTER TABLE BM_REVIEW
ADD CONSTRAINT REVIEW
FOREIGN KEY (ORDER_NUM)
REFERENCES BM_ORDER_USER(ORDER_NUM)
on delete cascade;
 
 
 
 
-- 찜하기 테이블
CREATE TABLE BM_LIKES (
    USER_ID NUMBER,
    STORE_ID NUMBER,
    LIKES_DATE TIMESTAMP DEFAULT SYSDATE
);
 
ALTER TABLE BM_LIKES
ADD CONSTRAINT LIKES_USER_ID
FOREIGN KEY (USER_ID)
REFERENCES BM_USER(ID)
on delete cascade;
 
ALTER TABLE BM_LIKES
ADD CONSTRAINT LIKES_STORE_ID
FOREIGN KEY (STORE_ID)
REFERENCES BM_STORE(ID)
on delete cascade;
 
CREATE TABLE BM_POINT (
    USER_ID NUMBER,
    USED_DATE TIMESTAMP DEFAULT SYSDATE,
    INFO VARCHAR2(100) NOT NULL,
    USED_POINT NUMBER NOT NULL
);
 
 
CREATE TABLE BM_GIFT_CARD (
    GIFT_CARD_NUM VARCHAR2(50) PRIMARY KEY,
    POINT NUMBER NOT NULL,
    INFO VARCHAR2(100) NOT NULL
);
 
 
 
 
 
 
 
INSERT INTO BM_STORE (ID, CATEGORY, STORE_NAME, STORE_ADDRESS1, STORE_ADDRESS2, STORE_PHONE) VALUES (STORE_ID_SEQ.NEXTVAL, 100, '도미노피자', '31099', '천안시 서북구 두정동 오성초등학교', '01012341234');
 
 
 
INSERT INTO BM_FOOD (ID, STORE_ID, FOOD_NAME, FOOD_PRICE, FOOD_DEC, FOOD_IMG, FOOD_THUMB) 
VALUES (FOOD_ID_SEQ.NEXTVAL, 1, '불고기피자', '11000', '불고기피자 입니다', '\img\none.gif', '\img\none.gif');
 
INSERT INTO BM_FOOD (ID, STORE_ID, FOOD_NAME, FOOD_PRICE, FOOD_DEC, FOOD_IMG, FOOD_THUMB) 
VALUES (FOOD_ID_SEQ.NEXTVAL, 1, '포테이토피자', '12000', '포테이토피자 입니다', '\img\none.gif', '\img\none.gif');
 
INSERT INTO BM_FOOD (ID, STORE_ID, FOOD_NAME, FOOD_PRICE, FOOD_DEC, FOOD_IMG, FOOD_THUMB) 
VALUES (FOOD_ID_SEQ.NEXTVAL, 1, '고구마피자', '4000', '고구마피자 입니다', '\img\none.gif', '\img\none.gif');
 
INSERT INTO BM_FOOD (ID, STORE_ID, FOOD_NAME, FOOD_PRICE, FOOD_DEC, FOOD_IMG, FOOD_THUMB) 
VALUES (FOOD_ID_SEQ.NEXTVAL, 1, '페퍼로니피자', '21000', '페퍼뢰니피자 입니다', '\img\none.gif', '\img\none.gif');
 
 
 
INSERT INTO BM_FOOD_OPTION VALUES (OPTION_ID_SEQ.NEXTVAL, 1, '치즈크러스트로 변경', 3000);
INSERT INTO BM_FOOD_OPTION VALUES (OPTION_ID_SEQ.NEXTVAL, 1, '파스타 추가', 4000);
INSERT INTO BM_FOOD_OPTION VALUES (OPTION_ID_SEQ.NEXTVAL, 1, '베이컨 토핑 추가', 1000);
INSERT INTO BM_FOOD_OPTION VALUES (OPTION_ID_SEQ.NEXTVAL, 1, '치즈 토핑 추가', 1000);
 
 
INSERT INTO BM_FOOD_OPTION VALUES (OPTION_ID_SEQ.NEXTVAL, 1, '치즈크러스트로 변경', 3000);
INSERT INTO BM_FOOD_OPTION VALUES (OPTION_ID_SEQ.NEXTVAL, 1, '파스타 추가', 4000);
INSERT INTO BM_FOOD_OPTION VALUES (OPTION_ID_SEQ.NEXTVAL, 1, '베이컨 토핑 추가', 1000);
INSERT INTO BM_FOOD_OPTION VALUES (OPTION_ID_SEQ.NEXTVAL, 1, '치즈 토핑 추가', 1000);
 
 
INSERT INTO BM_GIFT_CARD VALUES ('QKRTNALS' , 50000, '상품권 충전');
 
 
COMMIT;

```

<div style="text-align:center; font-size:0.8em;">Tables</div>

</div>
</details>

<br>
<br>
<br>
<br>
<br>

## 비밀번호 암호화 하기

---

비밀번호를 저장할때 암호화를 해준다.

build.gradle에 security implementation 추가

```java
implementation "org.springframework.boot:spring-boot-starter-security"
```

<div style="text-align:center; font-size:0.8em;">build.gradle</div>

<br>
<br>

코드를 추가하고 업데이트 후 다시 실행시켜 보면

![image]({{ site.baseurl }}/images/2023-06-12/20230615_163604.png)

<div style="text-align:center; font-size:0.8em;">로그인화면</div>

<br>
<br>

이런 로그인 화면이 뜬다.

콘솔창을 보면 서버가 실행될때 비밀번호가 뜨는데 Id 는 user, 비밀번호는 콘솔창에 나와있는 것을 치면 된다.

![image]({{ site.baseurl }}/images/2023-06-12/20230615_163759.png)

<div style="text-align:center; font-size:0.8em;">콘솔</div>

<br>
<br>

하지만 이건 그냥 진짜 security 로그인일뿐 배민 페이지 로그인이 아니다. 이를 커스텀해서 사용해야하기 때문에 코드를 짜준다.

config 패키지를 만들고 그 안에 SecurityConfig.java를 만들어준다.
<br>
<br>


```java
@EnableWebSecurity
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Bean
    public BCryptPasswordEncoder encodePwd() {
        return new BCryptPasswordEncoder();
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.csrf().disable();

        http.authorizeRequests()
                .antMatchers("/admin/**").hasRole("ADMIN")
                .antMatchers("/user/**").hasAnyRole("ADMIN, USER")
                .anyRequest().permitAll()
                .and()
                .formLogin()
                .loginPage("/")
                .loginProcessingUrl("/login")
                .and()
                .logout()
                .logoutSuccessUrl("/myPage");
    }
}
```

<div style="text-align:center; font-size:0.8em;">SecurityConfig.java</div>

<br>
<br>

 BCryptPasswordEncoder를 빈으로 등록해 비밀번호를 암호화 할수 있게했다.

 그리고 

 * antMatchers("/admin/**").hasRole("ADMIN") : "/admin"이 들어가는 주소는 ADMIN만 들어갈수 있게 설정

 * antMatchers("/user/**").hasAnyRole("ADMIN, USER") : "/user"가 들어가는 주소는 ADMIN과 USER만 들어갈수 있게 설정

 * .anyRequest().permitAll() : 그 외 다른 페이지들은 아무나 다 들어갈수 있음.

 * formLogin() <br>    .loginPage("/") <br>  .loginProcessingUrl("/login") : <br> 1. 권한이 없는데 권한이 있는 페이지에 들어가려 할 시 <br> 2. "/" 페이지로 이동 <br> 실제 로그인을 실행할 주소 "/login"

 * 로그아웃 성공 시 이동할 페이지 "/myPage" 

 <br>
<br>
<br>
<br>

그리고 Controller에서 회원가입을 할때 입력 받은 비밀번호를 암호화 할 수 있도록 코드를 수정해준다.

<br>

```java
@Autowired
    private BCryptPasswordEncoder pwdEncoder;
```

<div style="text-align:center; font-size:0.8em;">UserController.java 코드 추가</div>

<br>
<br>

```java
@PostMapping("/join")
    public String joinProc(Join join,Model model){
        String encPwd = pwdEncoder.encode(join.getPassword());  //코드추가
        join.setPassword(encPwd);   //코드 추가
        System.out.println(join);
        userService.join(join);
        return "redirect:/login";
    }
```

<div style="text-align:center; font-size:0.8em;">UserController.java 코드 추가</div>

<br>
<br>

<br>
<br>

다시 회원가입을 해보면


![image]({{ site.baseurl }}/images/2023-06-12/20230615_170744.png)

<div style="text-align:center; font-size:0.8em;">DB</div>


<br>
<br>

위에서 보이듯이 비밀번호가 암호화 되서 들어간다.