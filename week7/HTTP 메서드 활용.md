# 13 | HTTP 메서드 활용

[1. 클라이언트에서 서버로의 데이터 전송](#클라이언트에서-서버로의-데이터-전송) <br>
[2. HTTP API 설계 예시](#http-api-설계-예시) <br>
<br>

### 클라이언트에서 서버로의 데이터 전송
<hr>

#### **Query Parameter를 이용한 데이터 전송**
- GET method 사용 시
- 검색어 같은 정렬 필터

<br>

#### **Message Body를 이용한 데이터 전송**
- POST, PUT, PATCH
- 회원 가입, 상품 주문, 리소스의 등록, 리소스 변경 등의 작업에 주로 사용하는 방식

<br>


#### **클라이언트에서 서버로 데이터를 전송하는 4가지 예시 상황**
<hr>

1. 정적 데이터 조회
![image](https://github.com/codusl100/spring_study/assets/77263479/05c15a68-7beb-42d4-bd2d-a0e179b6d456)

    - 서버에게 내가 원하는 파일을 URL(URI)를 이용해 직접 명시하여 요청하는 형태를 취함
        - GET method를 이용해 request를 보낸다고 하더라도 query parameter를 사용할 필요 없음
    - 서버에서는 URL에 맞는 정적인 데이터를 클라이언트에게 전달하는 방식으로 동작할 것
        - 이미지, 정적 텍스트 문서를 전달하는 경우
        - 조회는 GET method 사용
        - 정적 데이터는 쿼리 파라미터 없이 리소스에 대한 경로(URL)로 단순 조회 가능

<br>

2. 동적 데이터 조회
![image](https://github.com/codusl100/spring_study/assets/77263479/a144d5bd-28c3-4d17-903f-bec94a3ffbf4)

    - GET method 이용
    - 정적 데이터 조회와 다르게 쿼리 파라미터를 이용해 원하는 정보(필터)를 전달하는 방식으로 동작
        - 주로 검색, 게시판 목록에서 정렬 필터(검색어)
        - 조회 조건을 줄여주는 필터, 조회 결과를 정렬하는 정렬 조건에 주로 사용
        - 조회에는 GET method 사용
        - GET은 query parameter를 사용해 데이터를 전달

<br>

3. HTML Form을 통한 데이터의 전송
![image](https://github.com/codusl100/spring_study/assets/77263479/c2fc2516-5adf-4a89-992e-76e8fcdb9d31)
![image](https://github.com/codusl100/spring_study/assets/77263479/58205e73-f1af-4a22-8649-87d33178c9f0)

    - form tag의 method 속성을 정의하는 방식에 따라 달라짐
        - GET method로 전송하는 경우 form에 담긴 정보가 URL에 query parameter 형태로 입력되어 서버로 요청
        - POST method로 전송하는 경우 message body에 실려서 서버로 전달
    - 전송되는 데이터의 유형에 따라 HTTP message의 header부분에 있는 Content-Type이 다르게 정의
        - query parameter로 데이터가 전달되는 경우 
            - "application/x-www-form-urlencoded"로 작성
        - message body에 이미지 등의 데이터가 함께 전달되는 경우 
            - "multipart/form-data"로 작성
        - content-type: application/x-www-form-urlencoded 사용
            - form의 내용을 메세지 바디를 통해 전송 (key = value, query parameter 형식)
            - 전송 데이터를 url encoding 처리함
        - content-type: multipart/form-data
    - HTML Form을 이용한 전송 방식은 GET, POST만 지원

<br>

4. HTTP API를 통한 데이터 전송
![image](https://github.com/codusl100/spring_study/assets/77263479/392c9133-ddaf-4691-a147-427f9bfa26b1)

    - GET, POST, PUT, PATCH 등과 같은 HTTP method 모두 사용 가능
    - 회원가입, 상품 주문, 데이터 변경 등의 작업 수행
    - 서버 to 서버
        - 백엔드 시스템 통신에 사용
    - app client 
        - 안드로이드, 아이폰 환경과 백엔드 서버 간 통신
    - web client(Ajax)
        - HTML에서 Form 전송 대신 Java Script를 이용한 통신에 사용
    - POST, PUT, PATCH : message body를 통해 데이터 전송
    - GET: 조회 기능, query parameter로 데이터를 전달
    - Content-Type: application/json을 주로 사용 (json이 최근 들어 사실상 표준으로 사용됨)
        - TEXT, XML, JSON 등

<br>

### HTTP API 설계 예시
<hr>

#### **회원 관리 시스템 - POST를 이용한 신규 자원 등록의 특징**

- 클라이언트는 등록될 resource의 URI를 모르는 상황 
    - /members URI에 POST 요청 전송함으로써 새로운 resource의 등록 요청
- 서버는 이러한 클라이언트의 요청에 따라 새로 등록된 resource URI를 생성
    - HTTP/1.1 201 Created
    - Location: /members/100
    - 위와 같은 response message를 resource 등록에 대한 reponse로 전송 
- 컬렉션(Collection)
    - 서버가 관리하는 resource directory
    - 서버가 resource의 URI를 생성하고 관리한다.
    - 이 예시에서 collection은 "/members"에 해당

<br>

#### **파일 관리 시스템 - PUT을 이용한 신규 자원 등록 방식의 특징**

- 클라이언트가 리소스 URI를 알고 있어야 함
    - /files/{filename} URI에 PUT method를 이용해 새로운 자원을 등록하는 방식
    - ex) PUT /files/star.jpg
- 클라이언트가 직접 리소스의 URI를 지정하는 방식으로 resource를 등록
- 스토어(Store)
    - 클라이언트가 관리하는 리소스 저장소를 의미
    - 클라이언트가 리소스의 URI를 알고 관리한다.
    - 여기서 스토어는 "/files"에 해당

<br>

#### **HTML FORM을 사용하는 조건에 맞춘 URI 설계 예시**

- 회원 목록 /members > GET
- 회원 등록 폼 /members/new > GET (등록 폼을 제공하기 위한 URI를 따로 설계)
- 회원 등록 /members/new, /members > POST (폼과 등록의 URI를 맞추거나, collection 관리하는 방법으로 통일하거나, 취향 차이)
- 회원 조회 /members/{id} > GET
- 회원 수정 폼 /members/{id}/edit > GET (수정 폼 또한 마찬가지)
- 회원 수정 /members/{id}/edit, /members/{id} > POST (어떻게 URI를 설계할지 두 가지중 한 개로 고민해야 함)
- 회원 삭제 /members/{id}/delete > POST
 
<br>

**컨트롤 URI**
이러한 제약으로 인해 HTML FORM만을 사용해 데이터를 전송하는 컨트롤 URI라는 것을 사용해야 함

- GET, POST만을 지원하는 것에 따른 제약이 발생
- 이러한 제약을 해결하기 위해 동사로 된 resource path를 사용
- 앞서 봤던, /new, /edit, /delete 이런 것들이 control URI에 해당
- HTTP method 사용 제약에 따라서 어쩔 수 없이 명사가 아닌 resource path를 사용하는 경우에 해당
- 이상적으로는 HTTP method만 사용하면 좋겠지만, 실무에서는 일부 control URI를 사용하는 경우가 더러 발생


<hr>

#### **최종 정리**
- 문서(document)
    - 단일 개념 (파일 하나, 객체 인스턴스, 데이터베이스의 row)
    - ex) /members/100, /files/star.jpg
- 컬렉션(collection)
    - 서버가 관리하는 resource directory
    - 서버가 resource의 URI를 생성하고 관리한다.
    - ex) /members
- 스토어(store)
    - 클라이언트가 관리하는 자원 저장소
    - 클라이언트가 resource URI를 알고 관리
- 컨트롤러(controller), 컨트롤 URI
    - 문서, 컬렉션, 스토어 등의 개념으로 해결하기 어려운 추가 프로세스의 실행을 관리
    - 동사를 직접 URI에 사용하는 방식을 의미 (이상적으로 REST API를 만들면 좋겠지만, 현실적으로 어려운 부분이 발생할 수 있음)
    - ex) members/{id}/delete
