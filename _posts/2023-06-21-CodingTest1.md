---
layout: single
title: "(Coding Test) 1단계 - 입출력과 사칙연산(2557, 1000, )"
categories:
  - CodingTest
---

## 1단계 - 입출력과 사칙연산

<br>
<br>

### 목차

* [2557번(Hello World 출력)](#2557번-문제)
* [1000번(사칙연산 - A+B)](#1000번-문제)
* [1008번(사칙연산 - A/B)](#1008번-문제)
* [10869번(사칙연산)](#10869번-문제)
* [10926번(문자열 출력)](#10926번-문제)
* [18108번](#18108번-문제)
* [10430번(나머지 연산)](#10430번-문제)
* [2588번](#2588번-문제)
* [11382번(문자열 입력 - BufferedReader)](#11382번-문제)
* [10171번(이스케이프문자)](#10171ebb288-ebacb8eca09c-1)
* [10172번(이스케이프문자)](#10172번-문제)

<br>
<br>

## 2557번 문제

<br>

문제 : Hello Word! 를 화면에 출력하는 문제

첫번째 시도


```java
class Test{
    public void main(args[]){
        System.out.println("Hello World");
    }
}
```

<div style="text-align:center; font-size:0.8em;">컴파일 에러 - error: identifier expected</div>

<br>

아예 백지에 작성하듯이 아무것도 없는 입력란을 보고 당황했다.

class앞에 public, main매서드 앞에 static, args[]앞에 String이 빠졌다..

<br>
<br>
<br>

두번째 시도

```java
public class Test{
    public static void main(String args[]){
        System.out.println("Hello World");
    }
}
```

<div style="text-align:center; font-size:0.8em;">컴파일 에러 - error: class Test is public, should be declared in a file named Test.java</div>

<br>

파일명이 Test여야 한다는데, 아무래도 기본 파일명이 Main으로 되어있는거 같아서 파일명을 고쳐줬다.

그랬는대도 틀렸다고 나왔는데, 이는 "Hello World!" 라고 나와야하는데 느낌표가 빠져서 그렇다. 예시 화면에 나온 그대로 나와야하나보다.

<br>
<br>
<br>

두번째 시도

```java
public class Main{
    public static void main(String args[]){
        System.out.println("Hello World!");
    }
}
```

<div style="text-align:center; font-size:0.8em;">최종</div>

<br>
<br>
<br>

<br>
<br>


## 1000번 문제

<br>

문제: 두 정수 A와 B를 입력받아 화면에 A+B 값을 출력하시오.

<br>

막막했다. 분명 Scan 뭐시기를 써야하는데 도저히 어떻게 쓰는지 기억이 안나는 것이다. 그래서 처음에는 C언어에서 쓰는 scanf가 생각나 해보려했지만, 결국 기억이 안나 인터넷으로 찾아봤다. 

답은 Scanner 객체 생성이었다..

```java
Scanner sc = new Scanner(System.in)
```

<div style="text-align:center; font-size:0.8em;">Scanner</div>

<br>

이걸 까먹다니. 바로 작성해주도록 한다.


<br>

<br>


```java
public class Main{
    public static void main(String args[]){
        Scanner in = new Scanner(System.in);
        int a = in.nextInt();
        int b = in.nextInt();
        
        System.out.println(a+b);
    }
}
```

<div style="text-align:center; font-size:0.8em;">컴파일 에러 - error: cannot find symbol</div>

<br>

Scanner를 인식을 못하는것 같은데, 알고보니 임포트도 직접 작성해줘야 한다.

<br>
<br>
<br>


```java
import java.util.Scanner;

public class Main{
    public static void main(String args[]){
        Scanner in = new Scanner(System.in);
        int a = in.nextInt();
        int b = in.nextInt();
        
        System.out.println(a+b);
    }
}
```

<div style="text-align:center; font-size:0.8em;">정답</div>

<br>
<br>
<br>
<br>


## 1001번 문제, 10998번 문제

<br>

기본 사칙연산이므로 생략

<br>
<br>
<br>
<br>

## 1008번 문제

<br>

문제: A, B를 입력받아 A/B를 출력하시오.

<br>

```java
import java.util.Scanner;

public class Main{
    public static void main(String args[]){
        Scanner sc = new Scanner(System.in);
        
        int a = sc.nextInt();
        int b = sc.nextInt();
        
        System.out.println(a/b);
    }
}
```

<div style="text-align:center; font-size:0.8em;">첫번째 시도</div>


<br>
<br>

쉽네하고 넘어가려던 찰나 오답이 떠버렸다. 문제는 형변환의 문제인데, 소수점까지 표시하려면 double형으로 바꿔줘야하는데 이걸 간과한 것이다. 

그래서 double 변수 하나더 생성해줬다.

<br>
<br>

```java
import java.util.Scanner;

public class Main{
    public static void main(String args[]){
        Scanner sc = new Scanner(System.in);
        
        int a = sc.nextInt();
        int b = sc.nextInt();

        double c = a/b;
        
        System.out.println(c);
    }
}
```

<div style="text-align:center; font-size:0.8em;">두번째 시도</div>

<br>
<br>

또 오답! 멍청하게 c만 double이면 뭐하나 a/b가 정수형으로 나오는데..
앞에 강제형변환 해준다.

<br>
<br>

```java
import java.util.Scanner;

public class Main{
    public static void main(String args[]){
        Scanner sc = new Scanner(System.in);
        
        int a = sc.nextInt();
        int b = sc.nextInt();

        double c = (double)a/b;
        
        System.out.println(c);
    }
}
```

<div style="text-align:center; font-size:0.8em;">정답</div>

<br>
<br>
<br>
<br>

## 10869번 문제

<br>

문제: 두 자연수 A와 B가 주어진다. 이때, A+B, A-B, A*B, A/B(몫), A%B(나머지)를 출력하는 프로그램을 작성하시오. 

<br>

```java
import java.util.Scanner;

public class Main{
    public static void main(String args[]){
        Scanner sc = new Scanner(System.in);
        
        int a = sc.nextInt();
        int b = sc.nextInt();
        
        System.out.printf((a+b)+"\n"+(a-b)+"\n"+(a*b)+"\n"+(a/b)+"\n"+(a%b));
    }
}
```

<div style="text-align:center; font-size:0.8em;">정답1</div>

<br>
<br>


```java
import java.util.Scanner;

public class Main{
    public static void main(String args[]){
        Scanner sc = new Scanner(System.in);
        
        int a = sc.nextInt();
        int b = sc.nextInt();
        
        System.out.println(a+b);
        System.out.println(a-b);
        System.out.println(a*b);
        System.out.println(a/b);
        System.out.println(a%b);
    }
}
```

<div style="text-align:center; font-size:0.8em;">정답2</div>

<br>
<br>

신기하게도 정답2가 컴파일 시간이 좀더 빠르다. 한줄에 다 몰아서 쓰는게 그래도 빠를거 같았는데 아니었다.


<br>
<br>
<br>
<br>

## 10926번 문제

<br>

문제: 준하는 사이트에 회원가입을 하다가 joonas라는 아이디가 이미 존재하는 것을 보고 놀랐다. 준하는 놀람을 ??!로 표현한다. 준하가 가입하려고 하는 사이트에 이미 존재하는 아이디가 주어졌을 때, 놀람을 표현하는 프로그램을 작성하시오.

```java
import java.util.Scanner;

public class Main{
    public static void main(String args[]){
        Scanner sc = new Scanner(System.in);
        
        String name = sc.next();
        
        System.out.println(name+"??!");
    }
}
```

<div style="text-align:center; font-size:0.8em;">정답</div>

<br>
<br>
<br>
<br>

## 18108번 문제

<br>

문제: ICPC Bangkok Regional에 참가하기 위해 수완나품 국제공항에 막 도착한 팀 레드시프트 일행은 눈을 믿을 수 없었다. 공항의 대형 스크린에 올해가 2562년이라고 적혀 있던 것이었다.

불교 국가인 태국은 불멸기원(佛滅紀元), 즉 석가모니가 열반한 해를 기준으로 연도를 세는 불기를 사용한다. 반면, 우리나라는 서기 연도를 사용하고 있다. 불기 연도가 주어질 때 이를 서기 연도로 바꿔 주는 프로그램을 작성하시오.

<br>

```java
import java.util.Scanner;

public class Main{
    public static void main(String args[]){
        Scanner sc = new Scanner(System.in);
        
        int y = sc.nextInt();
        
        int x = 2541-1998;
        
        System.out.println(y - x);
    }
}
```

<div style="text-align:center; font-size:0.8em;">정답</div>

<br>
<br>
<br>
<br>

## 10430번 문제

<br>

문제: (A+B)%C는 ((A%C) + (B%C))%C 와 같을까?

(A×B)%C는 ((A%C) × (B%C))%C 와 같을까?

세 수 A, B, C가 주어졌을 때, 위의 네 가지 값을 구하는 프로그램을 작성하시오.

첫째 줄에 (A+B)%C, 둘째 줄에 ((A%C) + (B%C))%C, 셋째 줄에 (A×B)%C, 넷째 줄에 ((A%C) × (B%C))%C를 출력한다.

<br>


```java
import java.util.Scanner;

public class Main{
    public static void main(String args[]){
        Scanner sc = new Scanner(System.in);
        
        int a = sc.nextInt();
        int b = sc.nextInt();
        int c = sc.nextInt();
        
        System.out.println((a+b)%c);
        System.out.println(((a%c)+(b%c))%c);
        System.out.println((a*b)%c);
        System.out.println(((a%c)*(b%c))%c);
    }
}
```

<div style="text-align:center; font-size:0.8em;">정답</div>

<br>
<br>
<br>
<br>

## 2588번 문제

<br>
<br>  

```java
import java.util.Scanner;

public class Main{
    public static void main(String args[]){
        Scanner sc = new Scanner(System.in);
        
        int a = sc.nextInt();
        int b = sc.nextInt();
        
        System.out.println(a*(b%10));
        System.out.println(a*((b%100)/10));
        System.out.println(a*(b/100));
        System.out.println(a*b);
    }
}
```

<div style="text-align:center; font-size:0.8em;">정답</div>

<br>
<br>
<br>
<br>

## 11382번 문제

<br>

문제: 꼬마 정민이는 이제 A + B 정도는 쉽게 계산할 수 있다. 이제 A + B + C를 계산할 차례이다!

<br>

입력: 첫 번째 줄에 A, B, C (1 ≤ A, B, C ≤ 1012)이 공백을 사이에 두고 주어진다.

<br>

출력: A+B+C의 값을 출력한다.

<br>  

```java
import java.util.Scanner;

public class Main{
    public static void main(String args[]){
        Scanner sc = new Scanner(System.in);
        
        int a = sc.nextInt();
        int b = sc.nextInt();
        int c = sc.nextInt();
        
        System.out.println(a+b+c);
        
    }
}
```

<div style="text-align:center; font-size:0.8em;">런타임 에러</div>

<br>
<br>

런타임을 줄이기 위해 BufferReader를 사용해야할 것 같다.

<br>
<br>  

```java
import java.io.BufferReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main{
    public static void main(String args[]) throws IOException{
        int a = 0;
        BufferReader br = new BufferReader(new InputStreamReader(System.in));
        
        String input = br.readLine();
        
        StringTokenizer st = new StringTokenizer(" ", input);
        
        while(st.hasMoreTokens){           
            a += Integer.parseInt(st.nextToken);
        }
        System.out.println(a);
        
    }
}
```

<div style="text-align:center; font-size:0.8em;">두번째 시도 - 컴파일에러(오타)</div>

<br>
<br>

<br>
<br>  

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main{
    public static void main(String args[]) throws IOException{
        int a = 0;
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        
        String input = br.readLine();
        
        StringTokenizer st = new StringTokenizer(input);
        
        while(st.hasMoreTokens()){
            int b = Integer.parseInt(st.nextToken());
            a += b;
        }
        System.out.println(a);
        
    }
}
```

<div style="text-align:center; font-size:0.8em;">세번째 시도 - 런타임에러(NumberFormat)</div>

<br>
<br>

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main{
    public static void main(String args[]) throws IOException{
        long a = 0;
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        
        String input = br.readLine();
        
        StringTokenizer st = new StringTokenizer(input, " ");
        
        while(st.hasMoreTokens()){
            long b = Long.parseLong(st.nextToken());
            a += b;
        }
        System.out.println(a);
      
    }
}
```

<div style="text-align:center; font-size:0.8em;">정답</div>





<br>
<br>
<br>
<br>

## 10171번 문제

<br>

문제: 아래 예제와 같이 고양이를 출력하시오.

<br>

출력 : 

```
\    /\
 )  ( ')
(  /  )
 \(__)|
```
<br>

```java
public class Main{
    public static void main(String args[]){
        System.out.println("\\    /\\");
        System.out.println(" )  ( \')"); 
        System.out.println("(  /  )"); 
        System.out.println(" \\(__)|"); 
    }
}
```

<div style="text-align:center; font-size:0.8em;">정답</div>



<br>
<br>
<br>
<br>

## 10172번 문제

<br>

문제: 아래 예제와 같이 고양이를 출력하시오.

<br>

출력 : 

```
|\_/|
|q p|   /}
( 0 )"""\
|"^"`    |
||_/=\\__|
```
<br>

```java
public class Main{
    public static void main(String args[]){
        System.out.println("|\\_/|");
        System.out.println("|q p|   /}");
        System.out.println("( 0 )\"\"\"\\");
        System.out.println("|\"^\"`    |");
        System.out.println("||_/=\\\\__|");
    }
}
```

<div style="text-align:center; font-size:0.8em;">정답</div>

<br>

> 역따옴표는 따로 역슬래시를 안 넣어줘도 된다.
