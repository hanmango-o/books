---
description: C/C++에 사용되는 표준 라이브러리 헤더 중 하나인 limits.h 에 대해 알아봅니다.
---

# limits.h

## limits.h 란?

***

limits.h 헤더 파일은 다양한 데이터 타입의 최소/최대값을 정의하고 있는 표준 라이브러리입니다.

해당 헤더 파일에는 데이터 타입의 최소값과 최대값을 정의하고 있으며, 데이터 타입의 범위와 특성을 확인하고 이를 활용할 때 사용할 수 있습니다.



## limits.h 사용

***

### 주요 기능

limits.h 가 가지고 있는 주요 기능 중 데이터 타입에 따른 최대/최소값은 아래와 같습니다.

<table><thead><tr><th width="210">Type</th><th>Maximum</th><th>Minimum</th></tr></thead><tbody><tr><td>정수(int, long)</td><td>INT_MAX, LONG_MAX</td><td>INT_MIN, LONG_MIN</td></tr></tbody></table>



### 주의할 점

limits.h를 사용할 경우 반드시 상단 헤더가 선언되어야 합니다.

```cpp
#include <limits.h>
```

또한, 해당 값은 해당 타입이 가질 수 있는 MAX 또는 MIN 값이기 때문에 해당 값을 이용하는 연산에서 데이터 타입의 범위를 넘지 않도록 주의해야 합니다.



## Summary

***

* 정수 타입의 최대/최소는 INT\_MIN, INT\_MAX 입니다.
* `#include <limits.h>` 를 선언해야 합니다.
