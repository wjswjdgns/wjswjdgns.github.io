---
title:  "union-find 알고리즘"
tag: 
  - 알고리즘
  - 이론 공부 
---

# union-find 알고리즘 이란?
### 일반적으로 여러 노드가 있을 때 특정 2 개의 노드를 연결해 1 개의 집합으로 묶는 union 연산과 두 노드가 집합에 속해 있는지를 확인하는 find 연산으로 구성되어 있는 알고리즘

출처 : Do it 코딩 테스트

>  Union 연산 : 각 노드가 속한 집합을 1 개로 합치는 연산 <br>
>  find 연산 : 특정 노드가 a 에 관해 a 가 속한 집합의 대표 노드를 반환하는 연산 (Root 노드를 반환)


## Union 진행과정
*일반적인 방법은 1 차원 리스트를 이용*
1. 처음에 노드가 연결되어 있지 않으므로 각 노드가 대표 노드가 됩니다.
<img width="1090" alt="image" src="https://user-images.githubusercontent.com/55444587/194046856-224ba6fc-70f8-4524-a004-3b32fe621db9.png">

2. 2 개의 노드를 선택해 각각의 대표 노드를 찾아 연결하는 union 연산을 수행
ex : union(1,4) , union(5,6)
4 의 대표노드를 1 로 설정 
6 의 대표노드를 5 로 설정

ex : union (4,6)
6 의 대표노드를 4 로 설정 해야 하지만
6 의 대표노드는 5 로 되어있으며 4 의 대표노드는 1 이기에 u
5 의 대표노드를 1 로 설정

<img width="1109" alt="image" src="https://user-images.githubusercontent.com/55444587/194046969-076da885-56b9-4558-b3a7-4a2fc071bef2.png">


## find 진행 과정
1. 대상 노드 리스트에 index 값과 value 값이 동일한지 확인 (동일한 경우에는 루트 노드)
2. 동일하지 않으면 value 값이 가리키는 index 위치로 이동
3. 이동 위치의 index 값과 value 값이 같을 때까지 1~2 번을 반복 (재귀, 또는 while 문)
4. 대표 노드에 도달하면 재귀 함수를 빠져나오며 모든 노드 값을 대표 노드 값으로 변경

## 장점
find 연산을 잘 생각하면 시간복잡도가 줄어드는 효과를 얻을 수 있다. 연산을 할 때 거치는 노드들이 대표 노드와 바로 연결되는 형태로 되기 때문에 find 연산 속도는 O(1) 로 변경
## 문제점
{1,2,3,4,5}의 집합에서 union 연산이 (4,5), (3,4), (2,3), (1,2)와 같다고 할때, 부모테이블은 다음과 같아집니다.
<pre>
노드번호 	1	2	3	4	5
부모	1	1	2	3	4
</pre>
이 경우 5의 루트노드를 찾기 위해서는 5->4->3->2->1 총 O(N)의 시간이 소요됩니다. 결과적으로 위의 find 함수를 그대로 사용하면 노드의 개수 N개, union나 find 연산의 개수 M개라고 할때 최악의 경우 O(N*M)의 시간이 소요됩니다.

## 사용 방법
### 사이클 판별법
유니온파인드 알고리즘을 이용해서 무방향 그래프 내에서 사이클을 판별할 수 있습니다.

1. 각 간선을 확인하면서 두 노드의 루트노드를 확인.
2. 루트 노드가 서로 다르면 -> union 연산 수행
3. 루트 노드가 서로 같으면 cycle 발생
4. 모든 간선에 대해 1 반복

```
for i in range(e):
  a, b = map(int, input().split())
  # 사이클이 발생  --> break
  if find(parent, a) == find(parent, b):
      cycle = True
      break
  # 사이클이 발생하지 않았다면 Union 연산 수행
  else:
      union(parent, a, b)
```
