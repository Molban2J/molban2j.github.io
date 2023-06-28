---
layout: single
title: "(Java) BufferedWriter란?"
categories:
  - Java
---

# BufferedWriter란?

백준 문제를 풀어보던 중 [15552번 문제](https://www.acmicpc.net/problem/15552 "백준")에서 System.out.println을 사용하지 않고 BufferedWriter를 사용해서 문자를 출력하면 더 빠르게 출력 할 수 있다고 한다.

기존의 BufferedReader의 반대이므로 사용법 자체는 어렵지 않았다. InputStreamReader는 **OutputStreamWriter**로, System.in은 **System.out**으로 모두 반대로 작성해주면 되기 때문이었다.

다만 차이점은 마지막에 **BufferedWriter.flush()**를 해줘야한다는 점이었다.


## BufferedWriter 사용법


```java
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.OutputStreamWriter;

class Main{
    public static void main(String args[]) throws IOException{
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out)); // 선언
        String str = "abcdef"; // 출력할 문자열
        bw.write(s); // 출력
        bw.newLine(); // 줄바꿈
        bw.flush(); // 남아있는 데이터 모두 출력
        bw.close();
}
```

<div style="text-align:center; font-size:0.8em;">BufferedWriter</div>

<br>
<br>
<br>

백준 문제를 풀 때 나는 **flush()**가 출력 기능이라 생각했다. **write()**로 버퍼에 쓰고 flush()로 출력인줄 알아서 반복문 안에다가 flush()를 넣었다. 그래서 시간 초과가 떴던 것이다. 하지만 그게 아니라 flush()는 그저 출력 버퍼를 비워주는 역할이었던 것이다.

