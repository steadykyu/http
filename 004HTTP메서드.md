# 1. HTTP API 만들기

## 1.1 API 요구사항

나에게 회원 정보 관리 API를 만들라는 요구가 들어왔다고 하자.(ex 안드로이드전용, IOS전용 API 생성)

필요한 기능은 아래와 같다.

```
• 회원 목록 조회
• 회원 조회
• 회원 등록
• 회원 수정
• 회원 삭제
```

## 1.2 API URI 설계

> 잘못된 URL 설계

```
• 회원 목록 조회 /read-member-list
• 회원 조회 /read-member-by-id
• 회원 등록 /create-member
• 회원 수정 /update-member
• 회원 삭제 /delete-member
```

URI(Uniform Resource Identifier)의 가장 중요한 역할은 **리소스 식별**이다.

- 리소스의 의미는 뭘까?

  - 회원을 등록하고 수정하고 조회하는게 리소스가 아니다!
  - 예) 미네랄을 캐라(X) -> 미네랄이 리소스(O)
  - 회원이라는 개념 자체가 바로 리소스다.

- 리소스를 어떻게 식별하는게 좋을까?

- 회원을 등록하고 수정하고 조회하는 것을 모두 배제

- **회원이라는 리소스만 식별하면 된다. -> 회원 리소스를 URI에 매핑**

> 리소스 식별 , URI 계층 구조 활용

```
 회원 목록 조회 /members
• 회원 조회 /members/{id}
• 회원 등록 /members/{id}
• 회원 수정 /members/{id}
• 회원 삭제 /members/{id}
• 참고: 계층 구조상 상위를 컬렉션으로 보고 복수단어 사용 권장(member -> members)
```

그러나 리소스 식별 완료, 행위 식별 x

```
• 회원 목록 조회 /members
• 회원 조회 /members/{id} -> 어떻게 구분하지?
• 회원 등록 /members/{id} -> 어떻게 구분하지?
• 회원 수정 /members/{id} -> 어떻게 구분하지?
• 회원 삭제 /members/{id} -> 어떻게 구분하지?
```

리소스 식별 후 기능들을 보자. 같은 회원(리소스, id)에 대해서 작업이 일어나는 행위이다. 어떻게 구분할수 있을까?

제일 먼저 중요한 것은 리소스와 행위를 분리해야한다.

## 1.3 리소스와 행위(메서드)를 분리

- URI는 리소스만 식별!

- 리소스와 해당 리소스를 대상으로 하는 행위을 분리

  - 리소스: 회원

  - 행위: 조회, 등록, 삭제, 변경

- 리소스는 명사, 행위는 동사 (미네랄을 캐라)

- 행위(메서드)는 어떻게 구분?

우리는 URI를 구분하여 리소스를 구분 하자. 행위는 아래에 나올 http 메서드를 통해 하면 된다.

## 1.4 주요 메서드

- GET: 리소스 조회

- POST: 요청 데이터 처리, 주로 등록에 사용

- PUT: 리소스를 대체, 해당 리소스가 없으면 생성

- PATCH: 리소스 부분 변경

- DELETE: 리소스 삭제

> 기타 메서드

- HEAD: GET과 동일하지만 메시지 부분을 제외하고, 상태 줄과 헤더만 반환

- OPTIONS: 대상 리소스에 대한 통신 가능 옵션(메서드)을 설명(주로 CORS에서 사용)

- CONNECT: 대상 자원으로 식별되는 서버에 대한 터널을 설정

- TRACE: 대상 리소스에 대한 경로를 따라 메시지 루프백 테스트를 수행

CONNECT, TRACE 메서드는 거의 사용하지 않는다.

# 2. HTTP 메서드 - GET, POST

가장 많이 사용하는 메서드들

최근 에는 resource 라는 표현 대신 representation 으로 사용하고 있다.

## 2.2 GET

- **리소스 조회**

- 조회 뿐만 아니라 데이터 전송 기능도 2종류로 존재한다.

  - 서버에 전달하고 싶은 데이터는 query(쿼리 파라미터, 쿼리 스트링)를 통해서 전달할 수 있음.

  - 최근 spec 에서 메시지 바디를 사용해서도 데이터를 전달할 수 있지만, 지원하지 않는 곳이 많아서 권장하지않음(실무적기능 X)

## 리소스 조회

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/http4_1.png" >
</p>

/members/100 : URI에서 100번

100번 member를 조회하여 달라고 요청

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/http4_2.png" >
</p>

서버는 데이터를 조회 한후 응답 데이터(패킷, 예시는 json 형식)를 만들어 보낸다.

응답 데이터에는 HTTP버전, 포트, OK(잘 받았다는 의미)와 메타데이터 정보를 가지고 있다.

클라는 조회해온 JSON을 사용한다.

## 2.3. Post

### 2.3.1 특징

- 요청 데이터 처리(클라 -> 서버)

- **메시지 바디를 통해 서버로 요청 데이터 전달**(주기능)

- 서버는 요청 데이터를 처리

- 메시지 바디를 통해 들어온 데이터를 처리하는 모든 기능을 수행한다.

- 주로 전달된 데이터로 신규 리소스 등록, 프로세스 처리에 사용

> 리소스 등록

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/http4_3.png" >
</p>

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/http4_4.png" >
</p>

1. 요청메세지는 리소스 URI(/members)과 메시지바디 속 메세지로 구성되어 있다.

2. 서버에서는 신규 리소스 식별자를 생성(/members/100)하고 메시지를 넣어둔다.(이런 방식이 되도록 우리가 만들어야 한다.) 그리고 이에 응답한다.

3. 서버에서 htttp가 응답텍스트로하여 created가 출력되며, location이 나오게 된다.

### 2.3.2 요청 데이터를 어떻게 처리한다는 뜻일까

- 표준 스펙: POST 메서드는 대상 리소스가 리소스의 고유 한 의미 체계에 따라 요청에 포함 된 표현을 처리하도록 요청합니다. (구글 번역)

- 예를 들어 POST는 다음과 같은 기능들에 사용됩니다.

  - 1.HTML 양식에 입력 된 필드셋와 같은 데이터 블록(\<\>)을 데이터 처리 프로세스에 제공

    - 예) HTML FORM에 입력한 정보로 회원 가입, 주문 등에서 사용
    - https://developer.mozilla.org/ko/docs/Web/HTML/Element/fieldset

  - 2.게시판, 뉴스 그룹, 메일링 리스트, 블로그 또는 유사한 기사 그룹에 메시지 게시

    - 예) 게시판 글쓰기, 댓글 달기

  - 3.서버가 아직 식별하지 않은 새 리소스 생성

    - 예) 신규 주문 생성

  - 4.기존 자원에 데이터 추가
    - 예) 한 문서 끝에 내용 추가하기

- 정리: **이 리소스 URI에 POST 요청이 오면 요청 데이터를 어떻게 처리할지 리소스마다 따로 정해야 함 -> 정해진 것이 없음**

## 정리

- **1.새 리소스 생성(등록)**

  - 서버가 아직 식별하지 않은 새 리소스 생성

- **2.요청 데이터 처리**

  - 단순히 데이터를 생성하거나, 변경하는 것을 넘어서 프로세스를 처리해야 하는 경우

  - 예) 주문에서 결제완료 -> 배달시작 -> 배달완료 처럼 단순히 값 변경을 넘어 프로세스의 상태가 변경되는 경우

  - POST의 결과로 새로운 리소스가 생성되지 않을 수도 있음(새로운 리소스 order가 생성되지 않았음.)

  - 예) POST /orders/{orderId}/start-delivery (컨트롤 URI)

```

URI 설계에서 한 종류의 URI(order), resource로 하는게 좋다고 했는데 현재 다른 URI(start-delivery) 가 나왔다. 복잡한 비즈니스 로직은 한종류의 URI로 설계할 수 없다. 그리하여 컨트롤 URI를 사용하는 식으로 설계한다.
```

- **3.다른 메서드로 처리하기 애매한 경우**

  - 예) 이전에 배운듯이 GET 메서드가 메시지 body를 통해 json을 담아서 (not query) 보내고 싶은데, 서버들이 잘 호환하지 않아 GET 메서드를 무시할떄가 있다.

  - 이런 애매한 경우에 POST를 사용하면 된다.

  - Post는 사실 메시지를 담아서 전송하는 모든걸 할수 있다. 그래도 조회하거나할때는 GET을 사용해야 한다.(캐싱등의 이유가 있음)

# 3. HTTP 메서드 - PUT, PATCH, DELETE

## 3.1 PUT

폴더에 파일을 넣는 방식과 같다.

- 리소스를 대체

  - 리소스가 있으면 **완전히 대체**
  - 리소스가 없으면 생성
  - 쉽게 이야기해서 덮어버림

- **중요! 클라이언트가 리소스를 식별**
  - 클라이언트가 구체적인 리소스 위치를 알고 URI 지정

```
PUT /members/100 HTTP/1.1       // resource의 위치를 알고 있다.
Content-Type: application/json
{
 "username": "hello",
 "age": 20
}
```

- POST와 차이점

```
POST /members HTTP/1.1          // 구체적인 resource 위치를 모른다.
Content-Type: application/json
{
 "username": "young",
 "age": 20
}
```

POST는 /members 까지만 요청 메시지를 보낸다. 이때까지 어느 식별자(리소스)에 메시지가 들어가는지는 알수 없다. 서버가 생성하여 응답메세지를 보내야만 리소스 식별자를 알 수 있다.

### 3.1.1 리소스가 있는 경우와 없는경우

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/http4_5.png" >
</p>

### 3.1.2 리소스를 완전히 대체한다

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/http4_6.png" >
</p>

put은 리소스를 수정하기 보다는 덮어쓰기위해 사용하는 기능이다.

## 3.2 PATCH

리소스 안의 부분을 변경한다.

형식은 put과 같다.

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/http4_7.png" >
</p>

PATCH를 지원하지 않는 서버가 가끔 있는데, 이럴때는 POST를 사용하면 된다.

## 3.3 DELETE

리소스 제거

형식은 put과 같다.

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/http4_8.png" >
</p>

# 4. HTTP 메서드의 속성

- 안전(Safe Methods)

- 멱등(Idempotent Methods)

- 캐시가능(Cacheable Methods)

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/http4_9.png" >
</p>

## 4.1 안전

호출해도 리소스를 변경하지 않는다.

- Q: 그래도 계속 호출해서, 로그 같은게 쌓여서 장애가 발생하면요?

- A: 안전은 해당 리소스의 변화만 고려한다. 그런 부분까지 고려하지 않는다.

## 4.2 멱등(Idempotent)

- f(f(x)) = f(x)

- 한 번 호출하든 두 번 호출하든 100번 호출하든 결과가 똑같다.

- 멱등 메서드

  - GET: 한 번 조회하든, 두 번 조회하든 같은 결과가 조회된다.
  - PUT: 결과를 대체한다. 따라서 같은 요청을 여러번 해도 최종 결과는 같다.

```
만약 똑같은 파일, 같은 클라이언트 요청을 계속 보낸다고 해보자. 처음에 데이터가 없을때는 데이터가 서버에 생성되고, 이후로는 같은 내용을 덮어쓰기만 한다. 그러므로 멱등 한다.
```

- DELETE: 결과를 삭제한다. 같은 요청을 여러번 해도 삭제된 결과는 똑같다.

```
같은 Delete 를 여러번 하면, 처음에 지운 상태인 결과가 이후 Delete한 결과는 항상 같은 결과를 가지게 된다.
```

> 주의

POST: 멱등이 아니다! 두 번 호출하면 같은 결제가 중복해서 발생할 수 있다

ex) POST 배송서비스 두번 -> 문제 발생

> 활용

멱등한 메서드는 같은 요청을 계속 해도 괜찮다. 아래의 기능을 가질 수 있다.

- 자동 복구 메커니즘

- "서버가 TIMEOUT 등으로 정상 응답을 못주었을 때, 클라이언트가 같은 요청을 다시 해도 되는가?" 의 판단 근거가 된다.

> 고려조건

- Q: 재요청 중간에 다른 곳에서 리소스를 변경해버리면?
  - 사용자1: GET -> username:A, age:20
  - 사용자2: PUT -> username:A, age:30
  - 사용자1: GET -> username:A, age:30 -> 사용자2의 영향으로 바뀐 데이터 조회
- A: 멱등은 외부 요인으로 중간에 리소스가 변경되는 것 까지는 고려하지는 않는다.

## 4.3 캐시가능(Cacheable)

쉽게 말하자면 우리가 Web browser에 매우 큰 img를 다운받았다고 하자. 그러면 Web browser 내부에 이 img를 가지고 있는 local 저장소가 있는 개념이다. 이 저장소에서 꺼내기에 더 빨리 img를 보여줄 수 있다.

- 응답 결과 리소스를 캐시해서 사용해도 되는가?

- GET, HEAD, POST, PATCH 캐시가능

- 실제로는 GET, HEAD 정도만 캐시로 사용한다.(거의 GET만 사용함)

  - POST, PATCH는 본문 내용까지 캐시 키로 고려해야 하는데, 구현이 쉽지 않기에 사용하기 어렵다.
