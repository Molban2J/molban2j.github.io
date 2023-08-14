---
layout: single
title: "(Spring) 좋은 객체 지향 프로그래밍이란? + SOLID"
categories:
  - Spring
---

## 좋은 객체지향 프로그래밍이란?

<br>
<br>

### 목차

<br>

- [객체 지향의 꽃 다형성](#객체지향의-꽃-다형성)
- [SOLID](#좋은-객체-지향-설계의-5가지-원칙solid)
    - [SRP](#srp-단일책임-원칙single-responsibility-principle)
    - [OCP](#ocp-개방-폐쇄-원칙-openclosed-princliple---가장-중요)
    - [LSP](#lsp-리스코프-치환-원칙liskov-substituion-principle)
    - [ISP](#isp-인터페이스-분리-원칙interface-segregation-principle)
    - [DIP](#dip-의존관계-역전-원칙dependency-inversion-principle---가장-중요)

<br>
<br>

### 객체지향의 꽃! 다형성



- 장점
    - 클라이언트는 대상의 역할(인터베이스)만 알면 된다.
    - 클라이언트는 구현 대상의 **내부구조를 몰라도 된다.**
    - 클라이언트는 구현 대상의 **내부 구조가 변경**되어도 영향을 받지 않는다.
    - 클라이언트는 구현 **대상 자체를 변경**해도 영향을 받지 않는다.

    -> **확장 가능한 설계** + **유연**하고 **변경**이 용이하다.

<br>

- **자바 언어**
    - 자바 언어의 다형성 활용
        - 역할 = 인터페이스
        - 구현 = 인터페이스를 구현한 클래스, 구현 객체
    - 객체를 설계할 때 **역할**과 **구현**을 명확히 분리
    - 객체 설계시 **역할(인터페이스)을 먼저 부여**하고, 그역할을 수행하는 **구현 객체** 만들기 
    - **오버라이딩**

    -> 인터페이스를 안정적으로 잘 설계하는 것이 중요


<br>
<br>

**스프링은 다형성을 극대화 해서 이용할 수 있게 도와준다.**

- 스프링에서 이야기하는 제어의 역전(IoC), 의존관계 주입(DI)은 다형성을 활용해서 역할과 구현을 편리하게 다룰 수 있도록 지원

<br>
<br><br>
<br>

## 좋은 객체 지향 설계의 5가지 원칙(SOLID)

<br>
<br>

### SOLID

- **SRP** : 단일 책임 원칙(Single Responsibility Principle)
- **OCP** : 개방-폐쇄 원칙(Open/Closed Principle)
- **LSP** : 리스코프 치환 원칙(Liskov Substitution Principle)
- **ISP** : 인터페이스 분리 원칙(Interface Segregation Principle)
- **DIP** : 의존관계 역전 원칙(Dependency Inversion Principle)

<br>
<br>

### SRP 단일책임 원칙(Single Responsibility Principle)

<br>

- 한 클래스는 하나의 책임만 가져야한다.
- **중요한 기준은 변경**이다. 변경이 있을 때 파급효과가 적으면 단일 책임 원칙을 잘 따른 것

<br>
<br>
<br>

### OCP 개방-폐쇄 원칙 (Open/Closed Princliple) - 가장 중요!

<br>

- 소프트웨어 요소는 **확장에는 열려** 있으나 **변경에는 닫혀** 있어야 한다.
- 인터페이스를 구현한 새로운 클래스를 하나 만들어서 새로운 기능 구현

**문제점**

- 클래스가 변경되면 클라이언트가 구현클래스를 직접 선택해야함

eg)

```java
public class MemberService {
    //private MemberRepository memberRepository = new MemoryRepository(); 변경 전
    private MemberRepository memberRepository = new JdbcMemberRepository(); // 변경 후
}
```

- **구현 객체를 변경하려면 클라이언트 코드를 변경해야한다.**
- **분명 다형성을 사용했지만 OCP원칙을 지킬 수 없다**

<br>

문제 해결

- 객체를 생성하고, 연관 관계를 맺어주는 별도의 조립, 설정자가 필요 -> **Spring Container**

<br>
<br>
<br>

### LSP 리스코프 치환 원칙(Liskov Substituion Principle) 

<br>

- 프로그램의 객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야한다.
- 다형성에서 하위 클래스는 인터페이스 규약을 다 지켜야한다는 것, 다형성을 지원하기 위한 원칙, 인터페이스를 구현한 구현체를 믿고 사용하려면, 이 원칙이 필요하다.

    eg) 자동차 인터페이스 엑셀은 앞으로만 가야함. 뒤로 가게되면 LSP위반. 느리더라도 앞으로 가야함.

<br>
<br>
<br>

### ISP 인터페이스 분리 원칙(Interface Segregation Principle)

<br>

- 특정 클라이언트를 위한 인터페이스 여러 개가 범용 인터페이스 하나보다 낫다.

    eg)
    - 자동차 인터페이스 -> 운전 인터페이스, 정비 인터페이스로 분리
    - 사용자 인터페이스 -> 운전자 클라이언트, 정비사 클라이언트로 분리

- 인터페이스가 명확해지고, 대체 가능성이 높아진다.

<br>
<br>
<br>

### DIP 의존관계 역전 원칙(Dependency Inversion Principle) - 가장 중요!

<br>

- 프로그래머는 "추상화(**인터페이스**)에 의존해야지, 구체화(**구현 클래스**)에 의존하면 안된다." **의존성 주입**은 이 원칙을 따르는 방법중 하나다.

- 구현체에 의존하게 되면 변경이 어려워짐.

- 그런데 [OCP](#ocp-개방-폐쇄-원칙-openclosed-princliple---가장-중요)에서 설명한 MemberService는 인터페이스에 의존하지만 구현 클래스도 동시에 의존

    - MemberService 클라이언트가 구현클래스를 직접 선택
    
    -> **DIP**위반


<br>
<br>
<br>
<br>

### 정리

- 객체 지향의 핵심은 **다형성**
- 다형성 만으로는 쉽게 부품을 갈아 끼우듯이 개발할 수 없다

-> **다형성 만으로는 OCP,DIP를 지킬 수 없다.**

<br>

뭔가 더 필요하다..
