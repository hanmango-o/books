---
description: 투포인터와 슬라이딩 윈도우에 대해 알아보고, 이를 활용하는 방법을 살펴봅니다.
---

# Two Pointer

## Sliding Window 란?

***

투포인터 알고리즘에 들어가기 앞서, 슬라이딩 윈도우 알고리즘에 대해 살펴봐야 합니다.

슬라이딩 윈도우 알고리즘이란, 고정된 윈도우(영역)를 옮기며 윈도우 내에 있는 데이터를 활용하여 문제를 해결하는 기법을 말합니다.

<img src="../../.gitbook/assets/file.excalidraw (2).svg" alt="갱신 시 교집합 영역" class="gitbook-drawing">

이때, 윈도우는 **고정된 크기**를 가지게 되며, 윈도우 이동 시 이전 단계 윈도우와 교집합 영역에 있는 요소들은 공유하고 변동되는 양 끝 요소만 갱신하며 진행되게 됩니다.

### Sliding Window 구현

슬라이딩 윈도우의 경우 아래와 같이 구현할 수 있습니다.

```cpp
// N : 전체 배열 크기, W : 윈도우 크기
int max_sum = 0; // 연속된 W 크기의 영역의 요소들의 합 중 가장 큰 값
for(int i = 0; i < W; i++) { // [1] 초기 index = 0 ~ W - 1 까지의 합
    max_sum += arr[i];
}
int sum = max_sum; // 현재 영역 요소들의 합
for(int left = 0; left < N - W; left++) { // [2] 윈도우를 옮기며 진행
    sum += arr[left + W]; // 맨 뒤 값 갱신
    sum -= arr[left]; // 맨 앞 값 갱신
    max_sum = max(max_sum, sum); // 현재 영역의 합과 기존 최대 합 비교
}
cout << max_sum; // N 배열에서 최대 합
```

위 예시의 경우 연속된 W 크기의 영역에 요소들의 합이 최대가 되는 경우의 값을 구하는 문제입니다.

#### \[1] 시작 영역 초기화

슬라이딩 윈도우의 경우 반드시 초기 영역에 대한 할당이 선행되어 있어야 합니다.

이에 초기 인덱스 0부터 W - 1 까지의 합을 구하고 초기화합니다.

#### \[2] 윈도우 이동 후 갱신

이후, 윈도우를 이동하며 슬라이딩 윈도우를 진행하게 됩니다.

이때 현재 윈도우의 모든 값을 더하는 것이 아닌 이전 영역에서 교집합이 아닌 부분만을 갱신하며 진행합니다.

위 예시의 경우 1씩 이동하고 있으므로, 이전 영역에서 맨 앞의 값은 빼주고, 현재 영역에서 추가되는 맨 뒤 값은 더하며 갱신합니다.

{% hint style="warning" %}
위 예시에서 `max_sum`에 해당하는 index는 `sum > max_sum` 일 경우의 `left + 1` 이 되게 됩니다.

핵심은 `left + 1` 이 해당 인덱스의 시작 지점이라는 것 입니다.
{% endhint %}

## Two Pointer

***

투 포인터는 앞서 살펴본 [#sliding-window-1](two-pointer.md#sliding-window-1 "mention")과 유사하게 구현합니다.

하지만 **윈도우의 영역이 고정되지 않는다**는 점이 슬라이딩 윈도우와의 차이점입니다.

투 포인터의 경우 윈도우의 크기를 고정하지 않고 조건에 따라 유동적으로 윈도우 시작 지점과 끝 지점을 이동하며 진행하는 알고리즘입니다.

또한, 슬라이딩 윈도우와 마찬가지로 이전 단계의 윈도우와 교집합되는 영역은 갱신하지 않고 공유하며 진행합니다.

### Two Pointer 구조

기본적인 Two Pointer의 구조는 아래와 같습니다.

```cpp
for((1) Left pointer condition) {
    while((2) Right pointer condition && (3) Problem condition) {
        (4) Update right position in current window
        (5) Update right pointer
    }
    (6) Update result in current window
    (7) Update left position for next window
}
```

#### \[1] Left pointer 조건

현재 주어진 범위에 맞게 Left 포인터의 조건을 설정합니다.

예를 들어 배열이 0부터 N - 1 까지라면, `int i = 0; i < N; i++` 와 같이 처리합니다.

#### \[2] Right pointer 조건

현재 주어진 범위에 맞게 Right 포인터의 조건을 설정합니다.

예를 들어 배열이 0부터 N - 1 까지라면,  `int j= 0; j < N; j++` 와 같이 처리합니다.

#### \[3] 문제 해결 조건

해당 while 반복문에서는 문제 조건이 만족할 때까지 현재 Left를 기준으로 Right를 1씩 이동시키며 진행되어야 합니다. 이에 조건에 맞는 문제 해결 조건을 추가합니다.

#### \[4] 현재 window에서 Right pointer 영역 갱신

현재 설정된 window 에서 문제에 맞는 값을 갱신합니다. [#sliding-window-1](two-pointer.md#sliding-window-1 "mention")과 마찬가지로 이전 단계와의 교집합 부분을 제외한 원소들을 갱신합니다.

이때, 확장되는 오른쪽 포인터 영역을 지속적으로 갱신합니다. 예를 들어 구간 합을 구하고 싶다면 아래와 같이 처리합니다.

```cpp
sum += arr[right];
```

#### \[5] Right pointer 갱신

만약 아직 [#id-3](two-pointer.md#id-3 "mention")을 만족하지 않았다면, Right 포인터를 1씩 이동시킵니다.

#### \[6] 현재 Window에서 정답 갱신

[#id-6-window](two-pointer.md#id-6-window "mention")을 통해 갱신된 정답, 즉 [#id-1-left-pointer](two-pointer.md#id-1-left-pointer "mention")에 따른 현재 left 값과 [#id-5-right-pointer](two-pointer.md#id-5-right-pointer "mention")을 통해 갱신된 right 값까지의 window 영역에서 나올 수 있는 값을 갱신합니다.

문제 조건에 따라 갱신 가능 여부를 판단하고 갱신하게 됩니다.

#### \[7] Left pointer 갱신

다음 window 진행을 위해 현재 window에서 left pointer 값을 빼주게 됩니다. 구간 합 예시의 경우 아래와 같이 처리합니다.

```cpp
sum -= arr[left];
```

### Two Pointer 구현

앞선 [#two-pointer-1](two-pointer.md#two-pointer-1 "mention")에 맞게 구현된 Two Pointer 예시는 아래와 같습니다.

```cpp
int right = sum = result = 0;
for(int left = 0; left < N; left++) {
    while(right < N && sum < M) {
        sum += arr[right];
        right++;
    }
    result = max(result, j - i);
    sum -= arr[left];
}
```

위 예시의 경우, 특정 구간의 모든 요소의 합이 M 이상일 때 가장 긴 구간의 크기를 구하는 투 포인터 코드입니다.



## Two Pointer 활용

***

투 포인터 알고리즘의 핵심은 규칙을 찾는 것입니다.

앞서 구조화된 투 포인터 알고리즘을 살펴 보았지만, 문제에 따라 해당 구조가 아닌 다른 구조로 해결해야 할 수도 있습니다.

중요한 것은 어떤 조건에서 left, right 포인터르 움직이고, 문제 조건에 따라 정답을 갱신할 것인지 찾고 이에 맞춰 구조화해야 한다는 것입니다.

이에 해당 문제가 투 포인터를 사용해야 하는 지 아래의 기준에 맞춰 판단하고 문제를 해결해야 합니다.

* 해당 문제가 완전 탐색으로 풀릴 경우, 중복되는 요소를 살펴보고 진행하면서 이를 갱신할 수 있는 경우
* 구간의 앞과 뒤 만을 제어하며 문제가 풀릴 경우

아래는 대표적인 투 포인터 알고리즘의 활용 예시입니다.

### 조건에 따른 구간 계산하기

문제의 조건에 맞는 구간을 계산하고 해당 구간을 구할 때, 투 포인터 알고리즘이 활용될 수 있습니다.

대표적으로는 `구간의 구분 합에 따라 가장 긴(짧은) 구간 크기 구하기` 등이 있습니다.

### 중복 요소에 따른 구간 구하기

특정 구간(윈도우)를 설정했을 때 중복된 요소가 없는 구간을 구할 때도 투 포인터 알고리즘이 활용될 수 있습니다.

대표적으로 `중복된 요소없이 구간을 잡았을 때, 가장 긴 구간 구하기` 등이 있습니다.

### 부분 수열 확인하기

주어진 수열이 부분 수열인지 판단하는 문제에서도 투 포인터 알고리즘을 사용할 수 있습니다.

주어진 수열에서 부분 수열이 될 때까지 포인터를 이동하며 부분 수열을 판단합니다.
