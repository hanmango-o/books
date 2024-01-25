---
description: HTTP 에 대해 알아보고 특징과 메시지 구조, HTTP Method를 확인합니다.
coverY: 0
---

# HTTP

## HTTP 란?

***

HTTP(HyperText Transfer Protocol)은 웹에서 HyperText 링크를 통해 **네트워크 장치 간에 정보를 전송**할 수 있도록 설계된 프로토콜입니다.

Server-Client 구조에서 주로 사용되며, OSI 7 계층에서  응용 계층(Application Layer)에 포함된 프로토콜입니다.

### HTTP의 특징

HTTP 는 요청-응답(Request-Response) 프로토콜로 TCP/IP 위에서 동작하며, 메시지 교환 형태의 프로토콜입니다.

이러한 HTTP는 아래의 특징을 가집니다.

#### 1. Server-Client 구조

HTTP의 가장 큰 특징은 Server-Client 구조라는 것 입니다. 요청과 응답으로 구성되어 HTTP 메시지를 통해 통신하게 됩니다.

#### 2. 무상태(Stateless) 프로토콜

HTTP 는 Client의 상태를 기억하지 않는 무상태 프로토콜입니다.

여기서  Client의 상태란 이전까지 Client와 통신한 내용에 대해 기억하지 않는, 즉 **각각의 요청이 별개로 동작**되는 것을 의미합니다.

이러한 무상태 특징으로 인해 서버는 요청에 대해 독립적으로 동작할 수 있게 됩니다. 따라서 서버 확장성(Scale Out)에 용이합니다.

{% hint style="danger" %}
단, 요청을 기억하지 못하기 때문에 세션, 쿠키와 같은 별도 작업이 필요합니다. 또한, API 설계 시   무상태를 고려한 설계를 해야 합니다.
{% endhint %}

#### 3. 비연결성(Connectionless)

기본적으로 TCP/IP는 연결성을 가집니다. 즉, 별도의 요청이 있기 전까지는 Client와의 연결을 유지합니다.

HTTP 는 TCP/IP 위에서 동작하지만 이러한 연결성을 가지지는 않습니다. 따라서 HTTP 는 **통신할 때만 TCP/IP 연결을 유지**하고, 이후 **통신이 끝나면 연결을 종료**합니다.

이러한 비연결성으로 인해 빠르고 효율적인 통신이 가능하게 됩니다. 단, 연결 실패로 인한 재요청 또는 트래픽 증가로 인해 요청이 많아질 경우 문제가 발생할 수 있습니다.

### HTTP 요청 구조

HTTP 요청을 보낼 경우 해당 요청에는 아래의 내용이 포함됩니다.

```http
GET /example/page.html HTTP/1.1
Host: www.example.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/100.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Connection: keep-alive
```

#### 1. HTTP 버전 유형

해당 HTTP 버전 유형이 포함됩니다. 보통의 경우 HTTP/1.1 또는 HTTP/2 와 같은 형태로 표현됩니다.

> 위 예시에서  `HTTP/1.1` 이 HTTP 버전 유형에 해당합니다.

#### 2. URL

HTTP 요청을 보낼 URL 내용이 포함됩니다. URL에 따라 요청을 보낼 때 필요한 파라미터와 Method Type이 달라지므로 유의해야 합니다.

> 위 예시에서 `/example/page.html` 이 이에 해당합니다.

#### 3. HTTP Method

요청이 어떤 동작을 해야 하는지 알려주는 메소드가 포함됩니다.

> 위 예시에서 `GET` 에 해당합니다.

#### 4. HTTP 요청 Header

요청에 대한 메타데이터를 나타내는 Header 가 포함됩니다. 요청의 유저 에이전트(User-Agent), 인증 정보(Authorization), 컨텐츠 타입(Content-Type) 등이 헤더로 포함될 수 있습니다.

> 위 예시에서 `User-Agent: ~ Connection:` 까지가 이에 해당합니다.

#### 5. HTTP 본문 (Option)

이 외에도 별도로 HTTP 요청 시 필요에 따라 요청 데이터를 추가할 수 있는 영역입니다.

> 주로 PUT, POST에 사용됩니다.

### HTTP 응답 구조

HTTP  요청에 대한 응답 구조는 아래와 같습니다.

```http
HTTP/1.1 200 OK
Date: Tue, 25 Jan 2024 12:00:00 GMT
Server: Apache/2.4.41 (Unix)
Content-Type: application/json; charset=utf-8
Content-Length: 67

{
    "message": "안녕하세요, 이것은 JSON 형식의 예제 응답입니다.",
    "status": "success"
}
```

#### 1. HTTP 상태 코드

HTTP 응답의 첫 번째 줄은 항상 상태 라인으로 시작합니다. 해당 라인을 통해 HTTP 의 버전, 상태 코드 및 상태 메시지를 알 수 있습니다.

> 위 예시에서 `HTTP/1.1 200 OK` 가 이에 해당합니다.

#### 2. HTTP 응답 Header

상태 라인 이후에는 여러 Header 정보가 나오게 됩니다. 각 Header는 key 와 value로 구성됩니다.

* Content-Type : 응답의 본문 형식을 나타냅니다.
* Server : 서버의 정보를 나타냅니다.
* Date : 시간 정보를 나타냅니다.

> 위 예시에서 `Date: ~ Content-Length` 가 이에 해당합니다.

#### 3. HTTP Body

만약 응답 내용에 값이 포함되어야 한다면 body 에 이를 나타낼 수 있습니다.

> 위 예시에서는 json 형태로 포함되었습니다.



## HTTP Method

***

HTTP 통신 시 각 요청에 대한 특정 작업 수행을 보다 쉽게 표현하기 위해 HTTP Method가 존재합니다.

대표적인 HTTP Method는 아래와 같습니다.

<table><thead><tr><th width="162">HTTP Method</th><th>Description</th></tr></thead><tbody><tr><td><strong>GET</strong></td><td><strong>리소스를 서버로부터 요청</strong>합니다. 주로 정보를 가져올 때 사용되며, 요청 시 본문(body)에 데이터를 넣지 않습니다. 예를 들어, 웹 페이지를 불러오거나 이미지를 가져올 때 사용됩니다.</td></tr><tr><td><strong>POST</strong></td><td>서버로 <strong>데이터를 제출하고 리소스를 생성</strong>합니다. 주로 양식 데이터를 제출하거나 새로운 리소스를 생성할 때 사용됩니다. 본문에 데이터를 넣어 요청합니다.</td></tr><tr><td><strong>PUT</strong></td><td>지정된 <strong>리소스를 업데이트</strong>합니다. 존재하지 않는 경우에는 새로운 리소스를 생성할 수도 있습니다. 본문에 데이터를 넣어 요청합니다.</td></tr><tr><td><strong>DELET</strong></td><td>지정된 <strong>리소스를 삭제</strong>합니다. 본문에 데이터를 넣지 않습니다.</td></tr><tr><td><strong>PATCH</strong></td><td><strong>리소스의 일부를 업데이트</strong>합니다. PUT과 유사하지만, 리소스의 일부만 업데이트하고 전체 리소스를 대체하지 않습니다. 주로 부분적인 업데이트를 수행할 때 사용됩니다.</td></tr></tbody></table>



## 상태 코드(Status Code)

***

HTTP 통신은 비연결성과 무상태 특징을 가집니다. 이에 통신의 성공 실패 여부를 알려주는 상태 코드가 필요합니다.

이러한 상태 코드를 통해 서버는 Client에게 요청의 성공, 실패 또는 다른 특정 상황을 알려주게 됩니다. 또한 Client는 상태 코드를 기반으로 응답 후 데이터 처리를 수행할 수 있습니다.

대표적인 상태 코드는 아래와 같습니다.

<table><thead><tr><th width="189">Status Code</th><th>Description</th></tr></thead><tbody><tr><td><strong>1xx (Information)</strong></td><td>요청은 받았으나 처리 중인 상태를 나타냅니다. 일반적으로 브라우저에서 직접 사용되지는 않습니다.</td></tr><tr><td><strong>2xx (Successful)</strong></td><td>요청이 성공적으로 처리되었음을 나타냅니다.<br><br>- <strong>200 OK</strong>: 요청이 성공적으로 처리되었음을 나타냅니다.<br>- <strong>201 Created</strong>: 요청에 의해 새로운 리소스가 생성되었음을 나타냅니다.<br>- <strong>204 No Content</strong>: 응답에 본문이 없음을 나타내며, 주로 PUT 또는 DELETE 요청에 사용됩니다.</td></tr><tr><td><strong>3xx (Redirection)</strong></td><td>추가 조치가 필요함을 나타내며, 클라이언트는 추가 조치를 취해야 합니다. 주로 리디렉션을 통해 다른 리소스로 이동합니다.<br><br>- <strong>301 Moved Permanently</strong>: 리소스의 위치가 변경되었으며, 클라이언트는 새 위치로 요청을 다시 보내야 합니다.<br>- <strong>302 Found (Temporary Redirect)</strong>: 리소스의 위치가 일시적으로 변경되었으며, 클라이언트는 임시로 새 위치로 요청을 보내야 합니다.<br>- <strong>304 Not Modified</strong>: 클라이언트의 캐시된 버전이 최신임을 나타내며, 실제 데이터를 받을 필요가 없음을 나타냅니다.</td></tr><tr><td><strong>4xx (Client Error)</strong></td><td>클라이언트의 요청에 오류가 있음을 나타냅니다. 요청이 잘못되었거나 권한이 없는 경우가 여기에 해당됩니다.<br><br>- <strong>400 Bad Request</strong>: 요청이 잘못되었음을 나타냅니다.<br>- <strong>401 Unauthorized</strong>: 인증이 필요하며, 클라이언트는 로그인 등의 인증을 해야 합니다.<br>- <strong>403 Forbidden</strong>: 클라이언트가 리소스에 접근 권한이 없음을 나타냅니다.<br>- <strong>404 Not Found</strong>: 요청한 리소스가 서버에서 찾을 수 없음을 나타냅니다.</td></tr><tr><td><strong>5xx (Server Error)</strong></td><td>서버에서 오류가 발생했음을 나타냅니다. 클라이언트의 요청은 올바르나 서버에서 처리하지 못한 경우가 여기에 해당됩니다.<br><br>- <strong>500 Internal Server Error</strong>: 서버 내부에서 오류가 발생했음을 나타냅니다.<br>- <strong>502 Bad Gateway</strong>: 게이트웨이나 프록시 서버에서 오류가 발생했음을 나타냅니다.<br>- <strong>503 Service Unavailable</strong>: 서버가 일시적으로 사용 불가능한 상태임을 나타냅니다.</td></tr></tbody></table>



## Summary

***

* HTTP 는 **Server-Client 구조의 데이터 요청** 시 사용하는 TCP/IP 기반의 프로토콜이다.
* HTTP 의 특징은 **요청-응답 구조**, **비연결성**, **무상태**이다.
* HTTP 요청에는 Version, HTTP Method, URL, Header, Body 가 포함된다.
* HTTP 응답에는 Status Code, Header, Body 가 포함된다.
* HTTP Method는 요청 시 사용하며, 어떤 요청인지 알려주는 역할을 한다.
  * 대표적으로 **GET, PUT, DELETE, UPDATE, PATCH** 가 있다.
* Status Code 는 응답 시, 해당 요청이 어떻게 처리되었는지 알려주는 역할을 한다.
  * 대표적으로 **2xx, 4xx, 5xx** 가 있다.
