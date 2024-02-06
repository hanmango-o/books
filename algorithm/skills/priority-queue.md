---
description: 우선순위 큐에 대해 알아보고, 이를 활용하는 방법에 대해 살펴봅니다.
---

# Priority Queue

## 우선순위 큐란?

***

우선순위 큐란 우선순위가 높은 데이터가 먼저 나가는 형태의 자료구조입니다.

기존의 큐와는 다르게 들어온 순서대로 나가는 것(FIFO)이 아니라 우선순위 판단 기준에 따라 현재 큐에서 가장 우선 순위가 높은 요소가 반환되게 됩니다.



### 언제 사용할까?

우선 순위 큐는 문제 조건에 따른 우선 순위를 기준으로 가장 우선 순위가 높은 값을 지속적으로 확인해야 할 때 사용합니다. 예를 들어 배열에서 가장 큰/작은 값을 찾는 경우 사용할 수 있습니다.

또한, 우선 순위 큐를 거친 데이터는 정렬된 효과를 가집니다. 우선 순위에 따라 값이 반환되기 때문에 이를 활용하여 정렬의 효과를 나타낼 수 있습니다.

{% hint style="info" %}
단, 우선순위 규칙과 정렬 순서는 반대가 됩니다.

예를 들어 우선순위가 높은 값이라면 우선 순위 큐를 통해 순차적으로 호출된 값은 내림차순 정렬된 형태가 되게 됩니다.
{% endhint %}



### 우선 순위 큐 구현(with STL)

우선 순위 큐는 C++의 queue 라이브러리에 정의되어 있는 priority\_queue 를 이용하여 쉽게 구현할 수 있습니다.

```cpp
#include <queue>

priority_queue<type> pq;
```

또한, 우선 순위 큐에서 사용할 수 있는 기능들은 아래와 같습니다.

```cpp
priority_queue<type> pq;

pq.push(value) // 삽입, Queue와 동일
pq.size() // 사이즈 반환, Queue와 동일
pq.empty() // 비어있는 지 확인, Queue와 동일

pq.top() // 우선순위가 높은 요소 반환, Queue의 front와 유사
pq.pop() // 우선순위가 높은 요소 삭제, Queue의 pop과 유사
```

여기서 주의해야 할 점은 top과 pop 입니다. Queue에서와는 다르게 가장 우선순위가 높은 요소를 기준으로 동작하게 되며, 요소 반환은 front()가 아닌 top() 을 사용하는 것에 유의해야 합니다.



## 우선 순위 큐 활용

***

우선 순위 큐는 요소의 자료형에 따라 우선 순위를 설정할 수 있습니다.

### 기본 자료형 사용

만약 우선 순위 큐에 기본 자료형을 사용할 경우 operator 에 의한 대소 비교를 기준으로 가장 값이 높은 요소가 최고 우선 순위를 가지게 됩니다.

예를 들어 Int 자료형을 사용할 경우 아래 예시와 같습니다.

```cpp
priority_queue<int> pq;
pq.push(1); pq.push(5); pq.push(10);

pq.top(); pq.pop(); // 10 반환
pq.top(); pq.pop(); // 5 반환
pq.top(); pq.pop(); // 1 반환
```

이때, 가장 낮은 값을 기준으로 우선 순위를 설정하고 싶다면, 2가지 방법이 존재합니다.

#### 1. 음수 활용

기본적으로 가장 큰 값을 우선 순위로 하는 우선 순위 큐의 성질을 이용하여 모든 요소 값에 음수(-) 를 붙여 우선 순위가 역으로 출력되도록 하는 방법입니다.

```cpp
priority_queue<int> pq;
pq.push(-1); pq.push(-5); pq.push(-10);

pq.top(); pq.pop(); // -1 반환
pq.top(); pq.pop(); // -5 반환
pq.top(); pq.pop(); // -10 반환
```

이때, 사용한 음수는 다시 복구하는 과정이 필요합니다.

```cpp
cout << pq.top() * -1;
```

#### 2. greater 활용

sort 와 마찬가지로 greater 조건을 활용하여 우선 순위 큐의 기본 설정을 낮은 값으로 하도록 할 수 있습니다.

```cpp
priority_queue<int, vector<int>, greater<int>> pq;
pq.push(1); pq.push(5); pq.push(10);

pq.top(); pq.pop(); // 1 반환
pq.top(); pq.pop(); // 5 반환
pq.top(); pq.pop(); // 10 반환
```

### pair, tuple 사용

단일 요소에 대한 우선 순위는 앞선 방법을 사용하면 되지만, 여러 조건에 따른 우선 순위는 C++에서 기본적으로 제공하는 pair, tuple을 활용하여 해결할 수 있습니다.

우선 순위 큐에서 pair, tuple을 사용하게 된다면 해당 묶음에서 가장 빠른 요소 순으로 우선 순위가 정해지게 됩니다.

```cpp
priority_queue<pair<int, int>> pq;
pq.push(make_pair(100, 10));
pq.push(make_pair(10, 1));
pq.push(make_pair(10, 5));

pq.top(); pq.pop(); // (100, 10) 첫번째 요소가 가장 큰 pair 반환
pq.top(); pq.pop(); // (10, 5) 첫번째 요소가 동일하다면 두번 째 요소가 제일 큰 pair 반환
pq.top(); pq.pop(); // (10, 1)

priority_queue<tuple<int, int, int>> pq;'
pq.push(make_tuple(1, 2, 3));
pq.push(make_tuple(1, 4, 10));
pq.push(make_tuple(1, 4, 7));

// pair 와 동일 규칙
pq.top(); pq.pop(); // (1, 4, 10)
pq.top(); pq.pop(); // (1, 4, 7)
pq.top(); pq.pop(); // (1, 2, 3)
```

pair와 tuple 역시 greater 사용이 가능합니다.

```cpp
priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
```

### Custom 구조체 사용

더욱 다양한 조건에 따라 우선순위를 정하고 싶다면, Custom 구조체를 사용하여 operator를 오버로딩하는 방식으로 우선순위 규칙을 할당할 수 있습니다.

```cpp
struct compare {
    bool operator() (type left, type right) {
        // Some priority code
    }
}

priority_queue<type, vector<type>, compare> pq; // 커스텀 우선순위 규칙 적용
```



## Summary

***

* 우선순위 큐는 우선 순위 규칙에 따라 가장 높은 우선 순위를 가지는 요소를 반환하는 자료구조입니다.
* 우선순위 큐는 지속적으로 우선 순위가 높은 값을 찾을 때 유용합니다.
  * 우선 순위 큐를 거친 데이터는 정렬된 효과를 가집니다.
* Queue 와 동일한 기능을 사용하되, front() 가 top() 으로 대체됩니다.
* 음수 값, greater, pair/tuple, custom struct 를 통해 우선 순위 제어가 가능합니다.
