---
layout: single
title: "(Coding Test) 3단계 - 반복문3"
categories:
  - CodingTest
---

2023-06-29

## 3단계 - 반복문

<br>
<br>

## 목차

* [11022(A+B를 바로 위 문제보다 아름답게 출력하는 문제)](#11022-번-문제)
* [2438(별을 찍는 문제 1)](#2438-번-문제)
* [2438(별을 찍는 문제 2)](#2439-번-문제)
* [10952(0 0이 들어올 때까지 A+B를 출력하는 문제)](#10952-번-문제)
* [10951(입력이 끝날 때까지 A+B를 출력하는 문제. EOF에 대해 알아 보세요.) -진행중](#10951-번-문제)

<br>
<br>

## 11022 번 문제


<br>

### 문제

두 정수 A와 B를 입력받은 다음, A+B를 출력하는 프로그램을 작성하시오.

<br>

### 입력

첫째 줄에 테스트 케이스의 개수 T가 주어진다.

각 테스트 케이스는 한 줄로 이루어져 있으며, 각 줄에 A와 B가 주어진다. (0 < A, B < 10)

<br>

### 출력

각 테스트 케이스마다 "Case #x: A + B = C" 형식으로 출력한다. x는 테스트 케이스 번호이고 1부터 시작하며, C는 A+B이다.



<br>

<br>



```java
import java.io.*;
import java.util.StringTokenizer;

class Main{
    public static void main(String args[]) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        int n = Integer.parseInt(br.readLine());
        for(int i = 0; i<n+1; i++){
            StringTokenizer st = new StringTokenizer(br.readLine(), " ");
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());
            bw.write("Case #"+ i+ ": "+ a + " + "+b+" = "+(a+b));
            bw.newLine();
        }
        br.close();
        bw.flush();
        bw.close();
    }
}
```

<div style="text-align:center; font-size:0.8em;">런타임 에러(Null Pointer)</div>

<br>
<br>



```java
import java.io.*;
import java.util.StringTokenizer;

class Main{
    public static void main(String args[]) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        int n = Integer.parseInt(br.readLine());
        for(int i = 1; i<n+1; i++){
            StringTokenizer st = new StringTokenizer(br.readLine(), " ");
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());
            bw.write("Case #"+i+": "+a+" + "+b+" = "+(a+b));
            bw.newLine();
        }
        br.close();
        bw.flush();
        bw.close();
    }
}
```

<div style="text-align:center; font-size:0.8em;">정답(for문이 틀렸었다)</div>

<br>

|**메모리**|**시간**|
|:--:|:--:|
|16364 KB|172 ms|

<br>
<br>

아무래도 StringBuilder를 생성하고 System.out.println이 더빠른듯 하다..

<br>
<br>
<br>
<br>

## 	2438 번 문제


<br>

### 문제

첫째 줄에는 별 1개, 둘째 줄에는 별 2개, N번째 줄에는 별 N개를 찍는 문제

<br>

### 입력

첫째 줄에 N(1 ≤ N ≤ 100)이 주어진다.
<br>

### 출력

첫째 줄부터 N번째 줄까지 차례대로 별을 출력한다.


<br>

<br>



```java
import java.io.*;

class Main{
    public static void main(String args[]) throws Exception{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int a = Integer.parseInt(br.readLine());
        StringBuilder sb = new StringBuilder();
        for(int i = 0; i<a; i++){
            sb.append("*");
            System.out.println(sb);
        }
    }
}
```

<div style="text-align:center; font-size:0.8em;">정답</div>

<br>


|**메모리**|**시간**|
|:--:|:--:|
|14176 KB|128 ms|


<br>



```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

class Main {
	
	public static void main(String[] args) throws IOException {
		
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		
		int N = Integer.parseInt(br.readLine());
		StringBuilder sb = new StringBuilder();
		for(int i = 0; i < N; i++) {
			for(int j = 0; j < i + 1; j++) {
				sb.append("*");
			}//inner for
			sb.append("\n");
		}//outer for
		br.close();
		System.out.println(sb);
	}
	
}
```

<div style="text-align:center; font-size:0.8em;">모범답안</div>

<br>

**이중 반복문을 사용해서 System.out.println의 사용을 최소화 하는것이 좋아보인다.**

<br>
<br>
<br>
<br>

## 	2439 번 문제


<br>

### 문제

첫째 줄에는 별 1개, 둘째 줄에는 별 2개, N번째 줄에는 별 N개를 찍는 문제첫째 줄에는 별 1개, 둘째 줄에는 별 2개, N번째 줄에는 별 N개를 찍는 문제

하지만, 오른쪽을 기준으로 정렬한 별(예제 참고)을 출력하시오.
<br>

### 입력

첫째 줄에 N(1 ≤ N ≤ 100)이 주어진다.
<br>

### 출력

첫째 줄부터 N번째 줄까지 차례대로 별을 출력한다.


<br>

```java
    *
   **
  ***
 ****
*****
```

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
        StringBuilder sb = new StringBuilder();
        for(int i = 0; i<a; i++){
            for(int j = a-i-1; j>0; j--){
                sb.append(" ");
            }
            for(int k = 0; k<i+1;k++){
               sb.append("*"); 
            }
            if(i < a-1){
            sb.append("\n");
            }
        }
        System.out.println(sb);
    }
}
```

<div style="text-align:center; font-size:0.8em;">정답</div>

<br>


|**메모리**|**시간**|
|:--:|:--:|
|14172 KB|124 ms|

<br>
<br>


```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.IOException;

public class Main{
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        
        int inputNum = Integer.parseInt(br.readLine());
        br.close();
        
        StringBuilder sb = new StringBuilder();
        
        for (int i=1; i <= inputNum; i++) {
            sb.append(" ".repeat(inputNum-i));
            sb.append("*".repeat(i));
            sb.append("\n");
        }
        System.out.println(sb);
    }
}
```

<div style="text-align:center; font-size:0.8em;">모범 답안</div>

<br>

StringBuilder의 **repeat(n)** 을 사용하여 반복문을 줄였다.

<br>
<br>
<br>
<br>


## 	10952 번 문제


<br>

### 문제

두 정수 A와 B를 입력받은 다음, A+B를 출력하는 프로그램을 작성하시오.

<br>

### 입력

입력은 여러 개의 테스트 케이스로 이루어져 있다.

각 테스트 케이스는 한 줄로 이루어져 있으며, 각 줄에 A와 B가 주어진다. (0 < A, B < 10)

입력의 마지막에는 0 두 개가 들어온다.

<br>

### 출력

각 테스트 케이스마다 A+B를 출력한다.


<br>
<br>

<br>


```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;
import java.io.BufferedWriter;
import java.io.OutputStreamWriter;

class Main{
    public static void main(String args[]) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        while(true){
            StringTokenizer st = new StringTokenizer(br.readLine()," ");
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());
            if(a==0 && b== 0) break;
            
            bw.write(a+b);
            bw.newLine();
        }
        bw.flush();
        bw.close();
        br.close();
    }
}
```

<div style="text-align:center; font-size:0.8em;">오답</div>

<br>
<br>


```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;
import java.io.BufferedWriter;
import java.io.OutputStreamWriter;

class Main{
    public static void main(String args[]) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        while(true){
            StringTokenizer st = new StringTokenizer(br.readLine()," ");
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());
            if(a==0 && b== 0) break;
            
            bw.write((a+b)+"\n");
        }
        bw.flush();
        bw.close();
        br.close();
    }
}
```

<div style="text-align:center; font-size:0.8em;">정답</div>

<br><br>


|**메모리**|**시간**|
|:--:|:--:|
|15720 KB|144 ms|

<br>
<br>

**차이점**


```java
bw.write(a+b);
bw.newLine(); //오답

bw.write((a+b)+"\n"); //정답
```

<br>

찾아보니 newLine() 함수가 환경에따라 항상 **"\n"**을 출력하지 않을 수 있다고 한다. 그러니 다음부터는 newLine대신 직접 "\n"를 써주도록 하자

<br>
<br>


```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;


public class Main {
  public static void main(String[] args) throws IOException {
    BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    StringBuilder sb = new StringBuilder();

    String input;
    while (true) {
      input = br.readLine();
      int a = (input.charAt(0) - 48);
      int b = (input.charAt(2) - 48);

      if (a == 0 && b == 0) {
        break;
      }

      sb.append(a + b).append("\n");
    }

    System.out.println(sb.toString());
    br.close();
  }
}
```

<div style="text-align:center; font-size:0.8em;">모범 답안</div>

<br><br>


|**메모리**|**시간**|
|:--:|:--:|
|14100 KB|116 ms|

<br>
<br>

언제는 StringBuilder를 쓰고 언제는 BufferedWriter를 쓰고... 어느 경우가 더 빠른지 헷갈린다. 

그리고 StringTokenizer보다는 **배열**이나 **charAt**을 사용하는 법을 익히도록 하자.

<br>
<br>
<br>
<br>



## 10951 번 문제


<br>

### 문제

두 정수 A와 B를 입력받은 다음, A+B를 출력하는 프로그램을 작성하시오.

<br>

### 입력

입력은 여러 개의 테스트 케이스로 이루어져 있다.

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

class Main{
    public static void main(String args[]) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        while(true){
            String input = br.readLine();
            int a = input.charAt(0)-'0';
            int b = input.charAt(2)-'0';
            System.out.println(a+b);
        }
    }
}
```

<div style="text-align:center; font-size:0.8em;">런타임에러</div>

<br>

<br>


```java
import java.io.*;
import java.util.StringTokenizer;

public class Main{
    public static void main(String args[]) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String input;
        while((input = br.readLine()) != null){
            StringTokenizer st = new StringTokenizer(input, " ");
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());
            System.out.println(a+b);
        }
    }
}
```

<div style="text-align:center; font-size:0.8em;">정답</div>

<br>
<br>

|**메모리**|**시간**|
|:--:|:--:|
|14128 KB|132 ms|


<br>

<br>


```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.IOException;
 
public class Main {
	public static void main(String args[]) throws IOException {
		
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringBuilder sb = new StringBuilder();
		String str;
 
		while( (str=br.readLine()) != null ){
		    
			int a = str.charAt(0) - 48;
			int b = str.charAt(2) - 48;
			sb.append(a+b).append("\n");
		
		}
		System.out.print(sb);
	}
}
```

<div style="text-align:center; font-size:0.8em;">모범 답안</div>

<br>
<br>
<br>
