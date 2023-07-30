## 15 | HTTP 헤더1 - 일반 헤더

[1. HTTP 헤더 개요](#http-헤더-개요) <br>
[2. HTTP 표준 변화](#http-표준-변화) <br>
[3. 표현](#표현) <br>
[4. 협상](#협상content-negotiation) <br>
[5. 전송 방식](#전송-방식) <br>
[6. 일반 정보](#일반-정보) <br>
[7. 특별한 정보](#특별한-정보) <br>
[8. 인증](#인증) <br>
[9. 쿠키](#쿠키) <br>

<br>

### HTTP 헤더 개요
<hr>

> header-field = field-name":"OWS field-value OWS (OWS: 띄어쓰기 허용)

#### **용도**

- HTTP 전송에 필요한 모든 부가정보 전달
    ⇒ 메시지 바디의 내용, 바디의 크기, 압축, 인증, 요청 클라이언트, 서버 정보, 캐시 관리 정보 등
- 필요하다면 임의의 헤더 추가도 가능
    ⇒ helloworld: hihi


<br>

#### **과거 분류**

![image](https://github.com/codusl100/spring_study/assets/77263479/25414378-a37d-4e2f-be61-6b27593affa3)


- General헤더
    - 요청 메세지/ 응답메시지 구분 없이 메세지 전체에 적용되는 정보
    - ex) `Connection : close`
- Request 헤더
    - 요청을 보낼때 들어가는 헤더
    - ex) `User-Agent:Mozilla/5.0(Macintosh;..)`
- Response 헤더
    - 응답할 때 들어가는 헤더
    - ex) `Server: Apache`
- Entity 헤더
    - 엔티티 바디 정보
    - 컨텐츠 타입이나 길이같은 메세지 바디에 들어가는 내용에 관련된 헤더
    - ex) `Content-Type: text/html, Content-Length: 3423`
    ![image](https://github.com/codusl100/spring_study/assets/77263479/dfd6d2a9-4ad7-413c-9745-7df99bb38aaa)
        - 메세지 본문(message body)은 엔티티 본문(entity body)를 전달하는데 사용 
        - 엔티티 본문: 요청이나 응답에서 전달할 실제 데이터
        - 엔티티 헤더: 엔티티 본문의 데이터를 해석할 수 있는 정보 제공 
            ⇒ 데이터 유형(html, json), 데이터 길이, 압축 정보 등
<br>


### HTTP 표준 변화
<hr>

#### message body 
- RFC2616(과거)
- RFC7230(최신)

<br>

![image](https://github.com/codusl100/spring_study/assets/77263479/1e34e0ac-8ba3-43b6-81f5-d795366d1594)

#### **HTTP BODY - RFC7230 (최신)**

- 메시지 본문을 통해 표현 데이터 전달
    - **메세지 본문 : Payload**
- 표현 : 요청이나 응답에서 전달할 실제 데이터 **(표현 헤더 + 표현 데이터)**
    - 표현 헤더는 표현 데이터를 해석할 수 있는 정보를 제공
    ⇒ 데이터 유형(html, json), 데이터 길이, 압축 정보 등

<br>

### 표현
![image](https://github.com/codusl100/spring_study/assets/77263479/d3c38e42-65a8-40ed-9c6a-7fdbfee0a4a2)

- 표현 헤더는 요청, 응답 모두 사용 가능
- 클라이언트와 서버간에 송/수신할 때 리소스를 무엇으로 표현할지 알려주고 표현

- Content-Type: 표현 데이터의 형식
    - ex) `text/html; charset=utf-8`, `application/json`, `image/png`
- Content-Encoding: 표현 데이터의 압축 방식
    - 표현 데이터 인코딩
    - 데이터를 전달하는 곳에서 압축 후 인코딩 헤더 추가
    - 데이터를 읽는 쪽에서 인코딩 헤더의 정보로 압축 해제
    - ex) `gzip`, `deflate`, `identity`
- Content-Language: 표현 데이터의 자연 언어
    - ex) `ko`, `en`, `en-US`
- Content-Length: 표현 데이터의 길이
    - 데이터 길이 자체는 표현과 무관하기에 Payload 헤더로 구분 가능
    - 전송, 응답 둘 다 사용 
 

<br>

### 협상(Content negotiation)
<hr>

- 클라이언트가 선호하는 표현 요청 
- 최대한 클라이언트 요청에 맞춰 표현 데이터를 만들어 주는 것
    => 협상 헤더는 요청시에만 사용 가능

#### Quality Values(q) 값을 이용하는 경우
> ex) Accept-Language: ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7 (0~1, 클수록 높은 우선순위, 생략시 1,)
1. ko-KR;q=1 (q생략)
2. ko;q=0.9
3. en-US;q=0.8
4. en:q=0.7

<br>

> ex) 구체적인 것을 우선하는 경우
Accept: text/*, text/plain, text/plain;format=flowed, */*
1. text/plain;format=flowed
2. text/plain
3. text/*
4. */*

<br>

 
### 전송 방식
<hr>

전송방식은 단순하게 4개로 분리할 수 있다. 
<br>

- 단순 전송
    - 요청을 하면 응답을 할때 메세지 바디에 대한 `Content-Length`를 지정하는 것
    - 메세지 바디의 길이를 다 알고있을 때 사용
    - 한번에 요청하고 응답함
- 압축 전송
    - 서버에서 메세지 바디를 gzip같은 방식을 이용해 압축해서 전달하는 방식
    - `Content-Encoding`이라는 항목을 헤더에 넣어서 어떻게 압축했는지 알려줘야 함
- 분할 전송
    - 서버에서 클라이언트로 응답 메세지를 특정 단위로 나눠서 보냄
    - 용량이 큰 경우 헤더에 `Transfer-Encoding: chunked` 지정
    - Content-Length를 넣으면 안됨
- 범위 전송
    - 전송 중 오류로 인해 재전송 하는 경우 Content 일부만 전송하도록 요청

<br>

### 일반 정보
<hr>

#### From
<br>

- **유저 에이전트의 이메일 정보**
- 일반적으로 잘 사용되지는 않음
- 검색 엔진 같은 곳에서 주로 사용
- 요청(Request)에서 사용

<br>

#### Referer
<br>

- 현재 요청된 페이지의 **이전 웹 페이지 주소**
- A페이지에서 B페이지로 이동하면 B를 요청할 때 Referer: A를 포함해서 요청
- 유입 경로 분석이 가능
- 요청(Request)에서 사용

<br>

#### User-Agent
<br>

- **클라이언트의 애플리케이션 (유저 에이전트) 애플리케이션 정보**
- 통계 정보 추출시 유용
- 어떤 종류의 브라우저에서 장애가 발생하는지 파악 가능
- 요청(Request)에서 사용
<br>

#### Server
<br>

- **요청을 처리하는 ORIGIN 서버의 소프트웨어 정보**
- HTTP요청을 보내면 여러 프록시 서버를 거치게되는데, 실제로 내 요청을 받고 응답을 해주는 엔드포인트 서버를 ORIGIN 서버라고 함
    - `Server: Apache/2.2.22(Debian)`
    - `server: nginx`
- 응답(Response)에서 사용
<br>

#### Date
<br>

- 메세지가 발생한 날짜와 시간
    - `Date: Fri, 09 Apr 2021 14:41:31 GMT`
- 응답(Response)에서 사용

<br>

### 특별한 정보
<hr>

#### Host
<br>

- 요청한 호스트 정보(도메인)
- 요청(Request)에서 사용
- 필수 헤더
- 하나의 서버가 여러 도메인을 처리할 때 들어오는 요청을 처리할 도메인을 구분하기 위해 사용
  
<br>

#### Location
<br>

- 페이지 리다이렉션
- 웹 브라우저는 **3xx 응답 결과에 Location 헤더가 있으면, Location 위치로 자동 이동**
    - 201(Created): Location 값은 요청에 의해 생성된 리소스 URI
    - 3xx(Redirection): Location값은 요청을 자동으로 리디렉션 하기 위한 대상 리소스를 가리킴

<br>

#### Allow
<br>

- 허용 가능한 HTTP 메서드
- 지원하지 않는 메서드로 요청이 들어오는 경우 405(Method Not Allowd)에서 응답에 허용하는 메서드 정보를 보내줌
- 405(Method Not Allowd)에서 응답에 포함해야 함
- Allow: GET, HEAD, PUT 

<br>

#### Retry-After
<br>

- 유저 에이전트가 다음 요청을 하기까지 기다려야 하는 시간 정보 제공
- 사용하기가 쉽지가 않음 (예측하기 힘듬) 
- 503(Service Unavailable): 서비스가 언제까지 불가능한지 알려줄 수 있음

<br>

### 인증
<hr>

#### Authorization
- 클라이언트 인증 정보를 서버에 전달
- `Authorization: BASIC xxxxxxxxxxxxxxxxxx`
- 인증 메커니즘과는 상관없이 헤더를 제공하는 것으로 인증과 관련된 값을 넣어줌

<br>

#### WWW-Authentication

- 리소스 접근시 필요한 인증 방법 정의
- 인증에 문제가 있는 경우 401 Unauthorized 응답과 함꼐 사용
- `WW-Authentication: Newauth realm="apps", type=1,title="Login to \"apps\"", Basic realm="simple"`

<br>

### 쿠키
<hr>

HTTP 쿠키(웹 쿠키, 브라우저 쿠키)는 서버가 사용자의 웹 브라우저에 전송하는 작은데이터 조각으로, 브라우저는 데이터 조각들을 저장해 놓았다가 **동일한 서버에 재요청시 저장된 데이터를 함께 전송**한다. 

<br>

- Set-Cookie: 서버에서 클라이언트로 쿠키 전달(응답)
- Cookie: 클라이언트가 서버에서 받은 쿠키를 저장하고, HTTP 요청시 서버로 전달

<br>

#### 주 사용 목적
1. 세션 관리(Session Management)
    ⇒ 서버에 저장해야 할 로그인, 장바구니, 게임 스코어등의 정보 관리
2. 개인화(Personalization)
    ⇒ 사용자 선호, 테마 등의 세팅
3. 트래킹(Tracking)
    ⇒ 사용자 행동을 기록하고 분석하는 용도

<br>

#### 쿠키가 없다면 
- HTTP는 무상태(Stateless) 프로토콜
- 클라이언트와 서버가 요청과 응답을 주고 받으면 연결이 끊어짐
- 클라이언트가 다시 요청하면 서버는 이전 요청을 기억하지 못함
- 클라이언트와 서버는 서로 상태를 유지하지 않음

<br>

#### 쿠키의 사용처 
1. 사용자 로그인 세션 관리
    - 보안적 측면을 고려해 사용자 정보를 그대로 전달하지 않고 세션 키(세션ID)를 서버 DB에 저장해두었다가 클라이언트에 반환
    - 서버는 클라이언트가 보낸 세션 ID와 DB상의 세션 ID를 비교하여 사용자를 식별
2. 광고 정보 트래킹

<br>

#### 쿠키의 단점
쿠키는 서버에 항상 전송되기 때문에 **네트워크 트래픽**을 유발할 수 있다. 

이를 방지하기 위해 **최소한의 정보만(세션ID, 인증 토큰)을 보내거나** 쿠키를 서버에 전송하지 않고 **웹브라우저상에 저장해두었다가 사용하는 웹 스토리지를 이용**한다.

<br>

#### 쿠키 도메인 지정
쿠키를 모든 사이트에 보내지 못하도록 도메인을 지정한다.

- 명시: 명시한 문서 기준 도메인 + 서브 도메인 포함 
- 생략: 현재 문서 기준 도메인만 적용

<br>

#### 쿠키 경로 지정
- 작성한 경로를 포함한 하위 경로 페이지에서만 쿠키 접근이 가능
- 일반적으로 path=/ 루트로 지정

<br>

#### 쿠키 보안 Secure, HttpOnly, SameSite
**Secure** <br>
쿠키는 http, https를 구분하지 않고 전송한다. Secure를 적용하면 https인 경우에만 전송한다.

<br>

**HttpOnly** <br>
XSS 공격 방지, 자바스크립트에서 접근 불가(document.cookie)하며 HTTP 전송에만 사용한다.

<br>

**SameSite** <br>
XSRF공격 방지, 요청 도메인과 쿠키에 설정된 도메인이 같은 경우에만 쿠키를 전송한다.