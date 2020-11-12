# HTTP status code

프론트엔드와 백엔드는 어떤 방식으로 통신을 할 것인지부터 시작하여 리소스의 생성과 삭제는 어떻게 정의할 것인지, 프론트엔드에서 요청한 백엔드 작업의 성공/실패 여부는 어떻게 알려줄 것인지 등 많은 규칙들을 정의해야한다.

HTTP status codes는 프론트엔드와 백엔드 간의 통신을 할 때 조금 더 명확한 정의를 위해 필요한 요소 중 하나이다.

---

<br/>

**모든 HTTP 응답 코드는 5개의 클래스(분류)로 구분된다.**
상태 코드의 첫 번째 숫자는 `응답(상태)의 클래스`를 정의한다. 마지막 두 자리는 클래스나 분류 역할을 하지 않는다.

첫자리에 대한 5가지 값들은 다음과 같다
* 1xx (정보): 요청을 받았으며 프로세스를 계속한다
* 2xx (성공): 요청을 성공적으로 받았으며 인식했고 수용하였다
* 3xx (리다이렉션): 요청 완료를 위해 추가 작업 조치가 필요하다
* 4xx (클라이언트 오류): 요청의 문법이 잘못되었거나 요청을 처리할 수 없다
* 5xx (서버 오류): 서버가 명백히 유효한 요청에 대해 충족을 실패했다


## HTTP status code

|응답(상태) 대역| 응답(상태)코드 |이름 | 설명 |
|---|---|---|---|
|정보전송, 조건부 응답        | 100| Continue | 클라이언트로 부터 일부 요청을 받았으며 나머지 정보를 계속 요청함|
|                           | 101|Switching protocols | 요청자가 서버에 프로토콜 전환을 요청했으며 서버는 이를 승인하는 중이다.|
|통신 성공                   | 200|OK | 클라이언트의 요청을 성공적으로 수행함(GET) |
|                           | 201| Created | 클라이언트가 어떠한 리소스 생성을 요청, 해당 리소스가 성공적으로 생성됨(POST를 통한 리소스 생성 작업 시)|
|                           | 202| Accepted | 웹 서버가 명령 수신함(요청 접수 O, 리소스 처리 X)|
|                           | 203| Non-authoritative information | 서버가 클라이언트 요구 중 일부만 전송|
|                           | 204|No content| PUT, POST, DELETE 요청의 경우 성공은 했지만 전송할 데이터가 없는 경우 (요청 성공 O, 내용없음) |
|리다이렉션 완료                  | 300| Multiple Choice| 요청 URI에 여러 리소스가 존재|
|                           | 301| Moved permanently | 요구한 데이터를 변경된 타 URL에 요청함 / Redirect된 경우 (요청 URI가 변경되었을때)|
|                           |    | |    (응답 시 Location header에 변경된 URI를 적어 줘야 한다|
|                           | 302| Not temporarily|현재 서버가 다른 위치의 페이지로 요청에 응답하고 있지만 요청자는 향후 요청 시 원래 위치를 계속 사용해야 한다.|
|                           | 304| Not modified |컴퓨터 로컬의 캐시 정보를 이용함, 대개 gif 등은 웹 서버에 요청하지 않음 (요청 URI의 내용이 변경X)|
|클라이언트 요청 오류          | 400|  Bad Request| 클라이언트의 잘못된 요청을 처리할 수 없음 (API에서 정의되지 않은 요청 들어옴)|
|                           | 401| Unauthorized| 클라이언트가 인증되지 않은 상태에서, 인증이 필요한 페이지를 요청한 경우 (인증 오류)|
|                           | 402|  Payment required| 예약됨 |
|                           | 403| Forbidden| 접근 금지, 디렉터리 리스팅 요청 및 관리자 페이지 접근 등을 차단(권한 밖의 접근 시도)|
|                           |    |           | 403 보다는 400이나 404를 사용할 것을 권고. 403 자체가 리소스가 존재한다는 뜻이기 때문에|
|                           | 404|  Not found| 요청한 페이지 없음 (요청 URI에 대한 리소스 존재 X)|
|                           | 405| Method not allowed | 클라이언트가 허용되지 않는 http method 사용함 (API에서 정의되지 않은 메소드 호출)|
|                           | 406| Not Acceptable| 처리 불가|
|                           | 407| Proxy authentication required | 프락시 인증 요구됨|
|                           | 408| Request timeout| 요청 시간 초과|
|                           | 409| Conflict | 모순|
|                           | 410| Gone| 영구적으로 사용 금지|
|                           | 412| Precondition failed| 전체 조건 실패|
|                           | 414| Request-URI too long| 요청 URL 길이가 긴 경우임|
|                           | 429| Too Many Request| 요청 횟수 상한 초과|
|서버 오류                   | 500| Internal server error| 내부 서버 오류|
|                           | 501| Not implemented| 웹 서버가 처리할 수 없음|
|                           | 502| Bad Gateway|게이트웨이 오류|
|                           | 503| Service unnailable| 서비스 제공(이용) 불가|
|                           | 504| Gateway timeout| 게이트웨이 시간 초과|
|                           | 505| HTTP version not supported| 해당 http 버전 지원되지 않음|

---

### HTTP Request 정보

```
GET /index.html HTTP/1.1              요청 URL정보 (Mehotd /URI HTTP버젼)
user-agent: MSIE 6.0; Window NT 5.0   사용자 웹 브라우져 종류
accept: test/html; */*                요청 데이터 타입 (응답의 Content-type과 유사)
cookie:name=value                     쿠키(인증 정보)
refere: http://abc.com                경유지 URL
host: www.abc.com                     요청 도메인

```

### HTTP Response 정보

```
HTTP/1.1 200 OK                       프로토콜 버젼 및 응답코드
Server: Apache                        웹 서버 정보
Content-type: text/html               MIME 타입
Content-length : 1593                 HTTP BODY 사이즈
<html><head>.....                     HTTP BODY 컨텐츠

```

<br/>
<br/>

---

#### 참고 및 출처

https://www.iana.org/assignments/http-status-codes/http-status-codes.xhtml
https://ko.wikipedia.org/wiki/HTTP_%EC%83%81%ED%83%9C_%EC%BD%94%EB%93%9C
https://javaplant.tistory.com/18
