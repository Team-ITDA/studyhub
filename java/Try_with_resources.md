# Try-with-resources

자바 라이브러리에는 close 메소드를 호출해 직접 닫아줘야하는 자원이 많다.
InputStream, OutputStream, java.sql.Connection 등이 좋은 예다.

자바를 이용해 외부 자원에 접근하는 경우 한가지 주의해야할 점은 외부자원을 사용한 뒤 제대로 자원을 닫아줘야한다는 점이다.

자원 닫기는 클라이언트가 놓치기 쉬운 부분이기 때문에 예측할 수 없는 성능 문제로 이어질 수 있기 때문이다.
<br/>
try-with-resources는 `try(...)`에서 선언된 객체들에 대해서 try가 종료될 때 자동으로 자원을 해제(종료)해주는 기능이다.

즉, 따로 finally 블록이나 모든 catch 블록에 종료 처리를 하지 않아도 된다.

try에서 선언된 객체가 `AutoCloseable`을 구현하였다면 Java는 try구문이 종료될 때 객체의 close() 메소드를 호출해 준다.

---

차이점을 알기위해 try-catch-finally를 이용한 자원해제를 살펴보자

## try-catch-finally로 자원 해제

Java7 이전에, 전통적으로 자원을 제대로 닫힘을 보장하는 수단으로 try-finally가 쓰였다. 예외가 발생하거나 메소드를 반환되는 경우를 포함해서 말이다.

try-catch-finally 구문을 통해 자원을 해제하려면 정말 귀찮았고 코드 양도 많고 매우 지저분했다.

예를 들어, 다음 코드는 try-catch-finally을 사용하여 파일을 열고 문자열을 모두 출력하는 코드이다.

```java
public static void main(String args[]) throws IOException {
    FileInputStream fis = null;
    BufferedInputStream bis = null;
    try {
        fis = new FileInputStream("file.txt");
        bis = new BufferedInputStream(fis);
        int data = -1;
        while((data = bis.read()) != -1){
            System.out.print((char) data);
        }
    } finally {
        // close resources
        if (fis != null) fis.close();
        if (bis != null) bis.close();
    }
}
```

코드를 보면 try에서 InputStream 객체를 생성하고 finally에서 close를 해주었다.

try 안의 코드를 실행하다 Exception이 발생하는 경우 모든 코드가 실행되지 않을 수 있기 때문에 finally에 close 코드를 넣어주어야 한다.

심지어 InputStream 객체가 null인지 체크해줘야 하며 close에 대한 Exception 처리도 해야 한다.

 cf) main에서 IOException를 throws한다고 명시적으로 선언했기 때문에 close에 대한 try-catch 구문을 작성하지 않았다.

이런 코드는 장황하고 보기힘들며, 이해하기 어렵고 중첩적이다.  그리고 무엇보다 개발자가 실수하기 쉽다.


##### 이러한 문제들은 JDK1.7(Java 7)부터 나온 try-with-resources로 모두 해결되었다.  

---

## Try-with-resources로 자원 해제

Java7부터 Try-with-resources 구문을 지원하고, 이것을 사용하면 자원을 쉽게 해제할 수 있다.

이 구조를 사용하려면 해당 자원이 `AutoCloseable` 인터페이스를 구현해야 된다.
단순히 void를 반환하는 close 메소드 하나만 정의된 인터페이스이다.

`자바 라이브러리`와 `서드파티 라이브러리`들의 수많은 클래스와 인터페이스가 이미 AutoCloseable을 구현하거나 확장해뒀다.

우리도 닫아야 하는 자원을 뜻하는 클래스를 작성한다면 AutoCloseable을 반드시 구현해야한다.

<br/>

다음 코드는 Try-with-resources를 사용하여 InputStream으로 파일의 문자열을 모두 출력하는 코드이다.   

실행 결과는 위의 예제와 동일하다.

```java
public static void main(String args[]) {
    try (
        FileInputStream fis = new FileInputStream("file.txt");
        BufferedInputStream bis = new BufferedInputStream(fis)
    ) {
        int data = -1;
        while ((data = bis.read()) != -1) {
            System.out.print((char) data);
        }
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

코드를 보면, `try(...)` 안에 InputStream 객체 선언 및 할당하였다. (try에 전달할 수 있는 자원은 AutoCloseable 인터페이스의 구현체로 한정된다.)

여기에서 선언한 변수들은 try {...}안에서 사용할 수 있다.

코드의 실행 위치가 try 문을 벗어나면 try-with-resources는 `try(...)` 안에서 선언된 객체의 `close()` 메소드들을 호출한다. 그래서 finally에서 `close()`를 명시적으로 호출해줄 필요가 없다.

---  

## try-with-resources로 자원 해제 설명

AutoCloeseable 인터페이스

```java
/**
 * @author Josh Bloch
 * @since 1.7
 */
public interface AutoCloseable {

    void close() throws Exception;
}
```

인터페이스 추가와 함께 소소한 문법적 추가도 이뤄졌는데 try 절에 ()가 들어갈 수 있게됐다.

```java
public static void main(String[] args) {
    try(FileInputStream fis = new FileInputStream("")){

    }catch(IOException e){

    }
}
```

try-with-resources에서 자동으로 close가 호출되는 것은 `AutoCloseable`을 구현한 객체에만 해당된다.

try(){ } 형태로 사용이 가능하며 () 안에 들어올 수 있는건 AutoCloseable 구현체뿐이다.

이런 문법을 `try-with-resources` 라고 부른다.

<br>

### 저렇게하면 무슨 이점이 있을까?

매우 직관적인 인터페이스 명칭이나 위 예제를 보고 눈치챌수도있겠지만, try catch 절이 종료되면서 자동으로 close() 메드를 호출해준다.

try-with-resources의 장점은 `코드를 짧고 간결`하게 만들어 `읽기 쉽고 유지보수가 쉬워진다`.

또한 명시적으로 close를 호출하려면 많은 if와 try-catch를 사용해야 하기 때문에 실수로 close를 빼먹는 경우가 있다.
이것을 이용하면 이런 자잘한 버그들이 발생할 가능성이 적어 진다.    

cf) 이 기능은 참 편리하고 좋은 기능인데 1.7에서 문법적 변화가 없다는 생각과함께 잘 모르는 사람들이 생각보다 많다.  자신의 개발환경이 1.7 이상이라면 유용하게 사용해보자.

---

## Try-with-resources로 close()가 호출되는 객체는?

Try-with-resources가 모든 객체의 `close()`를 호출해주지는 않는다.

AutoCloseable을 구현한 객체만 `close()`가 호출된다.

<br>

[AutoCloseable](https://chromium.googlesource.com/android_tools/+/refs/heads/master/sdk/sources/android-25/java/lang/AutoCloseable.java)은 인터페이스이며 자바7부터 지원.

```java
package java.lang;

public interface AutoCloseable {
    void close() throws Exception;
}
```

BufferedInputStream의 상속구조는 다음과 같다.

```java
java.lang.Object
  java.io.InputStream
    java.io.FilterInputStream
      java.io.BufferedInputStream
```

InputStream은 AutoCloseable를 상속받은 Closeable을 구현하였다.

```java
public abstract class InputStream extends Object implements Closeable {
  ....
}

public interface Closeable extends AutoCloseable {
    void close() throws IOException;
}
```

이런 이유로 위의 예제에서 BufferedInputStream 객체가 try-with-resources에 의해서 자원이 해제될 수 있다.

---

## AutoCloseable을 구현하는 클래스 만들기

내가 만든 클래스가 try-with-resources으로 자원이 해제되길 원한다면, AutoCloseable을 implements(구현)해야 한다.   

아래 코드에서 CustomResource 클래스는 AutoCloseable을 구현하였다.

main에서는 이 객체를 try-with-resources로 사용하고 있다.

```java
public static void main(String args[]) {
    try (CustomResource cr = new CustomResource()) {
        cr.doSomething();
    } catch (Exception e) {
    }
}

//AutoCloseable을 구현하는 클래스
private static class CustomResource implements AutoCloseable {
    public void doSomething() {
        System.out.println("Do something...");
    }

    @Override
    public void close() throws Exception {
        System.out.println("CustomResource.close() is called");
    }
}
```

실행해보면 다음과 같이 출력된다.

이 예제에서는 close()가 호출될 때 로그를 출력하기 때문에 close()가 실제로 호출되는지 눈으로 확인할 수 있다.

```
Do something...
CustomResource.close() is called
```

<br>
<br>
#### 핵심정리)

꼭 회수해야 하는 자원을 다룰때는 try-finally 말고, try-with-resources를 사용하자.

코드는 더 짧고 분명해지며, 만들어지는 예외 정보도 훨씬 유용하다.
try-finally로 작성하면 실용적이지 못할 만큼 코드가 지저분해지더라도, try-with-resource로는 정확하고 쉽게 자원을 회수할 수 있다.



---
##### **참고 및 출처)**

>[https://codechacha.com/ko/java-try-with-resources/](https://codechacha.com/ko/java-try-with-resources/)  
[http://tutorials.jenkov.com/java-exception-handling/try-with-resources.html](http://tutorials.jenkov.com/java-exception-handling/try-with-resources.html)
https://rok93.tistory.com/entry/%EC%95%84%EC%9D%B4%ED%85%9C9-try-finally%EB%B3%B4%EB%8B%A4%EB%8A%94-try-with-resources%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EB%9D%BC
