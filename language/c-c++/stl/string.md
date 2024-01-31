---
description: 문자열을 다루는 방법에 대해 알아보고 함께 사용 가능한 라이브러리에 대해 알아봅니다.
---

# string

## 문자열 다루기

C++에서 문자열은 크게 2가지 방법을 통해 다룰 수 있습니다.

### char 배열 활용하기

기본적인 방법으로는 char 배열을 활용하는 방법이 있습니다.

문자열의 각 문자를 char 배열에 담아 해당 char 배열을 하나의 문자열처럼 활용하는 방법입니다.

```cpp
char str[10];
str = "hello";
```

위 방법의 경우 정적인 배열을 활용하기 때문에 문자열의 길이와 배열의 크기에 대한 고민이 필요합니다.

또한, 문자열을 출력하기 위해서는 직접 반복문을 돌려 문자 단위로 접근해야 합니다.

#### 문자열의 길이 확인하기

위 경우, 배열의 5번 인덱스에 '\0' 문자가 입력되게 됩니다.\
즉, 배열을 통한 문자열 입력 시 문자열의 맨 끝 문자에 '\0' 가 추가되게 됩니다.

이를 활용하여 문자열의 길이를 아래와 같이 구할 수 있습니다.

```cpp
int length = 0;
for(int i = 0; str[i] != '\0'; i++) {
    length++;
}
cout << length; // 5
```

#### 주의할 점

배열을 활용할 경우 문자열의 길이 보다 1 큰 배열을 사용해야 합니다.

이는 문자열의 마지막 문자 이후에 '\0' 가 추가되기 때문에 이를 위해 1 큰 문자 배열을 사용하는 것 입니다.

{% hint style="danger" %}
만약 문자열 길이보다 작거나 같은 크기의 배열을 사용할 경우 예상했던 값이 나오지 않거나, 런타임 에러가 발생할 수 있습니다.
{% endhint %}

### string 헤더 파일 활용하기

앞서 살펴본 방법 외에도  \<string> 헤더 파일을 통해 보다 쉽게 다룰 수 있습니다.

이를 위해 #include \<string> 헤더 파일을 선언해야 합니다.

```cpp
#include <string>

string str = "hello";
```

#### 문자열 길이 확인하기

위 방법을 사용할 경우 문자열의 길이는 아래와 같이 쉽게 확인할 수 있습니다.

```cpp
str.length(); // 5
```

또한, 배열과 마찬가지로 '\0' 를 활용한 반복문으로 길이를 알 수도 있습니다.



## string 라이브러리 기능

string 라이브러리를 사용하여 문자열을 다룰 때 활용할 수 있는 기능들은 아래와 같습니다.

### 특정 문자 접근

특정 문자에 접근하는 것은 기존 배열에 접근하는 방법과 같이 인덱스를 통해 접근할 수 있습니다.

```cpp
string str = "hello";
cout << str[0]; // h
```

이때, 인덱스로 접근한 결과는 char 타입의 문자라는 것에 유의해야 합니다.

### 문자열 합치기

문자열끼리는 + 연산이 가능하기 때문에 문자열을 합치기 위해서는 + 연산을 통해 진행할 수 있습니다.

```cpp
string str1 = "hello";
string str2 = " world";
cout << str1 + str2; // hello world

string str3 = "";
str3 = str1 + str2;
cout << str3; // hello world
```

{% hint style="danger" %}
단 문자열 리터럴끼리는 + 연산이 불가능합니다. 반드시 둘 중 하나는 string 타입이여야 합니다.
{% endhint %}

또한, append 함수를 활용하여 문자열을 합칠 수 있습니다.

```cpp
str1.append(str2); // hello world;
```

### 부분 문자열 추출하기

문자열에서 특정 부분의 문자열만 추출하여 새로운 문자열을 만들려면 substr 함수를 활용할 수 있습니다.

```cpp
string str = "good mango";
string answer = str.substr(0, 4);
cout << answer; // good
```

substr 함수의 구조는 아래와 같습니다.

```cpp
// start index : 시작 지점
// length : 추출할 문자열의 길이
substr(start index, length);
```

### 특정 문자열 포함 여부 판단하기

문자열에서 특정 문자열(또는 문자)가 포함되어 있는지 확인하기 위해서 find 함수를 사용할 수 있습니다.

```cpp
str.find("good"); // 해당 문자열의 시작 인덱스 반환(0)
```

특정 문자열이 존재한다면 해당 문자열 중 가장 빠른 인덱스를 가진 부분 문자열의 인덱스를 반환하게 됩니다.

{% hint style="warning" %}
bool 값(true, false) 가 아닌 인덱스 size\_t(signed int)를 반환함에 유의해야 합니다.
{% endhint %}

만약 문자열을 찾는 것을 실패했다면, string::npos 를 반환하게 됩니다.

```cpp
cout < (str.find("apple") == string::npos); // true
```

또한, find 함수는 시작 지점을 지정할 수 있습니다.

```cpp
str.find(value); // value 값이 존재하는 지 판단

// first index 부터 value 값이 존재하는지 판단
str.find(value, first_index);

str.find("g", 1) // string::npos
```

{% hint style="warning" %}
string의 find 함수는 algorithm의 find 함수와 다름에 유의해야 합니다.
{% endhint %}

find 함수를 활용해서 특정 문자열이 몇 번 존재하는지 확인할 수 있습니다.

<pre class="language-cpp"><code class="lang-cpp">int cnt = 0;
string value = "target substr"; // 찾고자 하는 문자열

<strong>size_t index = str.find(value); // [1] 시작 지점 찾기
</strong>if(index != string::npos) cnt++; // [2] 존재 여부 카운팅하기

while(index != string::npos) { // [3] 반복문을 통한 존재 여부 확인
    index = str.find(value, index + 1); // [4] 탐색 지점 찾기
    if(index != string::npos) cnt++; // [5] 존재 여부 카운팅하기
}
</code></pre>

#### 1. 시작 지점 찾기

문자열에서 찾고자 하는 부분 문자열이 맨 처음 나오는 인덱스를 찾습니다. 해당 문자열이 존재할 경우 index 변수에 해당 문자열의 첫번째 위치를 저장합니다.

#### 2. 존재 여부 카운팅하기

처음 시작 지점에서 문자열을 찾았다면, 즉 string::npos 가 아니라면 개수를 +1 카운팅합니다.

#### 3. 반복문을 통한 탐색하기

이후 문자열을 반복문을 통해 탐색하며 해당 문자열이 존재하는 지 파악합니다. 이때 index 값을 그대로 사용할 경우 중복으로 값이 카운팅되므로 + 1 을 해줍니다.

#### 4-5. 존재 여부 카운팅하기

1-2 와 동일합니다.

{% hint style="info" %}
algorithm 라이브러리의 count 함수를 이용하여 문자열에서 부분 문자의 개수를 확인할 수 도 있습니다. 하지만 count 함수는 부분 문자열이 아닌 문자(char)만 탐색이 가능하기 때문에 이 점에 유의해야 합니다.
{% endhint %}

### 문자열 수정하기

문자열의 특정 문자를 수정하기 위해서는 해당 문자에 직접 접근 후 수정해 주면 됩니다.

```cpp
string str = "dango";
str[0] = 'm';
cout << str; // mango
```

만약 문자열의 경우 직접 문자를 반복하면 수정할 수도 있지만 replace를 활용하여 쉽게 변경이 가능합니다.

```cpp
// start index : 교체할 문자열이 들어갈 장소, 해당 인덱스부터 삽입됨
// target size : 교체할 문자열의 크기, 해당 크기만큼 사라짐
// replace str : 교체할 문자열
str.replace(start_index, target_size, replace_str);

string str = "I LOVE DDANGO";
str.replace(7, 6, "MANGO");
cout << str; // I LOVE MANGO
```

