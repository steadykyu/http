# 1. 클라이언트에서 서버로 데이터 전송

## 1.1 두가지의 데이터 전달 방식

### 1.쿼리 파라미터를 통한 데이터 전송

- GET
- 주로 정렬 필터(검색어)

### 2. 메시지 바디를 이용

- POST, PUT, PATCH
- 회원 가입, 상품 주문, 리소스 등록, 리소스 변경

## 1.2 4가지 상황

- 정적 데이터 조회

  - 이미지, 정적 텍스트 문서

- 동적 데이터 조회

  - 주로 검색, 게시판 목록에서 정렬 필터(검색어)

- HTML Form을 통한 데이터 전송

  - 회원 가입, 상품 주문, 데이터 변경

- HTTP API를 통한 데이터 전송
  - 회원 가입, 상품 주문, 데이터 변경
  - 서버 to 서버, 앱 클라이언트, 웹 클라이언트(Ajax)

아래 그림으로 더 구체적으로 이해해보자.

### 1.2.1 정적 데이터 조회

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/http5_1.png" >
</p>

URL 식별자(경로) 만 입력해주고, 경로에 존재하는 image/jpeg를 출력해주고 있다.

> 정리

- 이미지, 정적 텍스트 문서
- 조회는 GET 사용
- 정적 데이터는 일반적으로 쿼리 파라미터 없이 리소스 경로로 단순하게 조회 가능

### 1.2.2 동적 데이터 조회

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/http5_2.png" >
</p>

search 경로에 q = hello h1 = ko 가 입력되 있다. 이는 한국어이고, search 경로의 입력값은 hello라는 의미이다. 이 내용대로 정렬 필터해서 결과를 동적으로 생성해낸다.

> 정리

- 주로 검색, 게시판 목록에서 정렬 필터(검색어)

- 조회 조건을 줄여주는 필터, 조회 결과를 정렬하는 정렬 조건에 주로 사용

- 조회이므로 GET 사용

- GET은 쿼리 파라미터 사용해서 데이터를 전달
  - 이전 내용처럼 spec에서 GET도 메시지 바디로 데이터를 전송할 수는 있다고 했지만, 서버에서의 호환성의 이유로 실무에서 권장 되지 않는다.

### 1.2.3 HTML Form 데이터 전송

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/http5_3.png" >
</p>

- html의 form tag를 이용하여 데이터를 전송할 수 있다.

- 거의 query파라미터와 비슷 한 방식으로 서버에 전송을 한다.
  > query파라미터와 비슷

```
username=kim&age=20
```

- Content-Type: application/x-www-form-urlencoded
  > application/x-www-form-urlencoded

```
Content-Type 은 클라이언트와 서버끼리 이미 약속이 되어 있는 내용이다.
```

- HTML Form submit시 POST 전송

클라이언트 Web browser가 form tag로 를 읽으면 form tag안의 클라이언트 데이터를 username=kim&age=20 형식(key - value 스타일)으로 HTTP 메시지에 저장한다. 이때 요청 메시지 안의 Content-Type 에 맞추어 형식이 정해지는 것이다.

저장된 username=kim&age=20 형식 데이터를 메시지 바디에 넣고 서버에 전송한다.
(POST 이므로 메시지 바디에 넣는다.)

왠만한 웹서버들은 이러한 메시지 바디 내용을 파싱하여 사용할 수 있도록 구현 되어 있다.

GET, POST 두 메서드들이 사용 가능하다. 데이터가 바뀌는 경우에는 POST, 그대로 유지하고 조회할 경우에는 GET으로사용한다.

> GET 전송

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/http5_4.png" >
</p>

GET이므로 메시지 바디가 아닌 url 경로의 쿼리 파라미터에 넣어버린다.

/save는 회원을 저장하여 결과를 바꾸므로, GET을 쓰면 안된다. 아래처럼 조회때 사용하자.

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/http5_5.png" >
</p>

### 1.2.4 mulipart/form-data

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/http5_6.png" >
</p>

- multipart/form-data 파일 전송을 하기위해서 사용한다.

원래 POST메서드의 enctype="" 옵션의 default 값은 application/x-www-form-urlencoded 이다.

그러나 바이트 단위의 file까지 전송시키기 위해서는 multipart/form-data 형식에 맞는 메시지를 생성해야 한다.

form tag와 사용자의 입력값이 들어오면, 요청 메시지는 Web browser가 알맞게 만들어 준다.

> HTML Form을 통한 데이터 전송 정리

- HTML Form submit시 POST 전송

  - 예) 회원 가입, 상품 주문, 데이터 변경

* POST 전송시 Content-Type: application/x-www-form-urlencoded 사용

  - form의 내용을 메시지 바디를 통해서 전송(key=value, 쿼리 파라미터 형식)
  - 전송 데이터를 url encoding 처리

    - 예) abc김 -> abc%EA%B9%80

- HTML Form은 GET 전송도 가능

- Content-Type: multipart/form-data
  - 파일 업로드 같은 바이너리 데이터 전송시 사용
  - 다른 종류의 여러 파일과 폼의 내용 함께 전송 가능(그래서 이름이 multipart)

* 참고: HTML Form 전송은 GET, POST만 지원한다.

detail option들의 내용은 사용때마다 API나 자료를 찾아보자.

## 1.4 HTTP API 데이터 전송

### 1.4.1 API 란?

> 참고 API란?

API 란?: Application Programming Interface 응용 프로그램 프로그래밍 인터페이스)는 응용 프로그램에서 사용할 수 있도록, 운영 체제나 프로그래밍 언어가 제공하는 기능을 제어할 수 있게 만든 인터페이스를 뜻한다.

> HTTP API 정의

```
An HTTP API is an API that uses Hypertext Transfer Protocol as the communication protocol between the two systems. HTTP APIs expose endpoints as API gateways for HTTP requests to have access to a server.
```

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/http5_6.png" >
</p>

두 시스템(ex)클라, 서버) 사이의 소통 **protocol**(규약)을 http(hypertext Transfer **Protocol**) 로 사용하는 API를 HTTP API 라고한다.

### 1.4.2 HTTP API 데이터 전송

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/http5_7.png" >
</p>

> 정리

- 서버 to 서버

  - 백엔드 시스템 통신

- 앱 클라이언트

  - 아이폰, 안드로이드

- 웹 클라이언트

  - HTML에서 Form 전송 대신 자바 스크립트를 통한 통신에 사용(AJAX)
  - 예) React, VueJs 같은 웹 클라이언트와 API 통신

- POST, PUT, PATCH: 메시지 바디를 통해 데이터 전송

- GET: 조회, 쿼리 파라미터로 데이터 전달

- Content-Type: application/json을 주로 사용 (사실상 표준)

  - TEXT, XML, JSON 등등

  - 이전에는 XML이 표준이었으나 최근에는 JSON이 표준이 되었다.

# 2. HTTP API 설계 예시

- 1.HTTP API - 컬렉션

  - POST 기반 등록
  - 예) 회원 관리 API 제공

- 2.HTTP API - 스토어

  - PUT 기반 등록
  - 예) 정적 컨텐츠 관리, 원격 파일 관리

- 3.HTML FORM 사용
  - 웹 페이지 회원 관리
  - GET, POST만 지원

> 들어가기 전에

POST와 PUT은 둘다 리소스를 생성 할수 있다. 이 둘의 차이를 구별하려고 하면서 아래 내용을 보도록 하자.

## 2.1 회원 관리 시스템

### 2.1.1 API 설계 - POST 기반 등록

- 회원 목록 /members -> GET

- 회원 등록 /members -> POST

- 회원 조회 /members/{id} -> GET

- 회원 수정 /members/{id} -> PATCH, PUT, POST

- 회원 삭제 /members/{id} -> DELETE

> 회원 목록

목록에 정렬조건을 주고싶다면 쿼리파라미터를 주면 된다.

> 회원 등록

```
POST /members HTTP/1.1
Content-Type: application/json
{
 "username": "young",
 "age": 20
}
```

004메서드.md 에서 배운 내용이다. /members 만해도 서버에서 리소스식별자를 생성하여 회원을 등록해 준다.

> 회원 수정

- PATCH 가 부분 수정이므로 가장 좋다.

- PUT은 완전 대체가 일어난다. 그러므로 수정된 부분과 수정되지 않은 전체 부분을 포함하는 메시지가 존재해야 사용가능하다.

  - ex) 게시판의 게시글 수정 : 부분 수정하기 보다는 게시글 내용 전체를 가져와서 일부를 수정하고 게시글 내용 전체를 보내 덮어쓴다.

- Post 이것도 저것도 애매할떄 사용한다.

### 2.1.2 POST를 이용한 신규 자원 등록 특징

- 클라이언트는 등록될 리소스의 URI를 모른다.
  - 회원 등록 /members -> POST
  - POST /members
- 서버가 새로 등록된 신규 리소스 URI를 생성해준다.
  - 서버의 응답메시지

```
HTTP/1.1 201 Created
Location: /members/100
```

- 컬렉션(Collection)

  - 서버가 관리하는 리소스 디렉토리

  - 서버가 리소스의 URI를 생성하고 관리

  - 여기서 컬렉션은 /members 를 의미한다.

이러한 클라이언트와 서버의 형식을 컬렉션이라고 부르기로 정했다.

## 2.2 파일 관리 시스템

### 2.2.1 API 설계 - PUT 기반 등록

- 파일 목록 /files -> GET

- 파일 조회 /files/{filename} -> GET

- 파일 등록 /files/{filename} -> PUT

- 파일 삭제 /files/{filename} -> DELETE

- 파일 대량 등록 /files -> POST

files를 폴더디렉토리라고 생각해보자.

> 회원 등록

파일의 경우에는 PUT으로 설정해주어야 딱 알맞게 만들어진다.(덮어쓰기 기능)

> 파일 대량 등록

POST 대신 PUT이 파일을 등록해주기 때문에 POST는 원하는 기능을 만들어줄수 있다.
여기에선 대량 파일 입력을 했다.

### 2.2.2 PUT를 이용한 신규자원 등록 특징

- 클라이언트가 리소스 URI를 알고 있어야 한다.

  - 파일 등록 /files/{filename} -> PUT
  - PUT /files/star.jpg HTTP/1.1 (클라이언트의 요청 메세지)

- 클라이언트가 직접 리소스의 URI를 지정한다.

- 스토어(Store)

  - 클라이언트가 관리하는 리소스 저장소

  - 클라이언트가 리소스의 URI를 알고 관리

  - 여기서 스토어는 /files

이런 스타일의 관리를 스토어라고한다.

> 정리

POST와 PUT 를 이용한 신규자원 등록은 주로 POST 신규자원 등록을 사용하고, PUT은 특정 경우에만 사용한다.

## 2.3 HTML FORM 사용

- HTML FORM은 GET, POST만 지원

- AJAX 같은 기술을 사용해서 해결 가능 -> 회원 API 참고

- 여기서는 순수 HTML, HTML FORM 이야기

- **GET, POST만 지원하므로 제약이 있음**

### 2.3.1 FORM 형식 설계

- 회원 목록 /members -> GET

- 회원 등록 폼 /members/new -> GET

- 회원 등록 /members/new, /members -> POST

- 회원 조회 /members/{id} -> GET

- 회원 수정 폼 /members/{id}/edit -> GET

- 회원 수정 /members/{id}/edit, /members/{id} -> POST

- 회원 삭제 /members/{id}/delete -> POST

> 회원 등록 폼 - 회원 등록

- 참고 그림
<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/http5_6.png" >
</p>

회원 등록 폼에서 값을 입력하고 저장버튼을 누른다.
(그림에서의 전송버튼, form tag의 submit)

회원등록 /members/new, /members 둘중 하나를 선택하면 된다.
(그림에서의 /save)

개인적으로는 members/new 방식을 선호한다. 나중에 문제상황이 발생했을때 validation 할수 있다.(회원등록 -> 회원등록 폼)

> 회원 수정

- 수정도 마찬가지로 수정 버튼을 눌렀을때, 수정 폼과 수정 url을 맞추는 것을 선호

> 회원삭제

HTML FORM 에서는 HTTP의 DELETE 메서드를 쓸수가 없다. 대신 /delete라는 control URI를 사용하여 문제를 해결할 수 있다.

> 컨트롤 URI

- 컨트롤 URI
  - GET, POST만 지원하므로 제약이 있음
  - 이런 제약을 해결하기 위해 **동사**로 된 리소스 경로 사용(행위 역할이므로)
  - POST의 /new, /edit, /delete가 컨트롤 URI
  - HTTP 메서드로 해결하기 애매한 경우 사용(HTTP API 포함)

이상적으로 URI 설계 원칙에 따라 한종류의 리소스와 HTTP 메서드만 사용하는것이 좋지만, 설계상 제약이 발생한다. 그래서 실무에서는 자주 쓴다.

그렇다고 컨트롤 URI를 남발해서는 안된다. 최대한 URI(리소스) 설계 원칙 개념에 따라 설계를 하고, 메서드들로 해결이 안될때 컨트롤 URI를 사용하면 식으로 하자.

## 2.4 정리

- HTTP API - 컬렉션
  - POST 기반 등록
  - 서버가 리소스 URI 결정
- HTTP API - 스토어
  - PUT 기반 등록
  - 클라이언트가 리소스 URI 결정
- HTML FORM 사용
  - 순수 HTML + HTML form 사용
  - GET, POST만 지원

> 참고하면 좋은 URI 설계 개념

https://restfulapi.net/resource-naming/

URI 의 동작 방식들에 대해서 어느정도는 이해 할수 있었다. 그러나 확실한 개념의 정의와 의미를 알기에는 모호할 수 있다. 위 링크의 내용은 정답은 아니나, 여러 사람들에 경험에 비추어 보았을떄, 이러한 용어와 개념이 가장 좋았다고 정리한 문서이다.

### **문서(document)**

- 단일 개념(파일 하나, 객체 인스턴스, 데이터베이스 row)
- 예) /members/100, /files/star.jpg

### **컬렉션(collection)**

- 서버가 관리하는 리소스 디렉터리
- 서버가 리소스의 URI를 생성하고 관리
- 예) /members

### **스토어(store)**

- 클라이언트가 관리하는 자원 저장소
- 클라이언트가 리소스의 URI를 알고 관리
- 예) /files

### **컨트롤러(controller), 컨트롤 URI**

- 문서, 컬렉션, 스토어로 해결하기 어려운 추가 프로세스 실행
- 동사를 직접 사용
- 예) /members/{id}/delete

> 컨트롤러의 탄생

restful 문서나 자료를 보면 리소스 URI, 메서드들로만 설계한것이 가장 이상적으고 좋다고 말한다. 하지만 실무에서는 이것만으로는 설계를 할수 없다.
그리하여 컨트롤러가 탄생했다.

> 실무에서는 언제 컨트롤러를 사용해야 할까?

최대한 리소스만을 가지고 설계한다.(회원일 경우 /members/{id})

이 기존 리소스 개념만으로 해결 할수가 없을때, 컨트롤 URI를 넣어서 해결해보자.

위의 링크에는 좋은 practice들이 많으니 해보도록 하자.
