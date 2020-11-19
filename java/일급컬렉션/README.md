일급 컬렉션
===================

제가 더 좋은 프로그래머로 한 발자국 나아가기 위해 필요하다고 느낀

**일급 컬렉션** 에 대해 정리해보겠습니다


## **일급 컬렉션** 이란?

- 단 <u>한 개</u>의 컬렉션만 필드로 가지는 클래스

- 비즈니스 로직에서 사용되는 컬렉션 인스턴스를 **Wrapping** 하는 것

예제
```java
public class ITDA {
  private final List<Member> members;

	public TeamITDA(List<Member> members) {
		this.members = new ArrayList<>(members);
	}

	public List<Member> getMembers(){
		return Collections.unmodifiableList(members);
	}
}
```

언뜻보면 위 클래스는 단순히 하나의 컬렉션 필드만 가지고 있고

단순히 반환만 해주는 <code>getter</code> 를 가지고 있기 때문에

필요없다고 느끼실 수 있습니다

하지만 왜 이렇게 컬렉션 인스턴스를 <code>Wrapping</code> 하는 클래스를 작성해야 하는지 알아보겠습니다

### 1. 비즈니스에 종속적인 자료구조

=  비즈니스 로직을 위한 자료구조를 만든다 정도로 이해하시면 됩니다

우리말이 어려울 수 있으니 예시 상황를 통해 이해 해보겠습니다

- 상황
  1. 데이터베이스에서 <code>Post</code> 라는 인스턴스를 얻어옵니다

  2. <code>Post</code> 인스턴스가 필드로 가지고 있는 <code>Comment</code> 리스트들을 하나씩 조회하여

  3. 현재 사용자가 좋아요를 눌렀는지 검사합니다

```java
@Service
public class PostService {
  public void isLikedComment(long postId){
    Post post = postRepository.findPostById(postId);

    List<Comment> comments = post.getComments();

    for(Comment comment : comments){
      if(comment.isLiked){
        //logic..
      }
    }

    //etc..
  }
}
```

*위 예시는 제가 과거에 진행했었고 더티 코드의 진가를 보여주었던 프로젝트에서 따왔습니다*

위와 같은 메소드를 작성하였을 때 큰 문제점이 있습니다

다른 사람들이 위 코드를 보았을 때 중요한 점은 <u><code>Comment</code> 클래스가 변경되었을  때, PostService도 로직을 수정해야하는 사이드 이펙트가 발생합니다.</u>

또한 이러한 문제점을 극복하고 클래스에서 사용자들이 좋아요를 누른 댓글들을 얻으려고 할 때마다 <u>위와 같은 로직이 반복적으로 구현</u>하게 됩니다

이는 중복코드를 유발하고 리팩토링의 징조이죠

해당 메소드를 이용하는 클라이언트가 비즈니스에서 꼭 필요한 점만 이용할 수 있도록 일급 컬렉션을 적용해본다면

```java
public class LikedComments {
  private final List<Comment> list;

  public LikedComments(Post post){
    list = new ArrayList<>();

    List<Comment> comments = post.getComments();

    for(Comment comment : comments){
      if(comment.isLiked){
        //logic..
        list.add(comment);
      }
    }
  }

  //이후 필요한 메소드
}
```

정도로 구현하여 비즈니스 로직을 위한 자료구조인 <code>LikedComments</code> 만들고

```java
@Service
public class PostService {
  public LikedComments isLikedComment(long postId){
    return new LikedComments(postRepository.findPostById(postId));
  }
}
```

클라이언트는 자신의 로직에 불필요한 로직을 줄일 수 있겠습니다


### 2. 불변

일급 컬렉션을 사용하게 되면 컬렉션 인스턴스에 불변을 보장할 수 있습니다

즉, <u> 재할당 뿐만 아니라 내부 요소들이 바뀌지 않는</u> 안전한 인스턴스로 만들 수 있습니다  

<code>final</code> 의 의미는 **재할당** 금지하는 것입니다

제가 처음 프로그래밍을 배울 때에는 <code>final = 상수</code> 라는 키워드로

<code>final</code> 을 접하였기 때문에 값을 고정시킨다 정도의 의미로 이해하고 있었습니다

하지만 <code>final</code> 의 키워드를 정확히 알지 못하면 자신의 의도했던 흐름으로 코드가 동작하지 않을 수 있습니다

```java
public static void main(String[] args) {
  final List<Integer> numbers = new ArrayList<>(Arrays.asList(1,2,3));
  numbers.add(4); // 가능
  numbers = new ArrayList<>(); // 컴파일 에러!
}
```

위 예제를 보면 <code>List</code> 타입에 <code>final</code> 키워드를 선언했음에도 언제든지 가지고 있는 요소들이 변경될 수 있음을 알 수 있습니다

따라서

```java
class Numbers {
  private final List<Integer> list;

  public Numbers(List<Integer> list){
    this.list = new ArrayList<>(list);
  }

  public List<Integer> getList(){
    return Collections.unmodifiableList(list);
  }
}
```

생성자에서 파라미터로 받아온 컬렉션 객체를 이용하여 필드를 초기화하고

<code>Collections.unmodifiableList()</code> 메소드를 이용하여
<code>getter</code> 를 통한 참조한 필드 요소들이 변경될 위험을 방지한다면

불변함을 보장할 수 있습니다

### 3. 상태와 행위를 한 곳에서 관리

이에 대해서 저는 <code>SOLID</code> 에서 단일책임의 원칙과 가깝다고 생각합니다

하나의 클래스는 하나의 역할만 함으로써 이후 사이드 이펙트가 발생할 가능성을 줄이는 것이죠  

```java
public class ITDAGitHub {
  public void createGitHub(List<String> names){
    List<String> members = new ArrayList<>();

    for(String name : names){
      //사람인지 검사한다
      //멤버인지 검사한다
      //추가한다
    }

    //깃허브 만든다
    //등등..
  }
}
```

위와 같은 메소드를 작성했다고 생각해봅시다

우선 메소드 이름과도 일치하지 않는 행위들이 포함되어 있습니다

중요한건 팀 멤버 리스트를 만들기 위해 반복문을 돌리는 작업이 이 곳뿐만 아니라

다른 곳에서도 <u>반복적으로 일어날 가능성</u>이 높고

<u>해당 로직과 같지 않을 수</u>도 있습니다

```java
public class ITDA {
  private final List<String> members;

  public ITDA(List<String> names){
    members = new ArrayList<>();

    for(String name : names){
      //사람인지 검사한다
      //멤버인지 검사한다
      //추가한다
    }
  }

  public List<String> getMembers(){
    return Collection.unmodifiableList(members);
  }
}

public class ITDAGitHub {
  public void createGitHub(List<String> names) {    
    List<String> members = new ITDA(names).getMembers();

    //깃허브 만든다
    //등등..
  }
}
```

<code>members</code> 를 담는 일급 컬렉션을 만들어

**관련된 로직을 모아둘 수 있고 강제할 수 있게 됩니다**

그리고 다른 클래스들 자신이 맡은 역할들만 집중할 수 있게 됩니다

### 4. 이름 있는 컬렉션

흔히 컬렉션 인스턴스를 변수들을 담아두게 되고

인스턴스의 사용과 얼추 맞는 이름으로 선언하게 됩니다

```java
List<String> members = Arrays.asList("zeroGone", "jinminYang", "meeeansub");
```

이렇게 변수로 사용하는 것은 사람마다 다르게 지을 수 있기 때문에 보편적이지 못합니다

협업 시 이슈가 될 만한 잠재성을 가지고 있죠

그렇기 때문에 일급 컬렉션을 이용하여

```java
class Members {
  private final List<String> names;

  public Members(List<String> names){
    this.names = new ArrayList<>(names);
  }

  public List<String> getNames(){
    return Collections.unmodifiableList(names);
  }
}
```

해당 클래스 기반으로 이름을 통일시키고 확장해 나가야 합니다









> 원문
> [일급 컬렉션 (First Class Collection)의 소개와 써야할 이유](https://jojoldu.tistory.com/412)
