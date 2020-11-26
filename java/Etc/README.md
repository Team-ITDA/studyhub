[javable](https://woowacourse.github.io/javable/)에서 댓글을 보면서 배웠던 내용 중에 기억이 흐려져서 자꾸 다시 찾아보게 하는 댓글들이 있다. 다시 찾는게 번거로워서 확실히 기억하기 위해.. 혹은 다시 찾을 때 편하게 찾기 위해 글로 정리해놓고자 한다.

---

## Objects.isNull, Objects.nonNull

javable에 아래와 비슷한 예제를 작성한 글이 있었다.

```java
public void validateEmpty(List<String> names) {
    if (Objects.isNull(names)) {
        throw new IllegalArgumentException(ERROR_MESSAGE);
    }
    ...
}
```

위 코드는 메서드로 넘어온 `List<String> names`를 null 체크하는 평범한 코드로 보일 수 있지만 좋은 코드는 아니다.

Objects.isNull 메서드 때문이다. Objects.isNull을 사용해서 null 체크를 하면 단지 null인지만 판단한다. List가 null은 아니고 size가 0인 비어있는 경우면 아무 문제 없이 통과한다는 소리다.

비어있는 List를 일부러 허용하는 경우도 있을 수 있지만 대부분의 경우엔 비어있는 List는 걸러내는 게 적절하다. 그래서 아래와 같이 CollectionUtils.isEmpty 메서드를 사용해서 검증을 해주는 게 더 좋다.

```java
public void validateEmpty(List<String> names) {
    if (CollectionUtils.isEmpty(names)) {
        throw new IllegalArgumentException(ERROR_MESSAGE);
    }
    ...
}
```

CollectionUtils.isEmpty 메서드는 Collection이 null인 경우와 비어있는 경우를 둘 다 잡아준다.

또한 Objects.isNull은 위에서 봤던 예제처럼 if문에 사용하라고 있는 메서드가 아니다. [javaDoc](https://docs.oracle.com/javase/8/docs/api/java/util/Objects.html)을 보면 Objects.isNull의 설명은 아래와 같이 써있다.

> ApiNote  
> This method exists to be used as a Predicate, filter(Objects::isNull)

Objects.isNull 메서드는 Stream의 filter 메서드처럼 인자를 Predicate 인터페이스로 받는 메서드에 사용하라고 있는 메서드인 것이다. 아래의 예제와 같이 사용하는 것이 적절하다.

```java
public long countNull(List<String> names) {
    return names.stream()
            .filter(Objects::isNull)
            .count();
}
```

물론 Objects.nonNull 메서드도 마찬가지이다.

또한 가독성을 따졌을 때도 Objects.isNull을 사용하는 것보다 null == 로 비교하는게 가독성이 더 뛰어나다.

```java
if (Objects.isNull(target) {  
}

if (null == target) {  
}  
```

요약하자면 Objects.isNull, Objects.nonNull을 if문에서 사용하면 가독성을 오히려 해칠 수 있고 용도에 맞지 않다. Predicate 인터페이스를 인자로 받는 메서드에 사용하자.

---

## Validator

Spring Validation을 사용할 때

```java
public class PostRequest {

    @NotEmpty
    private String title;
    ...
}
```

String 객체에 null과 Empty String을 체크하기 위해 @NotEmpty 어노테이션으로 검증을 해주었다. 충분한 것 같지만 @NotBlank를 사용한다면 null과 빈 문자열 뿐 아니라 `" "` 같은 공백만 있는 문자열도 잡아준다.

아래와 같이 많이 사용한다고 하니 참고하자.

-   @NotBlank = 문자열
-   @NotEmpty = Collection
-   @NotNull = 객체 (보통 LocalDate 때 사용)

그리고 이메일을 검증할 때 아래와 같이 사용하면

```java
@Email(message = "올바르지 않은 이메일형식입니다: ${validatedValue}")
private String email;
```

잘못된 값 입력 시 MethodArgumentNotValidException에 입력 값과 함께 에러메세지가 나가게 된다.

Controller를 제외한 Bean(Service 등)에서 Validation을 사용할 때 Validator 객체를 직접 선언해서 사용할 수도 있지만 @Validated 어노테이션을 사용하면 아래와 같이 편하게 Validation을 사용할 수 있다.

```java
@Validated  
@Service  
public class AService {

    public List getList(@Valid Dto dto) {  
    }
}
```

---

### 참고

-   [Stream의 foreach 와 for-loop 는 다르다.](https://woowacourse.github.io/javable/2020-05-14/foreach-vs-forloop)
-   [Spring Boot에서 DTO 검증하기](https://woowacourse.github.io/javable/2020-09-20/validation-in-spring-boot)