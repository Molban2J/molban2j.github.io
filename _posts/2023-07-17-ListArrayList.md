---
layout: single
title: "(Java) List와 ArrayList"
categories:
  - Java
---

## List와 ArrayList의 차이

<br>

문제를 풀다보니 일반적인 배열과 ArrayList의 차이점이 뭔지 궁금해졌다.

먼저 객체 생성하는 방법은 다음과 같다.

List

```java
//int list 생성
int a[] = new int[n];

```

ArrayList

```java
//int ArrayList 생성
ArrayList<Integer> a = new ArrayList<Integer>();

```


<br>
<br>

우선 가장 큰 차이점을 말한다면

List는 **배열의 크기가 고정되어 있고**

ArrayList는 **배열의 크기가 가변적**이라는 것이다.

나머지 특징들은 표로 보겠다.

||List|ArrayList|
|:--:|:--:|:--:|
|사이즈|초기화시 고정|사이즈가 가변적임|
|속도|초기화시 메모리가 할당되기 때문에 빠름|데이터 추가/삭제시 메모리를 재할당 하기 때문에 느림|
|크기 변경| 불가 |추가/삭제 가능<br>add(),remove()|
|다차원|int a[][][] = new int[3][3][3];| 불가능|

||List|ArrayList|
|:--:|:--|:--|
|장점|- 인덱스를 통한 검색이 용이함<br>- 연속적이므로 메모리 관리가 편함|- 포인터를 가지고 있어 삽입/삭제에 용이<br>- 동적이므로 크기가 정해져있지 않음<br>- 메모리 재사용 편리<br>- 불연속적이라 메모리 관리 편리|
|단점| - 크기가 고정이라 엘리멘트가 삭제되면, 삭제된 공간을 빈공간으로 남겨놔야함 => 메모리 낭비<br> - 정적이라 컴파일 이전에 메모리 할당을 해줘야함.<br> - 컴파일 이후 메모리 변경 불가 | -검색 기능이 좋지 못함<br> - 포인터를 통해 다음 데이터를 가르키므로 추가적인 메모리 공간 발생|



