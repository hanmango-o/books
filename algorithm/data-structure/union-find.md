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

Union Find란 자료구조로, 주어진 원소들로 만들 수 있는 집합쌍 중 서로소 집합인 쌍을 만들 수 있는 자료구조합니다.

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

