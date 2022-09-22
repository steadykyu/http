http와 Restful의 큰 흐름을 정리해보자. 자세한 내용은 아래 001~008을 통해 볼수 있으니 간단히 정리만 해보는 글이다.

# HTTP의 역사

HTTP(HyperText Transfer Protocol)는 인터넷 상에서 가장 많이 쓰이고 적용된 애플리케이션 프로토콜이다.

```
예시) https://www.google.com/search?q=hello&oq=hello...
```

이 설명만으로는 무슨 뜻인지 와닿지가 않는다. 좀 더 쉽게 설명해보자.

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/0_1.png" width="70%" height="70%">
</p>

매우 초장기에는 통신이 주로 Text기반(+이미지)으로 이루어져 있었다. 그러다 HyperText란 개념이 탄생했다.

> hypertext

```
a software system that links topics on the screen to related information and graphics, which are typically accessed by a point-and-click method.

직역하자면 화면에 관련된 정보(Text) 또는 이미지들 과 연결해주는 소프트웨어 시스템으로 마우스 클릭으로 서로 접근이 가능하다.
```

쉽게 말하면 링크기능이다. 이 링크 기능은 browser들을 서로 연결해 인터넷을 만들게 되었다.(www - world wide net)

결국 http는 Hypertext를 transfer(주고받는) protocol 인 것이다.

---

## http의 속성

위의 Hypertext를 주고 받기 위해서 여러 방법들이 존재하지만 주로 html을 사용하여 hypertext를 표현했다.

> html

```
태그 등을 이용하여 문서나 데이터의 구조를 명기하는 Markup language 이다.
```

그리고 이 과정 속에서 몇가지 속성들이 필요하게되었다.

예시)

```
GET / index.html
Post
Put
...
```

---

# Restful 이란?

## Restful을 알기전에

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/0_2.png" width="70%" height="70%">
</p>

- URL : 리소스가 존재하는 주소를 의미한다(Location)
- URI : 그림의 .png, .html 파일등을 구별해주는 식별자를 의미한다.(리소스들을 식별해주는 식별자)

```
https://www.google.com/search?q=hello&hl=ko
```

URI, URL 정보를 입력하면

URL의 주소부분이 DNS 서버에서 IP를 받게 되고, 이후 TCP/IP를 이용하여 Resourse들을 다룬다.

이때 다루는 방식으로 클라와 서버 사이에서 메서드(GET,PUT,POST등)가 사용된다.

---

- CRUD : create, read, update, delete 로 DB를 다루는 기술이다.

이 APP과 DB 사이에서 CRUD로 다루는 철학과 비슷하게 RESTful이 탄생하게 되었다.

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/0_3.png" width="70%" height="70%">
</p>

> RESTful 정의

```
HTTP URI(Uniform Resource Identifier)를 통해 자원(Resource)을 명시하고, HTTP Method(POST, GET, PUT, DELETE)를 통해 해당 자원에 대한 CRUD Operation을 적용하는 것을 의미한다.
```

RESTful : Representational State Transfer

즉 표현 상태를 통신하는 방법으로, 클라와 서버 사이에서 (여러 방법들이 있지만) 주로 API를 통해 리소스의 상태를 통신하는(CRUD 옵션으로) 방법론이다.

> API

application programming interface

```
API의 맥락에서 애플리케이션이라는 단어는 고유한 기능을 가진 모든 소프트웨어를 나타냅니다. 인터페이스는 두 애플리케이션 간의 서비스 계약이라고 할 수 있습니다. 이 계약은 요청과 응답을 사용하여 두 애플리케이션이 서로 통신하는 방법을 정의합니다. API 문서에는 개발자가 이러한 요청과 응답을 구성하는 방법에 대한 정보가 들어 있습니다.
```

java에 구현 되어 있는 interface를 생각해보자. 클래스 속에서 필요한 상황에 interface를 구현시켜 문제를 해결해 나간다. API는 두 어플리케이션(여기서는 클라aPP과 서버APP) 프로그램 사이에서 작동하는 interface라고 보면 된다.

출처 : https://www.youtube.com/watch?v=MbgaYDSr6R4&ab_channel=%EA%B8%B0%EC%88%A0%EB%85%B8%ED%8A%B8with%EC%95%8C%EB%A0%89
