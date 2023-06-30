---
layout: single
title: "(Coding Test) 4단계 - 배열"
categories:
  - CodingTest
---

2023-06-30

## 4단계 - 배열

<br>
<br>

## 목차
<br>
<br>

* [10807(배열을 입력받고 v를 찾는 문제)](#10807-번-문제)
* [10871(배열을 입력받고 X보다 작은 수를 찾는 문제)](#10871-번-문제)

<br>
<br>

## 10807 번 문제


<br>

### 문제

총 N개의 정수가 주어졌을 때, 정수 v가 몇 개인지 구하는 프로그램을 작성하시오.

<br>

### 입력

첫째 줄에 정수의 개수 N(1 ≤ N ≤ 100)이 주어진다. 둘째 줄에는 정수가 공백으로 구분되어져있다. 셋째 줄에는 찾으려고 하는 정수 v가 주어진다. 입력으로 주어지는 정수와 v는 -100보다 크거나 같으며, 100보다 작거나 같다.

<br>

### 출력

첫째 줄에 입력으로 주어진 N개의 정수 중에 v가 몇 개인지 출력한다.



<br>

<br>



```java
import java.io.*;
import java.util.*;

class Main{
    public static void main(String args[]) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());
        StringTokenizer st = new StringTokenizer(br.readLine()," ");
        int count = 0;
        int search = Integer.parseInt(br.readLine());
        while(st.hasMoreTokens()){
            if(search == Integer.parseInt(st.nextToken())){
                count++;
            }
        }
        System.out.println(count);
    }
}
```

<div style="text-align:center; font-size:0.8em;">정답</div>

<br>
<br>

|**메모리**|**시간**|
|:--:|:--:|
|14220 KB|132 ms|

<br>
<br>


```java
public class Main {
    static int read() throws Exception {
        int c, n = System.in.read() & 15;
        while ((c = System.in.read()) > 32) n = (n << 3) + (n << 1) + (c & 15);
        return n;
    }
    
    public static void main(String... args) throws Exception {
        int n = read(), cnt = 0;
        int[] arr = new int[n];
        for(int i = 0; i < n; i++) {
            arr[i] = read();
        }
        int v = read();
        for(int i = 0; i < n; i++) {
            if(arr[i] == v)
                cnt++;
        }
        System.out.println(cnt);
    }
}
```

<div style="text-align:center; font-size:0.8em;">모범 답안</div>

<br>
<br>

|**메모리**|**시간**|
|:--:|:--:|
|14088 KB|120 ms|

<br>

아무래도 저 read매서드의 원리가 뭔지 알고, 활용할줄 알아야 할 것 같다. 정수를 입력 받을때 최단시간이 나온 사람들을 보면 거의 다 저 매서드를 사용하고 있다.

<br>
<br>
<br>
<br>

## 10871 번 문제


<br>

### 문제

정수 N개로 이루어진 수열 A와 정수 X가 주어진다. 이때, A에서 X보다 작은 수를 모두 출력하는 프로그램을 작성하시오.

<br>

### 입력

첫째 줄에 N과 X가 주어진다. (1 ≤ N, X ≤ 10,000)

둘째 줄에 수열 A를 이루는 정수 N개가 주어진다. 주어지는 정수는 모두 1보다 크거나 같고, 10,000보다 작거나 같은 정수이다.

<br>

### 출력

X보다 작은 수를 입력받은 순서대로 공백으로 구분해 출력한다. X보다 작은 수는 적어도 하나 존재한다.



<br>

<br>

```java
import java.util.StringTokenizer;
import java.io.*;

class Main{
    public static void main(String args[]) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine()," ");
        int n = Integer.parseInt(st.nextToken());
        int max = Integer.parseInt(st.nextToken());
        int a[] = new int[n];
        st = new StringTokenizer(br.readLine(), " ");
        for(int i = 0; i<n; i++){
            a[i] = Integer.parseInt(st.nextToken());
        }
        StringBuilder sb = new StringBuilder();
        for(int i = 0; i<n; i++){
            if(a[i]<max){
                sb.append(a[i]).append(" ");
            }
        }
        System.out.println(sb.toString().trim());
    }
}
```

<div style="text-align:center; font-size:0.8em;">정답1</div>

<br>
<br>

|**메모리**|**시간**|
|:--:|:--:|
|15532 KB|180 ms|

<br>
<br>



```java
import java.util.StringTokenizer;
import java.io.*;

class Main{
    public static void main(String args[]) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine()," ");
        int n = Integer.parseInt(st.nextToken());
        int max = Integer.parseInt(st.nextToken());
        int a[] = new int[n];
        st = new StringTokenizer(br.readLine(), " ");
        for(int i = 0; i<n; i++){
            a[i] = Integer.parseInt(st.nextToken());
        }
        StringBuilder sb = new StringBuilder();
        for(int i = 0; i<n; i++){
            if(a[i]<max){
                sb.append(a[i]).append(" ");
            }
        }
        System.out.println(sb);
    }
}
```

<div style="text-align:center; font-size:0.8em;">정답2</div>

<br>
<br>

|**메모리**|**시간**|
|:--:|:--:|
|15416 KB|172 ms|

<br>
<br>

마지막 공백을 없애기 위해 toString으로 변환 후 trim()을 해줬는데 안해줘도 정답으로 인정된다. 어느때는 공백까지 정확하게 보면서 이번에는 또 마지막에 공백이 붙어도 정답으로 인정되니 어느장단에 맞춰야하나..;;;


<br>
<br>



```java
import java.util.StringTokenizer;
import java.io.*;

class Main{
    public static void main(String args[]) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine()," ");
        int n = Integer.parseInt(st.nextToken());
        int max = Integer.parseInt(st.nextToken());
        int a[] = new int[n];
        st = new StringTokenizer(br.readLine(), " ");
        StringBuilder sb = new StringBuilder();
        for(int i = 0; i<n; i++){
            int c = Integer.parseInt(st.nextToken());
            if(c<max){
                sb.append(c).append(" ");
            }
        }
        System.out.println(sb);
    }
}
```

<div style="text-align:center; font-size:0.8em;">정답3</div>

<br>
<br>

|**메모리**|**시간**|
|:--:|:--:|
|15304 KB|164 ms|

<br>
<br>

이런식으로 코드를 줄여 봤다.