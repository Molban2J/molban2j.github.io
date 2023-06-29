---
layout: single
title: "(Coding Test) 3단계 - 반복문2"
categories:
  - CodingTest
---

2023-06-28

## 3단계 - 반복문

<br>
<br>

## 목차

* [25314](#25314-번-문제)
* [15552](#15552-번-문제)
* [11021](#11021-번-문제)

<br>
<br>

## 25314 번 문제


<br>

### 문제

오늘은 혜아의 면접 날이다. 면접 준비를 열심히 해서 앞선 질문들을 잘 대답한 혜아는 이제 마지막으로 칠판에 직접 코딩하는 문제를 받았다. 혜아가 받은 문제는 두 수를 더하는 문제였다. C++ 책을 열심히 읽었던 혜아는 간단히 두 수를 더하는 코드를 칠판에 적었다. 코드를 본 면접관은 다음 질문을 했다. “만약, 입출력이 
$N$바이트 크기의 정수라면 프로그램을 어떻게 구현해야 할까요?”

혜아는 책에 있는 정수 자료형과 관련된 내용을 기억해 냈다. 책에는 long int는 
$4$바이트 정수까지 저장할 수 있는 정수 자료형이고 long long int는 
$8$바이트 정수까지 저장할 수 있는 정수 자료형이라고 적혀 있었다. 혜아는 이런 생각이 들었다. “int 앞에 long을 하나씩 더 붙일 때마다 
$4$바이트씩 저장할 수 있는 공간이 늘어나는 걸까? 분명 long long long int는 
$12$바이트, long long long long int는 
$16$바이트까지 저장할 수 있는 정수 자료형일 거야!” 그렇게 혜아는 당황하는 면접관의 얼굴을 뒤로한 채 칠판에 정수 자료형을 써 내려가기 시작했다.

혜아가 
$N$바이트 정수까지 저장할 수 있다고 생각해서 칠판에 쓴 정수 자료형의 이름은 무엇일까?

<br>

### 입력

첫 번째 줄에는 문제의 정수 
$N$이 주어진다. 
$(4\le N\le 1\, 000$; 
$N$은 
$4$의 배수
$)$ 

<br>

### 출력

혜아가 
$N$바이트 정수까지 저장할 수 있다고 생각하는 정수 자료형의 이름을 출력하여라.



<br>

<br>



```java
import java.io.IOException;
import java.io.BufferedReader;
import java.io.InputStreamReader;

class Main{
    public static void main(String args[]) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int input = Integer.parseInt(br.readLine());
        StringBuilder sb = new StringBuilder();
        
        for(int i = 0; i<input/4; i++){
            sb.append("long ");
        }
        sb.append("int");
        System.out.println(sb);
    }
}
```

<div style="text-align:center; font-size:0.8em;">정답</div>

<br>

|**메모리**|**시간**|
|:--:|:--:|
|14180 KB|124 ms|

<br>
<br>
<br>
<br>

## 15552 번 문제


<br>

### 문제

본격적으로 for문 문제를 풀기 전에 주의해야 할 점이 있다. 입출력 방식이 느리면 여러 줄을 입력받거나 출력할 때 시간초과가 날 수 있다는 점이다.

C++을 사용하고 있고 cin/cout을 사용하고자 한다면, cin.tie(NULL)과 sync_with_stdio(false)를 둘 다 적용해 주고, endl 대신 개행문자(\n)를 쓰자. 단, 이렇게 하면 더 이상 scanf/printf/puts/getchar/putchar 등 C의 입출력 방식을 사용하면 안 된다.

Java를 사용하고 있다면, Scanner와 System.out.println 대신 BufferedReader와 BufferedWriter를 사용할 수 있다. BufferedWriter.flush는 맨 마지막에 한 번만 하면 된다.

Python을 사용하고 있다면, input 대신 sys.stdin.readline을 사용할 수 있다. 단, 이때는 맨 끝의 개행문자까지 같이 입력받기 때문에 문자열을 저장하고 싶을 경우 .rstrip()을 추가로 해 주는 것이 좋다.

또한 입력과 출력 스트림은 별개이므로, 테스트케이스를 전부 입력받아서 저장한 뒤 전부 출력할 필요는 없다. 테스트케이스를 하나 받은 뒤 하나 출력해도 된다.

자세한 설명 및 다른 언어의 경우는 이 글에 설명되어 있다.

이 블로그 글에서 BOJ의 기타 여러 가지 팁을 볼 수 있다.

<br>

### 입력

첫 줄에 테스트케이스의 개수 T가 주어진다. T는 최대 1,000,000이다. 다음 T줄에는 각각 두 정수 A와 B가 주어진다. A와 B는 1 이상, 1,000 이하이다.

<br>

### 출력

각 테스트케이스마다 A+B를 한 줄에 하나씩 순서대로 출력한다..



<br>

<br>



```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.StringTokenizer;

class Main{
    public static void main(String args[]) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        int n = Integer.parseInt(br.readLine());
        
        for(int i = 0; i<n; i++){
            StringTokenizer st = new StringTokenizer(br.readLine(), " ");
            bw.write(Integer.parseInt(st.nextToken())+Integer.parseInt(st.nextToken())+"\n");
            bw.flush();
        }
        
    }
}
```

<div style="text-align:center; font-size:0.8em;">시간 초과</div>

<br>
<br>



```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.StringTokenizer;

class Main{
    public static void main(String args[]) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        int n = Integer.parseInt(br.readLine());
        
        for(int i = 0; i<n; i++){
            StringTokenizer st = new StringTokenizer(br.readLine(), " ");
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());
            bw.write(a+b+"\n");
        
        }
        bw.flush();
        
    }
}
```

<div style="text-align:center; font-size:0.8em;">정답</div>

<br>

|**메모리**|**시간**|
|:--:|:--:|
|236556 KB|872 ms|

<br>
<br>

flush()는 마지막에 한번만 써야하나 보다. Bufferedwriter에 대해서 좀 더 알아봐야겠다.

<br>

[BufferedWriter란?](https://github.com/Molban2J/Baemin.git "github")

<br>
<br>

모범 답안들을 보니 가장 적은 시간이 걸린 답안은 봐도 잘 모르겠고, 가장 많이 쓴 답안들은 모두 똑같았는데, 어디 공식으로 나와있는 문제인가 보다. 변수 명까지 똑같이 한걸 보면.. 

<br>
<br>



```java
import java.io.*;

public class Main {
    public static void main(String... args) throws IOException {
        int testCases = sumUntilNewLine();
        StringBuilder answer = new StringBuilder();
        while (testCases-- > 0) {
            answer.append(sumUntilNewLine()).append('\n');
        }
        System.out.print(answer.toString());
    }

    public static int sumUntilNewLine() throws IOException {
        int sum = 0;
        int buf = 0;
        int c;
        while((c = System.in.read()) != '\n') {
            if (c == ' ') {
                sum += buf;
                buf = 0;
            } else {
                buf = (buf * 10) +(c - '0');
            }
        }
        sum += buf;
        return sum;
    }
}
```

<div style="text-align:center; font-size:0.8em;">모범답안</div>

<br>

|**메모리**|**시간**|
|:--:|:--:|
|32000 KB|428 ms|

<br>
<br>

모범 답안의 코드를 한번 살펴보자. 모범답안은 나와는 다르게 **StringTokenizer와 BufferedReader, BufferedWriter 모두 쓰지않았다.** 기존 내장 객체인 **System.in**을 사용했다. 

```java
while((c = System.in.read()) != '\n') {
    if (c == ' ') {
        sum += buf;
        buf = 0;
    } else {
        buf = (buf * 10) +(c - '0');
    }
}
sum += buf;         
```

<br>

이 부분이 이해가 잘 안됐었다.
<br>

```java
buf = (buf * 10) +(c - '0');       
```

특히나 이 부분이 이해가 잘 안 됐었다. 위 코드는 입력받은 아스키 코드를 정수형으로 받는 코드이다. 

여기서 핵심으로 알아야할 것은 **c -'0'** 을 하는 이유이다. 

**먼저 문자 '0'의 ASCII 코드는 48이다. 그리고 '9'까지 ASCII코드는 순차적으로 1씩 증가 한다.**

그래서 c가 받아온 문자의 ASCII코드에서 '0'의 ASCII코드 값을 빼면 정수가 되는 것이다.

그리고 c는 System.in.read()를 통해 한 문자 씩 받아온다. 그래서 처음 입력된 정수가 여러자리 수 인 경우 이전 buf값에 *10을 해줘서 한자리씩 높이는 것이다.

<br>

그러다가 공백을 만나면 sum값에 buf값을 저장해 놓고 buf값을 0으로 초기화 한다. 그리고선 그 다음 정수를 다시 buf에 저장하는 것이다.

<br>
<br>
<br>
<br>

## 11021 번 문제


<br>

### 문제

두 정수 A와 B를 입력받은 다음, A+B를 출력하는 프로그램을 작성하시오

<br>

### 입력

첫째 줄에 테스트 케이스의 개수 T가 주어진다.

각 테스트 케이스는 한 줄로 이루어져 있으며, 각 줄에 A와 B가 주어진다. (0 < A, B < 10)

<br>

### 출력

각 테스트 케이스마다 "Case #x: "를 출력한 다음, A+B를 출력한다. 테스트 케이스 번호는 1부터 시작한다.



<br>

<br>



```java
import java.io.*;
class Main{
    public static void main(String args[]) throws IOException{
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());
        int a;
        int i = 1;
        
        while(n-- >0){
            a = sum();
            bw.write("Case #"+i+": "+a+"\n");
            i++;
        }
        bw.flush();
        bw.close();
    }
    public static int sum() throws IOException{
        int sum = 0;
        int buf = 0;
        int c = 0;
        
        while((c = System.in.read()) != '\n'){
            if(c == ' '){
                sum += buf;
                buf = 0;
            } else {
                buf = (buf*10)+(c-'0');
            }   
        }
        sum += buf; 
        return sum;
    }
}
```

<div style="text-align:center; font-size:0.8em;">시간 초과</div>

<br>
<br>



```java
import java.io.*;
import java.util.StringTokenizer;

class Main{
    public static void main(String args[]) throws IOException{
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st;
        int n = Integer.parseInt(br.readLine());

        for(int i = 1 ; i<n+1 ; i++){
            st = new StringTokenizer(br.readLine(), " ");
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());
            bw.write("Case #"+ i + ": "+(a+b));
            bw.newLine();
        }
        bw.flush();
        bw.close();
        br.close();
    }
}
```

<div style="text-align:center; font-size:0.8em;">정답</div>

<br>

|**메모리**|**시간**|
|:--:|:--:|
|16020 KB|156 ms|

<br>
<br>
<br>
<br>