---
layout: single
title: "(Coding Test) 2단계 - 조건문"
categories:
  - CodingTest
---

2023-06-22

## 2단계 - 조건문



<br>
<br>

## 목차

* [1330(두 수 비교하기)](#1330번-문제)
* [9498(시험 성적)](#9498번-문제)
* [2753(윤년)](#2753번-문제)
* [14681(사분면 고르기)](#14681번-문제)

<br>
<br>

## 1330번 문제

<br>

### 문제

두 정수 A와 B가 주어졌을 때, A와 B를 비교하는 프로그램을 작성하시오.

<br>

### 입력

 첫째 줄에 A와 B가 주어진다. A와 B는 공백 한 칸으로 구분되어져 있다.

<br>

### 출력

첫째 줄에 다음 세 가지 중 하나를 출력한다.

* A가 B보다 큰 경우에는 '>'를 출력한다.
* A가 B보다 작은 경우에는 '<'를 출력한다.
* A와 B가 같은 경우에는 '=='를 출력한다.

<br>

<br>



```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main{
    public static void main(String args[]) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        
        String input = br.readLine();
        
        StringTokenizer st = new StringTokenizer(input, " ");
        
        int a = Integer.parseInt(st.nextToken());
        int b = Integer.parseInt(st.nextToken());
        
        if(a > b){
            System.out.println(">");
        } 
        else if(a <b){
            System.out.println("<");
        } else {
            System.out.println("==");
        }
    }
}
```

<div style="text-align:center; font-size:0.8em;">정답</div>

<br>

<br>

<br>

<br>

## 9498번 문제 

<br>

### 문제

 시험 점수를 입력받아 90 ~ 100점은 A, 80 ~ 89점은 B, 70 ~ 79점은 C, 60 ~ 69점은 D, 나머지 점수는 F를 출력하는 프로그램을 작성하시오.

<br>

### 입력

 첫째 줄에 시험 점수가 주어진다. 시험 점수는 0보다 크거나 같고, 100보다 작거나 같은 정수이다.

<br>

### 출력

 시험 성적을 출력한다.

<br>
<br>

```java
import java.io.IOException;
import java.io.BufferedReader;
import java.io.InputStreamReader;

public class Main{
    public static void main(String args[]) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        
        int A = Integer.parseInt(br.readLine());
        
        if(A >= 90){
            System.out.println("A");
        } else if(89>=A && A>=80){
            System.out.println("B");
        } else if(79>=A && A>=70){
            System.out.println("C");
        } else if(69>=A && A>=60){
            System.out.println("D");
        } else {
            System.out.println("F");
        }
    }
}
```

<div style="text-align:center; font-size:0.8em;">정답</div>


<br>
<br>

```java
import java.io.IOException;
public class Main{
    public static void main(String args[]) throws IOException{
        int A = Character.getNumericValue((char)System.in.read());
        
        if(A >= 90){
            System.out.println("A");
        } else if(89>=A && A>=80){
            System.out.println("B");
        } else if(79>=A && A>=70){
            System.out.println("C");
        } else if(69>=A && A>=60){
            System.out.println("D");
        } else {
            System.out.println("F");
        }
    }
}
```

<div style="text-align:center; font-size:0.8em;">이런식으로도 해봤지만 어째서인지 오답이 뜬다.</div>

<br>
<br>
<br>
<br>


## 2753번 문제 

<br>

### 문제

 연도가 주어졌을 때, 윤년이면 1, 아니면 0을 출력하는 프로그램을 작성하시오.

윤년은 연도가 4의 배수이면서, 100의 배수가 아닐 때 또는 400의 배수일 때이다.

예를 들어, 2012년은 4의 배수이면서 100의 배수가 아니라서 윤년이다. 1900년은 100의 배수이고 400의 배수는 아니기 때문에 윤년이 아니다. 하지만, 2000년은 400의 배수이기 때문에 윤년이다.

<br>

### 입력

 첫째 줄에 연도가 주어진다. 연도는 1보다 크거나 같고, 4000보다 작거나 같은 자연수이다.

<br>

### 출력

 첫째 줄에 윤년이면 1, 아니면 0을 출력한다.

<br>
<br>

```java
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.BufferedReader;

public class Main{
    public static void main(String args[]) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int a = Integer.parseInt(br.readLine());
        
        if(a%4 == 0 && a%100 != 0 || a%400==0){
            System.out.println(1);
        } else {
            System.out.println(0);
        }
    }
}
```

<div style="text-align:center; font-size:0.8em;">정답</div>

<br>
<br>
<br>
<br>

## 14681번 문제 

<br>

### 문제

 흔한 수학 문제 중 하나는 주어진 점이 어느 사분면에 속하는지 알아내는 것이다. 사분면은 아래 그림처럼 1부터 4까지 번호를 갖는다. "Quadrant n"은 "제n사분면"이라는 뜻이다. 

예를 들어, 좌표가 (12, 5)인 점 A는 x좌표와 y좌표가 모두 양수이므로 제1사분면에 속한다. 점 B는 x좌표가 음수이고 y좌표가 양수이므로 제2사분면에 속한다.

점의 좌표를 입력받아 그 점이 어느 사분면에 속하는지 알아내는 프로그램을 작성하시오. 단, x좌표와 y좌표는 모두 양수나 음수라고 가정한다.

<br>

### 입력

첫 줄에는 정수 x가 주어진다. (−1000 ≤ x ≤ 1000; x ≠ 0) 다음 줄에는 정수 y가 주어진다. (−1000 ≤ y ≤ 1000; y ≠ 0)

<br>

### 출력

점 (x, y)의 사분면 번호(1, 2, 3, 4 중 하나)를 출력한다.

<br>
<br>

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.IOException;
import java.util.StringTokenizer;

class Main{
    public static void main(String args[])throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        
        String input = br.readLine();
        
        StringTokenizer st = new StringTokenizer(input);
        
        int a = Integer.parseInt(st.nextToken());
        int b = Integer.parseInt(st.nextToken());
        
        if(a > 0 && b >0){System.out.println(1);}
        else if (a<0 && b>0){System.out.println(2);}
        else if (a<0 && b<0){System.out.println(3);}
        else {System.out.println(4);}
    }
}
```

<div style="text-align:center; font-size:0.8em;">런타임 에러</div>

<br>
<br>

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.IOException;
import java.util.StringTokenizer;

class Main{
    public static void main(String args[])throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        
        int a = Integer.parseInt(br.readLine());
        int b = Integer.parseInt(br.readLine());
        
        if(a > 0 && b >0){System.out.println(1);}
        else if (a<0 && b>0){System.out.println(2);}
        else if (a<0 && b<0){System.out.println(3);}
        else {System.out.println(4);}
    }
}
```

<div style="text-align:center; font-size:0.8em;">정답</div>

<br>
<br>
<br>
<br>