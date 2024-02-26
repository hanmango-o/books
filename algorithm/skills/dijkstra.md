---
description: 다익스트라 알고리즘에 대해 알아보고, 이를 활용하는 방법을 살펴봅니다.
---

# Dijkstra

## 다익스트라란?

***

다익스트라란 모든 간선의 가중치가 양수인 그래프에서 최단 거리를 구하는 알고리즘입니다.

특정 지점에서 다른 모든 지점으로의 최단 거리를 구할 때 사용하며, 시작 정점에서 최소거리를 [greedy.md](greedy.md "mention")하게 선택하며 진행하는 알고리즘입니다.

{% hint style="info" %}
다익스트라는 반드시 모든 간선의 가중치가 양의 정수일 경우에만 사용해야 합니다.

만약 모든 간선의 가중치가 동일하다면, [bfs.md](bfs.md "mention")를 사용합니다.
{% endhint %}

### 다익스트라 구조

기본적인 다익스트라는 아래의 형태를 가지고 있습니다.

```cpp
int dist[N]; // [1] dist 배열

void DIKS() {
    // 우선 순위 큐(greater)
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
    
    for(int i = 0; i < N; i++) {
        dist[N] = (int)1e9; // [2] dist 배열 초기화
    }
    
    dist[S] = 0; // [3] 시작 지점 설정
    pq.push(make_pair(dist[S], S));
    
    while(!pq.empty()) {
        int now_node, now_vol;
        tie(now_vol, now_node) = pq.top();
        pq.pop();
        
        for(int i = 0; i < graph[now_node].size(); i++) { // [4] 그래프 탐색
            int next_node, next_vol;
            tie(next_vol, next_node) = graph[now_node][i];
            if(dist[next_node] <= dist[now_node] + next_vol) continue; // [4] 탐색 조건
            dist[next_node] = dist[now_node] + next_vol;
            pq.push(make_pair(next_node, next_vol));
        }
    }
}
```

다익스트라는 [priority-queue.md](priority-queue.md "mention")(우선 순위 큐)를 활용하여 그래프를 탐색합니다.

{% hint style="warning" %}
우선 순위 큐의 요소는 (간선 가중치, 노드 번호) 형태의 pair가 주로 사용됩니다.

이때, (간선 가중치, 노드 번호)의 순서에 유의하여 값을 할당해야 합니다.
{% endhint %}

#### \[1] dist 배열

다익스트라는 탐색을 위한 거리 저장 배열인 dist가 반드시 필요합니다.

dist 배열의 크기는 전체 노드의 개수만큼 되도록 설정하여 [dat.md](dat.md "mention")의 효과를 낼 수 있도록 합니다.

{% hint style="info" %}
문제에 따라 노드 번호가 1번부터 시작하는 경우가 있을 수 있습니다.

이에 주의하여 dist 배열의 크기를 선언해야 합니다.
{% endhint %}

이러한 dist 배열은 [backtracking.md](backtracking.md "mention")으로 각 노드까지의 거리를 최단 거리로 갱신할 수 있도록 합니다.

#### \[2] dist 배열 초기화

본격적인 다익스트라 탐색에 앞서, dist 거리 배열 초기화가 필요합니다.

다익스트라의 경우 dist 배열의 값을 기준으로 우선 순위 큐에 다음 노드를 push 합니다. 다익스트라는 최단 거리 알고리즘이기 때문에 초기 dist 배열의 요소값, 즉 각 노드까지의 거리는 최대값(1e9)으로 설정합니다.

{% hint style="warning" %}
최대값은 1e9와 같이 최대한 큰 값으로 할당해야 합니다.

하지만 INT\_MAX 와 같이 해당 데이터 타입에서 나올 수 있는 최대값은 오버플로우 발생을 방지하기 위해 사용하면 안됩니다.
{% endhint %}

#### \[3] 시작 지점 설정

탐색이 처음 시작되는 노드의 dist 배열값을 0으로 갱신합니다.

#### \[4] 그래프 탐색

[bfs.md](bfs.md "mention")를 활용한 기본적인 그래프 탐색과 동일하게 탐색을 진행합니다. 단, 다익스트라는 큐가 아닌 우선 순위 큐를 사용합니다.

우선 순위 큐를 사용하여 가장 우선 순위가 큰, 즉 현재 큐에 들어있는 노드 중 간선의 크기가 가장 작은 노드를 선택합니다.

이후 탐색 조건에 따라 다음 노드를 선택하고 dist 배열을 갱신한 후, 우선 순위 큐에 push 합니다.

여기서 탐색 조건이란, dist 배열에 기록된 다음 노드의 거리가 dist 배열에 기록된 현재 노드의 거리와 다음 노드의 간선의 크기의 합보다 클 경우를 의미합니다.

즉, `dist[next] > dist[now] + now_vol` 일 경우, `dist[next]를 dist[now] + now_vol` 로 갱신하고 `next` 노드를 우선 순위 큐에 넣어 다음 탐색 노드로 지정하게 됩니다.



## 다익스트라 활용

***

다익스트라를 활용하는 대표적인 예시는 아래와 같습니다.

### 특정 노드에서 모든 노드에 대한 최단거리

기본적으로 특정 시작 노드에서 모든 노드에 대한 최단 거리를 구할 때 다익스트라 알고리즘을 활용합니다.

이때, 모든 간선의 가중치는 양의 정수이여야 하며 만약 모든 가중치의 크기가 동일하다면 [bfs.md](bfs.md "mention")를 사용하는 것이 더 효과적입니다.

### 모든 노드에서 특정 노드에 대한 최단거리

앞서 살펴본 [#undefined-3](dijkstra.md#undefined-3 "mention")와는 반대로 모든 노드에서 특정 노드에 대한 최단 거리를 구할 때도 다익스트라 알고리즘이 사용됩니다.

기본적인 다익스트라 사용 방법은 동일하나, 그래프의 변경이 선행되어야 합니다. 그래프의 변경이란 그래프의 모든 간선의 방향을 뒤집는 것을 의미합니다.

{% hint style="info" %}
단, 양방향 그래프의 경우 간선의 방향을 뒤집을 필요는 없습니다.
{% endhint %}

이후, 특정 노드에서 모든 노드로 다익스트라를 진행하여 모든 노드에서 특정 노드에 대한 최단거리를 구합니다.

### 특정 도착 지점까지의 최단 경로 확인

특정 도착 지점까지의 최단 거리를 구하기 위해서는 다익스트라 알고리즘이 효과적입니다. 하지만 단순히 거리가 아닌 경로를 구하고 싶다면 어떻게 해야 할까요?

이를 위해 경로를 담는 PATH 배열을 활용한 [backtracking.md](backtracking.md "mention")으로 특정 도착 지점까지의 최단 경로를 구할 수 있습니다.

기본적인 다익스트라를 진행하면서, dist 배열이 갱신될 때 마다 PATH 배열을 갱신합니다. PATH 배열은 다음 노드가 인덱스이며 해당 인덱스의 요소 값이 현재 노드가 되도록 갱신합니다.

```cpp
int PATH[N];

int DIKS() {
    // 기존 다익스트라 구조와 동일
    while(!pq.empty()) {
        // 기존 그래프 탐색 구조 동일
        
        dist[new_node] = dist[now_node] + new_vol;
        PATH[new_node] = now_node; // PATH 배열 갱신        
    }
}
```

이후 모든 노드에 대한 탐색이 끝나면, 아래와 같이 도착 지점에서 시작하여 시작 지점으로 경로를 출력할 수 있습니다. 이때 출력되는 순서는 `도착 지점 > 시작 지점`이 되게 되며, 이를 뒤집어야 `시작 지점에서 도착 지점으로의 경로`가 되게 됩니다.

```cpp
int node = END_NODE; // 도착 노드
vector<int> vect; // 결과 저장 vector
vect.push_back(node); // 도착 노드 push
while(node != START_NODE) { // 시작 노드까지 반복
    node = PAHT[node]; // 다음 node 경로 갱신
    vect.push_back(node); // node push
}
for(int i = vect.size() - 1; i >= 0; i--) {
    cout << vect[i]; // 최단 경로 출력
}
```

### 2개 이상의 최단 경로에서 조건에 따른 경로 선택

만약 존재하는 최단 경로가 여러 개일 경우, 조건에 따라 최단 경로를 선택해야 한다면 기존의 dist 배열과 graph 를 활용하여 선택할 수 있습니다.

다익스트라를 통해 최단 경로를 구한 상태라면, dist 배열 역시 최종적인 최단 경로 값으로 초기화된 상태라고 할 수 있습니다.

이때, dist 배열과 graph 간에는 아래의 규칙이 성립합니다.

```cpp
dist[now] + graph[now][next] == dist[next] // 성립 시 최단 경로에 해당하는 노드임을 의미
```

위 식의 의미는 `현재 노드까지의 최단 거리 + 현재 노드와 다음 노드까지의 간선의 크기 = 다음 노드까지의 최단 거리` 가 되게 됩니다.

이를 활용하여 아래와 같이 조건에 따라 최단 경로를 선택할 수 있습니다.

```cpp
while(node != START_NODE) {
    for(int i = 0; i < graph[node].size(); i++) {
        if(dist[node] + graph[node][i] == dist[graph[node][i]]) {
            node = i;
            break;
        }
    }
    vect.push_back(node);
}
```

### 지도에서의 최단 거리

앞서 살펴본 인접 리스트를 활용한 그래프 뿐만 아니라, 인접 리스트에서도 다익스트라를 활용할 수 있습니다.

또한, 지도 환경에서도 다익스트라를 활용할 수 있습니다. 이때 [dx-dy.md](dx-dy.md "mention")를 통해 진행하며 지도에서의 최단 거리를 구할 수 있습니다.
