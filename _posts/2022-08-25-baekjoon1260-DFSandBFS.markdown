---
title:  "[Python] 백준문제풀이 - 1260번 DFS 와 BFS"
tag: 
  - 백준 
  - 문제풀이 
  - Python 
  -  DFS 
  -  BFS
---

## 문제설명
<a href="https://www.acmicpc.net/problem/1260">문제 링크</a>

### 문제
<p>
그래프를 DFS로 탐색한 결과와 BFS로 탐색한 결과를 출력하는 프로그램을 작성하시오. 단, 방문할 수 있는 정점이 여러 개인 경우에는 정점 번호가 작은 것을 먼저 방문하고, 더 이상 방문할 수 있는 점이 없는 경우 종료한다. 정점 번호는 1번부터 N번까지이다.
</p>

### 입력
<p>
첫째 줄에 정점의 개수 N(1 ≤ N ≤ 1,000), 간선의 개수 M(1 ≤ M ≤ 10,000), 탐색을 시작할 정점의 번호 V가 주어진다. 다음 M개의 줄에는 간선이 연결하는 두 정점의 번호가 주어진다. 어떤 두 정점 사이에 여러 개의 간선이 있을 수 있다. 입력으로 주어지는 간선은 양방향이다.
</p>

### 출력
<p> 
첫째 줄에 DFS를 수행한 결과를, 그 다음 줄에는 BFS를 수행한 결과를 출력한다. V부터 방문된 점을 순서대로 출력하면 된다.
</p>

<div>
  <strong>예제 입출력 01</strong>
  <pre>
4 5 1
1 2
1 3
1 4
2 4
3 4
</pre>
  <pre>
1 2 4 3
1 2 3 4
</pre>
</div>
<div>
<strong>예제 입출력 02</strong>
  <pre>
5 5 3
5 4
5 2
1 2
3 4
3 1
</pre>
  <pre>
3 1 2 5 4
3 1 4 2 5
</pre>
</div>
<div>
<strong>예제 입출력 03</strong>
  <pre>
1000 1 1000
999 1000
</pre>
  <pre>
1000 999
1000 999
</pre>
</div>

## 문제풀이
### 문제 해결 시 고려사항
<ul>
<li>DFS 와 BFS 둘다 사용 필요</li>
<li>정점이 여러가지 일 경우 정점 번호가 작은 것을 먼저 방문 <strong>(정렬 필요)</strong></li>
</ul>
### 부분 코드 
#### 그래프 구조 코드
```python
N,M,V = map(int, input().split())
value_list = [[ ] for _ in range(N)] 
```
```python
for _ in range(M):
    a,b = map(int, input().split())
    value_list[a-1].append(b)
    value_list[b-1].append(a)
```
#### DFS 와 BFS 사용을 위한 Visited
```python
visited01 = [False for _ in range(N)]
visited02 = [False for _ in range(N)]
```
#### DFS 코드
```python
def DFS(start):
    visited01[start] = True
    print(start+1, end=" ")
    for i in value_list[start]:
        if visited01[i-1] != True:
            DFS(i-1)
```
#### BFS 코드
```python
def BFS(start):
    child = deque()
    child.append(start+1) 
    visited02[start] = True 
    
    while child:
        road = child.popleft() #
        print(road, end=" ")
        for child_value in value_list[road-1]:
            if visited02[child_value-1] == False: 
                child.append(child_value)
                visited02[child_value-1] = True
```
### 전체 코드
<script src="https://gist.github.com/wjswjdgns/97207f900c7486326bec0241f4d2a8f9.js"></script>

