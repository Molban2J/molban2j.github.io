---
layout: single
title: "(Coding Test) 2단계 - 조건문2"
categories:
  - CodingTest
---

2023-06-23

## 2단계 - 조건문
<br>
<br>

## 목차

* [2884(알람시계)](#2884번-문제)
* [2525(조리시간)](#2525번-문제)
* [2480(주사위)](#2480번-문제)

<br>
<br>

## 2884번 문제

<br>

### 문제

상근이는 매일 아침 알람을 듣고 일어난다. 알람을 듣고 바로 일어나면 다행이겠지만, 항상 조금만 더 자려는 마음 때문에 매일 학교를 지각하고 있다.

상근이는 모든 방법을 동원해보았지만, 조금만 더 자려는 마음은 그 어떤 것도 없앨 수가 없었다.

이런 상근이를 불쌍하게 보던 창영이는 자신이 사용하는 방법을 추천해 주었다.

바로 "45분 일찍 알람 설정하기"이다.

이 방법은 단순하다. 원래 설정되어 있는 알람을 45분 앞서는 시간으로 바꾸는 것이다. 어차피 알람 소리를 들으면, 알람을 끄고 조금 더 잘 것이기 때문이다. 이 방법을 사용하면, 매일 아침 더 잤다는 기분을 느낄 수 있고, 학교도 지각하지 않게 된다.

현재 상근이가 설정한 알람 시각이 주어졌을 때, 창영이의 방법을 사용한다면, 이를 언제로 고쳐야 하는지 구하는 프로그램을 작성하시오.
<br>

### 입력

 첫째 줄에 두 정수 H와 M이 주어진다. (0 ≤ H ≤ 23, 0 ≤ M ≤ 59) 그리고 이것은 현재 상근이가 설정한 알람 시간 H시 M분을 의미한다.

입력 시간은 24시간 표현을 사용한다. 24시간 표현에서 하루의 시작은 0:0(자정)이고, 끝은 23:59(다음날 자정 1분 전)이다. 시간을 나타낼 때, 불필요한 0은 사용하지 않는다.

<br>

### 출력

첫째 줄에 상근이가 창영이의 방법을 사용할 때, 설정해야 하는 알람 시간을 출력한다. (입력과 같은 형태로 출력하면 된다.)
<br>

<br>



```java
import java.util.StringTokenizer;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

class Main{
    public static void main(String args[])throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));       
        StringTokenizer st = new StringTokenizer(br.readLine()," ");
        
        int hour = Integer.parseInt(st.nextToken());
        int minute = Integer.parseInt(st.nextToken());
        
        if(minute < 45){
            minute += 15;
            if(hour == 0){hour = 23;}
            else {hour -= 1;}
        } else{
            minute -= 45;
        }  
        System.out.println(hour+" "+minute);
    }
}
```

<div style="text-align:center; font-size:0.8em;">정답</div>

<br>

|**메모리**|**시간**|
|:--:|:--:|
|16084 KB|152 ms|

<br>

<br>



```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {

    public static void main(String[] args) throws IOException {

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine(), " ");
        int h = Integer.parseInt(st.nextToken());
        int m = Integer.parseInt(st.nextToken());

        br.close();

        StringBuilder sb = new StringBuilder();

        if (m < 45) {
            if (h == 0) {
                h = 23;
                sb.append(h).append(" ");
            }
            else {
                h--;
                sb.append(h).append(" ");
            }
            sb.append(m = 60 - (45 - m));
        }
        else {
            sb.append(h).append(" ").append(m - 45);
        }

        System.out.print(sb);
    }
}
```

<div style="text-align:center; font-size:0.8em;">모범 정답</div>

<br>

|**메모리**|**시간**|
|:--:|:--:|
|14140 KB|120 ms|

<br>

다른 사람이 한 것을 보니 **StringBuilder**를 사용했다. 이로써 시간과 메모리가 절약됐는데 한번 알아보자.

<br>

[String과 StringBuilder](https://molban2j.github.io/java/StringBuilder/ "github")

<br>

추가로 BufferedReader를 닫아주는 것을 습관화 하도록 하자.

<br>
<br>
<br>
<br>

## 2525번 문제

<br>

### 문제

KOI 전자에서는 건강에 좋고 맛있는 훈제오리구이 요리를 간편하게 만드는 인공지능 오븐을 개발하려고 한다. 인공지능 오븐을 사용하는 방법은 적당한 양의 오리 훈제 재료를 인공지능 오븐에 넣으면 된다. 그러면 인공지능 오븐은 오븐구이가 끝나는 시간을 분 단위로 자동적으로 계산한다. 

또한, KOI 전자의 인공지능 오븐 앞면에는 사용자에게 훈제오리구이 요리가 끝나는 시각을 알려 주는 디지털 시계가 있다. 

훈제오리구이를 시작하는 시각과 오븐구이를 하는 데 필요한 시간이 분단위로 주어졌을 때, 오븐구이가 끝나는 시각을 계산하는 프로그램을 작성하시오.

<br>

### 입력

첫째 줄에는 현재 시각이 나온다. 현재 시각은 시 A (0 ≤ A ≤ 23) 와 분 B (0 ≤ B ≤ 59)가 정수로 빈칸을 사이에 두고 순서대로 주어진다. 두 번째 줄에는 요리하는 데 필요한 시간 C (0 ≤ C ≤ 1,000)가 분 단위로 주어진다. 

<br>

### 출력

첫째 줄에 종료되는 시각의 시와 분을 공백을 사이에 두고 출력한다. (단, 시는 0부터 23까지의 정수, 분은 0부터 59까지의 정수이다. 디지털 시계는 23시 59분에서 1분이 지나면 0시 0분이 된다.)

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
        StringTokenizer st = new StringTokenizer(br.readLine()," ");
        int time = Integer.parseInt(br.readLine());
        br.close();
        
        int hour = Integer.parseInt(st.nextToken());
        int minute = Integer.parseInt(st.nextToken());
        
        StringBuilder sb = new StringBuilder();
        
        int end = minute + time;
        
        if(end >59){
            hour += end/60;
            if(hour >23){
                hour -= 24;
            }
            minute = end%60;
            sb.append(hour).append(" ").append(minute);
        } else {
            sb.append(hour).append(" ").append(end);
        }
        System.out.println(sb);
    }
}
```

<div style="text-align:center; font-size:0.8em;">정답</div>

<br>

|**메모리**|**시간**|
|:--:|:--:|
|14168 KB|120 ms|

<br>

<br>

<br>

<br>

## 2480번 문제

<br>

### 문제

1에서부터 6까지의 눈을 가진 3개의 주사위를 던져서 다음과 같은 규칙에 따라 상금을 받는 게임이 있다. 

1. 같은 눈이 3개가 나오면 10,000원+(같은 눈)×1,000원의 상금을 받게 된다. 
2. 같은 눈이 2개만 나오는 경우에는 1,000원+(같은 눈)×100원의 상금을 받게 된다. 
3. 모두 다른 눈이 나오는 경우에는 (그 중 가장 큰 눈)×100원의 상금을 받게 된다.  

예를 들어, 3개의 눈 3, 3, 6이 주어지면 상금은 1,000+3×100으로 계산되어 1,300원을 받게 된다. 또 3개의 눈이 2, 2, 2로 주어지면 10,000+2×1,000 으로 계산되어 12,000원을 받게 된다. 3개의 눈이 6, 2, 5로 주어지면 그중 가장 큰 값이 6이므로 6×100으로 계산되어 600원을 상금으로 받게 된다.

3개 주사위의 나온 눈이 주어질 때, 상금을 계산하는 프로그램을 작성 하시오.

<br>

### 입력

첫째 줄에 3개의 눈이 빈칸을 사이에 두고 각각 주어진다. 

<br>

### 출력

첫째 줄에 게임의 상금을 출력 한다.

<br>

<br>



```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.IOException;
import java.util.StringTokenizer;

class Main{
    public static void main(String args[]) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine(), " ");
        
        int a = Integer.parseInt(st.nextToken());
        int b = Integer.parseInt(st.nextToken());
        int c = Integer.parseInt(st.nextToken());
        
        int prize = 0;
        
        if(a == b && b==c) {prize = 10000+a*1000;}
        else if (a!=b && b!=c && c!=a){prize = big(a,b,c)*100;}
        else{prize = equal(a,b,c)*100+1000;}
        System.out.println(prize);
    }
    
    public static int big(int a, int b, int c){
        if(a>b && a>c){return a;}
        else if(b>a && b>c){return b;}
        else {return c;}
    }
    
    public static int equal(int a, int b, int c){
        int x = 0;
        if(a==b && a!=c){x=a;}
        else if(b==c && b!=a){x=b;}
        else if(a==c && a!=b){x=c;}
        return x;
    }
}
```

<div style="text-align:center; font-size:0.8em;">정답</div>

<br>

|**메모리**|**시간**|
|:--:|:--:|
|14184 KB|120 ms|

<br>

세 수중 최댓값을 구할때 **Math.max** 함수를 쓰는것이 좋아보인다.

<br>

<br>



```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.IOException;
import java.util.StringTokenizer;
 
public class Main {
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
 
		StringTokenizer st = new StringTokenizer(br.readLine(), " ");
 
		int a, b, c;
		a = Integer.parseInt(st.nextToken());
		b = Integer.parseInt(st.nextToken());
		c = Integer.parseInt(st.nextToken());
 
		// 만약 모든 변수가 다른 경우
		if (a != b && b != c && a != c) {
			int max = Math.max(a, Math.max(b, c));
			System.out.println(max * 100);
		}
		// 3개의 변수가 모두 같은 경우
		else if (a == b && a == c) {
			System.out.println(10000 + a * 1000);
		}
		// a가 b혹은 c랑만 같은 경우
		else if(a == b || a == c) {
			System.out.println(1000 + a * 100);
		}
		// b가 c랑 같은 경우
		else {
			System.out.println(1000 + b * 100);
		}
	}
}
```

<div style="text-align:center; font-size:0.8em;">모범 답안</div>

<br>

|**메모리**|**시간**|
|:--:|:--:|
|14080 KB|116 ms|

<br>

<br>

<br>

<br>

<br>