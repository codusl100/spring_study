## 12 | HTTP 메서드

[1. API의 URI(Uniform Resource Identifier)를 설계하며 고민해야 할 사항들](#api의-uriuniform-resource-identifier를-설계하며-고민해야-할-사항들) <br>
[2. HTTP method](#http-method) <br>
[3. HTTP method의 속성](#http-method의-속성) <br>

<br>

### API의 URI(Uniform Resource Identifier)를 설계하며 고민해야 할 사항들
<hr>

- 리소스와 행위 분리
    - URI는 리소스 식별에만 집중
    - **리소스**와 해당 리소스를 대상으로 하는 **행위** 분리
        - 리소스 : 회원
        - 행위 : 조회, 등록, 삭제, 수정

<br>

### HTTP method
<hr>

- GET: 리소스 조회
- POST: 요청 데이터 처리, 주로 등록에 사용
- PUT: 리소스 전체를 대체, 해당 리소스가 없는 경우 생성
- PATCH: 리소스의 일부를 변경
- DELETE: 리소스 삭제
- 기타
    - HEAD: GET과 동일하지만 HTTP message의 body부분을 제외하고 header와 status line만 반환
    - OPTIONS: 대상 리소스에 대한 통신 가능 옵션을 설명 (주로 CORS에서 사용)
    - CONNECT, TRACE: 이 두가지는 거의 사용되지 않음

- 최근에는 Resource가 아니라 Representation이라는 개념으로 대체

<br>

#### **GET**

> **HTTP method를 이용한 리소스 조회 과정**
>
> 1. 클라이언트에서 서버로 request message 전달
>
> 2. 메세지가 서버에 도착
>
> 3. 서버에서 클라이언트로 response message 전달


- 리소스를 조회하는 method
- 서버에 전달하고 싶은 데이터를 query(쿼리 파라미터, 쿼리 스트링)을 통해 전달
- 메시지 바디를 이용할 수 있지만 지원하지 않는 곳이 많아 권장하지 않음

<br>

#### **POST**

> 1. 미리 약속된 URL로 message body를 통해 server로 request data를 전달
>
> 2. server는 request data를 처리
>
> 3. message body 파트를 통해 들어온 데이터를 처리하는 모든 기능 수행

- 요청 데이터를 처리하는 method
- 주로 전달된 데이터를 이용해 신규 리소스 등록, 프로세스 처리 과정 진행
- 다른 메서드로 처리하기 애매한 경우 사용
    - ex) JSON으로 조회 데이터를 넘겨야 하는데, GET method를 사용하기 어려운 경우 -> 애매하면 POST 사용

<br>

#### **PUT**

- 리소스 완전히 대체
    - 이미 존재하는 리소스는 대체하고 존재하지 않는 리소스의 경우 POST와 같은 방식으로 동작하여 새롭게 생성
    - 쉽게 이야기해서 덮어버림
- 클라이언트가 리소스를 식별
    - 클라이언트가 리소스의 위치를 아는 상황에서 지정한 URI(URL)
    - POST와는 이미 존재하는 리소스를 덮어씌운다는 점에서 차이가 있음
- PUT은 리소스를 제공된 정보를 바탕으로 완전히 대체하기 때문에 기존에 리소스에 저장된 정보는 모두 사라짐
    - 제공하지 않은 필드에 대해서는 삭제
    - 해당 부분 보완하기 위해 PATCH method 사용

<br>


#### **PATCH**

- PUT method와 다르게 리소스를 부분적으로 대체

<br>

#### **DELETE**

- 리소스를 제거하기 위해 사용하는 method

<br>

### HTTP method의 속성
<hr>

![image](https://github.com/codusl100/spring_study/assets/77263479/e2b4f84a-8ddd-47bf-95e8-ce497cb79f4d)

#### **안전 (Safe Methods)**
- 호출해도 리소스를 변경하지 않는 method를 정의하는 속성
- 이런 문제들은 여기 선에서 다루지 않음
    - Q: 아무리 리소스 변경이 없어도, 계속 호출해서 로그가 쌓여 오류가 발생해도 안전하다고 판단하나요?
    - A: 안전 속성은 해당 리소스만 고려한다. 그런 부분까지는 고려 X

<br>

#### **멱등 (Idempotent Methods)**
- 계속 호출해도 결과가 똑같은 method를 정의하는 개념
- f(f(x)) = f(x)라고 설명되어 있으나, f(x), (fx), f(x) 후의 DB == f(x) 후의 DB 가 조금 더 어울림
- 한 번 호출하든 여러 번 호출하든 결과가 같음을 의미
- 멱등 메서드
    - GET: 한 번 조회하든 여러 번 조회하든 결과는 똑같음
    - PUT: 결과를 대체하는 메서드, 따라서 같은 요청을 여러 번 보내도 결과는 같음
    - DELETE: 리소스를 삭제하는 메서드, 여러번 요청을 보내더라도 한 번만 삭제될 것
    - POST(X): *멱등이 아님*, 두 번 호출하면 같은 process가 중복해서 발생이 가능 (예를 들어 결제 프로세스)
- 멱등 속성 활용
    - 자동 복구 메커니즘
    - 서버가 TIMEOUT 등으로 정상 응답을 못한 경우, 클라이언트가 같은 요청을 다시 해도 되는지에 대한 판단의 근거로 삼을 수 있음
- 이런 경우에는 멱등이 아닌 것 같은데?
    - Q: 재요청 중간에 다른 곳에서 리소스를 변경하면?
        - 사용자1: GET -> username: A, age: 20
        - 사용자2: PUT -> username: A, age: 30
        - 사용자1: GET -> username: A, age: 30
    - A: 멱등은 외부 요인으로 중간에 리소스가 변경되는 것 까지는 고려하지 않고 판단하는 속성

<br>

#### **캐시 가능 (Cacheable Methods)**
- 응답 결과 리소스를 캐시해서 사용해도 되는가?
    - GET, HEAD, POST, PATCH는 가능
    - 실제로는 GET, HEAD 정도만 캐시로 사용
    - POST, PATCH는 본문 내용까지 캐시 키로 고려해야 하는데, 이는 구현이 까다로움
