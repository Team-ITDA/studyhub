Exception
=================

오류는 시스템에서 발생하는 것.

예외는 오류와 다르게 기본적으로 개발자가 구현한 코드가 원인이 된다
따라서 개발자가 예측과 처리할 수 있기 때문에 그만큼 신중을 가해야 한다

Java Exception 에는 크게 두 가지 Exception 이 있다

### <code>checkedException</code> vs <code>UncheckedException</code>


- <code>checkedException</code> 은 컴파일 과정에서 확인되는 Exception

- <code>UncheckedException</code>은 자바 코드가 컴파일되어 바이트 코드로 변환된 후 실제 JVM이 변환된 **바이트 코드를 실행**하면서 발생하는 <code>Exception</code> 들이다. 대표적인 예로 <code>NullPointException</code>, <code>IndexOutOfBoundException</code>

따라서

<code>UncheckedException</code>은 <u><code>RuntimeException</code>을 상속한 모든 하위 클래스들</u>이고

<code>checkedException</code>은 <code>RuntimeException</code>을 제외한 모든 <code>Exception</code> 클래스들이다

그리고

두 <code>Exception</code> 클래스들의 큰 차이점은

<code>Checked Exception</code>은 예외가 발생하면 트랜잭션을 roll-back하지 않고 예외를 던진다

<code>Unchecked Exception</code>은 예외 발생 시 트랜잭션을 roll-back한다는 점에서 차이가 있다

---------------

발생한 예외를 처리하는 방법은 3가지가 있다

1. **예외 복구**

예외가 발생하면 catch 문의 흐름으로 유도하는 예외 복구

```
public void count(int index){
  try{

  }catch(Exception e){

  }
}

```

2. **예외 회피**

처리를 하지 않고 호출한 쪽으로 던져버리는 예외처리 회피

```
public void count(int index) throws Exception{
  //
}
```

3. **예외 전환**

다른 예외로 전환하여 던지는 예외 전환

```
public void count(int index) throws ChangedException{
  try{

  }catch(Exception e){
    new throw ChangedException();
  }
}
```

예외에 대해 단순하게 생각하면 안 된다

언뜻 보면 예외 전환이 왜 필요하지? 라는 생각이 들 수도 있다

알아보자.











> 원문
[Java 예외(Exception) 처리에 대한 작은 생각](http://www.nextree.co.kr/p3239/)
