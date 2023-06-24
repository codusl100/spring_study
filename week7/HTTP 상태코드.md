# 14 | HTTP 메서드 활용

[1. HTTP 상태 코드](#http-상태-코드) <br>
[2. 1xx (Informational)](#1xx-informational) <br>
[3. 2xx (Succssful)](#2xx-successful) <br>
[4. 3xx (Redirection)](#3xx-redirection) <br>
[5. 4xx - 클라이언트 오류, 5xx - 서버 오류](#4xx---클라이언트-오류-5xx---서버-오류) <br>
<br>

### HTTP 상태 코드
<hr>

client에 보낸 요청(request)의 처리 상태를 응답(response)에서 알려주는 기능

- 1XX (Informational): 요청이 수신되어 처리 중
- 2XX (Successful): 요청 정상 처리
- 3XX (Redirection): 요청을 완료하려면 추가 행동이 필요
- 4XX (Client Error): 클라이언트 오류, 잘못된 문법 등으로 서버가 요청을 수행할 수 없음
- 5XX (Server Error): 서버 오류, 서버가 정상 요청을 처리하지 못함
<br>

만약에 모르는(정의되지 않은) 상태 코드가 나타난다면?
- 클라이언트는 상위 상태 코드로 해석해서 처리
-   디테일한 사항을 몰라도 큰 틀에서는 이해 가능
- 미래에 새로운 상태 코드가 추가되어도 클라이언트를 변경하지 않아도 됨
    
    ex)

        299 ??? > 2XX (Successful로 처리)
    
        451 ??? > 4XX (Client Error)
    
        599 ??? > 5XX (Server Error)

<br>

### 1xx (Informational)
<hr>

> 요청이 수신되어 처리 중을 알리는 응답 상태
>
>(거의 사용되지 않아 생략)

<br>

### 2xx (Successful)
<hr>

> 클라이언트가 보낸 요청을 성공적으로 수행했음을 알리는 상태 코드(status code)

 
일반적으로 사용하는 <status code, status message> 쌍

- <200, OK>
    - 가장 많이 보는 케이스
- <201, Created>
    - 요청에 성공하여 새로운 리소스 생성됨
    - 생성된 리소스는 응답의 Location 헤더 필드로 식별
- <202, Accepted>
    - 배치 처리 같은 곳에서 사용

        ex) 요청 접수 후 1시간 뒤에 배치 프로세스가 요청을 처리하는 프로세스인 경우에 사용
    - 잘 사용하지 않음
- <204, No Content>
    
    - save 버튼의 결과로 아무 내용이 없어도 괜찮은 경우
    - save 버튼이 눌려도 같은 화면을 유지해야 함
    - 결과 내용이 없어도 204 message (2xx)만으로 성공 인식
    
        ex) 웹 문서 편집기에서 save 버튼을 눌러 저장하는 경우 (응답이 굳이 필요 없음)


<br>

### 3xx (Redirection)
<hr>

요청을 완료하기 위한 에이전트(client program, 주로 web browser에 해당)의 추가 조치가 필요한 경우 사용

#### **리다이렉션(Redirection)의 이해**
<br>

![image](https://github.com/codusl100/spring_study/assets/77263479/29ddc67f-81a1-478c-8c3d-06274558ccc5)

웹 브라우저는 3XX 응답(response)의 결과에 Location header가 있으면, Location header의 값으로 작성된 위치로 자동으로 이동 (redirect)

<br>

#### **리다이렉션의 종류**

> 영구 리다이렉션 - 특정 리소스 URI가 영구적으로 이동한 경우
>
> 일시 리다이렉션 - 일시적인 URI의 변경
>
> 특수 리다이렉션

<br>

1. 영구 리다이렉션
- 리소스의 URI가 영구적으로 이동
- 원래 URL 사용하지 않음
- 검색 엔진 등에서도 변경을 인지
- **301 Moved Permanently**
    - redirect시 요청 메서드가 GET으로 변하고, 본문(등록하려 한 정보 등)이 제거될 수 있음
- **301 Moved Permanently**
    - 301과 기능은 같음
    - redirect시 요청 메서드와 본문 유지

<br>

2. 일시 리다이렉션
- 리소스의 URI가 일시적으로 변경
- 검색 엔진 등에서 URL 변경하면 안됨
- 303과 307은 302의 모호성으로 새로이 만들어진 status code지만 현업에서는 대부분 302 많이 사용
- **302 Found**
    - 리다이렉트시 요청(request) 메서드가 GET으로 변하고 본문이 제거될 수 있음
    - 원래 스펙에서는 당연히 method 유지되는 방식으로 생각하고 만들었으나, 브라우저의 구현이 다른 방식으로 됨에 따라서 307과 303이 등장
    - 현업에서 자주 사용
- **307 Temporary Redirect**
    - 302와 기능은 동일
    - 리다이렉트시 요청 메서드와 본문 모두 유지
- **303 See Other**
    - 302와 기능은 동일
    - 리다이렉트시 요청 메서드가 모두 GET으로 변경 

<br>

![image](https://github.com/codusl100/spring_study/assets/77263479/8fc115c0-d52d-4bb9-b1c5-32439145d6a0)

일시적인 리다이렉션의 예시 상황
- 만약 POST로 주문 후에 웹 브라우저를 새로고침 한다면?
- 새로고침은 다시 요청 (같은 요청을 서버로 다시 요청)
- 중복 주문이 될 수 있는 상황 발생

<br>

4번: 실수로 주문 완료 이후에 브라우저에서 새로 고침을 하는 경우, 이미 한 번 전송한 요청을 반복해서 전송하게 됨

*이로 인해 주문 요청을 반복해서 서버로 전송하는 문제가 발생*

<br>

![image](https://github.com/codusl100/spring_study/assets/77263479/b899bbc2-0c0f-4dd8-8279-b8ccf487a0a0)

**PRG: Post / Redirect / Get (이런 경우를 해결하기 위해 정말 많이 사용하는 방식)**

- POST로 주문한 후에 새로 고침으로 인한 중복 주문을 방지 
- POST로 주문 후 주문 결과 화면을 GET method로 리다이렉트
- 새로고침해도 결과 화면을 GET으로 조회하기 때문에 중복 주문 문제가 발생하지 않음
- 중복 주문 대신에 결과 화면만 GET으로 다시 요청

<br>

**PRG 이후의 리다이렉트**

- URL이 이미 POST > GET으로 리다이렉트 됨
- 새로 고침을 해도 GET으로 결과 화면을 조회하기 때문에 중복 조회가 막힘
- 사용자는 잘못된 POST로 인한 오류 메시지가 줄어들어 굉장히 좋고, 서버 입장에서도 오류가 줄어들어 PRG 기법을 적용하는 것에 따르는 이점이 많음

<br>

3. 기타 리다이렉션

- 300 Multiple Choices : 사용하지 않음
- 304 Not Modified
    - 캐시를 목적으로 사용
    - 클라이언트에게 해당 resource가 수정되지 않았음(만료되지 않음)을 알려줌

<br>

### 4xx - 클라이언트 오류, 5xx - 서버 오류
<hr>

- **4XX (Client Error)**
    - 클라이언트의 요청에 잘못된 문법 등으로 서버가 요청을 수행할 수 없는 경우 오류의 원인이 클라이언트에게 있음
    - *클라이언트가 이미 잘못된 요청 또는 데이터를 보내고 있기 때문에, 똑같은 재시도가 실패할 것*

<br>

- 400 - Bad Request
    -클라이언트가 잘못된 요청을 해서 서버가 요청을 처리할 수 없음
    - 요청 구문, 메시지 등등의 오류
    - 클라이언트는 요청 내용을 다시 검토하고 보내야 함
    - ex) 요청 parameter가 잘못되었거나, API spec에 맞지 않았을 경우 서버 개발자는 철저하게 validation 해서 클라이언트의 잘못임을 명시해줘야 함

<br>

- 401 - Unauthorized
    - 클라이언트가 해당 리소스에 대한 인증이 필요함
    - 인증(Authentication)이 되지 않음을 의미 (로그인이 되어있지 않음)
        - 인증(Authentication): 본인이 누구인지 확인하는 과정, 즉 로그인
        - 인가(Authorization): 권한 부여 (admin 권한처럼 특정 리소스에 접근할 수 있는 권한을 의미, 인증이 되어 있어야 인가 또한 가능)
        - 오류 메시지가 Unauthorized이지만, 인증되지 않음을 의미
    - 401 오류 발생 시 응답에 www-Authenticate 헤더와 함께 인증 방법이 설명되어야 함

<br>

- 403 - Forbidden
    - 서버가 요청을 이해했지만, 승인을 거부한 경우
    - 주로 인증 자격 증명은 있지만, 접근 권한이 불충분한 경우
    - ex) admin 권한이 없는 사용자가 로그인은 했지만 admin 등급의 resource에 접근하는 경우
    - 사실상 인가가 되지 않은 계정임을 명시

<br>

- 404 - Not Found
    - 요청한 리소스를 찾을 수 없음
    - 요청 resource가 서버에 없기 때문에 클라이언트 오류
    - 또는 클라이언트가 권한이 부족한 resource에 접근할 때 해당 resource를 숨기고 싶은 경우에 사용
    - 403도 사용하지 않고, resource를 숨기고 싶은 경우에 사용

<br>

- **5XX (Server Error)**
    - 서버의 문제로 오류가 발생하는 경우를 명시하기 위해 사용
    - 서버에 문제가 있기 때문에 재시도하면 성공할 수도 있음 

<br>

- 500 - Internal Server Error
    - 서버 문제로 오류가 발생함, 애매하면 500 오류로 표기
    - 서버 내부의 문제로 오류가 발생했음을 알리기 위해 사용

<br>

- 503 - Service Unavailable
    - 서비스 이용 불가
    - 서버가 일시적인 과부하 또는 예정된 작업으로 잠시 요청을 처리할 수 없음을 알리기 위해 사용
    - Retry-After header-field로 얼마 뒤에 복구되는지 알려줄 수도 있음
    - 웬만하면 서버에서는 500대의 에러를 만들면 안됨 

<br>

비지니스 로직 상의 오류 또한 서버의 에러로 처리하면 안됨

쿼리에서의 문제 발생, null pointer exception, DB가 내려감 등의 문제 상황에서 서버의 에러를 내는 식으로 코딩, 나머지는 400이나 200으로 처리
<br>