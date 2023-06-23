---
layout: single
title: "(Java) String과 StringBuilder"
categories:
  - Java
---

## String과 StringBuilder

<br>
<br>

백준 코딩테스트 문제(2884번)를 풀던 중 모범 답안에 사람들이 **String** 대신 **StringBuilder** 객체를 사용하는 것을 봤다. 그렇게 함으로써 메모리와 시간을 절약했는데, 차이점이 뭘까?

## String

**String** 객체는 변경이 불가능하다. 즉, 한 번 생성되면 내용을 변경하는 것이 불가능 하다는 뜻이다. 

하지만 평소에 우리는 String 객체끼리 잘 합치지 않는가?

A와 B 두 문자열을 연결하게 되면 새로운 C 문자열 객체를 생성해서 거기에 저장한 다음 합치기 전의 객체들은 가비지 콜렉터로 들어가게 되는 것을 우리는 합쳐졌다고 보는 것이다. 한두개 정도의 작은 문자열들을 합칠 때는 이것이 별로 차이가 안나겠지만, 이것이 양이 많아지면 그만큼 객체가 생성되기 때문에 메모리와 시간이 오래 걸리게 되는 것이다. 

그래서 변경 가능한 **StringBuiler**를 사용하는 것이다.

**StringBuilder** 객체를 생성 후, **.append**를 통해서 문자열을 뒤에다가 계속 연결시켜주면 된다.

다만, 주의해야할 점은 StringBuilder를 바로 String 객체로 저장할 수 없다는 점이다. **toString**으로 변환해서 저장해줘야한다. 


```java

class Main{
    public static void main(String args[])throws IOException{
        StringBuilder stringBuilder = new StringBuilder();
        stringBuilder.append("문자열 ").append("연결");

        String str = stringBuilder.toString();
    }
}
```

<div style="text-align:center; font-size:0.8em;">toString 사용</div>

