---
layout: single
title: "(Coding Test) 3단계 - 반복문"
categories:
  - CodingTest
---

2023-06-26

## 2단계 - 조건문
<br>
<br>

## 목차

* [2739(구구단)](#2739-번-문제)
* [10950(A+B를 여러 번 출력하는 문제)](#10950-번-문제)
* [8393(1부터 N까지의 합을 구하는 문제.)](#8393-번-문제)
* [25304(영수증)](#25304-번-문제)

<br>
<br>

## 2739 번 문제


<br>

### 문제
N을 입력받은 뒤, 구구단 N단을 출력하는 프로그램을 작성하시오. 출력 형식에 맞춰서 출력하면 된다.

<br>

### 입력

첫째 줄에 N이 주어진다. N은 1보다 크거나 같고, 9보다 작거나 같다.

<br>

### 출력

출력형식과 같게 N*1부터 N*9까지 출력한다.

<br>

<br>



```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

class Main{
    public static void main(String args[]) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int a = Integer.parseInt(br.readLine());
        br.close();
        for(int i = 1; i <10 ; i++){
            System.out.println(a + " * "+i+" = "+i*a);
        }
    }
}
```

<div style="text-align:center; font-size:0.8em;">정답</div>

<br>

|**메모리**|**시간**|
|:--:|:--:|
|16144 KB|160 ms|

<br>

<br>



```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int a = Integer.parseInt(br.readLine());
        StringBuilder sb = new StringBuilder();
        for (int i = 1; i < 10; i++) {
            sb.append(a).append(" * ").append(i).append(" = ").append(a * i).append('\n');
        }
        System.out.println(sb);
    }
}
```

<div style="text-align:center; font-size:0.8em;">모범답안</div>

<br>

|**메모리**|**시간**|
|:--:|:--:|
|14116 KB|116 ms|


<br>
<br>
<br>
<br>


## 10950 번 문제


<br>

### 문제

두 정수 A와 B를 입력받은 다음, A+B를 출력하는 프로그램을 작성하시오.

<br>

### 입력

첫째 줄에 테스트 케이스의 개수 T가 주어진다.

각 테스트 케이스는 한 줄로 이루어져 있으며, 각 줄에 A와 B가 주어진다. (0 < A, B < 10)

<br>

### 출력

각 테스트 케이스마다 A+B를 출력한다.

<br>

<br>



```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

class Main{
    public static void main(String args[]) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringBuilder sb = new StringBuilder();
        int n = Integer.parseInt(br.readLine());
        
        for(int i=0; i<n; i++){
            String input = br.readLine();
            StringTokenizer st = new StringTokenizer(input, " ");
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());
            sb.append(a+b).append("\n");
        }
         System.out.println(sb);
    }
}
```

<div style="text-align:center; font-size:0.8em;">정답</div>

<br>

|**메모리**|**시간**|
|:--:|:--:|
|14168 KB|128 ms|

<br>
<br>
<br>
<br>

## 8393 번 문제


<br>

### 문제

n이 주어졌을 때, 1부터 n까지 합을 구하는 프로그램을 작성하시오.

<br>

### 입력

첫째 줄에 n (1 ≤ n ≤ 10,000)이 주어진다.

<br>

### 출력

1부터 n까지 합을 출력한다.

<br>

<br>

이 문제는 굳이 반복문을 안써도 구할 수 있어서 반복문을 쓴 것과 안 쓴 것 두가지로 해보았다.

<br>

<br>



```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

class Main{
    public static void main(String args[]) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int a = Integer.parseInt(br.readLine());
        int total = 0;
        br.close();
        for(int i = 0; i<a;i++){
            total += i+1;
        }
        System.out.println(total);
    }
}
```

<div style="text-align:center; font-size:0.8em;">반복문 정답</div>

<br>

|**메모리**|**시간**|
|:--:|:--:|
|14208 KB|124 ms|


<br>

<br>



```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

class Main{
    public static void main(String args[]) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int a = Integer.parseInt(br.readLine());
        br.close();
        if(a%2 == 0){
            System.out.println((a+1)*a/2);   
        } else {
            System.out.println(a*(a/2+1));
        }
    }
}
```

<div style="text-align:center; font-size:0.8em;">반복문 없이 정답</div>

<br>

|**메모리**|**시간**|
|:--:|:--:|
|14136 KB|132 ms|

<br>
<br>
<br>
<br>

## 25304 번 문제


<br>

### 문제

준원이는 저번 주에 살면서 처음으로 코스트코를 가 봤다. 정말 멋졌다. 그런데, 몇 개 담지도 않았는데 수상하게 높은 금액이 나오는 것이다! 준원이는 영수증을 보면서 정확하게 계산된 것이 맞는지 확인해보려 한다.

영수증에 적힌,

 * 구매한 각 물건의 가격과 개수
 * 구매한 물건들의 총 금액

 을 보고, 구매한 물건의 가격과 개수로 계산한 총 금액이 영수증에 적힌 총 금액과 일치하는지 검사해보자.

<br>

### 입력

첫째 줄에는 영수증에 적힌 총 금액 
$X$가 주어진다.

둘째 줄에는 영수증에 적힌 구매한 물건의 종류의 수 
$N$이 주어진다.

이후 
$N$개의 줄에는 각 물건의 가격 
$a$와 개수 
$b$가 공백을 사이에 두고 주어진다.

<br>

### 출력

구매한 물건의 가격과 개수로 계산한 총 금액이 영수증에 적힌 총 금액과 일치하면 Yes를 출력한다. 일치하지 않는다면 No를 출력한다.

<br>

<br>



```java
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.BufferedReader;
import java.util.StringTokenizer;

class Main{
    public static void main(String args[])throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int total = Integer.parseInt(br.readLine());
        int count = Integer.parseInt(br.readLine());
        int subTotal = 0;
        for(int i = 0; i<count; i++){
            StringTokenizer st = new StringTokenizer(br.readLine(), " ");
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());
            subTotal += a*b;
        }
        if(subTotal == total){
            System.out.println("Yes");
        } else {
            System.out.println("No");
        }
    }
}
```

<div style="text-align:center; font-size:0.8em;">정답</div>

<br>

|**메모리**|**시간**|
|:--:|:--:|
|14220 KB|124 ms|

<br>
<br>
<br>
<br>