Decorator Pattern
===================


자바의 입출력 스트림은 데코레이터 패턴을 사용한다.

데코레이터 패턴이란?
- 실제 입출력기능을 가진 객체(컴포넌트)와 그 외 다양한 기능을 제공하는 데코레이터(보조스트림)을 사용하여 다양한 입출력 기능을 구현한다.
- 상속보다 유연한 확장성을 가진다.
- 지속적인 서비스의 증가와 제거가 용이하다.
- 즉, 기본 기능에 추가할 수 있는 기능의 종류가 많은 경우에 각 추가 기능을 Decorator 클래스로 정의한 후 필요한 Decorator 객체를 조합함으로써 추가 기능의 조합을 설계하는 방식이다.

데코레이터 패턴 구조
![데코레이터 구조](https://user-images.githubusercontent.com/43127088/98900918-1e124280-24f6-11eb-8515-a5cffc919eb3.jpg)

* 역할이 수행하는 작업
    * ConcreteComponent 실질적으로 기반이 되는 클래스(입,출력 관련) / Decorator 기능을 제공해주는 클래스
    * 위 2개의 클래스는 같은 클래스(Component)로부터 상속을 받는다.
    * Decorator는 혼자 돌아갈 수가 없고 항상 다른 Component를 가지고 있다. Component는 또 다른 Decorator일 수도 있고, 기반 클래스 일수도 있다.
    * ConcreteDecoratorA, ConcreteDecoratorB는 Decorator의 하위 클래스로 기본 기능에 추가되는 개별적인 기능을 뜻한다.
    * Decorator의 종류는 여러가지 일 수도 있기 때문에, 이 기능들이 다 보조 기능 일 수도 있다. 본인이 보조적으로 하는 기능, 원래 해야하는 기능 등

* 위 그림만 보고는 이해하기가 힘들다. 데코레이터 패턴을 활용하여 커피 예제를 만들어보며 데코레이터 패턴을 이해해보도록 하자.
    * 아메리카노
    * 라떼 = 아메리카노 + 우유
    * 모카커피 = 아메리카노 + 우유 + 모카시럽
    * Whipping cream 모카 커피 = 아메리카노 + 우유 + 모카시럽 + whipping cream

데코레이터를 어떻게 붓냐에 따라서 각각 다른 커피가 만들어진다.
아메리카노에 우유를 넣으면 라떼, 모카시럽까지 섞으면 모카커피가 된다.

```
package DesignPattern.decorator;
// Coffee.java
public abstract class Coffee {

    public abstract void brewing();
}
```

```
package DesignPattern.decorator;
// Decorator.java
public abstract class Decorator extends Coffee{

    Coffee coffee;
    public Decorator(Coffee coffee) {
        this.coffee = coffee;
    }

    // 만약 fileInput Class라면 read하는 기능
    @Override
    public void brewing() {
        coffee.brewing();
    }

}

```

```
package DesignPattern.decorator;
// EtiopiaAmerivano.java
public class EtiopiaAmericano extends Coffee{

    // 오리지널 컴포넌트
    @Override
    public void brewing() {
        System.out.print("EtiopiaAmericano");
    }
}
```

```
package DesignPattern.decorator;
// KenyAmericano.java
public class KenyaAmericano extends Coffee{

    @Override
    public void brewing() {
        System.out.print("KenyaAmericano");

    }
}
```

```
package DesignPattern.decorator;
// Latte.java
public class Latte extends Decorator{

   public Latte(Coffee coffee) {
        super(coffee);
    }

    // 제조 (상위클래스의 제조하는 법을 한 번 호출한후에, 커피를 만든다.
    public void brewing() {
       super.brewing();
       System.out.print(" Adding Milk");
    }
}
```

```
package DesignPattern.decorator;
// Mocha.java
public class Mocha extends Decorator{

    public Mocha(Coffee coffee) {
        super(coffee);
    }

    // 제조 (상위클래스의 제조하는 법을 한 번 호출한후에, 커피를 만든다.
    public void brewing() {
        super.brewing();
        System.out.print(" Adding Mocha Syrup");
    }
}
```

```
package DesignPattern.decorator;
// CoffeeTest.java
public class CoffeeTest {

    public static void main(String[] args) {
        Coffee americano = new KenyaAmericano();
        americano.brewing();
        System.out.println();

        Coffee KenyaLatte  = new Latte(new KenyaAmericano()); // 장식자
        KenyaLatte.brewing();
        System.out.println();

        // 장식자가 생성자에서 또다른 데코레이터를 받을 수 있고, 일반 컴포넌트를 받을 수도 있고
        // 메인 안에있는건 일반 컴포넌트여야 한다. 그래야 읽고 쓸 수 있으니깐
        Coffee KenyaMocha = new Mocha(new Latte((new KenyaAmericano())));
        KenyaMocha.brewing();
        System.out.println();

        Coffee etiopiaMocha = new Mocha(new Latte(new EtiopiaAmericano()));
        etiopiaMocha.brewing();

    }
}
```

```
// 출력결과  
KenyaAmericano
KenyaAmericano Adding Milk
KenyaAmericano Adding Milk Adding Mocha Syrup
EtiopiaAmericano Adding Milk Adding Mocha Syrup   
```

