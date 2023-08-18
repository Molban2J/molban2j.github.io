---
layout: single
title: "(Spring) 스프링 예제 2 - 객체지향 원리 적용"
categories:
  - Spring
---

## 주문할인 도메인 설계 / 개발 / 실행 및 테스트

<br>
<br>

### 주문 할인 도메인 설계

<br>
<br>

- 주문과 할인 정책
    - 회원은 상품을 주문할 수 있다.
    - 회원 등급에 따라 할인 정책을 적용할 수 있다.
    - 할인 정책은 모든 VIP는 1000원을 할인해주는 고정 금액 할인을 적용(**나중에 변경 될 수 있다.**)
    - 할인 정책은 변경 가능성이 높다. 회사의 기본 할인 정책을 아직 정하지못했고, 오픈 직전까지 고민을 미루고 싶다. 최악의 경우 할인을 적용하지 않을 수도 있다.(**미확정**)

<br>
<br>

1. 주문 생성 > 2. 회원 조회 > 3. 할인 적용 > 4. 주문 결과 반환

<br>
<br>

**역할과 구현을 분리**해서 정액이든 정률이든 자유롭게 구현 객체를 조립할 수 있도록 설계

<br>
<br>
<br>

### 주문, 할인 개발

<br>
<br>

```java
package hello.core.discount;

import hello.core.member.Member;

public interface DiscountPolicy {
    /**
     * @return 할인 대상 금액
     */
    int discount(Member member, int price);
}
```

<div style="text-align:center; font-size:0.8em;">할인 정책 인터페이스 DiscountPolicy.java</div>

<br>
<br>

```java
package hello.core.discount;

import hello.core.member.Grade;
import hello.core.member.Member;

public class FixDiscountPolicy implements DiscountPolicy{

    private int discountFixAmount = 1000;
    @Override
    public int discount(Member member, int price) {
        if(member.getGrade() == Grade.VIP){
            return discountFixAmount;
        } else{
            return 0;
        }
    }
}

```

<div style="text-align:center; font-size:0.8em;">정액 할인 구현체 FixDiscountPolicy.java</div>

<br>
<br>

```java
package hello.core.order;

public class Order {
    private Long memberId;
    private String itemName;
    private int itemPrice;
    private int discountPrice;

    public Order(Long memberId, String itemName, int itemPrice, int discountPrice) {
        this.memberId = memberId;
        this.itemName = itemName;
        this.itemPrice = itemPrice;
        this.discountPrice = discountPrice;
    }

    public int calculatePrice(){
        return itemPrice - discountPrice;
    }

    public Long getMemberId() {
        return memberId;
    }

    public void setMemberId(Long memberId) {
        this.memberId = memberId;
    }

    public String getItemName() {
        return itemName;
    }

    public void setItemName(String itemName) {
        this.itemName = itemName;
    }

    public int getItemPrice() {
        return itemPrice;
    }

    public void setItemPrice(int itemPrice) {
        this.itemPrice = itemPrice;
    }

    public int getDiscountPrice() {
        return discountPrice;
    }

    public void setDiscountPrice(int discountPrice) {
        this.discountPrice = discountPrice;
    }

    @Override
    public String toString() {
        return "Order{" +
                "memberId=" + memberId +
                ", itemName='" + itemName + '\'' +
                ", itemPrice=" + itemPrice +
                ", discountPrice=" + discountPrice +
                '}';
    }
}


```

<div style="text-align:center; font-size:0.8em;">주문 엔티티 Order.java</div>

<br>
<br>

```java
package hello.core.order;

public interface OrderService {
    Order createOrder(Long memberId, String itemName, int itemPrice);
}


```

<div style="text-align:center; font-size:0.8em;">주문 서비스 인터페이스 OrderService.java</div>

<br>
<br>

```java
package hello.core.order;

import hello.core.discount.DiscountPolicy;
import hello.core.discount.FixDiscountPolicy;
import hello.core.discount.RateDiscountPolicy;
import hello.core.member.Member;
import hello.core.member.MemberRepository;
import hello.core.member.MemoryMemberRepository;

public class OrderServiceImpl implements OrderService{

    private final MemberRepository memberRepository = new MemoryMemberRepository();

    @Override
    public Order createOrder(Long memberId, String itemName, int itemPrice) {
        Member member = memberRepository.findById(memberId);
        int dicountPrice = discountPolicy.discount(member, itemPrice);
        return new Order(memberId, itemName, itemPrice, dicountPrice);
    }
}


```

<div style="text-align:center; font-size:0.8em;">주문 서비스 구현체 FixDiscountPolicy.java</div>

<br>
<br>
<br>
<br>

### 테스트

<br>
<br>


```java
package hello.core.order;

import hello.core.member.Grade;
import hello.core.member.Member;
import hello.core.member.MemberService;
import hello.core.member.MemberServiceImpl;
import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.Test;

public class OrderServiceTest {

    MemberService memberService = new MemberServiceImpl();
    OrderService orderService = new OrderServiceImpl();

    @Test
    void testOrder(){
        Long memberId = 1L;

        Member member = new Member(memberId, "memberA", Grade.VIP);
        memberService.join(member);

        Order order = orderService.createOrder(memberId, "itemA", 10000);
        Assertions.assertThat(order.getDiscountPrice()).isEqualTo(1000);
    }
}


```

<div style="text-align:center; font-size:0.8em;">주문,할인 서비스 테스트 OrderServiceTest.java</div>

<br>
<br>
<br>
<br>

## 새로운 할인 정책 개발

<br>
<br>

기존 정액 할인에서 정률 할인으로 변경하고자 한다.

객체지향 설계를 준수 했으니 정률 할인 서비스 클래스를 생성해준다.

<br>
<br>


```java
package hello.core.discount;

import hello.core.member.Grade;
import hello.core.member.Member;

public class RateDiscountPolicy implements DiscountPolicy{

    private int discountPercent = 10;
    @Override
    public int discount(Member member, int price) {
        if(member.getGrade() == Grade.VIP){
            return price * discountPercent/100;
        } else{
            return 0;
        }
    }
}


```

<div style="text-align:center; font-size:0.8em;">정률 할인 서비스 구현체 RateDiscountPolicy.java</div>

<br>

주문 서비스 구현체에 정액 할인 정책을 정률 할인 정책으로 바꿔준다.

<br>

```java
public class OrderServiceImpl implements OrderService{

    private final MemberRepository memberRepository = new MemoryMemberRepository();
//    private final DiscountPolicy discountPolicy = new FixDiscountPolicy();    변경 전
    private final DiscountPolicy discountPolicy = new RateDiscountPolicy();   //변경 후
    @Override
    public Order createOrder(Long memberId, String itemName, int itemPrice) {
        Member member = memberRepository.findById(memberId);
        int dicountPrice = discountPolicy.discount(member, itemPrice);
        return new Order(memberId, itemName, itemPrice, dicountPrice);
    }
}
```

<div style="text-align:center; font-size:0.8em;">주문 서비스 구현체 RateDiscountPolicy.java</div>

<br>
<br>


### 테스트


<br>
<br>


```java
package hello.core.discount;

import hello.core.member.Grade;
import hello.core.member.Member;
import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

import static org.assertj.core.api.Assertions.*;
import static org.junit.jupiter.api.Assertions.*;

class RateDiscountPolicyTest {
    RateDiscountPolicy discountPolicy = new RateDiscountPolicy();

    @Test
    @DisplayName("VIP는 10% 할인이 적용되어야 한다")
    void vip_o(){
        //given
        Member member = new Member(1L, "memberVIP", Grade.VIP);
        //when
        int discount = discountPolicy.discount(member, 10000);

        //then
        assertThat(discount).isEqualTo(1000);
    }

    @Test
    @DisplayName("VIP가 아니면 10% 할인이 적용이 안되야 한다")
    void vip_x(){
        //given
        Member member = new Member(1L, "memberVIP", Grade.BASIC);
        //when
        int discount = discountPolicy.discount(member, 10000);

        //then
        assertThat(discount).isEqualTo(1000);
    }

}
```

<div style="text-align:center; font-size:0.8em;">정액 할인 서비스 구현체 테스트 RateDiscountPolicyTest.java</div>

<br>
<br>
<br>
<br>

## 문제점

<br>
<br>

할인 정책을 변경하려면 클라이언트인 *OrderServiceImpl* 코드를 고쳐야 한다.

#### 하지만 나는 객체지향 설계를 준수해서 설계했는데?

<br>

- 역할과 구현을 분리 -> OK
- 다형성 활용, 인터페이스와 구현객체 분리 ->OK
- **OCP / DIP 등 객체지향 설계 원칙을 충실히 준수했다** ->> **그렇게 보이지만 사실은 아니다**
- **DIP** : 주문 서비스 클라이언트(OrderServiceImpl)는 DiscountPolicy 인터페이스에 의존하는데?
    - **추상(인터페이스(DiscountPolicy))에 의존** 하지만 **구체(구현) 클래스(FixDiscountPolicy, RateDiscountPolicy)에도 의존**한다

- **OCP** : 변경에는 닫혀있고 확장에는 열려있는가? -> **NO**
    - 현재 코드는 정책을 변경하려면 클라이언트 코드도 변경해줘야한다(FixDiscountPolicy -> RateDiscountPolicy) -> **OCP 위반**

<br>
<br>
<br>

## 문제 해결 방법

<br>
<br>

**인터페이스에만 의존하도록 설계 변경**