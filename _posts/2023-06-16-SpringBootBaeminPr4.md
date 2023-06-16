---
layout: single
title: "(SpringBoot) 배민 클론 코딩4"
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

## 목차


<br>
<br>
<br>

## Error 해결하기

<br>
<br>

로그인이 제데로 작동하지 않는다..

우선 테이블 생성할 때 role 칼럼의 default 값이 잘못 됐다. 'ROLE_USER' -> 'USER' 로 고쳐준다.

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
    ROLE VARCHAR2(20) DEFAULT 'USER'  --수정
); 
```

<div style="text-align:center; font-size:0.8em;">테이블</div>

<br>
<br>

다시 회원가입을 하고 로그인을 했더니 그래도 이번에는 마이페이지로 잘 넘어간다.

![image]({{ site.baseurl }}/images/2023-06-16/20230616_121635.png)

<div style="text-align:center; font-size:0.8em;">로그인 후 마이페이지 화면</div>

<br>
<br>
<br>

하지만 다시 로그인 버튼이 생기는 것을 보니 제데로 세션을 물고 오질 못하는 것 같다. 확인해보니 역시나 세션이 이 없다.

![image]({{ site.baseurl }}/images/2023-06-16/20230616_123201.png)

<div style="text-align:center; font-size:0.8em;">f12로 세션 확인</div>

<br>
<br>

LoginSuccess.java 와 LoginFail.java에 출력문을 넣어보니 그래도 유저 정보는 잘 갖고 온다.

![image]({{ site.baseurl }}/images/2023-06-16/20230616_124016.png)

<div style="text-align:center; font-size:0.8em;">유저정보 잘 가져오는 모습</div>

<br>
<br>

그렇다면 일단 LoginFail에서 requestDispatcher가 제데로 작동 안하는 것으로 보이고, LoginSuccess에서는 myPage에서 **SPRING_SECURITY_CONTEXT != null** 이부분을 확인해 봐야할 것 같다.