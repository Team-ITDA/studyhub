# REST 란?

**"웹에 존재하는 모든 자원(이미지, 동영상, DB 자원)에 고유한 URI를 부여해 활용"**

하는 것으로, 자원을 정의하고 자원에 대한 주소를 지정하는 방법론을 의미한다.

## Rest의 정의

REST는 REpresentational State Transfer라는 용어의 약자이다.

자원을 이름(자원의 표현)으로 구분하여 해당 자원의 상태(정보)를 주고 받는 모든 것을 의미한다.
즉, 자원(resource)의 표현(representation)에 의한 상태 전달이다.

#### 자원의 표현

- 자원을 표현하기 위한 이름

  ex) DB의 학생 정보가 자원일때, 'student'를 자원의 표현으로 정한다.

<br/>

- 자원(Resource)
: 해당 소프트웨어가 관리하는 모든것

  ex) 문서, 그림, 데이터, 해당 소프트웨어 자체 등

#### 상태(정보)전달

- 데이터가 요청되어지는 시점에서 자원의 상태(정보)를 전달한다.
- JSON 혹은 XML를 통해 데이터를 주고 받는 것이 일반적이다.

<br/>

WWW(월드 와이드 웹)과 같은 분산 하이퍼 미디어 시스템을 위한 소프트웨어 개발 아키텍쳐의 한 형식

- REST는 기본적으로 웹의 기존 기술과 HTTP 프로토콜을 그대로 활용하기 때문에 웹의 장점을 최대한 활용할수 있는 아키텍쳐 스타일이다.
- 네트워크 상에서 Client와 Server 사이의 통신 방식 중 하나이다.

---

## REST가 필요한 이유

- ‘애플리케이션 분리 및 통합’
- ‘다양한 클라이언트의 등장’

최근의 서버 프로그램은 다양한 브라우저와 안드로이폰, 아이폰과 같은 모바일 디바이스에서도 통신을 할 수 있어야 한다.
이러한 멀티 플랫폼에 대한 지원을 위해 서비스 자원에 대한 아키텍처를 세우고 이용하는 방법을 모색한 결과, REST에 관심을 가지게 되었다.

---

## REST의 구체적인 개념

HTTP URI (Uniform Resource Identifier)를 통해 자원(Resource)을 명시하고,
HTTP Method(POST, GET, PUT, DELETE)를 통해 해당 자원에 대한 CRUD Operation을 적용하는 것을 의미한다.

- 즉, REST는 자원 기반의 구조(ROA, Resource Oriented Architecture) 설계의 중심에 Resource가 있고 HTTP Method를 통해 Resource를 처리하도록 설계된 아키텍쳐를 의미한다.
- 웹 사이트의 이미지, 텍스트, DB 내용 등의 모든 자원에 고유한 ID인 HTTP URI를 부여한다.


#### CRUD Operation

|Method|의미|Idempotent|
|---|---|---|
|POST|Create|No|
|GET|Select|Yes|
|PUT|Update|Yes|
|DELETE|Delete|Yes|


※ Idempotent : (멱등성) 한 번만 수행하는가, 여러 번 수행했을 때 결과가 같은가

- Create : 생성(POST), 정보를 입력하기위해 사용
    - POST를 통해 해당 URI를 요청하면 리소스를 생성한다.

<br/>

- Read : 조회(GET), 정보를 요청하기위해 사용
    - GET를 통해 해당 리소스를 조회한다. 리소스 조회하고 해당 도큐먼트에 대한 자세한 정보를 가져온다.

<br/>

- Update : 수정(PUT), 정보를 수정하기위해 사용
    - PUT를 통해 해당 리소스를 수정합니다.

<br/>

- Delete : 삭제(DELETE),  정보를 삭제하기 위해 사용
    - DELETE를 통해 리스소를 삭제합니다.


#### 참고)
REST API는 POST, GET, PUT, DELETE 이 4가지 Method는 반드시 해야되고,
다른 HTTP Method(HEAD, PATCH, OPTIONS..)를 사용하여 완성도 높은 API를 만드는 것도 좋은 방향이다.

---

## REST 구성 요소

1. 자원(Resource): URI
    - 모든 자원에 고유한 ID가 존재하고, 이 자원은 Server에 존재한다.
    - 자원을 구별하는 ID는 ‘/groups/:group_id’와 같은 HTTP URI 다.
    - Client는 URI를 이용해서 자원을 지정하고 해당 자원의 상태(정보)에 대한 조작을 Server에 요청한다.

<br/>

2. 행위(Verb): HTTP Method
    - HTTP 프로토콜의 Method를 사용한다.
    - HTTP 프로토콜은 GET, POST, PUT, DELETE 와 같은 메서드를 제공한다.

<br/>

3. 표현(Representation of Resource) : Message
    - Client가 자원의 상태(정보)에 대한 조작을 요청하면 Server는 이에 적절한 응답(Representation)을 보낸다.
    - REST에서 하나의 자원은 JSON, XML, TEXT, RSS 등 여러 형태의 Representation으로 나타내어 질 수 있다.
    - JSON 혹은 XML를 통해 데이터를 주고 받는 것이 일반적이다. (최근에는 JSON을 쓴다)

```
HTTP POST, http://myweb/users/
{
  "user" : {
    "name" : "terry"
  }
}
```

---

## REST 의 특징

1. Server-Client(서버-클라이언트 구조)
    - 자원이 있는 쪽이 Server, 자원을 요청하는 쪽이 Client가 된다.

        - REST Server
            : API를 제공하고 비즈니스 로직 처리 및 저장을 책임진다.

        - Client
            : 사용자 인증이나 context(세션, 로그인 정보) 등을 직접 관리하고 책임진다.


    - 서로 간 의존성이 줄어든다.

<br/>

2. Stateless(무상태)
    - HTTP 프로토콜은 Stateless Protocol이므로 REST 역시 무상태성을 갖는다.
        - 작업을 위한 상태정보를 따로 저장하고 관리하지 않는다

    - Client의 context를 Server에 저장하지 않는다.
        - 즉, 세션과 쿠키와 같은 context 정보를 신경쓰지 않아도 되므로 구현이 단순해진다.

    - Server는 각각의 요청을 완전히 별개의 것으로 인식하고 처리한다.
        - 각 API 서버는 Client의 요청만을 단순 처리한다.

            이전 요청이 다음 요청의 처리에 연관되어서는 안된다.

            물론 이전 요청이 DB를 수정하여 DB에 의해 바뀌는 것은 허용한다.

        - Server의 처리 방식에 일관성을 부여하고 부담이 줄어들며, 서비스의 자유도가 높아진다.

<br/>

3. Cacheable(캐시 처리 가능)
    - 웹 표준 HTTP 프로토콜을 그대로 사용하므로 웹에서 사용하는 기존의 인프라를 그대로 활용할 수 있다.
    따라서, HTTP가 가진 가장 강력한 특징 중 하나인 캐싱 기능을 적용할 수 있다.
        - HTTP 프로토콜 표준에서 사용하는 Last-Modified 태그나 E-Tag를 이용하면 캐싱 구현이 가능하다.
    - 대량의 요청을 효율적으로 처리하기 위해 캐시가 요구된다.
    - 캐시 사용을 통해 응답시간이 빨라지고 REST Server 트랜잭션이 발생하지 않기 때문에

        전체 응답시간, 성능, 서버의 자원 이용률을 향상시킬 수 있다.

<br/>

4. Layered System(계층형 구조)
    - Client는 REST API Server만 호출한다.
    - REST Server는 다중 계층으로 구성될 수 있다.
        - API Server는 순수 비즈니스 로직을 수행하고 그 앞단에 보안, 로드밸런싱, 암호화, 사용자 인증 등을 추가하여 구조상의 유연성을 줄 수 있다.
        - 또한 로드밸런싱, 공유 캐시 등을 통해 확장성과 보안성을 향상시킬 수 있다.

    - PROXY(프록시), Gateway(게이트웨이) 같은 네트워크 기반의 중간 매체를 사용할 수 있다.

<br/>

5. Code-On-Demand(optional)
    - Server로부터 스크립트를 받아서 Client에서 실행한다.
    - 반드시 충족할 필요는 없다.

<br/>

6. Uniform Interface(인터페이스 일관성)
    - URI로 지정한 Resource에 대한 조작을 통일되고 한정적인 인터페이스로 수행하는 아키텍처 스타일
    - HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용이 가능하다.
        - 특정 언어나 기술에 종속되지 않는다.

---

## REST의 장단점

### 장점

1. 사용이 쉽다.

* REST API 메시지가 의도하는 바를 명확하게 나타내므로 의도하는 바를 쉽게 파악할 수 있다.

* HTTP 프로토콜의 인프라를 그대로 사용하기 때문에 REST API 사용을 위한 별도의 인프라를 구축할 필요가 없다.

* HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용이 가능하다.

  * 플랫폼과 독립적이기 때문에, 멀티플랫폼 상황에서 서비스를 쉽게 개발할수 있다.  


2. Server-Client의 역할을 명확하게 분리한다.

* 클라이언트는 REST API를 통해 서버와 정보를 주고 받는다.

* Stalesss 이기 때문에 session(세션) 과 request(요청) 내용이 연관되지 않기 때문에 수행 컨텍스트(context)에 독립적이다.
 서버의 입장에서는 서버는 별도의 세션 정보를 유지할 필요가 없고, 단지 클라이언트에서 요청한 내용만 처리해서 응답하면된다.

* 여러가지 서비스 디자인에서 생길 수 있는 문제를 최소화한다.


3. 원하는 데이터 표현을 할 수 있다.
* REST API는 헤더부분에 URL에 대한 처리 메소드를 명시하고, 필요한 실제 데이터는 body부분에 표현하도록 분리 시켯다. (JSON, XML, YAML등 원하는 표현 언어로 사용할 수 잇다)

4. HTTP 프로토콜의 표준을 최대한 활용하여 여러 추가적인 장점을 함께 가져갈 수 있게 해준다.

5. Hypermedia API의 기본을 충실히 지키면서 범용성을 보장한다.


### 단점

1. 표준이 존재하지 않기 때문에 관리하기 어렵다.
* REST API는 API 설계 가이드일 뿐이지 명시적인 표준이 아니다.
따라서 관리하기 어렵고, 좋은 API 디자인에 대한 가이드가 쉽지 않다.
(성공적인 REST API 사례를 근거로 암묵적인 규칙이 생기기도 하는데 Google, Netflix, Amazone 등의 기업들이 REST의 표준역할을 하고 있다.)

2. HTTP Method 형태가 제한적이다.
* REST API는 HTTP 메소드를 이용하여 URI에 대한 행위를 정의한다. 따라서 간단한 수준의 메소드만 지원할 수 있다.
예를 들어 '메일을 보낸다'라는 행위는 단일 메소드로 구현하기 쉽지 않은 일이다.

3. RDBMS의 표현에 적합하지 않다.
* RDBMS는 다양한 키들이 존재한다. REST API는 리소스를 표현할때 나열하기 용의한 형태(JSON, XML...)를 사용하기 대문에 RDBMS에 표현에 적합하지 않을 수 있다.

  * 리스크를 극복하기위해, MongoDB같은 Nosql을 사용하여 JSON 형태 그대로 사용할 수 있 DB를 사용하거나,
구글의 경우 Alternative Key를 사용하거나, 링크 방식을 권장하고 있다.

4. 구형 브라우저가 아직 제대로 지원해주지 못하는 부분이 존재한다.
    - PUT, DELETE를 사용하지 못하는 점
    - pushState를 지원하지 않는 점

<br/>

---

### 용어 정리

※ [pushState](https://webisfree.com/2020-09-10/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-history-%EA%B0%9D%EC%B2%B4-%ED%8E%98%EC%9D%B4%EC%A7%80-%EA%B0%B1%EC%8B%A0-%EC%97%86%EC%9D%B4-%ED%8E%98%EC%9D%B4%EC%A7%80-%EC%A0%84%ED%99%98-pushstate)
* 페이지를 reload하지 않고 url만 변경해야 할 경우 사용한다.

<br>

※ 인프라

* 애플리케이션을 가동시키기 위해 필요한 하드웨어나 OS, 미들웨어, 네트워크 등 시스템의 기반을 말한다.

시스템 요구사항이라고하면 두 가지로 정리할 수 있다.

1. 기능적 요구사항
   * 어떤 기능을 하는지, 무엇을 할 수 있는지
2. 비기능적 요구사항
   * 시스템의 성능, 안정성, 확장성, 보안 등

---

참고 및 출처)

[https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html](https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html)

https://sanghaklee.tistory.com/57
