## Strategy Pattern (전략 패턴)

<br>

### 1. 전략 패턴이란?

>  동일 계열의 알고리즘을 정의하고, 각 알고리즘을 캡슐화하여 이들을 상호교환이 가능하도록 만든다. 알고리즘을 사용하는 클라이언트와 상관없이 독립적으로 알고리즘을 다양하게 변경할 수 있게 한다.

​	즉, 특정한 기능을 수행하는데 있어서 다양한 알고리즘이 적용될 수 있는 경우에 상황에 따라 필요한 알고리즘을 선택하여 해결할 수 있는 디자인 패턴이다.



![](https://github.com/Team-ITDA/studyhub/blob/leehj/design%20pattern/Strategy%20Pattern/strategy-pattern.png)

> **Strategy**
>
> + 사용할 행위를 추상화한 인터페이스
>
> **Concrete Strategy**
>
> + Strategy에서 명시한 행위를 구현한 클래스
>
> **Context**
>
> + 전략을 사용하는 역할
> + 필요에 따라 전략을 바꿔 사용할 수 있음

<br >

### 2. 예제

```java
class abstract Kart {
    public void drive() {
        System.out.println("열심히 운전한다.");
    }
    public abstract void attack();
}
```

차를 타고 경주를 하는 게임에 필요한 차를 구현해보자. 모든 차는 drive라는 메소드를 공통적으로 가지고 있어야 하고, 각 차마다 한 가지 공격 기능을 가지고 있다. 

```java
class ClassicKart extends Kart {
    public void attack() {
        System.out.println("미사일로 공격한다.");
    }
}

class BananaKart extends Kart {
    public void attack() {
        System.out.println("바나나 껍데기로 공격한다.");
    }
}
```

지금 상태에서새로운 차를 추가해달라는 요구사항이 생겼다. 새로운 차는 ClassicKart와 동일하게 미사일로 공격한다.

```java
class SpeedKart extends Kart {
    public void attack() {
        System.out.println("미사일로 공격한다.");
    }
    
    public void speedUp() {
        System.out.println("일정시간동안 속도가 빨라진다.");
    }
}
```

요구사항을 적용하고 나니 코드가 중복되는 문제가 발생했다. 공격 방법을 변경하자니 기존 코드를 변경해야 하므로 OPC원칙에 어긋난다.  전략 패턴을 사용해 문제를 해결해보자.

```java
interface Weapon {
    public void attack();
}

class Missile implements Weapon {
    public void attack() { System.out.println("미사일로 공격한다."); }
}

class Banana implements Weapon {
    public void attack() { System.out.println("바나나 껍데기로 공격한다."); }
}

class abstract Kart {
    private Weapon weapon;
    
    public Kart(Weapon weapon) { this.weapon = weapon; }
    
    public void setWeapon(Weapon weapon) { this.weapon = weapon; }
    
    public void drive() { System.out.println("열심히 운전한다."); }
    
    public void attack() { weapon.attack(); }
}

class ClassicKart extends Kart {
    public ClassicKart(Weapon weapon) { super(weapon); }
}

class BananaKart extends Kart {
    public BananaKart(Weapon weapon) { super(weapon); }
}

class SpeedKart extends Kart {
    public SpeedKart(Weapon weapon) { super(weapon); }
    public void speedUp() { System.out.println("일정시간동안 속도가 빨라진다."); }
}
```

공격 방법을 인터페이스로 분리하면서 코드의 중복이 사라졌다. 공격 방법을 변경하는 요구사항도 기존 코드의 수정 없이 새로운 공격 전략을 생성하고 setWeapon 메소드를 사용하여 변경해주면 된다.

<br >

#### 장점

+ 재사용이 쉽기 때문에 코드의 중복이 적다.
+ 확장과 유지보수가 쉽다.

<br >

#### 단점

+ 사용자는 각 전략이 어떻게 동작하는지 이해하고 사용해야 한다.
+ Concrete Strategy 클래스는 Strategy 인터페이스의 정보를 공유하기 때문에 필요하지 않은 정보를 갖는 경우가 발생한다.
+ 각 전략은 인터페이스를 구현하여 객체로 만들어지기 때문에 전략의 개수만큼 객체가 생성되어야 한다.

<br >

<hr >

**참고**

https://gmlwjd9405.github.io/2018/07/06/strategy-pattern.html

https://johngrib.github.io/wiki/strategy-pattern/