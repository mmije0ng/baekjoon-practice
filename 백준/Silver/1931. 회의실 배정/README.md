# [Silver I] 회의실 배정 - 1931 

[문제 링크](https://www.acmicpc.net/problem/1931) 

### 성능 요약

메모리: 46060 KB, 시간: 616 ms

### 분류

그리디 알고리즘, 정렬

### 제출 일자

2023년 11월 14일 01:20:55

### 문제 설명

<p>한 개의 회의실이 있는데 이를 사용하고자 하는 N개의 회의에 대하여 회의실 사용표를 만들려고 한다. 각 회의 I에 대해 시작시간과 끝나는 시간이 주어져 있고, 각 회의가 겹치지 않게 하면서 회의실을 사용할 수 있는 회의의 최대 개수를 찾아보자. 단, 회의는 한번 시작하면 중간에 중단될 수 없으며 한 회의가 끝나는 것과 동시에 다음 회의가 시작될 수 있다. 회의의 시작시간과 끝나는 시간이 같을 수도 있다. 이 경우에는 시작하자마자 끝나는 것으로 생각하면 된다.</p>

### 입력 

 <p>첫째 줄에 회의의 수 N(1 ≤ N ≤ 100,000)이 주어진다. 둘째 줄부터 N+1 줄까지 각 회의의 정보가 주어지는데 이것은 공백을 사이에 두고 회의의 시작시간과 끝나는 시간이 주어진다. 시작 시간과 끝나는 시간은 2<sup>31</sup>-1보다 작거나 같은 자연수 또는 0이다.</p>

### 출력 

 <p>첫째 줄에 최대 사용할 수 있는 회의의 최대 개수를 출력한다.</p>

### 어려웠던 점
 <p>그리디 알고리즘을 이용하는 문제. </p>
 <p>처음에는 정렬의 순서를 시작점을 기준으로 오름차순으로 정렬했었는데 그렇게 되면 벡터의 원소들을 한 번씩 순회하면서 또 다시 시작 회의실을 찾는 작업이 필요하게 된다. 그러다 보니 시작 회의실을 다시 찾고 그곳에서 끝점과 다음 벡터의 원소의 시작점과 비교하는 부분에서 반례가 생긴 것 같았다.</p>
<p>다른 풀이들을 찾아보니 시작점이 기준이 아닌 끝점을 기준으로 오름차순으로 정렬해야 문제를 해결할 수 있음을 깨달았다. 최단 시간안에 끝내야 최대한 많은 회의를 추가할 수 있기 때문에 끝점을 기준으로 오름차순으로. 또한 끝점이 같다면 시작점이 더 작은 것이 더 먼저 오도록(그래야 더 많은 회의를 담을 수 있기 때문. 예를 들어 (4,5) (5,5)가 있으면 (4,5)다음에 (5,5)가 선택되어야 함 ) 정렬 기준을 바꾸었다.</p>
<p>그런 다음 기준점의 끝점보다 벡터의 원소의 시작점이 더 크다면 count를 증가시켜주었다. 회의가 1개라면 반드시 count 되어야 하고 count의 초기값을 0으로 주었기 때문에 출력은 count+1이 되어야 한다.</p>
<br>
<p>알고리즘 스터디에서 자바에서 Comparator과 Comparable를 통해 sort함수를 쓰는 법에 대해 공부하였었는데 시간이 까먹어서 다른 코드를 참고하였다. 심지어 원래는 Comparator 인터페이스를 구현한 클래스를 따로 만들었었는데  <mark>java.lang.IllegalArgumentException</mark> 잘못된 인수 값이 메소드에 전달될 때 (아마 compare를 잘 못 서서 그런듯) 발생하는 예외 때문에 컴파일 에러가 났었다. 시간이 날 때 다시 공부해야겠다. (반성해라 ^^) </p>
