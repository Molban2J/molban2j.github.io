---
layout: single
title: "(Coding Test) 4단계 - 배열2"
categories:
  - CodingTest
---

2023-07-03

## 4단계 - 배열

<br>
<br>

## 목차
<br>
<br>

* [10818(최솟값과 최댓값을 찾는 문제)](#10818-번-문제)
* [2562(최솟값과 최댓값을 찾는 문제)](#2562-번-문제)

<br>
<br>

## 	10818 번 문제


<br>

### 문제

N개의 정수가 주어진다. 이때, 최솟값과 최댓값을 구하는 프로그램을 작성하시오.

<br>

### 입력

첫째 줄에 정수의 개수 N (1 ≤ N ≤ 1,000,000)이 주어진다. 둘째 줄에는 N개의 정수를 공백으로 구분해서 주어진다. 모든 정수는 -1,000,000보다 크거나 같고, 1,000,000보다 작거나 같은 정수이다.

<br>

### 출력

첫째 줄에 주어진 정수 N개의 최솟값과 최댓값을 공백으로 구분해 출력한다.



<br>

<br>



```java
import java.io.*;
import java.util.StringTokenizer;

class Main{
    public static void main(String args[]) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());
        int a[] = new int[n];
        StringTokenizer st = new StringTokenizer(br.readLine(), " ");
        for(int i = 0; i<n;i++){
            a[i] = Integer.parseInt(st.nextToken());
        }
        int min = a[0];
        int max = a[n-1];
        for(int i = 0; i<n; i++){
            if(a[i]<min){
                min = a[i];
            }
            if(a[i]>max){
                max = a[i];
            }
        }
        StringBuilder sb = new StringBuilder();
        sb.append(min).append(" ").append(max);
        System.out.println(sb);
    }
}
```

<div style="text-align:center; font-size:0.8em;">정답</div>

<br>
<br>

|**메모리**|**시간**|
|:--:|:--:|
|91528 KB|484 ms|

<br>
<br>
너무 오래 걸렸다.. Math.min()과 Math.max를 써보자

<br>

<br>



```java
import java.io.*;
import java.util.StringTokenizer;

class Main{
    public static void main(String args[]) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());
        int a[] = new int[n];
        StringTokenizer st = new StringTokenizer(br.readLine(), " ");
        for(int i = 0; i<n;i++){
            a[i] = Integer.parseInt(st.nextToken());
        }
        int min = 1000000;
        int max = -1000000;
        for(int i = 0; i<n; i++){
            min = Math.min(a[i],min);
            max = Math.max(a[i],max);
        }
        StringBuilder sb = new StringBuilder();
        sb.append(min).append(" ").append(max);
        System.out.println(sb);
    }
}
```

<div style="text-align:center; font-size:0.8em;">정답(Math함수 사용)</div>

<br>
<br>

|**메모리**|**시간**|
|:--:|:--:|
|91504 KB|480 ms|

<br>
<br>

별 차이가 없다.

<br>

<br>



```java
import java.io.IOException;

public class Main {
    public static void main(String[] args) throws IOException {
        StringBuilder sb = new StringBuilder();
        int times = readInt();

        int min = Integer.MAX_VALUE;
        int max = Integer.MIN_VALUE;

        for(int i=0; i<times; i++){
            int num = readInt();

            min = Math.min(num, min);
            max = Math.max(num, max);
        }
        sb.append(min).append(" ").append(max);
        System.out.println(sb);
    }


    //method
    public static int readInt() throws IOException {
        boolean numSign = false;
        int value = 0; //리턴 시킬 숫자 합계

        while(true) {
            int num = System.in.read();

            if(num == '-'){ //음수라면
                numSign = true;
            } else if(num == ' ' || num == '\n') { //숫자 끝나는 지점 인식
                return numSign ? (-1 * value) : value;   
            } else {
                value = (value*10) + (num - '0');/*기존의 일의 자리를 다음 자리로 승격 */
            }
        }
    }
}
```

<div style="text-align:center; font-size:0.8em;">모범답안</div>

<br>
<br>

|**메모리**|**시간**|
|:--:|:--:|
|14260 KB|304 ms|

<br>
<br>

```java
int min = Integer.MAX_VALUE;
int max = Integer.MIN_VALUE;
```

<br>

이 코드를 보면 Integer.MAX_VALUE는 int 타입에서 표현할수있는 최댓값을 min값으로 지정해줬다. max또한 int 타입에서 표현 할 수 있는 최솟값으로 max값으로 지정해준 것이다.

그리고 readInt()매서드를 보면 numSign으로 음수와 양수를 판별해 음수라면 삼항 연산자로 -1을 곱해서 음수, 외에는 양수로 반환해준다.



<br>

<br>

<br>

<br>


## 	2562 번 문제


<br>

### 문제

9개의 서로 다른 자연수가 주어질 때, 이들 중 최댓값을 찾고 그 최댓값이 몇 번째 수인지를 구하는 프로그램을 작성하시오.

예를 들어, 서로 다른 9개의 자연수

3, 29, 38, 12, 57, 74, 40, 85, 61

이 주어지면, 이들 중 최댓값은 85이고, 이 값은 8번째 수이다.

<br>

### 입력

첫째 줄부터 아홉 번째 줄까지 한 줄에 하나의 자연수가 주어진다. 주어지는 자연수는 100 보다 작다.

<br>

### 출력

첫째 줄에 최댓값을 출력하고, 둘째 줄에 최댓값이 몇 번째 수인지를 출력한다.



<br>

<br>



```java
import java.io.*;

class Main{
    public static void main(String args[]) throws IOException{
        StringBuilder sb = new StringBuilder();
        int a[] = new int[9];
        int max = Integer.MIN_VALUE;
        int index = 0;
        for(int j = 0; j<9; j++){
            if(max< a[j]){
                max = a[j];
                index = j+1;
            }
        }
        sb.append(max).append("\n").append(index);
        System.out.println(sb);
    }
    
    private static Integer read() throws IOException{
        boolean sign = true;
        int a = 0;
        while(true){
            int num = System.in.read();
            if(num == '-'){
                sign = false;
            } else if(num == '\n'){
                return sign ? (-1*a) : a;
            } else {
                a = (a*10)+(a-'0');
            }
        }
    }
}
```

<div style="text-align:center; font-size:0.8em;">오답</div>

<br>
<br>
왜 틀렸는지 잘 모르겠는데, 나머지는 내일 마저 풀어보겠다.

<br>

<br>
