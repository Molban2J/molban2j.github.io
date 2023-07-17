---
layout: single
title: "(Coding Test) 4단계 - 배열3"
categories:
  - CodingTest
---

2023-07-17

## 4단계 - 배열

<br>
<br>

## 	10810 번 문제


<br>

### 문제

도현이는 바구니를 총 N개 가지고 있고, 각각의 바구니에는 1번부터 N번까지 번호가 매겨져 있다. 또, 1번부터 N번까지 번호가 적혀있는 공을 매우 많이 가지고 있다. 가장 처음 바구니에는 공이 들어있지 않으며, 바구니에는 공을 1개만 넣을 수 있다.

도현이는 앞으로 M번 공을 넣으려고 한다. 도현이는 한 번 공을 넣을 때, 공을 넣을 바구니 범위를 정하고, 정한 바구니에 모두 같은 번호가 적혀있는 공을 넣는다. 만약, 바구니에 공이 이미 있는 경우에는 들어있는 공을 빼고, 새로 공을 넣는다. 공을 넣을 바구니는 연속되어 있어야 한다.

공을 어떻게 넣을지가 주어졌을 때, M번 공을 넣은 이후에 각 바구니에 어떤 공이 들어 있는지 구하는 프로그램을 작성하시오.

<br>

### 입력

첫째 줄에 N (1 ≤ N ≤ 100)과 M (1 ≤ M ≤ 100)이 주어진다.

둘째 줄부터 M개의 줄에 걸쳐서 공을 넣는 방법이 주어진다. 각 방법은 세 정수 i j k로 이루어져 있으며, i번 바구니부터 j번 바구니까지에 k번 번호가 적혀져 있는 공을 넣는다는 뜻이다. 예를 들어, 2 5 6은 2번 바구니부터 5번 바구니까지에 6번 공을 넣는다는 뜻이다. (1 ≤ i ≤ j ≤ N, 1 ≤ k ≤ N)

도현이는 입력으로 주어진 순서대로 공을 넣는다.
<br>

### 출력

1번 바구니부터 N번 바구니에 들어있는 공의 번호를 공백으로 구분해 출력한다. 공이 들어있지 않은 바구니는 0을 출력한다.


### 예시 입력

```
5 4
1 2 3
3 4 4
1 4 1
2 2 2
```

### 예시 출력

```
1 2 1 1 0
```

<br>

<br>



```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        StringBuilder sb = new StringBuilder();
        int n = readInt();
        int m = readInt();
        int a[] = new int[n];
        for(int i = 0; i<n; i++){
            a[i]=0;
        }
        
        for(int i = 0; i<m;i++){
            int start = readInt();
            int num = readInt();
            int end = readInt();
            
            for(int j = start-1; j<end; j++){
                a[j]=num;
            }
        }
        
        for(int i = 0; i<n; i++){
            sb.append(a[i]+" ");
        }
        System.out.println(sb);
    }
    private static int readInt() throws IOException{
        int a = 0;
        int num = System.in.read();
        while(true){
            if(num == ' '|| num=='\n'){
                return a;
            } else {
                a = a*10 + (num-'0');
            }
        }
    }
    
}
```

<div style="text-align:center; font-size:0.8em;">시간 초과</div>

<br>

<br>



```java
import java.io.*;
import java.util.StringTokenizer;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st;
        StringBuilder sb = new StringBuilder();
        st = new StringTokenizer(br.readLine()," ");
        int n = Integer.parseInt(st.nextToken());
        int m = Integer.parseInt(st.nextToken());
        int a[] = new int[n];
        for(int i = 0; i<m;i++){
            st = new StringTokenizer(br.readLine(), " ");
            int start = Integer.parseInt(st.nextToken());
            int end = Integer.parseInt(st.nextToken());
            int num = Integer.parseInt(st.nextToken());
            
            for(int j = start-1; j<end; j++){
                a[j]=num;
            }
        }
        
        for(int i = 0; i<n; i++){
            sb.append(a[i]+" ");
        }
        System.out.println(sb);
    }
    
}
```

<div style="text-align:center; font-size:0.8em;">정답</div>

<br>
<br>

|**메모리**|**시간**|
|:--:|:--:|
|15892 KB|156 ms|

<br>

<br>



```java
public class Main{
    public static void main(String[]args) throws Exception{
        int arrlen=read();
        int []arr = new int[arrlen];
        int count = read();
        for(int i=0;i<count;i++){
            int a = read();
            int b = read();
            int num = read();
            for(int k = a-1;k<b;k++){
                arr[k] = num;
            }
        }
        StringBuilder sb = new StringBuilder();
        for(int i=0;i<arrlen;i++){
            sb.append(arr[i]).append(" ");
        }
        System.out.println(sb);
    }
    private static int read() throws Exception{
        int c, n = System.in.read() & 15;
        while((c=System.in.read())>32){
            n = (n<<3) + (n<<1) + (c&15);
        }
        return n;
    }
}
```

<div style="text-align:center; font-size:0.8em;">모범 답안</div>


<br>
<br>

|**메모리**|**시간**|
|:--:|:--:|
|14172 KB|124 ms|

<br>

<br>

비트연산자를 이용해서 정수를 입력받는 방법도 공부해봐야겠다.