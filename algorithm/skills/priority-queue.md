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



## 우선 순위 규칙 설정

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



## 우선 순위 큐 활용

***

우선 순위 큐를 활용하여 문제를 해결하는 유형 중 대표적인 예시는 아래와 같습니다.

### 변화하는 환경에서 조건에 맞는 값 찾기

우선 순위 큐는 우선 순위 규칙에 따라 값을 반환시킬 수 있습니다. 만약 제공되는 환경이 지속적으로 변화가 발생한다면(ex. 값이 계속 들어오는 상황) 우선 순위 큐를 활용하여 조건에 맞는 값을 찾을 수 있습니다.

모든 환경이 설정되고 난 후, 조건에 맞는 값을 찾는 상황이라면 우선 순위 큐를 사용해도 되고, 그 외에 알고리즘을 적용해도 괜찮을 것 입니다.

하지만 환경이 변화되는 상황에서 지속적으로 조건에 맞는 값을 찾아야하는 상황이라면 우선 순위 큐를 사용하여 굉장히 효율적으로 문제를 해결할 수 있습니다.

앞선 [#undefined-5](priority-queue.md#undefined-5 "mention")를 활용하여 해결할 수 있는 대표적인 문제 중 하나인 정렬되지 않은 리스트에서 최대/최소/중앙값 찾기가 가능합니다.

정렬되지 않은 경우, 모든 환경(데이터)이 설정된 후 최대/최소/중앙값을 찾는 상황에서도 우선 순위 큐가 효율적입니다.

특히 환경 변화가 발생하는 상황에서 최대/최소/중앙값을 지속적으로 찾아야 하는 상황에서는 우선 순위 큐를 활용하는 것이 가장 좋은 방법입니다.

```cpp
// Priority Queue 없이 정렬만으로 최대값 찾기 예시
vector<int> vect;

for(int i = 0; i < N; i++) {
    int x;
    cin >> x;
    vect.push_back(x);
    
    sort(vect.begin(), vect.end()); // 환경 변화에 맞춰 매번 정렬 필요
    cout << vect[vect.size() - 1];
}
```

위 예시는 우선 순위 큐를 사용하지 않고 새로운 값이 추가될 때마다 현재 값들 중 최대값을 찾는 방법을 나타낸 예시입니다.

최대값을 찾기 위해 정렬을 사용하였지만, 위 방식을 사용하게 된다면 매번 정렬을 수행하여 시간 복잡도에서 문제가 발생할 수 있습니다.

```cpp
priority_queue<int> pq;

for(int i = 0; i < N; i++) {
    int x;
    cin >> x;
    pq.push(x); // 우선 순위 큐에 등록
    
    cout << pq.top(); // 가장 큰 값(높은 우선순위)을 반환
}
```

반면 우선 순위 큐를 사용한다면 정렬되어 있지 않은 환경에서도 위와 같이 최대/최소값을 찾을 수 있습니다. 이와 같은 방법은 정렬을 수행하지 않고 힙 자료구조인 우선 순위 큐만을 사용하기에 O(nlogn)을 보장합니다.

#### 중앙값 찾기

우선 순위 큐를 이용한 중앙값 찾기는 기존 방식과는 다른 시각이 필요합니다.

만약 우선 순위 큐를 사용하지 않는 상황에서 데이터가 들어올 때 마다 중앙값을 찾아야 한다면 아래와 같이 찾을 수 있습니다.

```cpp
vector<int> vect;

for(int i = 0; i < N; i++) {
    int num1, num2, num3;
    cin >> num1, num2, num3;
    
    vect.push_back(num1);
    vect.push_back(num2);
    vect.push_back(num3);
    
    sort(vect.begin(), vect.end()); // 정렬 필요
    cout << vect[vect.size() / 2 + 1]; // 중앙 인덱스 접근
}
```

위 방식은 매번 정렬이 필요하므로 공간/시간복잡도에서 문제가 발생할 수 있습니다. 위 코드는 O(nlogn)의 시간 복잡도를 가지지만 매번 데이터를 갱신해줘야 하기 때문에 공간복잡도가 커지고, 입력 N 이 커질 경우 정렬에 대한 비용이 커지기 때문에 시간 복잡도를 보장하지 않습니다.

반면 우선 순위 큐를 사용한다면 아래와 같이 작성할 수 있습니다.

```cpp
priority_queue<int, vector<int>, greater<int>> max_pq;
priority_queue<int> min_pq;
int median;

for(int i = 0; i < N * 3; i++) {
    int num;
    cin >> num;
    
    if(i == 0) { // 첫 번째 값은 median으로 바로 할당
        median = num;
        continue;
    }
    
    if(median > num) { //  현재 중앙값보다 큰 값이라면,
        min_pq.push(num); // min_pq에 push
    } else {
        max_pq.push(num); // 아니라면, max_pq에 push
    }
    
    if(min_pq.size() > max_pq.size()) { // min_pq의 최대값이 중앙값
        int temp = min_pq.top();
        min_pq.pop();
        max_pq.push(median);
        median = temp;
    } else if(min_pq.size() < max_pq.size()) { // max_pq의 최솟값이 중앙값
        int temp = max_pq.top();
        max_pq.pop();
        min_pq.push(median);
        median = temp;
    }
    cout << medain;
}
```

#### \[1] median 할당

위 로직의 핵심 개념은 기준이 되는 median 값을 갱신하며, 해당 값보다 큰 값이 들어오면 max\_pq로, 작은 값이 들어오면 min\_pq로 push 하여 max\_pq와 min\_pq의 사이즈 균형을 맞춰주는 것입니다.

이때, min\_pq의 우선 순위는 최대값으로 하고 max\_pq의 우선 순위는 최소값으로 해야합니다.

#### \[2] 데이터 갱신

min\_pq가 max\_pq 보다 크다면, 즉 median을 기준으로 좌측 데이터가 우측 데이터보다 많다면 min\_pq의 최대값을 median으로 갱신합니다. 반대로 max\_pq가 더 크다면, max\_pq의 최소값을 median으로 갱신합니다.

### 순차적 조합 만들기

N 개의 숫자들만 사용하여 각각의 연산을 통해 만들 수 있는 숫자들을 구할 때(조합)에도 우선 순위 큐가 효율적일 수 있습니다.

```cpp
// 숫자 K 구하기(K 는 A, B 로 최소 공배수를 만들 수 있는 숫자)
priority_queue<int, vector<int>, greater<int>> pq;
int vect[M];

pq.push(1); // 최소 K는 1이라 가정

for(int i = 0; i < M; i++) { // M 은 구하고자 하는 K 개수
    int K = pq.top();
    vect[i] = K; // 크기 순서대로 vect에 저장
    pq.push(K * A);
    pq.push(K * B);
}

```

위 문제에서는 나올 수 있는 모든 조합의 K 가 크기 순서대로 우선 순위 큐에 들어가게 됩니다. 이와 같이 순서를 가지는 K 조합을 구할 수 있습니다.

{% hint style="info" %}
우선 순위 큐의 특징으로 인해 나올 수 있는 K 가 작은 순서대로 vect 배열에 할당되는 것이 보장됩니다.
{% endhint %}

## Summary

***

* 우선순위 큐는 우선 순위 규칙에 따라 가장 높은 우선 순위를 가지는 요소를 반환하는 자료구조입니다.
* 우선순위 큐는 지속적으로 우선 순위가 높은 값을 찾을 때 유용합니다.
  * 우선 순위 큐를 거친 데이터는 정렬된 효과를 가집니다.
* Queue 와 동일한 기능을 사용하되, front() 가 top() 으로 대체됩니다.
* 음수 값, greater, pair/tuple, custom struct 를 통해 우선 순위 제어가 가능합니다.
