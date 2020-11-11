### 자바에서 가장 많이 사용하는 String 클래스

대부분의 회사에서 가장 자주 사용되고 생성되는 객체는 String과 관련된 객체이다. 

여러 회사에서 String과 관련된 객체의 사용 빈도를 조사해본 결과  몇 백 개의 객체중에서 항상 TOP5를 차지한다고 한다.

이와같은 조사 결과가 의미하는 바는, **String**은 취업 준비를 할 때 에도, 개발자가 되어 실무에 투입될 때도 String 클래스에 대해서 좀 더 확실하게 공부할 필요가 있다는 뜻이다.



```java
public final class String extneds Object implements Serializable, Comparable<String>, CharSequence
```

- public final로 선언되었다. 'public'은 누구나 다 사용할 수 있는 클래스 이다. 

  여기서 final로 선언되어있으면 무엇이 다를까?

  - class에 final 키워드가 붙어있다면 **상속을 금지(확장 금지)** 라는 의미이다.
  - 다시 말해서 String 클래스는 자식 클래스를 양산할 수 없다. 그냥 있는 그대로 사용해야 한다.

- String은 Object 클래스를 상속(확장) 했다. 즉, 모든 클래스의 부모 클래스는 Object 클래스 이므로 따로 확장한 클래스는 없다는 의미이다.

- 다음으로 implements 부분을 살펴보자.

  - ```java
    implements Serializable, Comparable<String>, CharSequence
    ```

  -  String은 Serializable, Comparable, charSequence 라는 인터페이스를 구현한 클래스다.

    



#### 1. 객체의 널 체크는 반드시 필요하다.

사실 String 뿐만 아니라 모든 객체를 처리할 때에는 null 체크를 반드시 해야만 한다. 

어떤 참조 자료형도 널이 될 수 있으며, **객체가 널 이라는 것은 객체가 아무런 초기화가 되어 있지 않으며, 클래스에 선언되어 있는 어떤 메소드도 사용할 수 없다는 것을 의미한다.**

```java
public class StringNull {
    public static void main(String [] args){
        StringNull sample = new StringNull();
        sample.nullCheck(null);
    }
    
    public boolean nullCheck(String text) {
        int textLength = text.length();
        System.out.println(textLength);
        if(text == null) return true;
        else return false;
    }
    
}
```

- 바로 위의 코드는 정상적으로 컴파일은 정상적으로 되지만,  NullPointerException이 발생한다. 앞서 설명한대로 **매개변수**로 받은 text는 null인데,  null인 객체의 메소드에 접근(length()) 했기 때문이다.



- 이제 NullPointerException이 발생하지 않도록 코드를 변경해보자.

```java
public boolean nullCheck2(String text) {
    if(text==null) {
        return true;
    }else {
        int textLength = text.length();
        System.out.println(textLength);
        return false;
    }
}
```

- 이런식으로 null 체크를 하게되면 예상치 못한 NullPointerException이 발생하지 않는다.
- null체크는 아무리 강조해도 지나치지 않는 것이므로, 항상 체크하는 습관을 들여야 한다.





#### 2. 문자열이 동일한지 비교하는 클래스

String 클래스에서 제공하는 문자열이 같은지 비교하는 메소드들은 매우 많다.

| 리턴 타입 | 메소드 이름 및 매개 변수            |
| --------- | ----------------------------------- |
| boolean   | equals(Object anObject)             |
| boolean   | equalsIgnoreCase(String anotherStr) |
| int       | compareTo(String anotherStr)        |
| int       | compareToIgnoreCase(String str)     |
| boolean   | contentEquals(CharSequence cs)      |
| boolean   | contentEquals(StringBuffer sb)      |

메소드의 이름대로 분류하면 **equals**로 시작하는 메소드, **compareTo**로 시작하는 메소드, **contentEquals** 로 총 세 가지로 분류할 수 있다. 이름들은 상이하지만, 이 모든 메소드들은 **매개 변수로 넘어온 값과 String 객체가 같은지를 비교하기 위한 메소드다.** 

IgnoreCase는 말 그대로 '대소문자 무시'를 뜻한다.



위 메소드들 중 가장 많이 사용하는 equals 메소드를 살펴보자.  

- 먼저아래 메소드를 보고 어떤 결과가 나올지 생각해보자.

```java
public void checkCompare() {
    String s1 = "Cat";
    String s2 = "Cat";
    if(s1==s2) {
        System.out.println("s1==s2 result is same");
    }else { 
    	System.out.println("s1==s2 result is different.");
    }
    if(s1.equals("Cat")) {
        System.out.println("s1.equals(s2) result is same");
    }
}
```

결과부터 보자면 아래와 같다.

```
s1==s2 result is same.
s1.equals(s2) result is same
```

일반적으로 생각하기에는 두 번째 if 문만 통과할 것이라고 생각할 수 있다. 그 이유는 객체는 반드시 equals() 메서드로 비교해야 한다고 배웠기 때문이다. 올바르게 배운 것이 맞다. 객체는 반드시 equals() 메소드로 비교해야 한다.

하지만, 여기서 첫 번째 if문이 통과한 이유는 바로 자바에 **String Constant Pool** 이라는 것이 존재하기 때문이다. **String Constant Pool** 에 대해서 간단하게 이야기 하면, 자바에서는 객체들을 재사용 하기 위해 **String Constant Pool**  이라는 것이 만들어져 있고, String의 경우 동일한 값을 갖는 객체가 있으면 이미 존재하는(만든) 객체를 재사용한다. 따라서 s1과 s2의 객체는 실제로 같은 객체가 되는것이다.

- 만약에 첫 번째 if문의 결과를 다르게 출력하고 싶다면 아래 코드처럼 String 객체를 직접 생성하면 된다

  - ```java
    String s2 = new String("Cat");
    ```

  - 이렇게 String 객체를 생성하면 값이 같은 String 객체를 생성한다고 하더라도 Constant Pool의 값을 재활용하지 않고 별도의 객체를 생성하게 된다.
  
- 아래 그림을 통해 String Constant pool에 대해 정확히 이해할 수 있을 것이다.

  ![StringPooq](https://user-images.githubusercontent.com/39195377/98771141-1b94e780-2427-11eb-8657-8f65b1b9dc94.PNG)



### 3. String은 불변(immutable)의 객체이다.



**String은 최초에 한 번 생성되면 절대로 그 값이 변하지 않는다.**

```java
String str = "KH";
str = "KI HYUK";
```

위와 같이 str 이라는 String 객체를 생성한 이후에, "KH"를 "KI HYUK"로 바꾼다고 해도 실제 내부적으로는 String 객체의 값이 변경된것이 아닌, 새로운 String 객체가 생성된 것이다. 

즉, 아래 그림처럼 heap 영역을 살펴보면 처음에는 str이 "KH"를 참조하다가,  "KI HYUK" 객체를 참조하도록 재할당했을 뿐이다. 따라서 "KH"라는 단어를 갖고있는 객체는 쓰레기값(Garbage Collection)의 대상이 된다.

![String](https://user-images.githubusercontent.com/39195377/98771138-1a63ba80-2427-11eb-86a2-bddec37e2f06.PNG)



 #### 그렇다면, String은 왜 불변객체로 설계되었을까?

이 글의 맨 앞에서 소개했듯이, String은 자바에서 가장 많이 사용되는 객체이다. 그렇다는 말은 String타입의 객체들이 가장 많은 메모리를 차지한다는 뜻으로 해석할 수 있다.

결론부터 말하자면 **String 객체는 재사용 될 가능성이 높기때문에 같은 값일 경우, 어플리케이션 당 하나의 String 객체만을 생성해두어 JVM의 힙(heap)을 절약하기 위함이다.**



###### 1. String 객체의 캐싱

- 예를들어, '축구'와 관련된 웹사이트가 Java 언어로 만들어졌다고 가정해보자. 여기서 "축구" 라는 문자열이 아마도 굉장히 많이 쓰일것이다. 만약에 이 사이트가 세계적으로 유명한 사이트라면, 1초에 수백, 수천만의 HTTP request 요청을 받게 될 것이다.

- "축구"라는 값을 가지는 문자열 객체를 사용자의 요청을 처리할때마다 계속해서 한개씩 생성한다면, 힙(heap)에는 "축구"라는 값을 가진 문자열 객체가 사용자의 요청 만큼 생성되어 있을 것이다.

- 조금 더 작은 예로, 우리가 "Hello" 라는 문자열을1000번 출력한다고 가정해보자.

  ```java
  for(int i=0; i<1000; i++){
      String str = "Hello";
      System.out.println(str);
  }
  ```

  위 코드는 "Hello" 라는 값을 갖는 1000개의 String 객체를 생성하게 된다. 그러나 String의 immutable(불변)한 성질 덕분에 실제로는 "Hello"라는 문자열 객체는 단 하나만 생성된다.

  다만, "Hello"라는 참조값을 갖는 참조변수인 str변수 자체는 스택상에서 1000번 생성되었다가 사라질 뿐이다.

- 이러한 원리는 앞에서 설명한 **String Constant Pool**  이라는 특별한 공간에 의해 성립한다.

  동일한 문자열이 **String Constant Pool** 에 존재한다면, 새로 객체를 생성하지 않고 **String Constant Pool** 에 있는 객체를 사용하는 것이다.

- 결론적으로, 객체의 캐싱으로 인해 [메모리 절약]과 자주 쓰이는 값을 CPU와 가까운 곳에 위치시킴으로써 [속도 향상] 의 효과를 얻을 수 있다.

##### 2. 보안 기능

- **String이 불변이 아니라면 보안상의 문제를 야기할 수 있다.** 
  - 예를 들어, DB의 username과 password 라던가, 소켓 통신에서 host와 port에 대한 정보가 String으로 다루어지기 때문에 String이 불변이 아니라면, 해커의 공격으로부터 값이 변경되는 될 수 있다.
  - 네트워크 연결시 포트,파일 경로, db 연결에 필요한 URL도 모두 String으로 이루어져 있다. 그런데 이러한 String이 가변적이라면 누군가가 고의로든 실수로든 a를 b로 바꿔버리면 심각한 문제를 초래할 수 있다.

##### 3. 안전성(Thread-safe)

- String 객체가 변경될 수 없다는것은 여러 쓰레드에서 동시에 특정 String 객체를 참조하더라도 안전하다는 뜻이다.
  - 만약, 여러 어플리케이션에서 특정 String 객체를 참조하고 있을때, 그 값은 절대 변하지 않으므로 안전하다고 할 수 있다.
  - 앞에서 설명했듯이, 만약 String str= "KH"에서 str = "KI HYUK"로 새로운 값을 할당한다고 해도, String 객체의 값이 변경된 것이 아니다.





#### immutable한 String을 보완하기 위한 클래스, StringBuffer와 StringBuilder의 차이점

String은 불변의 객체라고 했다. 분명 String이 불변으로 설계된대는 충분한 이유가 있고, 여러가지 장점이 있다. 하지만 String이 불변으로 설계되어서 생기는 단점도 있다.

바로 GC(Garbage Collection) 이다.  기존 문자열을 수정하게 되면 그 전에 사용하던 String 객체는 GC의 대상이 된다.

- 동기화란 : A스레드와 B스레드가 한 객체를 작업중일 때, A가 값을 바꿔버리면 B가 엉뚱한 값으로 작업을 시도할 수 있다. 여러 스레드가 한 자원을 사용하려고 할 때 다른 스레드의 접근을 막는 것을 동기화라 한다. 데이터의 무결성을 보장해준다.
  다른 스레드의 접근을 막는 동기화를 지원하니, 여러 스레드(멀티 스레드)가 작업하기에 매우 안전한 환경이 만들어진다.
  그래서 동기화를 지원한다는 말은 멀티스레드 환경을 지원한다는 말과 같다고 볼 수 있다.

이러한 단점을 보완하기 위한 **StringBuffer**와 **StringBuilder**가 있다. 

### 1. StringBuffer

- Synchronized- 동기화를 지원한다.

  <u>동기화</u>를 지원한다는 것은 멀티스레드 환경을 지원한다는 것이다. 즉, **멀티스레드 환경에서 안전**하다는 뜻이다. (=Thread safe)

  - <u>동기화</u> : A스레드와 B스레드가 한 객체를 작업중일 때, A가 값을 바꿔버리면 B가 엉뚱한 값으로 작업을 시도할 수 있다. 여러 스레드가 한 자원을 사용하려고 할 때 다른 스레드의 접근을 막는 것을 동기화라 한다. 데이터의 무결성을 보장해준다. 다른 스레드의 접근을 막는 동기화를 지원하니, 여러 스레드(멀티 스레드)가 작업하기에 매우 안전한 환경이 만들어진다. 그래서 동기화를 지원한다는 말은 멀티스레드 환경을 지원한다는 말과 같다고 볼 수 있다.

### 2. StringBuilder

- non-Synchronized -동기화를 지원하지 않는다.

  동기화를 지원하지 않기 때문에 **단일 스레드 환경에서 사용**해야 한다. 

  또한 StringBuilder는 동기화를  고려하지 않기때문에 단일 쓰레드 환경에서 사용시 StringBuffer에 비해 **속도가 빠르다**.





#### StringBuffer와 StringBuilder 정리

- StringBuffer와 StringBuilder의 차이는 **'Thread safe 하다'** 와 **'Thread safe 하지 않다'** 는 것이다. 

- 결론적으로, StringBuffer가 StringBuilder보다 더 안전하다는 것만 기억해두자. 

- 다만 속도는 StringBuilder가 더 빠르다. ~~단순하게, 안전에는 대가가 따른다고 외우면 편하다.~~





### 결론

- String 클래스는 자바에서 가장 많이 사용되는 클래스 중 하나다. 따라서 String에 대해 깊게 공부할 필요가 있다.

- String 클래스를 잘 사용해야만 메모리를 효율적으로 사용할 수 있다

- StringBuffer와 StringBuilder를 상황에 맞게 잘 사용하자

  

  

------

#### StringBuffer와 StringBuilder 추가내용

글을 다 작성하고  StringBuffer와 StringBuilder의 적절한 상황에 대해서 더 알아보았다.

- StringBuffer와 StringBuilder는 성능으로만 보면 2배의 속도 차이를 보인다고 한다. 
- 하지만 참고한 사이트의 속도 실험 결과, append() 연산이 약 1억6천만번 일어날 때 약 2.6초  정도의 속도 차이를 보인다고 한다.
- 2.6초라는 수치가 무의미한지 유의미한지는 프로그램의 스펙에 따라 다르다.
- **따라서, 문자열 연산이 엄청나게 많이 일어나지 않는 환경이라면 StringBuffer를 이용해 thread-safe하게 구현하는 것이 좋다는 의견이 있었다.**



참고 자료

------

[자바의 신](http://www.yes24.com/Product/Goods/42643850) VOL1 14장 String class

