IOC (Inversion of Control)
===========

- 제어의 역전
- 개발자가 아닌 프로그램이 코드의 흐름을 제어하는 것

### Container
인스턴스의 생명주기를 관리하고, 인스턴스에게 추가적인 기능을 제공한다

대표적인 컨테이너 : Tomcat의 Servlet Container

--------------------

DI (Dependency Injection)
===========
- 의존성 주입
- 클래스들간 의존관계를 설정한 정보를 토대로 컨테이너가 자동으로 연결하는 것


예제)
```
class Engine{

}

class Car{
  Engine v5 = new Engine();
}
```

위 예제를 보면 <code>Car</code> 클래스에 <code>Engine</code> 인스턴스를 생성하는 코드를 개발자가 직접 작성한 것을 알 수 있으므로
DI가 적용이 안된 것을 알 수 있다

```
@Component
class Engine{

}

@Component
class Car{
  @Autowired
  Engine v5;
}
```

위 코드에서는 <code>Engine</code> 클래스 타입의 <code>v5</code> 필드에 인스턴스를 생성하지 않아도 우리는 <code>v5</code>를 쓸 수 있다
왜냐하면 <code>Engine</code> 클래스에 <code>Component</code> 어노테이션을 붙여주면 Spring의 컨테이너가 알아서 인스턴스를 생성해주고 클래스 사이를 자동으로 연결해준다


이렇게 Spring에서 관리하는 클래스들을 Spring에선 <code>Bean</code> 이라고 부르며

개발자가 설정한 이 <code>Bean</code> 들을 관리해주는 두 가지 컨테이너는

- <code>BeanFactory</code> : IoC/DI 기본 기능
- <code>ApplicationContext</code> : <code>BeanFactory</code> 모든 기능 포함, 일반적으로 추천

> 원문
(Spring IoC/DI 컨테이너)[https://www.edwith.org/boostcourse-web/lecture/20656/]
