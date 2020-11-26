# final

final 키워드를 떠올릴 때면 그냥 상수로만 생각할 때가 종종 있다.
final을 변수, 메소드, 클래스에 선언하면 조금씩 할 수 있는 부분들이 제한된다.

Java에서 final 키워드는 여러 context에서 오로지 한 번 할당 될 수 있는 entity를 정의할 때 사용한다.
[위키피디아]

* 재 정의를 못하게 하는것
* 한 번 인스턴스가 할당되면 절대 재할당 될 수 없는 것


#### final 키워드는 총 3가지에 적용할 수 있다.

1. fianl 변수

    * 원시 타입
    * 객체 타입
    * 클래스 필드
    * 메소드 인자
2. final 메소드(Method)

3. final 클래스(Class)

---

# final 키워드 활용 및 사용법

## final 변수(필드)
> 해당 변수가 생성자나 대입 연산자를 통해 한 번만 초기화 가능함을 의미한다.

* 상수를 만들때 응용한다.

### 1. 원시타입
> 로컬 원시 변수에서 final로 선언하면 한 번 초기화된 변수는 변경할 수 없는 상수값이 된다.

상수를 정의하는 방법 : 필드 맴버에 final 키워드를 덧붙여 상수를 정의한다.

**ex)**

```java
public class FinalFieldClass {
  final int ROWS = 10; // 상수 정의, 이때 초기값을 반드시 설정
                       // 한번 선언되면 변경할 수 없다.
  void func(){
    int[] intArray = new int[ROWS]; // 상수 활용(배열의 크기)
    ROWS = 30; //컴파일 오류!, final 필드 값은 상수이므로 변경할 수 없다.
  }
}

```
* final로 상수 필드를 정의할 때, 선언 시에 초기 값을 지정해야 한다.
* 상수 필드는 한 번 정의되면 값을 변경할 수 없다.
* final 키워드만을 사용하여 상수를 만들면 FinalFieldClass의 객체들만 사용할 수 있는 상수가 된다.


### 2. 객체 타입
> 객체 변수에 final로 선언하면, 그 변수에 다른 참조 값을 지정할 수 없다.
* 원시타입과 동일하게 한 번 쓰여진 변수는 재변경 불가
  단, 객체 자체가 immutable(불변)하다는 의미는 아니다. `객체의 속성`은 `변경 가능`하다.


```java

public class FinalFieldClass {
  final Pet cat = new Pet();

  cat = new Pet(); // (x), 다른 객체로 변경할 수 없다.

  cat.setWeight(15); // 객체 속성(필드)는 변경할 수 있다.  
}

```

### 3. 메소드 인자(Arguments)
> 메소드 인자(매개변수)에 final 키워드를 붙이면, 메소드 안에서 변수값을 변경할 수 없다.


```java

public class Pet{
    int weight;

    public void setWeight(final int weight) {
        weight = 1; // (x) final로 선언된 인자는 메소드안에서 변경할 수 없음

        this.weight = weight;
    }
}

```

### 4. 맴버 변수
> 클래스의 맴버 변수에 final로 선언하면 상수값이 되거나 [write-once](https://andman.tistory.com/entry/JAVA%EC%9D%98-%ED%81%B0-%EC%9E%A5%EC%A0%90%EC%9D%B8-Write-once-run-anywhere%EC%9D%98-%EC%9D%98%EB%AF%B8) 필드로 한 번만 쓰이게 된다.

* final로 선언하면 초기화되는 시점은 생성자 메소드가 끝나기 전에 초기화 된다.
  하지만, static 이냐 / 아니냐에 따라서 초기화 시점이 달라진다.

1. static final 맴버 변수
<code> static final int x = 1</code>

    * 값과 함께 선언시
      * 정적 초기화 블록에서(static initialization block)


2. instance final 맴버 변수
<code> final int x = 1;</code>

    * 값과 함께 선언시
      * 인스턴스 초기화 블록에서 (instance initialization block)
      * 생성자 메서드에서

### 4.1 인스턴스 초기화블록 VS 정적 초기화블록

| 인스턴스 초기화 블록| 정적 초기화 블록|
|---|---|
| 객체 생성할때마다 블록이 실행됨 <br/> 실행됨 부모 생성자 이후에 실행됨 <br/> 생성자보다 먼저 실행됨| 클래스 로드시 한번만 블록이 실행됨|

**ex)**

```java

public class Pet {
    public Pet() {
        System.out.println("super construtor : Pet");
    }
}
```

```java

public class Cat extends Pet {
    final int i_value;
    static int s_value;

    // 인스턴스 초기화블록
    {
        System.out.println("instance initializer block");
        i_value = 3;
        System.out.println("i_value: " + i_value);
        System.out.println("s_value: " + s_value);
    }

    // 정적초기화블록
    static {
        System.out.println("static initializer block");
      //System.out.println("i_value: " + i_value); //static 블록에서 필드 접근 안됨
        s_value = 10;
        System.out.println("s_value: " + s_value);
    }

    public Cat() {
        System.out.println("contructor: Cat");
    }
}

```

```java
class FinalTest {
    public static void main(String[] args) {
        FinalTest finalTest = new FinalTest();
        finalTest.initializeBlockTest();
    }

    public void initializeBlockTest() {
        Cat.s_value = 5; // static 초기화 블록 호출됨
        System.out.println("s_value: " + Cat.s_value);

        System.out.println("");
        System.out.println("Cat 객체 생성1");
        Cat cat1 = new Cat();

        System.out.println("");
        System.out.println("Cat 객체 생성2");
        Cat cat2 = new Cat();
    }
}

```

`정적(static) 초기화 블록`은 클래스가 로딩되는 시점에 한번만 호출되고 static 블록 안에서 static 맴버 변수를 초기화 할 수 있다.
`인스턴스 초기화 블록`은 객체를 생성할 때마다 호출되고 자식 객체의 생성자가 호출되기 전에 그리고 부모 생성자 이후에 실행된다.

**결과)**

```
static initializer block
s_value: 10
s_value: 5

Cat 객체 생성1
super construtor : Pet
instance initializer block
i_value: 3
s_value: 5
contructor: Cat

Cat 객체 생성2
super construtor : Pet
instance initializer block
i_value: 3
s_value: 5
contructor: Cat

```

### 프로그램 전체에서 공유하여 사용할 수 있는 상수

final 키워드 + static

**ex)**

<code>public static final</code>

```java

class ShareClass {
  public static final double PI = 3.141592653589793;
}

/* ShareClass 내에서의 사용 */
double area = PI * radius * radius;
/* 다른 클래스에서의 사용 */
double area = ShareClass.PI * radius * radius;

```

<br/>

---

## fianl 메소드(method)
> 메소드 앞에 final 속성이 붙으면, 해당 메소드를 오버라이드 하거나 숨길 수 없음을 의미한다.

```java
public class SuperClass {
    protected final int finalMethod() { ... }
}

class ChildClass extends SuperClass { // SuperClass를 상속 받음
    protected int finalMethod() { ... } // 컴파일 오류
}

```
- 자식 클래스가 부모 클래스의 특정 메서드를 오버라이딩하지 못하게 하고 무조건 상속받아 사용하도록 하고자 한다면 final로 지정하면 된다.
- 구현한 코드의 변경을 원하지 않을때 사용한다.

**참고)**
```
private 메소드와 final 클래스의 모든 메소드는 명시하지 않아도 final 처럼 동작한다.

이유)

왜냐면 오버라이드가 불가능하기 때문이다.

하지만 private 메소드에 여전히 `final` 명시는 가능하다.

불필요한가?  사실 그렇다.

그래도 일단 의미는 구분된다.

- private: 자식 클래스에서 안 보인다. (오버라이드 물론 금지)
- final: 자식 클래스에서 보이지만, 오버라이드가 금지된다.

이렇게 불필요한 명시를 그렇다고 특별 취급해서 막을 필요는 없기 때문에, 컴파일러가 에러를 내거나 경고하지는 않는다.

비슷한 예로 인터페이스의 메소드에 `public`을 붙이거나, final 클래스 메소드에 `final`을 붙이는 등의 경우도 문제가 생기지는 않는다.
```

<br/>

---

## final 클래스
> final이 클래스 이름 앞에 사용되면 클래스를 상속받을 수 없음을 지정한다.
- 해당 클래스는 상속할 수 없음을 의미한다. 문자 그대로 상속 계층 구조에서 ‘마지막’ 클래스이다.
- 보안과 효율성을 얻기 위해 자바 표준 라이브러리 클래스에서 사용할 수 있는데,
  대표적으로 `java.lang.System`, `java.lang.String` 등이 있다.

```java
final class FinalClass {
	...
}

class OtherClass extends FianlClass{ //컴파일 오류
	...
}
```

- FinalClass를 상속받아 OtherClass를 만들 수 없다.

    그냥 클래스 그대로 사용해야 한다.  

- `Util 형식의 클래스`나 여러 상수 값을 모아둔 `Contants(상수) 클래스`를 `final`로 선언한다.

### 1. 상수 클래스

- 상수 값을 모아준 클래스는 굳이 상속해서 쓸 필요 없다.

```java
public final class Constants {
    public static final int SIZE = 10;
}
//public class SubConstants extends Constants {
//}
```

### 2. Util 형식의 클래스

- JDK에서 String도 final 클래스로 선언되어 있다.
  자바의 코어 라이브러리이기 때문에 side-effect가 있으면 안된다.
  다른 개발자가 상속을 해서 새로운 SubString을 만들어 라이브러리로 다른 곳에서 사용하게 되면 유지보수, 정상 실행 보장이 어려워질 수 있다.

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
}
```

**참고)**

[side-effect](https://hyeonk-lab.tistory.com/43)
```
ISO/IEC 14882는 side effect라는 용어를 다음과 같이 정의한다.

Accessing an object designated by a volatile lvalue, modifying an object, calling a library I/O function,
or calling a function that does any of those operations are all side effects,
which are changes in the state of the execution environment.

쉽게 말해서 실행 중에 어떤 객체를 접근해서 변화가 일어나는 행위(라이브러리 I/O, 객체 변경 등)이다.

```

<br/>

---
# 더 알아보기

## final, finally, finalize의 차이

### final 키워드

변수나 메서드 또는 클래스가 `변경 불가능`하도록 만든다.

- 원시(Primitive) 변수에 적용 시
  : 해당 변수의 값은 변경이 불가능하다.
- 참조(Reference) 변수에 적용 시
  : 참조 변수가 힙(heap) 내의 다른 객체를 가리키도록 변경할 수 없다.
- 메서드에 적용 시
  : 해당 메서드를 오버라이드할 수 없다.
- 클래스에 적용 시
  : 해당 클래스의 하위 클래스를 정의할 수 없다.

### finally 키워드

try/catch 블록이 종료될 때 항상 실행될 `코드 블록`을 정의하기 위해 사용한다.

- finally는 선택적으로 try 혹은 catch 블록 뒤에 정의할 때 사용한다.
- finally 블록 { }은 예외가 발생하더라도 항상 실행된다.
  단, JVM이 try 블록 실행 중에 종료되는 경우는 제외한다.
- finally 블록은 종종 뒷마무리 코드를 작성하는 데 사용된다.
- finally 블록은 try와 catch 블록 다음과, 통제권이 이전으로 다시 돌아가기 전 사이에 실행된다.

```java
public class Finally_Test {
  public static String lem() {
    System.out.println("lem");
    return "return from lem";
  }

  public static String foo() {
    int x = 0;
    int y = 5;

    try {
      System.out.println("start try");
      int b = y / x;
      System.out.println("end try");
      return "returned from try";
    } catch (Exception ex) {
      System.out.println("catch");
      return lem() + " | returned from catch";
    } finally {
      System.out.println("finally");
    }
  }

  public static void bar() {
    System.out.println("start bar");
    String v = foo();
    System.out.println(v);
    System.out.println("end bar");
  }

  // 출력
  public static void main(String[] args) {
    bar();
  }
}
```

**결과)**
```java
start bar
start try
catch
lem
finally
return from lem | returned from catch
end bar
```

### finalize() 메소드

쓰레기 수집기(GC, Garbage Collector)가 더 이상의 참조가 존재하지 않는 객체를 메모리에서 삭제하겠다고 결정하는 순간 호출된다.

- Object 클래스의 finalize() 메서드를 오버라이드해서 맞춤별 GC를 정의할 수 있다

```java
protected void finalize() throws Throwable {
/* 파일 닫기, 자원 반환 등등 */
}
```

---

# 정리)

`final`은 `마지막의` 또는 `변경될 수 없는`의 의미를 가지고 있다. 거의 모든 대상에 사용 될 수 있다.

```java
final이 사용될 수 있는 곳 - 클래스, 매소드, 맴버변수, 지역변수
```

변수에 사용되면 값을 변경할 수 없는 상수가 되며, 메소드에 사용되면 오버라이딩을 할 수 없게되고 클래스에 사용되면 자신을 확장하는 자손클래스를 정의하지 못하게 된다.

| 대상 | 의미 |
|:---:|:---:|
| 변수, 인자 |  변수 앞에 final이 붙으면, 값을 변경할 수 없는 상수가 된다. |
|메소드|  변경 될 수 없는 메소드, <br/> final로 지정된 메소드는 오버라이딩을 통해 재정의 될 수 없다. |
|클래스| 변경될수 없는 클래스, 확장(상속)될 수 없는 클래스가 된다. <br/> 그래서 final로 지정된 클래스는 다른 클래스의 조상이 될 수 없다.  |


### 주의 할점)

final 변수는 초기화 이후 값 변경이 발생하지 않도록 만든다.

다음 코드를 보면 List에 final을 선언하여 `list` 변수의 변경은 불가능하다.

하지만 list 내부에 있는 변수들은 변경이 가능하여 문자열을 계속 추가할 수 있다.

```java
final List<String> list = new ArrayList<>();
list.add("Hello");
list.add("World");
```

따라서, final 변수의 변경을 막아주지만 final 변수 내부에 갖고 있는 변수들은 변경이 가능하다는 점을 기억해야된다.


<br/>

---

**참고 및 출처)**

[https://gmlwjd9405.github.io/2018/08/06/java-final.html](https://gmlwjd9405.github.io/2018/08/06/java-final.html)  
[https://advenoh.tistory.com/13](https://advenoh.tistory.com/13)  
[https://djkeh.github.io/articles/Why-should-final-member-variables-be-conventionally-static-in-Java-kor/](https://djkeh.github.io/articles/Why-should-final-member-variables-be-conventionally-static-in-Java-kor/)  
