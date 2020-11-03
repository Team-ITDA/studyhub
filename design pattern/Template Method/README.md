템플릿 메소드 패턴
===================
- 행위(Behavioral) 패턴 중 하나
- 특정 부분을 서브 클래스로 캡슐화하는 것
- 코드 중복을 최소화할 때 유용하다


1. 공통된 부분을 추상 클래스로 추상화한다
2. 추상 클래스를 상속받아 해당 클래스의 역할에 맞게 작성한다


*예제*
김영곤과 양진민이 있다
영곤이는 밥을 볶아먹는 것을 좋아하여 밥을 정상적으로 짓고 볶아 먹는다
진민이는 꼬들밥을 좋아하여 밥을 오랫동안 짓는다

```
class YoungGon{
  public void haveDinner(){
    makeRice();
    fryRice();
    eatRice();
  }

  private void makeRice(){
    // 밥을 짓는다
  }

  private void fryRice(){
    // 밥을 볶는다
  }

  private void eatRice(){
    // 밥을 먹는다
  }
}
```
```
class Jinmin{
  public void haveDinner(){
    makeRice();
    eatRice();
  }

  private void makeRice(){
    // 밥을 오랫동안 짓는다
  }

  private void eatRice(){
    // 밥을 먹는다
  }
}
```

<code>YoungGon</code> 클래스 <code>Jinmin</code> 클래스의
- <code>eatRice()</code>
- <code>haveDinner()</code>
- <code>makeRice()</code>

메소드가 중첩되어있기 때문에 추출하고 상속을 도입할 생각은 쉽게 할 수 있다.
하지만 여기서 이슈가 되는 것은
<code>YoungGon</code> 클래스는 <code>haveDinner()</code>를 실행할 시 <code>fryRice()</code> 를 호출해야하고
<code>Jinmin</code> 클래스의 <code>makeRice()</code> 메소드는 오래동안 밥을 지어야한다

따라서 해당 클래스들의 성격에 맞게 구현할 수 있도록 <code>haveDinner()</code> 를 추상 메소드로 선언하고 <code>Jinmin</code> 클래스는 makeRice() 를 오버라이딩 하면 구현할 수 있다.


```
abstract class Human{
	public abstract void haveDinner();

	protected void makeRice() {
		//밥을 짓는다
	}

	protected void eatRice() {
		//밥을 먹는다
	}
}
```
```
class YoungGon extends Human{
	@Override
	public void haveDinner() {
		super.makeRice();
		fryRice();
		super.eatRice();
	}

	private void fryRice() {
		//밥을 볶는다
	}
}
```
```
class Jinmin extends Human{
	@Override
	public void haveDinner() {
		super.makeRice();
		eatRice();
	}

	@Override
	protected void makeRice() {
		//밥을 오래 짓는다
	}
}
```

템플릿 메소드 패턴을 적용하였을 때, 얻을 수 이점으로는 **확장성을 가질 수 있고**, 클라이언트에서는 **로직이 단순**해진다.

하지만 애플리케이션 규모에 따라
클래스 사이를 왔다갔다 해야한다는 점을 감안하였을 때
<u>가독성이 떨어질 수 있다</u>는 점을 참고하여 기존의 코드와 장단점을 비교하여 충분히 고려한 후 구현해야 한다.
선택은 여러분의 몫이다
