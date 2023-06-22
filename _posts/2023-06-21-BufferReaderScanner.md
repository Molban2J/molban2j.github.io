---
layout: single
title: "(Java) BufferedReader와 Scanner의 차이"
categories:
  - Java
---

## BufferedReader와 Scanner의 차이

<br>

백준 코딩 테스트를 하다보니 모범 답안들, 즉, 컴파일 시간이 별로 안걸리는 코딩들을 보면 Scanner 대신 BufferedReader를 사용한다.

그리고 뒤로 넘어갈 수록 런타임에러 때문에 시간단축이 필수적이다.

그렇다면 BufferedReader와 Scanner의 차이점이 뭘까?

<br>
<br>
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

<div style="text-align:center; font-size:0.8em;">BufferedReader</div>

<br>
<br>

![image]({{ site.baseurl }}/images/2023-06-21/20230621_175754.png)

<div style="text-align:center; font-size:0.8em;">언어별 입력 속도 비교 <br><br> 

출처: [백준](https://www.acmicpc.net/blog/search/%EC%9E%85%EB%A0%A5+%EC%86%8D%EB%8F%84 "github")

</div>

<br>
<br>

이 표를 보면 BufferedReader가 6위 Scanner가 17위인 것을 볼 수 있다.

<br>
<br>
<br>

### 왜 이런 차이가 날까?

 먼저 속도의 차이가 나는 이유는 버퍼의 사용여부다. 

 **BufferedReader** 는 8KB 크기의 버퍼를 가지고 여기에 입력들을 저장했다가 버퍼가 차거나 개행문자가 나오면 데이터를 한번에 프로그램으로 전송한다.

**Scanner** 는 입력이 들어오는대로 프로그램으로 보내는 방식이다.

또한 **Scanner**는 전송 과정에서 정규 표현식 적용, 입력값 분할, 파싱 과정 등을 거치기 때문에 속도가 비교적 떨어진다.


<br>
<br>
<br>


* **BufferedReader**
  * java.io.BufferedReader
  * 8KB 버퍼 사용
  * try-catch나 throws Exception 적용해줘야함.
  * 주로 FileReader나 **InputStreamReader**와 함께 사용

  <br>

* **Scanner**
  * 입력받는 대로 전송
  * 정규 표현식 적용, 입력값 분할, 파싱 등을 거침
  * 자동으로 데이터 형을 변환 하여 반환시켜주기 때문에 편리함.

<br>
<br>
<br>
<br>
<br>

### 참고


```java
import java.io.IOException;

public class Main {
    public static void main(String[] args) throws IOException{
            int data = System.in.read();

            System.out.println(data);
    }
}
```

<div style="text-align:center; font-size:0.8em;">System.in</div>

<br>
<br>

이런식으로 System.in 만 사용해서도 입력 받을 수 있다.

0에서 255 사이의 정수를 반환하며, 문자가 입력되면 아스키 코드로 반환된다.

