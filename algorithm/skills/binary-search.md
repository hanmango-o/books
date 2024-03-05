---
description: 이진탐색 기법에 대해 알아보고, 이를 활용하는 방법을 살펴봅니다.
---

# Binary Search

## Binary Search 란?

***

이진 탐색이란 중앙에서 시작해서 찾고자 하는 값과 중앙값을 비교하며 찾는 범위를 절반씩 줄이는 탐색 기법입니다.

이진 탐색은 **반드시 정렬된 상태로 진행**되어야 하기에, [sort.md](sort.md "mention")가 선행되어야 합니다.

{% hint style="warning" %}
**반드시 정렬**을 진행해야 합니다.

정렬이 선행되지 않으면 이진 탐색이 불가능합니다.
{% endhint %}

### 이진 탐색 구현

기본적인 이진 탐색의 구조는 아래와 같습니다.

```cpp
// target : 찾고자 하는 값, arr : 대상 배열
int BS(int left, int right, int target) {
    int mid = 0; // [1] 현재 중앙 인덱스
    
    while(left <= right) { // [2] 남은 영역의 크기가 1이하일 경우 종료
        mid = (left + right) / 2; // [1] 중앙 인덱스 갱신
        if(arr[mid] == target) return mid; // 대상 인덱스 반환
        else if(arr[mid] < target) left = mid + 1; // 범위 제어
        else right = mid - 1;
    }
    return -1; // 못 찾으면 -1 return
}
```

#### \[1] 중앙 인덱스 설정

현재 범위(left \~ right)의 이진 탐색 대상이 되는 중앙 인덱스 mid 를 설정합니다.

#### \[2] 범위 제어

중앙 인덱스 mid 는 현재 범위인 left, right 인덱스에 의해 `mid = (left + right) / 2` 로 갱신됩니다.

이에 left, right의 값을 갱신해야 하며 찾고자 하는 대상 값과 비교하여,

* 대상 값보다 현재 중앙 값이 더 작으면, `left = mid + 1`
* 대상 값보다 현재 중앙 값이 더 크면, `right = mid - 1`

로 갱신되게 됩니다.

{% hint style="info" %}
left, right 갱신은 가장 많은 실수가 나오는 부분입니다.

left와 right를 mid 인덱스로 이동할 경우 target을 안 만나는 인덱스(left or right)를 갱신한다고 생각하면 쉽습니다.
{% endhint %}

위 이진 탐색 코드는 찾고자 하는 값이 있을 경우 해당 인덱스 반환하고, 없을 경우 -1을 반환하게 됩니다.



### STL 사용

이진 탐색은 이미 C++ 표준 라이브러리에 정의되어 있습니다.

해당 STL은 아래와 같이 사용합니다.

```cpp
#include <algorithm>

binary_search(first, last, target);
```

파라미터로 first, last 는 iterator로서, 만약 배열을 대상으로 이진 탐색을 해야 한다면 아래와 같이 사용해야 합니다.

```cpp
binary_search(arr, arr + N, target);
```

#### STL 사용 시 주의할 점

이진 탐색 STL은 편리하지만 제한 사항이 있습니다.

STL로 주어진 binary\_search 함수는 **bool 타입을 반환**하게 되며, 해당 값이 있을 경우 true, 없을 경우 false를 반환하게 됩니다.

즉, 찾고자 하는 값의 인덱스를 알아야 하는 경우 [#undefined](binary-search.md#undefined "mention")을 통해 직접 구현해야 합니다.



### Lower Bound

만약 찾고자 하는 값이 현재 2개 이상 존재한다면, 동일한 값의 인덱스가 여러 개 존재하기 때문에 단순히 이진 탐색만으로 어떤 인덱스가 나오게 될지 확신할 수 없습니다.

이럴 때 사용하는 것이 Lower Bound입니다.

LowerBound는 **찾고자 하는 값보다 같거나 큰 값이 처음 나오는 위치**를 반환합니다.

```cpp
int LowerBound(int left, int right, int target) {
    int mid = 0;
    int ret = N; // [1] target보다 크거나 같은 첫 번째 요소의 인덱스
    while(left <= right) {
        mid = (left + right) / 2;
        if(arr[mid] < target) { // [2] 탐색 범위 및 인덱스 갱신
            left = mid + 1;
        } else {
            right = mid - 1;
            ret = min(ret, mid);
        }
    }
    return ret;
}
```

#### \[1] 초기 인덱스 설정

찾고자 하는 값보다 크거나 같은 첫 번째 요소의 인덱스를 저장할 변수를 생성하고, 현재 범위 상 나올 수 없는 큰 수 중 가장 작은 값을 할당합니다.

{% hint style="info" %}
예를 들어 현재 배열의 범위가 0 \~ N - 1 이라면 ret은 N 으로 초기화합니다.
{% endhint %}

#### \[2] 이진 탐색 진행 및 변수 갱신

기본적인 이진 탐색을 진행하면서, 앞선 [#id-1-1](binary-search.md#id-1-1 "mention")에서 생성한 변수 ret을 갱신합니다.

갱신하는 조건은 현재 중앙 인덱스에 해당하는 요소 값이 찾고자 하는 값보다 크거나 같을 경우 실행합니다.



### Upper Bound

[#lower-bound](binary-search.md#lower-bound "mention")의 경우 찾고자 하는 값보다 크거나 같은, 즉 찾고자 하는 값 이상인 요소 중 첫 번째 요소의 인덱스를 반환합니다.

반면 Upper Bound는 찾고자 하는 값보다 초과한 값, 즉 **찾고자 하는 값보다 큰 첫 번째 요소를 반환**합니다.

```cpp
int UpperBound(int left, int right, int target) {
    int mid = 0, ret = N;
    while(left <= right) {
        mid = (left + right) / 2;
        if(arr[mid] <= target) left = mid + 1; // [1] 인덱스 갱신
        else {
            right = mid - 1;
            ret = min(ret, mid);
        }
    }
    return ret;
}
```

#### \[1] 인덱스 갱신

앞선 [#lower-bound](binary-search.md#lower-bound "mention")와 구현 코드는 동일하며, [#id-2-1](binary-search.md#id-2-1 "mention")부분에서 = 만 빼고 진행되게 됩니다.



### Upper/Lower Bound 특징

Lower Bound의 결과값이 찾고자 하는 값이 아니라면, 현재 배열에는 해당 값이 존재하지 않는다고 할 수 있습니다. Upper Bound의 경우 항상 찾고자 하는 값보다 큰 첫 번째 값을 반환하게 됩니다.

즉, **Lower/Upper Bound의 결과값이 동일하다면 현재 배열에는 찾고자 하는 값이 없다**는 것을 의미합니다.

또한, `Upper Bound의 결과 값 - Lower Bound의 결과 값` 은 **현재 배열 내 찾고자 하는 값의 개수**가 되게 됩니다.



### Custom Bound

앞서 살펴본 Upper/Lower Bound와는 다르게 Custom Bound는 찾고자 하는 값보다 작거나 같은 첫 번째 요소를 반환합니다.

```cpp
int CustomBound(int left, int right, int target) {
    int mid = 0;
    int ret = -1; // [1] 초기 값 설정
    while(left <= right) {
        mid = (left + right) / 2;
        if(arr[mid] <= target) { // [2] 변수 갱신
            left = mid + 1;
            ret = max(ret, mid);
        } else {
            right = mid - 1;
        }
    }
    return ret;
}
```

#### \[1] 초기 값 설정

[#upper-bound](binary-search.md#upper-bound "mention"), [#lower-bound](binary-search.md#lower-bound "mention")와는 다르게 ret의 초기 값에 -1을 할당합니다.

Custom Bound는 찾고자 하는 값보다 작거나 같은 첫 번째 요소를 반환해야 합니다.

즉, **찾고자 하는 값보다 작은 값들 중에서 최대인 값**을 반환해야 하기에 초기 값은 현재 배열에서 **나올 수 없는 작은 값인 -1을 할당**합니다.

#### \[2] 변수 갱신

앞선 두 Bound와는 다르게 left를 이동시켜야 최대값을 찾을 수 있습니다.

이에 `arr[mid] <= target` 일 경우 ret 변수를 갱신하고, 갱신 시 최대인 값을 할당합니다.



