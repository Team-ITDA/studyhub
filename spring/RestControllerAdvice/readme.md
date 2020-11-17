@RestControllerAdvice
====================


@ExceptionHandler를 이용해서 메소드마다 예외처리를 해주다보니 같은 에러를 처리할때 마다
중복된 코드를 쓰게되었다. 중복된 코드를 쓰다보니 코드도 길어지고 유지보수 할 때 불편한 점이 많았다.
스프링 프레임워크의 큰 장점중에 하나가 AOP인데 중복된 코드를 쓴다는 것은 AOP에 어긋난다는 생각이 들었다.
그래서 나는 재사용성을 높여 중복된 코드를 없애 객체 지향적인 코드를 구현하고 싶었고,
(메서드는 한 가지 일만 해야하기 때문에) 
에러를 한 곳에서 처리해서 controller 전역에 적용되는 예외처리 구문을 만들어야겠다는 생각을 하였다.


그래서 공부하던 중 ControllerAdvice annotation의 @ControllerAdvice와@RestControllerAdvice을 알게되었고, 


API 서버를 위해 json형식의 파싱 및 전역척인 예외처리가 가능한 @RestControllerAdvice라는 어노테이션이 있다는 걸 알았다. 그래서 공부한 후 적용해보았고 리팩토링 하고나서 코드가 훨씬 클린해졌다는 것을 느꼈다.


전역에서 예외처리를 하는 이유는 무엇일까?

@ExceptionHandler를 사용하여 해당 컨트롤러에 대한 예외 처리를 한다고 하자.

```java
@RestController
public class PersonController {

    @GetMapping("/{id}")
    public Person getPerson(@PathVariable Long id) {
        //
    }

    @PostMapping
    public void postPerson(@RequestBody @Valid PersonDto personDto) { 
        personService.put(personDto);
        //
    }

    @PutMapping("/{id}")
    public void modifyPerson(@PathVariable Long id, @RequestBody PersonDto personDto) {
        //
    }
    
     @ExceptionHandler(PersonException.class)
        public ResponseEntity<ErrorResponse> handlePersonException(final PersonException error) {
           //
        }

}
```

해당 컨트롤러에서 발생하는 예외는 handlePersonException 메서드에서 처리할 수 있다. 하지만 컨트롤러가 늘어난다면 어떨까? 컨트롤러가 늘어나면서 예외 처리에 대한 중복 코드도 늘어날 것이고 많은 양의 코드를 다룰 것이다. 그러면 유지보수 또한 어려워질 것이다.

그러나

전역에서 예외 처리를 하면 위와 같은 문제를 해결할 수 있다.
그래서 전역에서 예외를 어떻게 처리하느냐?

익셉션 패키지 디렉토리 구조


![3](https://user-images.githubusercontent.com/43127088/99095313-53af4c80-2618-11eb-95a7-ab35ad3b022e.PNG)



우선 익셉션 - 핸들러 패키지를 만들고 GlobalExceptionHandler 클래스를 추가하였고, 에러 코드들을 작성했다. @ExceptionHandler을 이용하여 인자로 클래스들을 받아와 필요한 에러 코드들을 작성해주었다.
```java
@RestControllerAdvice //restController에 전역적으로 적용되는 핸들링이 가능
public class GlobalExceptionHandler {
    @ExceptionHandler(value = RenameNotPermittedException.class)
    public ResponseEntity<ErrorResponse> handleRenameNoPermittedException(RenameNotPermittedException ex) {
        return new ResponseEntity<>(ErrorResponse.of(HttpStatus.BAD_REQUEST, ex.getMessage()), HttpStatus.BAD_REQUEST);
    }

    @ExceptionHandler(value = PersonNotFoundException.class)
    public ResponseEntity<ErrorResponse> handlePersonNotFoundException(PersonNotFoundException ex) {
        return new ResponseEntity<>(ErrorResponse.of(HttpStatus.BAD_REQUEST, ex.getMessage()), HttpStatus.BAD_REQUEST);
    }
}
```
또 익셉션 패키지에 RenameNotPermittedException 클래스를 만들어 에러메시지를 작성해 주었다.
```java
@Slf4j
public class RenameNotPermittedException extends RuntimeException{
    private static final String MESSAGE = "이름을 변경하지 않습니다.";

    public RenameNotPermittedException() {
        super(MESSAGE);
        log.error(MESSAGE);
    }
}
```
그 후 ControllerTest에서 필드에

```java
@Autowired
    private GlobalExceptionHandler globalExceptionHandler;
```

빈 주입을 해주었고 (사실 이렇게 필드 주입을 하는 것 보다 생성자 주입을 하는 것이 더 좋은 코드고 생성자 주입은 [여기](https://github.com/Team-ITDA/studyhub/tree/main/spring/Autowired) 볼 수 있다.)

```java
public PersonControllerTest {

@Autowired
    private GlobalExceptionHandler globalExceptionHandler;

@BeforeEach     // 해당 어노테이션은 메서드 매 test마다 먼저 실행되게 하는 구문
    void beforeEach() {
        mockMvc = MockMvcBuilders
                .standaloneSetup(personController)
                .setControllerAdvice(globalExceptionHandler)
                .alwaysDo(print())
                .build();
    }
}
```

BeforeEach 메서드에 setControllerAdvice를 이용해 로직을 작성해준다.

 
@BeforeEach란 PersonControllerTest 클래스에서 해당 메서드가 매 test마다 먼저 실행되게 하여 모든 메서드에 로직을 추가해주지 않아도, BeforeEach를 사용해서 전역적으로 로직이 먼저 실행되개 해준다.


PersonControllerTest에서 modifyPersonIfNameIsDifferent() 메서드 로직에 이름 인자 값을 바꾸어 로직을 구현한 후 메서드를를 실행해주었다.

원래 data 'hyungil'에서


```java
insert into person(`id`, `name`, `year_of_birthday`, `month_of_birthday`, `day_of_birthday`) values (1, 'hyungil', 1994, 10, 10);
```

'hyungil' -> 'minsub' 으로, 아래 메서드 참고

```java
@Test
    void modifyPersonIfNameIsDifferent() throws Exception {
        PersonDto dto = PersonDto.of(1, 'minsub', 1994, 10, 10);

        mockMvc.perform(
                MockMvcRequestBuilders.put("/api/person/1")
                        .contentType(MediaType.APPLICATION_JSON_UTF8)
                        .content(toJsonString(dto)))
                .andExpect(status().isBadRequest())
                .andExpect(jsonPath("$.code").value(400))
                .andExpect(jsonPath("$.message").value("이름을 변경하지 않습니다."));
    }
```

![2](https://user-images.githubusercontent.com/43127088/99090540-1e9ffb80-2612-11eb-8042-2edc88339662.PNG)

json형태로 body 값이 찍히는 것을 볼 수 있다.


 modifyPersonIfNameIsDifferent() 메서드의 마지막줄
```java
.andExpect(jsonPath("$.message").value("이름을 변경하지 않습니다."));
```
와 RenameNotPermittedException 클래스의 에서 받은 

```java
public class RenameNotPermittedException extends RuntimeException{
    private static final String MESSAGE = "이름을 변경하지 않습니다.";
``` 

응답 문자열을 비교하여 똑같기 때문에 body에서 "이름을 변경하지 않습니다." 이 출력되는 것을 볼 수 있다.

```java
// {"message":"val"} 이라는 response를 받았는지 검증하려면 아래처럼 사용
.andExpect(jsonPath("$.message").value("val"));
```


    
@RestControllerAdvice와 @ExceptionHandler를 이용하여 중복된 코드들을 없애고 전역적인 예외처리를 해주었더니
가독성이 훨씬 좋아지고 유지보수를 하는 일에 있어 훨씬 편리해졌다.