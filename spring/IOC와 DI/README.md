Spring 프레임워크는 **자바 엔터프라이즈 애플리케이션을 개발**하기 위한 프레임워크다 <br>
즉, 기업들에게 애플리케이션을 보다 쉽게 개발할 수 있도록 서비스를 제공한다

어떻게? <br>

데이터베이스 연동이나 다른 시스템과의 연동같은 복잡한 로직을 분리시켜

개발자가 <u>비즈니스 로직</u> 에만 신경쓰도록

만들어주는 핵심 기능이 IOC와 DI 다

IOC (Inversion of Control)
===========

- 제어의 역전
- 개발자가 아닌 프로그램이 코드의 흐름을 제어하는 것

예제)
```
public class YoungGon{
  private Child child;

  public YoungGon(){
    this.child = new Jinmin();
  }

  public void carry(){
    this.child.study();
  }
}

interface Child{
  void study();
}

class Jinmin implements Child{
  @Override
  void study(){
    System.out.println("찡찡대기");
  }
}

class Minsub implements Child{
  @Override
  void study(){
    System.out.println("주말이니 쉬러가기");
  }
}
```

위 코드를 보면 <code>YoungGon</code> 클래스는
본인의 <code>Child</code> 필드를 <code>Jinmin</code> 클래스로 결정하고 있다 <br>
이는 본인의 비즈니스 로직을 작성할 뿐만 아니라 아닌 필드의 인스턴스까지 결정과 생성하고 있음을 알 수 있다

이렇게 되면 무엇이 문제인가?


예를 들어

<code>Jinmin</code> 클래스 생성자의 파라미터가 필요하게 되었다고 가정하자 <br>
그러면 <code>YoungGon</code> 클래스의 생성자도 바꾸어 주어야 한다 <br>
만약 <code>YoungGon</code> 과 같은 클래스가 100개라고 생각해보자  <br>
100개의 클래스를 모두 바꾸어야 한다 <br>
이러한 사실들로 볼때 그 시스템은 클래스 간의 결합도가 높다

따라서 이러한 결합도를 줄이고 <br>
SOLID 의 **Single Responsibility Principle** <br>
즉, **단일 책임 원칙**을 지키고자 나온 개념이 IOC 이다

*IOC 개념은 Spring에서만 국한되지 않는다*

IOC 는 제 3자에게 제어권을 주면서 자신의 비즈니스 로직에만 집중하는 개념이다

따라서 위 예제를 고쳐보면

```
public class ITDA{
  public YoungGon work(){
    return new YoungGon();
  }

  public Jinmin smoke(){
    return new Jinmin();
  }

  public Minsub gaepodong(){
    return new Jinmin();
  }
}

public class YoungGon{
  private Child child;

  public YoungGon{
    this.child = new ITDA().smoke();
  }

  //이하 동일
}
```

<code>YoungGon</code> 클래스의 <code>child</code> 필드의 인스턴스 생성을 <code>ITDA</code> 클래스에 맡김으로써 만약 <code>Jinmin</code> 클래스가 바뀌더라도 <code>YoungGon</code>
클래스는 신경을 안써도 된다

<code>ITDA</code> 클래스같은 역할을 하는 클래스를 컨테이너라 부를 수 있다

### Container
- 인스턴스의 생명주기를 관리하고, 인스턴스에게 추가적인 기능을 제공한다

- 대표적인 컨테이너 : Tomcat의 Servlet Container

--------------------

DI (Dependency Injection)
===========
- IOC를 구현하는 방법 중 하나
- 의존성 주입
- 클래스들간 의존관계를 설정한 정보를 토대로 컨테이너가 자동으로 연결하는 것

- 지켜야할 조건
  1. 코드에 의존 관계가 드러나지 않는다. 그렇기 위해 인터페이스에 의존한다
  2. 의존관계는 제 3자에 의해 결정되어야 한다
  3. 의존관계는 사용할 인스턴스의 참조는 제 3자로부터 받는다

예제)
```
public class YoungGon{
  private Child child;

  public YoungGon(){
    this.child = new Jinmin();
  }
}

interface Child{
  void study();
}

class Jinmin implements Child{
  @Override
  void study(){
  }
}

```

앞서 나왔던 예제에서 <code>YoungGon</code> 클래스는 <code>child</code> 필드의 인스턴스를 <code>Jinmin</code> 인스턴스로 결정하는 것이 보임으로써 DI가 적용이 안된 것을 알 수 있다

```
public class YoungGon{
  private Child child;

  public YoungGon(Child child){
    this.child = child;
  }
}
```

제 3자로부터 <code>child</code> 필드의 인스턴스를 받아와서 적용하기 때문에 제 3자가 인스턴스를 결정하고 

위 코드에서는 <code>Engine</code> 클래스 타입의 <code>v5</code> 필드에 인스턴스를 생성하지 않아도 우리는 <code>v5</code>를 쓸 수 있다
왜냐하면 <code>Engine</code> 클래스에 <code>Component</code> 어노테이션을 붙여주면 Spring의 컨테이너가 알아서 인스턴스를 생성해주고 클래스 사이를 자동으로 연결해준다


이렇게 Spring에서 관리하는 클래스들을 Spring에선 <code>Bean</code> 이라고 부르며

개발자가 설정한 이 <code>Bean</code> 들을 관리해주는 두 가지 컨테이너는

- <code>BeanFactory</code> : IoC/DI 기본 기능
- <code>ApplicationContext</code> : <code>BeanFactory</code> 모든 기능 포함, 일반적으로 추천

> 참고 <br>
> [[Spring] Spring의 정의와 기본 개념](https://sabarada.tistory.com/66?category=803157) <br>
> [Spring IoC/DI 컨테이너](https://www.edwith.org/boostcourse-web/lecture/20656/) <br>
> [[Spring] Spring의 핵심 기술 IoC/DI](https://sabarada.tistory.com/67) <br>
> [IoC, DI란 무엇일까](https://biggwang.github.io/2019/08/31/Spring/IoC,%20DI%EB%9E%80%20%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C/) <br>
