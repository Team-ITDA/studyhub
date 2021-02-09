## Web Server와 WAS의 차이를 알아보기 전에 Web과 Server은 무엇일까?

### Web
인터넷을 기반으로 하여 정보를 공유, 검색 할 수 있게 하는 서비스
- URL(주소)
- HTTP(통신 규칙)
- HTML(내용)

### Server
클라이언트에게 네트워크를 통해 정보나 서비스를 제공하는 컴퓨터 시스템.
- H/W : 서버가 설치되어 있는 소형, 대형 컴퓨터.
- S/W : HTML, CSS, Javascript와 같은 정적 컨텐츠를 처리하는 서버를 의미.
    - Web Server는 정적 컨텐츠를 제공할뿐 동적 컨텐츠의 처리는 할 수 없다.
    
## 이제 차이를 알아보자.

### Web Server 
클라이언트로부터 HTTP 요청을 받아 HTML 문서나 각종 리소스(Resource)를 전달하는 컴퓨터
- 클라이언트는 웹서버에게 주소(url)를 가지고 통신 규칙(http)에 맞게 요청하면 알맞은 내용(html)을 응답 받는다.
- 웹 서버는 클라이언트의 요청을 기다리고, 웹 요청(http)에 대한 데이터를 만들어서 응답. 이때 데이터는 웹에서 처리할 수 있는 html, css 이미지 등 정적인 데이터로 한정.

그런데 왜 웹 서버(html)는 동적인 컨텐츠를 다룰 수 없을까? 
- html은 프로그래밍 언어가 아니다. html은 웹사이트의 구조나 의미를 주기 위한 내용을 주기 위해 사용되는 마크업 언어이다. db에서 데이터를 가져와서 전달해 주는 기능은 할 수 없다.

## 그래서 was라는 개념이 나왔다.

### WAS (Web Application Server)란?

위에서 말한 Web Server가 Web Appplication Server로 바꼈네?

Web Application
- 웹에서 실행되는 응용 프로그램.
- 우리는 html의 한계를 application을 통해서 극복할 수가 있다.

WAS(Web Application Server)
- web Application과 server 환경을 만들어 동작시키는 기능을 제공하는 소프트웨어 프레임워크
- web Application을 실행시켜 필요한 기능을 수행하고 그 결과를 web server에 전달
- web server + web container (container : jsp, servlet를 실행시킬 수 있는 소프트웨어)
- HTML 같은 정적인 페이지에서 처리할 수 없는 비지니스 로직이나 DB 같은 동적인 컨텐츠를 제공
- 프로그램 실행 환경과 데이터베이스 접속 기능 제공

## WAS의 동작과정
![1](https://user-images.githubusercontent.com/43127088/107265910-2a238700-6a88-11eb-9a9d-99f22872b604.PNG)
`* 정적인 컨텐츠 (HTML, CSS, Image 등), 동적인 컨텐츠 (JSP, Servlet, ASP, PHP 등)`

1. Client에서 WAS로 요청을 보낸다.
2. WAS안에 있는 Web Server에서 정적 요청인지, 동적 요청인지 파악을 한다.
3. 요청이 정적 페이지 요청이면 바로 Client에게 전송한다.
4. 하지만, 동적 페이지 요청이면 Web Container에게 전송.
5. Container에서 Servlet이 실행이 되고, 동적인 데이터를 생성하여 다시 web server에게 보낸다.
6. 다시 Web Server는 Client에게 전송한다.

Was는 Web Server과 Web Containe를 둘 다 가지고 있어서 정적, 동적 데이터 처리를 다 할 수있다.

## 정리
- Web Server : 정적인 컨텐츠, 상황에 따라 변하는 정보를 처리할 수 없다.
- WAS : 정적 + 동적인 컨텐츠, 상황에 따라 변하는 정보를 처리할 수 있다.

## 그런데
### "WAS가 정적 + 동적 컨텐츠를 다루고 있는데, Web Server인 Apache 같은 것을 굳이 써야하나? Tomcat만 써도 되지 않을까?" 라는 의문이 들 수 있다.
![2](https://user-images.githubusercontent.com/43127088/107266102-66ef7e00-6a88-11eb-9473-cc61095d84af.PNG)
대규모 프로젝트를 보면 Apache와 Tomcat을 함께 사용해서 web Server와 WAS를 따로 두는 경우가 있다. 이유가 무엇일까?

### 기능을 분리하여 서버 부하 방지 
- WAS 하나만 두게 되었을 때, WAS는 DB 조회 등 페이지를 만들기 위한 다양한 로직을 처리한다. 이것만 하기도 바쁜데 정적인 데이터까지 처리하려다 보니깐 WAS가 하는 일이 너무 많다. 그래서 정적인 데이터를 처리할 Web server와 동적인 데이터를 처리할 WAS를 따로 둠으로써 기능을 분리하여 부하를 방지한다.
  
  
### 물리적으로 분리하여 보안 강화
- SSL에 대한 암복호화 처리에 Web Server를 사용한다.
- 공격에 대해 Web Server를 앞단에 두어 중요한 정보가 담긴 DB 로직까지 (WAS까지) 전파되지 못하게 한다.

### 여러 대의 WAS 연결 가능
- Load Balancing : 하나의 인터넷 서비스가 발생하는 트래픽이 많을 때 여러 대의 서버가 분산처리하여 서버의 로드율 증가, 부하량, 속도저하 등을 고려하여 적절히 분산처리하여 해결해주는 서비스
- fail over(장애 극복) : 하나의 WAS가 작동을 중지하게 됬을때, 다른 WAS들로도 돌릴수가 있으니 다른 서버로 다시 돌릴 수 있다는 개념
- fail back : 작동이 중지된 서버를 재가동시킨다.
- 대용량 웹 어플리케이션의 경우(여러 개의 서버 사용) Web Server와 WAS를 분리하여 무중단 운영을 위한 장애 극복에 쉽게 대응할 수 있다.
  ![3](https://user-images.githubusercontent.com/43127088/107266282-9ef6c100-6a88-11eb-8fb6-63403371ab99.PNG)

### 다른 종류의 WAS로 서비스 가능
- 하나의 서버에서 PHP Application과 Java Application을 함께 사용하는 경우
