---
layout: single
title: "(Project) 시간별 지하철 혼잡도"
categories:
  - Project
---

## 지하철 혼잡 노선도를 들어가며

<br>

지하철 노선도 api를 찾아서 가져오려 했으나 결국 못 찾았다. 그래서 직접 그려보려 한다...(카카오는 지하철api를 오픈api로 제공하지 않는다고 한다.)

우선은 서울역을 중심으로 1~9호선 순으로 순차적으로 그려나갈 예정이다. 다 그리면 공공데이터 포털에서 시간별 지하철 혼잡도 데이트를 받아와 시간별로 체크할수 있도록 할 예정이다.

<br>
<br>
<br>
<br>

## 프로젝트 생성

![image]({{ site.baseurl }}/images/subway/20230627_150503.png)

<div style="text-align:center; font-size:0.8em;">프로젝트 생성</div>

<br>
<br>
<br>
<br>

기준점인 서울역을 찍어준다.

![image]({{ site.baseurl }}/images/subway/20230627_155332.png)

<div style="text-align:center; font-size:0.8em;">서울역</div>

<br>

이제 시작이지만 뭔가 두근거린다.

원은 div태그 생성후 border-radius를 50%로 해주고, border 생성

<br>
<br>
<br>
<br>

![image]({{ site.baseurl }}/images/subway/20230627_182158.png)

<div style="text-align:center; font-size:0.8em;">노선</div>

<br>
<br>

역 사이 노선은 canvas를 활용해서 그리려했으나 나중에 혼잡도를 표시하기에 어려울것 같아 div태그를 얇게 생성해서 이어주도록 했다. 이렇게 하면 나중에 양방향 혼잡도를 나타낼때 양쪽 테두리 선의 색을 변경해 주면 될 것 같기때문에 추가로 선을 안그어줘도 될 것 같다.