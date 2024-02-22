---
description: Union Find 자료구조에 대해 알아보고, 이를 통해 Disjoint Set을 구현하는 방법을 살펴봅니다.
---

# Union Find

## Disjoint Set(서로소 집합) 이란?

***

서로소 집합이란 상호 배타적인 집합들로 구성된 Set 을 의미합니다. 즉, 서로 중복된 원소가 없는 집합을 서로소 집합이라 할 수 있습니다.

{% hint style="info" %}
서로 중복된 원소가 없는 집합이란, 구성된 집합들이 교집합이 없다는 것을 의미합니다.
{% endhint %}

## Union Find란?

***

Union Find란 자료구조로, 주어진 원소들로 만들 수 있는 집합쌍 중 서로소 집합인 쌍을 만들 수 있는 자료구조입니다.

### Union Find 구성

기본적인 Union Find 자료구조는 아래의 구성 요소로 이루어져 있습니다.

#### Root 원소 보관 배열(parent)

각 집합의 Root 원소를 보관하는 보관 배열이 필요합니다. 해당 보관 배열의 원소값에는 해당 원소(인덱스)가 속해 있는 집합의 Root 값이 할당됩니다.

만약 해당 원소가 Root 원소라면 원소값과 인덱스값이 동일하게 할당됩니다.

#### Find 함수

Union Find 자료구조에서 Root 원소 보관 배열(parent)을 재귀적으로 탐색하며 해당 집합의 Root 원소를 찾는 Find 함수가 필요합니다.

#### Union 함수

주어진 2개의 원소를 하나의 집합으로 만드는 Union 함수가 필요합니다.&#x20;

이때 주어진 원소는 무조건 집합을 이루고 있어야 하며(단일 집합도 가능), 주어진 원소가 속해 있는 2개의 집합이 합쳐지며 하나의 집합이 되게 됩니다.



## Union Find 구현

***

Union Find를 사용하기 위해서는 3가지 구현이 필요합니다.

### 집합 초기화

먼저 주어진 원소들을 사용하여 단일 원소로 이루어진 집합을 구성해야 합니다.

```cpp
int parent[N]; // Root 원소 보관 배열(DAT)
for(int i = 0; i < N; i++) { // 단일 원소 집합 생성
    parent[i] = i;
}
```

### Find 함수 구현

앞선 [#find](union-find.md#find "mention")는 아래와 같이 구현할 수 있습니다.

```cpp
int Find(int tar) { // 요소 tar, tar가 포함된 집합의 Root 원소 return
    if(tar == parent[tar]) return tar; // [1] Base case
    int ret = Find(parent[tar]); // [2] Recursive case
    return ret;
}
```

#### \[1] Base case

Find 함수에서 Base case는 [#root-parent](union-find.md#root-parent "mention")에서 인덱스값과 원소값이 동일할 때, 즉 해당 요소가 Root 원소일 때입니다.

#### \[2] Recursive case

재귀문의 경우 현재 원소의 Root 원소를 대상으로 동작하게 됩니다.

해당 재귀문을 반복하여 [#id-1-base-case](union-find.md#id-1-base-case "mention")를 만족하는 Root 원소를 찾게 됩니다.

{% hint style="warning" %}
위 Find 구현은 완료된 상태가 아닙니다.

아래의 [#undefined-3](union-find.md#undefined-3 "mention")을 완료한 상태가 완전한 Find함수입니다.
{% endhint %}

### Union 함수 구현

마지막으로 [#union](union-find.md#union "mention")는 아래와 같이 구현합니다.

```cpp
void Union(int a, int b) { // a : 기준 원소, b : 합칠 원소
    int pa = Find(a); // [1] 각 원소의 Root 원소를 찾음
    int pb = Find(b);
    if(pa == pb) return; // [2] 동일 집합 확인
    parent[pb] = pa; // [3] 그룹화
}
```

Union은 그룹화하고 싶은 두 원소 a, b가 주어지게 됩니다. 이때, a는 기준 원소, b는 합칠 원소를 나타냅니다.

{% hint style="info" %}
Union이 정상적으로 동작할 경우, b 원소가 포함된 집합은 a 원소가 포함된 집합에 포함되게 됩니다.
{% endhint %}

#### \[1] Root 원소 찾기

주어진 두 원소(a, b)가 각각 포함된 집합의 Root 원소를 [#find](union-find.md#find "mention")를 이용하여 찾습니다.

#### \[2] 동일 집합 확인

만약 두 원소가 이미 동일 집합에 포함되어 있는 경우라면, Union을 하는 것이 무의미하므로 함수를 종료합니다.

#### \[3] 그룹화

두 원소가 서로 다른 집합에 포함되어 있다면, [#root-parent](union-find.md#root-parent "mention")의 합칠 원소(b)의 Root 원소를 기준 원소(a)의 Root 원소로 할당합니다.



## Union Find 특징

***

### 전체 집합의 개수 확인

Union Find의 대표적인 특징은 Union에 성공할 때마다 전체 집합의 수는 -1이 되게 됩니다.

[#undefined](union-find.md#undefined "mention")를 통해 단일 원소로 구성된 집합을 생성한다면, 원소의 개수만큼 집합이 생기게 됩니다.

이후 조건에 맞게 Union 수행 시 아래와 같이 - 1을 하게 된다면, 전체 집합의 개수를 알 수 있습니다.

<pre class="language-cpp"><code class="lang-cpp">int result = N; // 전체 집합의 개수
void Union(int a, int b) {
    int pa = Find(a);
    int pb = Find(b);
    if(pa == pb) return;
    parent[pb] = pa;
<strong>    result--; // Union 성공 시 -1
</strong>}
</code></pre>

#### 주의할 점

Union 성공 시 -1을 수행하며 전체 집합의 개수를 확인하는 것은 문제 조건에 따라 정답이 아닐 수 있습니다.

만약 문제 조건에서 단일 원소로 구성된 집합은 집합이 아니라고 한다면, 위와 같은 방법으로는 전체 집합의 개수를 확인할 수 없습니다.

이런 경우&#x20;

```cpp
int result = N, cnt = 0;
void Union(int a, int b) {
    int pa = Find(a);
    int pb = Find(b);
    if(pa == pb) return;
    parent[pb] = pa;
    cnt++;
    answer--; // Union 성공 시 -1
}

result - (N - (N - result))
```





### 경로 압축

Find 함수는 재귀적으로 동작하게 됩니다.

```cpp
// Example Group
1(Root) --- 2 --- 3 --- 4

Find(4);
```

만약 위와 같이 그룹의 Root가 구성되어 있다면, depth 4에 따른 8번의 동작이 이루어지게 됩니다.

하지만 위 그룹은 아래와 같이 나타낼 수도 있습니다.

```cpp
2 --- 1(Root) --- 3
        |
        4
```

이렇듯Root 원소가 1인 것은 동일하고, 그룹 구성원도 변함없지만 depth는 1로 표현이 가능합니다.

위 그룹은 Find 함수 적용 시 depth 가 1이기 때문에 2의 비용이 발생하여 효율적이라 할 수 있습니다.

위 그룹과 같이 그룹의 Root 원소와 그 외 원소들 간의 depth를 1로 바꿔 Find 시 비용을 줄이는 방법을 **경로 압축**이라 합니다.

경로 압축은 Find 함수에서 아래와 같이 구현할 수 있습니다.

<pre class="language-cpp"><code class="lang-cpp">int Find(int tar) {
    if(tar == parent[tar]) return tar;
    int ret = Find(parent[tar]);
<strong>    parent[tar] = ret; // 경로 압축 코드 추가
</strong>    return ret;
}
</code></pre>

이러한 **경로 압축은 반드시 필요**하며, 만약 경로 압축이 없을 경우 Union Find는 탐색 시 O(logN)의 시간을 만족하지 못하게 되어 비효율적인 자료구조가 되게 됩니다.



## Union Find 응용

***

Union Find를 활용하여 문제를 해결하는 경우는 대표적으로 아래와 같습니다.

### 동일 그룹 여부확인

기본적으로 Union Find를 활용한다면, 주어진 원소들이 동일 그룹인지 확인이 가능합니다.

만약 특정 원소 2개가 주어졌을 때, 두 원소가 동일 그룹인지 확인하기 위해서는 Find 함수를 사용할 수 있습니다.

```cpp
if(Find(a) == Find(b)) { /* 동일 집합 원소 */ } 
else { /* 동일 집합 아님 */ }
```

### 순환(Cycle) 여부확인

Union Find를 활용하여 무방향 그래프에서 사이클이 발생하는 지 확인할 수 있습니다.

만약 아래와 같은 그래프가 있다고 가정하겠습니다.

```cpp
1 --- 2 --- 3
|     |
4 --- 5
```

위 그래프가 순환하는 지, 즉 사이클이 발생하는 지를 확인하고 싶다면 아래와 같이 Union Find를 적용하여 확인할 수 있습니다.

```cpp
// 그래프는인접 리스트로 구현했다고 가정
// 각 노드는 단일 원소 집합으로 초기화했다고 가정

void isCycle() {
    for(int i = 1; i <= 5; i++) {
    visited[i] = true;
        for(int j = 0; j < graph[i].size(); j++) {
            if(visited[graph[i][j]] == true) continue;
            if(Find(i) == Find(graph[i][j])) return true; // 사이클 발생 O
            Union(i, graph[i][j]);
        }
    }
    return false; // 사이클 발생 X
}
```

### 순환하지 않는 경로 선택

앞선 [#cycle](union-find.md#cycle "mention")를 응용하여 무방향 그래프에서 순환하지 않는 경로가 존재하는 지
