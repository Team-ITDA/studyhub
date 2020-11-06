# 객체지향 5원칙 (SOLID)

<br />

## 1. SRP (단일책임의 원칙: Single Responsibility Principle)

- **하나의 클래스는 하나의 책임**을 수행하는 데 집중해야 한다.
- SRP를 적용한 클래스는 하나의 책임을 갖기 때문에 **응집도가 높고**, 다른 클래스와는 책임이 분리되어 **낮은 결합도**를 가진다.
- 하나의 책임에 집중하므로 외부의 변화에 의해 클래스가 변경되어야 하는 이유는 여러가지여서는 안된다.
- 모든 클래스가 하나의 책임에 집중하기 때문에 다른 클래스가 변경되어도 연쇄적으로 변경이 일어나지 않는다. → **사이드 이펙트가 발생할 여지가 적다.**

```java
class Animal {
	String name;
	int age;

	// DB 로직에 변경이 생기면 영향을 받음 => 단일 책임 원칙에 위반
	void saveAnimal(Animal animal) {
		// DB에 접근하여 animal 정보를 저장
	}
}
```

Animal 클래스는 동물의 정보만 담고있는 것이 아니고 데이터베이스와 관련된 로직을 담고있다. 단일 책임 원칙을 위반하지 않도록 역할을 분리해야 한다.

```java
class Animal {
	String name;
	int age;
}

class AnimalDAO {
	void saveAnimal(Animal animal) {
		// DB에 접근하여 animal 정보를 저장
	}
}
```

<br />

## 2. OCP (개방 폐쇄의 원칙: Open Close Principle)

- **확장에는 열려있고, 변경에는 닫혀있어야 한다.**
- 요구사항의 변경이나 추가사항이 발생할 때, 기존 코드의 수정은 일어나지 않고 기존 구성요소를 쉽게 확장해서 재사용이 가능해야 한다.
- **추상화**화 다형성이 OCP의 핵심 원리

```java
class MusicPlayer {
	public void play() {
		System.out.println("play melon");
	}
}
```

기존에 멜론으로 노래를 듣던 사용자가 애플뮤직을 추가로 구독하기로 했다. 위 코드에서 요구사항을 적용하려면 기존의 코드가 변경되어야 하고 확장과 재사용 또한 힘들다. 요구사항을 추가하기 전에 기존 코드를 OCP 원칙을 적용해 수정해보자.

```java
interface MusicApp {
	public void play();
}

class Melon implements MusicApp {
	public void play() {
		System.out.println("play melon");
	}
}

class MusicPlayer {
	private MusicApp musicApp;

	public MusicPlayer(MusicApp musicApp) {
		this.musicApp = musicApp;
	}

	public void play() {
		this.musicApp.play();
	}
}
```

확장과 재사용이 가능하도록 수정한 코드이다. 이전과는 달리 애플뮤직을 추가하기 위해 기존 코드는 수정할 필요가 없고 확장을 통해 추가해주면 된다.

```java
interface MusicApp {
	public void play();
}

class Melon implements MusicApp {
	//생략
}

class AppleMusic implements MusicApp {
	public void play() {
		System.out.println("play apple music");
	}
}

class MusicPlayer {
	//생략
}
```

클라이언트는 MusicApp 구현체 중 원하는 것을 MusicPlayer 생성자 매개변수로 담아 객체로 생성하고, play() 해주면 된다.

<br />

## 3. LSP (리스코브 치환의 원칙: The Liskov Substitution Principle)

- **자식 클래스는 최소한 자신의 부모 클래스에서 가능한 행위는 수행할 수 있어야 한다.**
- 프로그램에서 부모 클래스의 인스턴스 대신 자식 클래스의 인스턴스로 대체해도 프로그램의 의미가 변하지 않는다.
- 이를 만족시키는 방법은 **재정의를 하지 않는 것**이다.

```java
class Rectangle {
	private int width;
	private int height;

	public void setHeight(int height) {
		this.height = height;
	}

	public void setWidth(int width) {
		this.width = width;
	}

	public int area() {
		return this.width * this.height;
	}
}

class Square extends Rectangle {
	@Override
	public void setHeight(int value) {
		this.height = value;
		this.width = value;
	}

	@Override
	public void setWidth(int value) {
		this.height = value;
		this.width = value;
	}
}
```

Rectangle 클래스를 상속받은 Square 클래스는 setHeight와 setWidth를 Override하였다.

```java
class Test {
	static boolean checkAreaSize(Rectangle rectangle) {
		rectangle.setHeight(4);
		rectangle.setWidth(5);
		
		return rectangle.area() == 20;
	}
	
	public static void main(String[] args){
    Test.checkAreaSize(new Rectangle()); // true
    Test.checkAreaSize(new Square()); // false
  }
}
```

위의 코드는 LSP를 위반한 예시이다. 부모 클래스를 자식 클래스로 대체하였을 때 프로그램이 동일하게 기능하지 않는다. 자식 클래스는 부모 클래스의 책임을 무시하거나 재정의하지 않고 확장만 수행해야 한다.

<br />

## 4. ISP (인터페이스 분리의 원칙: Interface Segregation Principle)

- 클라이언트는 자신이 이용하지 않는 기능에는 영향을 받지 않아야 한다.
- 인터페이스의 책임을 분리하여 클라이언트가 꼭 필요한 기능만 사용할 수 있도록 한다.

```java
interface Animal {
	public void eat();
	public void sleep();
	public void fly();
}

class Tiger implements Animal {
	public void eat() { ... }
	public void sleep() { ... }
	public void fly() {} // 필요하지 않은 기능
}
```

Tiger 클래스는 불필요한 기능까지 구현해야하는 문제가 발생한다. 이러한 문제를 해결하기 위해 인터페이스의 기능을 분리한다.

```java
interface Animal {
	public void eat();
	public void sleep();
}

interface Bird {
	public void fly();
}

class Tiger implements Animal {
	public void eat() { ... }
	public void sleep() { ... }
}
```

<br />

## 5. DIP (의존성 역전의 원칙: Dependency Inversion Principle)

- **의존관계를 맺을 때, 구체적인 클래스보다 인터페이스나 추상 클래스와 관계를 맺는다는 것을 의미한다.**

- 상위 모듈은 하위 모듈의 구현에 의존해서는 안되고 하위 모듈은 상위 모듈의 추상에 의존해야 한다.

  상위 모듈은 변경이 적게 일어나는 추상화된 인터페이스나 상위 클래스, 하위 모듈은 변경이 쉬운 구체 클래스를 의미한다.

- 하위 레벨에서의 구현이 변경되더라도 상위 레벨에 영향을 주지 않는다.

- 즉, **변경에 강하며 유지보수가 쉽다**.

<br />

------

**참고**

https://medium.com/@jaeyeong951/%EA%B0%9D%EC%B2%B4%EC%A7%80%ED%96%A5-5%EC%9B%90%EC%B9%99-solid-ac7d4d660f4d

https://nesoy.github.io/articles/2018-03/LSP

https://dev-momo.tistory.com/entry/SOLID-%EC%9B%90%EC%B9%99

https://velog.io/@amobmocmo/Software-Design-DIP-Dependency-Inversion-Principle-mok20fv2y3