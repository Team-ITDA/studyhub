[자바 제네릭(Generics) 기초](https://dundung.tistory.com/269)에서 못 알아본 내용을 다루고자 한다. 이 글을 읽고 이해했다면 제네릭을 조금은 알고 사용한다고 말할 수 있을 것 같다.

---

## Wildcards

와일드 카드는 제네릭의 하위 개념으로 **?** 로 표시되며 알 수 없는 유형을 참조하는 데 사용한다. 어떤 타입이든 올 수 있다는 말이다.

와일드 카드를 사용한 예제를 살펴보자.

```java
public static void printList(List<?> list) {
    list.forEach(System.out::println);
}

public static void main(String[] args) {
    List<Integer> numbers = Arrays.asList(1, 2, 3);
    printList(numbers);
}
```

위 예제의 printList는 와일드 카드를 사용하고 있기에 String, Integer 등등 어떤 타입의 List가 오든 상관이 없다. 여기서 한 가지 알아둬야 하는 것이 있다.

와일드 카드를 사용하지 않고, 물음표 대신 Object를 넣는다면 main 메서드의 실행 결과는 어떨까? 아래 예제를 보자.

```java
public static void printList(List<Object> list) {
    list.forEach(System.out::println);
}

public static void main(String[] args) {
    List<Integer> numbers = Arrays.asList(1, 2, 3);
    printList(numbers);    // 컴파일 에러
}
```

위 main 메서드를 실행하면 컴파일 에러가 난다. 타입이 맞지 않기 때문이다. 제네릭은 다른 타입이 들어와서 충돌하는 것을 방지하기 위한, 타입 안전성이 주된 목적이기에 제네릭을 구현한 List에도 위와 같이 컴파일 에러가 나는 것이다.

Object 클래스는 모든 클래스의 상위 타입이지만 `List<Object>`는 `List<Integer>`, `List<String>`등 모든 타입 List의 상위 타입은 아니라는 점을 기억해야 한다.

비슷한 상황으로 CharSequence 인터페이스를 구현한 타입의 List만 메서드 파라미터로 받고 싶을 때도 아래와 같이 구현하면 컴파일 에러가 발생한다.

```java
public static void printList(List<CharSequence> list) {
    list.forEach(System.out::println);
}

public static void main(String[] args) {
    List<String> numbers = Arrays.asList("가", "나", "다");
    printList(numbers);    // 컴파일 에러: 타입이 다르다.
}
```

CharSequece 인터페이스 하위 타입의 List만을 메서드 인자로 받고 싶었지만 불가능한 것이다. 하지만 와일드 카드와 extends 키워드를 사용하면 CharSequence 인터페이스를 구현한 타입의 List만 파라미터로 받을 수 있다. 아래 예제를 보자.

```java
public static void printList(List<? extends CharSequence> list) {
    list.forEach(System.out::println);
}

public static void main(String[] args) {
    List<String> numbers = Arrays.asList("가", "나", "다");
    printList(numbers);    // 정상 작동
}
```

이처럼 와일드 카드는 제네릭과 함께 메서드 파라미터에서 유용하게 사용할 수 있다.

[자바 제네릭(Generics) 기초](https://dundung.tistory.com/269)에서 봤던 것처럼 위와 같은 구현은 아래와 같이 제한된 제네릭 메서드로도 충분히 구현할 수 있다.

```java
public static <E extends CharSequence> void printList(List<E> list) {
    list.forEach(System.out::println);
}

public static void main(String[] args) {
    List<String> numbers = Arrays.asList("가", "나", "다");
    printList(numbers);    // 정상 작동
}
```

그렇다면 와일드 카드와 제네릭 메서드의 차이는 뭘까?

위에 예제에서도 보이듯이 제네릭 메서드는 파라미터에서 직접 extends로 제한을 줄 수 없는 등 여러 차이점이 있지만 내 생각에 핵심적인 차이는 제네릭 메서드는 타입 안전성을 추구하고 와일드 카드는 편리함과 가독성을 추구한다는 점이다.

예를 들어 아래와 같은 메서드가 있다.

```java
public static <E extends Number> void something(List<E> list1, List<E> list2) {
    ...
}

public static void main(String[] args) {
    List<Integer> numbers1 = Arrays.asList(1, 2, 3);
    List<Double> numbers2 = Arrays.asList(1.0, 2.0, 3.0);
    something(numbers1, numbers2);    // 컴파일 에러
}
```

E extends Number로 파라미터 변수의 타입을 정의했지만 Number의 하위 타입일지라도 두 개의 파라미터 List에는 같은 타입이 와야만 한다.

반면에 와일드 카드를 사용한 아래의 예제 같은 경우에는 정상 작동을 한다.

```java
public static void something(List<? extends Number> list1, List<? extends Number> list2) {
    ...
}

public static void main(String[] args) {
    List<Integer> numbers1 = Arrays.asList(1, 2, 3);
    List<Double> numbers2 = Arrays.asList(1.0, 2.0, 3.0);
    something(numbers1, numbers2);    // 정상 작동
}
```

또한 눈에 띄는 문법적인 차이가 몇 가지 있다.

첫 번째 아래와 같은 Multiple Bounds(다중 제한)는 제네릭 메서드만 가능하고 와일드 카드는 안된다.

```java
<T extends Number & Comparable>
```

두 번째 와일드 카드는 하위 타입, 상위 타입을 둘 다 제한할 수 있고 제네릭 메서드는 하위 타입만을 제한할 수 있다. 즉 제네릭 메서드는 extends 키워드밖에 사용할 수 없고 와일드 카드는 아래와 같이 extends와 super 키워드를 둘 다 사용할 수 있다.

```java
public static void printList(List<? super Integer> list) {
    list.forEach(System.out::println);
}

public static void printList(List<? extends Number> list) {
    list.forEach(System.out::println);
}
```

어느 상황에 무엇을 사용하면 좋을 것인가는 상황에 따라 다르게 판단할 수 있을 것 같다. 판단에 도움을 줄 만한 의견이 [SLiPP](https://www.slipp.net/questions/202)에 있었다.

![](http://cfile4.uf.tistory.com/image/991F4D475FC6251707DF03)

추가로 Arrays 클래스에서는 제네릭 메서드와 와일드 카드를 어떻게 썼는지도 살펴보며 각각의 용도나 쓰임새에 대해 다시 한번 스스로 정리해보자.

```java
public void sort(Comparator<? super E> c) {
    ...
}

public static <T> void sort(T[] a, Comparator<? super T> c) {
    ...        
}

public static <T,U> T[] copyOfRange(U[] original, int from, int to, Class<? extends T[]> newType) {
    ...
}
```

---

## Type Erasure(타입 소거)

제네릭의 [Type Erasure(타입 소거)](https://docs.oracle.com/javase/tutorial/java/generics/erasure.html)는 컴파일 시 타입 체크를 해서 타입이 안 맞는 것을 잡아낸 후 컴파일 에러를 발생시키고 문제없이 컴파일 됐다면 런타임 중에는 타입 정보를 전부 버리는 것이다.

무슨 소린가 싶다. 아래 예제를 살펴보자.

```java
public class Car<T> {
    private final T name;

    public Car(T name) {
        this.name = name;
    }

    public T getName() {
        return name;
    }
}

public static void main(String[] args) {
    Car<String> car = new Car<>("sport-car");
    String name = car.getName();
}

```

제네릭을 구현한 Car 클래스가 있고 main 메서드에서는 String 타입으로 Car 객체를 생성했다. 문제가 없는 코드이기에 컴파일이 잘 된다.

런타임이 되면 Car 클래스의 T가 String으로 바뀔 것 같지만 사실은 아래와 같이 Object로 바뀐다.

```java
public class Car {
    private final Object name;

    public Car(Object name) {
        this.name = name;
    }

    public Object getName() {
        return name;
    }
}

```

그리고 main 메서드는 아래와 같이 컴파일된다.

```java
public static void main(String[] args) {
    Car car = new Car("sport-car");
    String name = (String) car.getName();
}

```

이 처럼 컴파일 시 타입 체크를 통과했다면 제네릭 클래스의 타입 정보를 저장하지 않고 소거시켜 버린다. 바이트 코드 어딘가에는 저장되어 있지만 필요하다면 위 main 메서드처럼 형 변환을 해놓기에 컴파일러가 특별히 그 정보를 꺼내 사용할 일은 거의 없다.

이를 검증하기 위해 아래와 같이 Reflection API로 Car 객체의 Type을 출력하는 main 메서드를 실행해보면

```java
public static void main(String[] args) throws NoSuchFieldException {
    Car<String> car = new Car<>("sport- car");
    Class<?> carNameType = car.getClass().getDeclaredField("name").getType();
    System.out.println(carNameType.getName());
}
```

**java.lang.Object**가 찍히는 것을 확인할 수 있다.

만약 제네릭에 제한(bounded)을 걸어줬다면 Object가 아닌 첫 번째 제한된 클래스로 초기화한다.

```java
public class Car<T extends CharSequence> {
    private final T name;

    public Car(T name) {
        this.name = name;
    }

    public T getName() {
        return name;
    }
}

// 런타임

public class Car {
    private final CharSequence name;

    public Car(CharSequence name) {
        this.name = name;
    }

    public CharSequence getName() {
        return name;
    }
}
```

그렇다면 이런 귀찮은 타입 소거를 자바 설계자들은 왜 적용했을까? 답은 호환성에 있다.

제네릭은 JDK 5부터 도입했다. 기존의 코드를 모두 수용하면서 제네릭을 사용하는 새로운 코드와의 호환성을 유지하기 위해 타입 소거를 하는 방향으로 간 것이다.
다시말하면 타입 안전성을 지키기 위해 컴파일시 타입 체크를 하고 호환성을 지키기 위해 런타임에는 타입 정보를 전부 소거시키는 것이다.

```java
List list = new ArrayList();

```

위처럼 컴파일 시 타입 안전성을 보장받을 수 없는 로타입을 써도 컴파일 에러를 안 내는 것도 타입 소거와 마찬가지로 호환성 때문이다.

타입 소거는 이 내용이 전부가 아니다. 타입 소거로 발생할 수 있는 문제점, 주의 사항 등 더 깊은 다른 내용이 있다. 이번에 꽤 오랜 시간 타입 소거에 대해 공부해봤는데 완벽히 이해하고 글로 쓸 정도는 여기까지이고 나머지는 아래의 참고 자료를 보며 스스로 공부하는 게 더 좋을 것 같다.  참고 자료 중에서도 특히 쟈미님 블로그 글과 조졸주님 블로그 글을 추천한다.

---

## 결론

제네릭의 장점으로 형 변환이 일어나지 않아서 성능이 효율적이라는 다른 블로그 글을 몇 개 보았다. 하지만 결국 타입 소거 후 형 변환은 일어나고 눈에 보이지만 않을 뿐이기에 성능 측면에서 효율적이라는 말이 맞나? 하는 의구심이 든다. 

결론적으로 사람이 직접 형 변환을 해주면 실수할 수 있지만 제네릭을 사용한다면 실수할 확률이 없어지고 **눈에 보이지 않는(컴파일러가 해주는) 형 변환**으로 코드가 깔끔해진다. 또한 컴파일 시 타입 체크를 통해 안전성을 보장받을 수 있다는 게 큰 장점인 것 같다.

기회가 된다면 제네릭과 타입 소거에 대해 더 심화 버전을 써 볼 생각이다.

---

### 참고 자료

- [The Basics of Java Generics -Baeldung](https://www.baeldung.com/java-generics)
- [(Java) Generic 메소드 vs Wildcard 차이점에 대해 -SLiPP](https://www.slipp.net/questions/202)
- [When to use generic methods and when to use wild-card? -stack overflow](https://stackoverflow.com/questions/18176594/when-to-use-generic-methods-and-when-to-use-wild-card)
- [Type Erasure in Java Explained -Baeldung](https://www.baeldung.com/java-type-erasure)
- [Java generics type erasure: when and what happens? -stack overflow](https://stackoverflow.com/questions/339699/java-generics-type-erasure-when-and-what-happens)
- [자바의 제네릭 타입 소거, 리스트에 관하여 (Java Generics Type Erasure, List) -쟈미님 블로그](https://jyami.tistory.com/99)
- [Generic 타입 추론시 주의할 점! (super type token 문제) - jojoldu님 블로그](https://jojoldu.tistory.com/25)
