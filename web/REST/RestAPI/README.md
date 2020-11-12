## REST API 란

### API

Application Programming Interface

- 데이터와 기능의 집합을 제공하여 컴퓨터 프로그램간에 상호작용을 촉진하며, 서로 정보를 교환가능 하도록 하는것

### REST API의 정의

- REST 기반으로 서비스 API를 구현한 것

##### 참고)

최근 OpenAPI, 마이크로 서비스 등을 제공하는 업체 대부분은 REST API를 제공한다.

- 마이크로 서비스:

    하나의 큰 애플리케이션을 여러 개의 작은 애플리케이션으로 쪼개어 변경과 조합이 가능하도록 만든 아키텍처

- OpenAPI:

    누구나 사용할 수 있도록 공개된 API: 구글 맵, 공공 데이터 등

---

## REST API의 특징

- 사내 시스템들도 REST 기반으로 시스템을 분산해, 확장성과 재사용성을 높여 유지보수 및 운용을 편리하게  할 수 있다.
- REST는 HTTP 표준을 기반으로 구현하므로, HTTP를 지원하는 프로그램은 언어로 클라이언트, 서버를 구현할 수 있다.

즉, REST API를 제작하면 델파이 클라이언트 뿐 아니라, 자바, C#, 웹 등을 이용해 클라이언트를 제작할 수 있다.

---

## REST API 설계 기본 규칙

REST API 설계 시 가장 중요한 항목은 다음의 2가지로 요약할 수 있다.

**첫 번째,**

URI는 정보의 자원을 표현해야 한다.

**두 번째,**

자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE)로 표현한다.

#### 자세히)

> **리소스 원형**
>- Document : 객체 인스턴스나 데이터베이스 레코드와 유사한 개념 (단순히 문서 or 한 객체)
>- Collection : 서버에서 관리하는 Directory라는 구조 (문서들의 집합, 객체들의 집합)
>- Store : Client에서 관리하는 Resource 저장소

<br/>

1. **URI는 정보의 자원을 표현해야 한다.**
    - resource는 동사보다는 `명사`를, 대문자보다는 `소문자`를 사용한다.
    - resource의 `도큐먼트 이름`으로는 `단수 명사`를 사용해야 한다.
    - resource의 `컬렉션 이름`으로는 `복수 명사`를 사용해야 한다.
    - resource의 `스토어 이름`으로는 `복수 명사`를 사용해야 한다.

```basic
GET /Member/1 (x) -> GET /members/1 (o)
```

<br/>

2. **자원에 대한 행위는 HTTP Method(GET, PUT, POST, DELETE 등)로 표현한다.**

- URI에 HTTP Method가 들어가면 안된다.

```basic
GET /members/delete/1 (x) -> DELETE /members/1 (o)
```

- URI에 행위에 대한 동사 표현이 들어가면 안된다.
(즉, CRUD 기능을 나타내는 것은 URI에 사용하지 않는다.)

```basic
GET /members/show/1 (x)-> GET /members/1 (o)

GET /members/insert/2 (x) -> POST /members/2 (o)
```

- 경로 부분 중 변하는 부분은 유일한 값으로 대체한다.
(즉, :id는 하나의 특정 resource를 나타내는 고유값이다.)

```basic
student를 생성하는 route: POST /students

id = 12인 student를 삭제하는 route: DELETE /students/12
```

---

## REST API 설계 규칙 및 주의할 점

1. 슬래시 구분자(/ )는 계층 관계를 나타내는데 사용한다.

    ```basic
    http://restapi.example.com/houses/apartments
    ```

2. URI 마지막 문자로 슬래시(/ )를 포함하지 않는다.
    - URI에 포함되는 모든 글자는 리소스의 유일한 식별자로 사용되어야 한다.
    URI가 다르다는 것은 리소스가 다르다는 것이고, 역으로 리소스가 다르면 URI도 달라져야 한다.
    - URI 경로의 마지막에는 슬래시(/)를 사용하지 않는다.
    REST API는 분명한 URI를 만들어 통신을 해야 하기 때문에 혼동을 주지 않기위해서

    ```basic

    http://restapi.example.com/houses/apartments/ (X)
    ```

3. 하이픈(- )은 URI 가독성을 높이는데 사용
    - 불가피하게 긴 URI경로를 사용하게 된다면 하이픈을 사용해 가독성을 높인다.
4. 밑줄(_ )은 URI에 사용하지 않는다.
    - 밑줄은 보기 어렵거나, 밑줄 때문에 문자가 가려지기도 하므로 가독성을 위해 밑줄은 사용하지 않는다.
5. URI 경로에는 소문자가 적합하다.
    - URI 경로에 대문자 사용은 피하도록 한다.
    RFC 3986(URI 문법 형식)은 URI 스키마와 호스트를 제외하고는 대소문자를 구별하도록 규정하기 때문이다.
6. 파일확장자는 URI에 포함하지 않는다.
    - REST API에서는 메시지 바디 내용의 포맷을 나타내기 위한 파일 확장자를 URI 안에 포함시키지 않는다.
    - Accept header를 사용한다.

    ```basic
    http://restapi.example.com/members/soccer/345/photo.jpg    (X)

    http://restapi.example.com/members/soccer/345/photo.jpg    (O)
    GET / members/soccer/345/photo HTTP/1.1
    Host: [restapi.example.com](http://restapi.example.com/)
    Accept: image/jpg                                       
    ```

7. 리소스 간의 연관 관계가 있는 경우
    - /리소스명/리소스 ID/관계가 있는 다른 리소스명

```basic
GET : /users/{userid}/devices (일반적으로 소유 ‘has’의 관계를 표현할 때)
```

---

## REST API 설계 예시

|CRUD|HTTP verbs (Method)| Route|
|---|:---:|:---:|
|resource들의 목록을 표시 (SELECT)|GET| /resource|
|resource 하나의 내용을 표시 (SELECT)| GET | /resource/:id|
|resource를 생성 (CREATE)|POST|/resource|
|resource를 수정 (UPDATE)|PUT|/resource/:id|
|resource를 삭제 (DELETE)|DELETE|resource/:id|

---

## HTTP 응답 상태 코드

잘 설계된 REST API는 URI만 잘 설계된 것이 아닌 그 리소스에 대한 응답을 잘 내어주는 것까지 포함되어야 한다.
정확한 응답의 상태코드만으로도 많은 정보를 전달할 수가 있기 때문에 응답의 상태코드 값을 명확히 돌려주는 것은 생각보다 중요한 일이 될 수도 있다.

200이나 4XX관련 특정 코드 정도만 사용하고 있다면 처리 상태에 대한 좀 더 명확한 상태코드 값을 사용할 수 있기를 권장하는 바이다.

상태코드에 대해서는

[여기로!](../../HTTP응답코드/README.md))


---

### 참고)

## RESTful

### RESTful 이란

- RESTful은 일반적으로 REST라는 아키텍처를 구현하는 웹 서비스를 나타내기 위해 사용되는 용어이다.
즉, ‘REST API’를 제공하는 웹 서비스를 ‘RESTful’하다고 할 수 있다.
- RESTful은 REST를 REST답게 쓰기 위한 방법으로, 누군가가 공식적으로 발표한 것이 아니다.
즉, REST 원리를 따르는 시스템은 RESTful이란 용어로 지칭된다.

### RESTful의 목적

- 이해하기 쉽고 사용하기 쉬운 REST API를 만드는 것
- RESTful한 API를 구현하는 근본적인 목적은 성능 향상에 있는 것이 아니라 일관적인 컨벤션을 통한 API의 이해도 및 호환성을 높이는 것이 주 동기이다.
성능이 중요한 상황에서는 굳이 RESTful한 API를 구현할 필요는 없다.

### RESTful 하지 못한 경우

- CRUD 기능을 모두 POST로만 처리하는 API
- route에 resource, id 외의 정보가 들어가는 경우

    ```basic
    /students/updateName
    ```

---

참고 및 출처)

[https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html](https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html)
