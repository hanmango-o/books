---
description: C/C++의 수학 수식을 정의한 math.h에 대해 알아봅니다.
---

# math.h

## math.h 란?

***

math.h 란 C/C++에서 자주 사용하는 수학 수식을 미리 정의한 표준 라이브러리입니다.



## math.h 사용

***

### 주요 기능

math.h 에서 주로 사용하는 기능은 아래와 같습니다.

#### 1. 소수점 계산 및 제어(올림, 버림, 반올림)

특정 소수점을 정수로 올림, 버림, 반올림하고 싶다면 아래의 함수를 사용합니다.

```cpp
ceil(24.13); // 가장 가까운 정수로 올림, 25.0
floor(24.13); // 가장 가까운 정수로 내림, 24.0
round(24.13); // 가장 가까운 정수로 반올림(소수점 첫째자리에서 반올림), 24.0
```

여기서 주의할 점은 반환 타입이 정수가 아닌 double 이라는 것 입니다.

또한 특정 자리 수에서 연산을 수행하는 것이 아닌 무조건 소수점 첫째 자리에서 수행하게 됩니다.

{% hint style="info" %}
만약 특정 n 에서 소수점 연산이 필요할 경우 `10 ^ (n - 1)` 을 기존 소수에 곱하여 특정 n 번째가 소수점 첫째 자리가 되도록 해야 합니다.

계산 이후 다시 원래 숫자로 돌리기 위해서 `10 ^ -(n - 1)` 연산이 필요함에 유의해야 합니다.
{% endhint %}

#### 2. 지수 함수(제곱 또는 제곱근) 계산

지수 함수, 즉 제곱과 제곱근은 아래와 같이 계산할 수 있습니다.

```cpp
pow(10, 3) // 제곱, 10 ^ 3
sqrt(16) // 루트, 루트 16 -> 4
```

#### 3. 로그 함수 계산

로그함수 중 상용로그와 자연로그는 아래와 같이 나타낼 수 있습니다.

```cpp
log(10); // 자연로그, ln(10)
log10(10); // 상용로그, 1
```

#### 4. 절대값 계산

특정 값의 절대값을 확인하는 방법은 아래와 같습니다.

```cpp
abs(-10) // 절대값, 10
abs(-10.2) // 10.2
```

### 주의할 점

반드시 math.h 가 상단에 아래와 같이 선언되어야 합니다.

```cpp
#include <math.h>
```

또한, 두 값 중 최대/최소를 찾는 min / max 함수는 math.h 가 아닌 algorithm에 정의되어 있음에 유의해야 합니다.



## Summary

***

* 주요 기능은 아래와 같습니다.

<table><thead><tr><th width="153">Function</th><th width="151">Used</th><th>Result</th></tr></thead><tbody><tr><td>올림</td><td>ceil(value)</td><td>value의 소수점 첫째 자리 올림</td></tr><tr><td>내림</td><td>floor(value)</td><td>value의 소수점 첫째 자리 내림</td></tr><tr><td>반올림</td><td>round(value)</td><td>value의 소수점 첫째 자리 반올림</td></tr><tr><td>제곱</td><td>pow(a, b)</td><td><span class="math">a^b</span></td></tr><tr><td>제곱근(루트)</td><td>sqrt(value)</td><td><span class="math">\sqrt{value}</span></td></tr><tr><td>로그</td><td>log(value)  log10(value)</td><td><span class="math">\log_e value</span> <span class="math">\log value</span></td></tr><tr><td>절대값</td><td>abs(value)</td><td><span class="math">|value|</span></td></tr></tbody></table>

