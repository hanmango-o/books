---
description: Kruskal 알고리즘에 대해 알아보고, 이를 활용한 문제 해결 방법을 살펴봅니다.
---

# Kruskal

## Kruskal 이란?

***

크루스칼 알고리즘은 [minimum-spanning-tree-mst.md](../data-structure/minimum-spanning-tree-mst.md "mention")를 찾는 알고리즘입니다.

모든 간선이 양의 가중치를 가져야 하는 [dijkstra.md](dijkstra.md "mention")와 다르게 크루스칼 알고리즘은 음의 가중치를 가지고 있어도 최단거리를 찾을 수 있습니다.

### Kruskal 구조

크루스칼 알고리즘은 간선의 가중치를 기준으로 오름차순 정렬 ([sort.md](sort.md "mention")) 후, 순차적으로 선택하며 가중치가 가장 작은 간선들로 이루어진 노드들을 선택하게 됩니다.

이때, [union-find.md](../data-structure/union-find.md "mention")를 활용하여 트리 구조, 즉 그래프 상에 사이클이 발생하지 않도록 제어하여, 최소한의 비용을 사용한 MST 구조가 되도록 합니다.

크루스칼 알고리즘의 구조는 아래와 같습니다.

```cpp
vector<tuple<int, int, int>> vect; // (가중치, 노드1, 노드2) 저장 vector
int parent[N], cnt = N, answer; // Union Find 설정 변수

int Find(int tar) { // Union Find
    if(tar == parent[tar]) return tar;
    int ret = Find(parent[tar]);
    parent[tar] = ret;
    return ret;
}

void Union(int a, int b) { // Union Find
    int pa = Find(a);
    int pb = Find(b);
    if(pa == pb) return;
    parent[pb] = pa;
    cnt--; // 전체 집합의 수가 1인지 확인하기 위한 배열
}

sort(vect.begin(), vect.end()); // [1] 가중치에 따른 오름차순 정렬

for(int i = 1; i <= N; i++) parent[i] = i; // Union Find init

for(int i = 0; i < vect.size(); i++) { // [2] 간선 탐색
    int vol, from, to;
    tie(vol, from, to) = vect[i];
    if(Find(from) == Find(to)) continue; // 사이클 제어
    Union(from, to); // 트리 갱신
    answer += vol; // 누적 가중치 갱신
}

if(cnt == 1) cout << answer; // 모든 노드를 포함한 트리 생성 성공
else cout << -1; // 트리 생성 실패(모든 노드 포함 X)
```

#### \[1] 가중치에 따른 오름차순 정렬

존재하는 모든 간선 정보를 담은 vector를 가중치에 따라 오름차순 정렬하여 가장 작은 가중치의 간선부터 탐색이 가능하도록 합니다.

#### \[2] 간선 탐색

정렬된 간선 vector를 0번 인덱스부터 탐색하며 트리를 생성합니다.

이때 사이클이 생기지 않는 그래프, 즉 트리 구조가 되도록 Union Find를 활용하여 사이클 여부를 판단합니다.



## Kruskal 활용

***

크루스칼 알고리즘은 대표적으로 아래의 경우에 사용합니다.

### 음의 가중치에서의 최단 거리 탐색

[dijkstra.md](dijkstra.md "mention")와는 다르게 크루스칼 알고리즘은 정렬 후 사용하기에 음의 가중치를 가지는 경우에도 최단 거리 탐색이 가능합니다.

### 양의 가중치에서 MST 탐색

양의 가중치를 가지는 그래프에서 MST, 즉 최소 신장 트리를 찾을 때도 크루스칼 알고리즘을 활용할 수 있습니다.

{% hint style="info" %}
그래프가 양의 가중치여도 항상 다익스트라를 사용하는 것은 아닙니다.
{% endhint %}

이때, 그래프에  존재하는 모든 노드가 포함된 트리여야 한다는 것에 주의해야 합니다.



## 주의할 점

***

크루스칼 알고리즘은 다익스트라 알고리즘과 유사하게 최단 거리를 찾는 알고리즘입니다.

그래프가 음의 가중치를 가지게 된다면, 다익스트라의 사용이 불가능하기에 크루스칼의 사용에 고민이 필요없지만 양의 가중치라면 선택이 필요합니다.

이러한 선택은 아래의 조건에 따라 진행하게 됩니다.

### 특정 시작점과 도착점이 존재하지 않는 경우

크루스칼은 다익스트라와 다르게 특정 시작점과 도착점이 존재하지 않고, 현재 선택된 전체 트리의 가중치의 합이 최소가 되는 것이 중요합니다.

즉, 특정 지점에 영향을 받지 않으며 구하고자 하는 값이 전체 경로의 가중치의 합이라면 크루스칼이 사용됩니다.



