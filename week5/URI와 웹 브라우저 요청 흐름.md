## 10 | URI와 웹 브라우저 요청 흐름

[1. URI(Uniform Resource Identifider)](#uriuniform-resource-identifier) <br>
[2. 웹 브라우저 요청 흐름](#웹-브라우저-요청-흐름) <br>

<br>

### URI(Uniform Resource Identifier)
<hr>

#### **URI**
> resource를 식별하는 통합된 방법

- URI는 locator, name 또는 둘 다 추가로 분류될 수 있음
- URI 안에 URL, URN이 있음
- URI 단어 뜻
    - Uniform : 리소스 식별하는 통일된 방식
    - Resource : 자원,URI로 식별할 수 있는 모든 것 (제한 없음)
    - Identifier : 다른 항목과 구분하는데 필요한 정보

<br>

#### **URL, URN**

- URL - Locator : 리소스가 있는 위치를 지정
- URN - Name : 리소스에 이름을 부여
- 위치는 변할 수 있지만, 이름은 변하지 않음
- 예시
    - URN -> urn:isgn:8960777331
    - URN만으로 실제 리소스를 찾는 방법은 보편화되어 있지 않아 주로 URL 사용

<br>

#### **URL 분석**
> scheme://[userinfo@]host[:port][/path][?query][#fragment]
> https://www.google.com:443/search?q=hello&hl=ko

- 요약
    - 스키마(프로토콜): https
    - 호스트: wwww.google.com
    - 포트: 443
    - 패스: /search
    - 쿼리 파라미터: q=hello&hl=ko

#### *scheme*
- 주로 프로토콜 사용
    - 프로토콜은 어떤 방식으로 자원에 접근할 것인가 하는 약속
- E.g., https, https, ftp, etc.

#### *userinfo*
- URL에 사용자 정보를 포함해서 인증
- 거의 사용하지 않음

#### *host*
- 호스트명: 도메인명 또는 IP 주소 직접 사용 가능

#### *port*
- 일반적으로 생략
- 생략시 http는 80, https는 443

#### *path*
- 리소스 경로, 계층적 구조
- E.g.
    - /home/file1.jpg
    - /members
    - /members/100, /items/iphone12

#### *query*
- key=value 형태
- ?로 시작하고, &로 추가 가능
- E.g., ?keyA=valueA&keyB=valueB
- query parameter, query string 등으로도 부름

#### *fragment*
- html 내부 북마크 등에 사용
- 서버로 전송되는 정보는 아님

<br>

### 웹 브라우저 요청 흐름
<hr>

> 웹 브라우저가 `https://www.google.com:443/search?q=hello&hl=ko`로 요청 보낸다 가정
>
> `www.google.com`의 실제 IP 주소를 DNS 통해 조회
>
> HTTPS PORT 생략이지만 https이기에 기본적으로 443 사용

<br>

#### *HTTP 요청 메시지*

> GET /search?q=hello&hl=ko HTTP/1.1
> Host: www.google.com

- 웹브라우저가 http 메시지 생성하여 여러 계층을 통해 TCP/IP 패킷으로 감싼 후 인터넷을 통해 서버로 전송
- 요청 받은 서버는 다음과 같이 그에 맞는 응답 메세지를 만들어 다시 클라이언트(웹 브라우저)로 전송

<br>


#### *HTTP 응답 메시지*

>HTTP/1.1 200 OK
>
>Content-Type: text/html;charset=UTF-8
>
>Content-Length: 3423
>
```
<html>
	<body>...</body>
</html>
```

- 서버의 응답을 받은 클라이언트의 웹 브라우저가 http 응답 메시지를 렌더링 해서 결과를 볼 수 있음