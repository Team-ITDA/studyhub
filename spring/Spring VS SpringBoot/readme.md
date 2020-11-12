그러다가 SpringBoot와 Spring 차이를 대강 알고만 있었지, 누가 물어보면 정확히 말을 하지 못할 것 같아서 이론 학습을 좀 해보았다.


궁금해서 스프링 공식 문서를 살펴보다가,

[스프링 공식 문서](docs.spring.io/spring-framework/docs/current/reference/html/overview.html#overview-spring)

스프링이 왜 스프링이라고 부르게 되었을까? 를 알게되었다. 

'개발자들의 겨울은 끝났다, 이 프레임워크로 인해서 봄이왔다' 그래서 스프링이라고 부르게 되었다고 한다.

 

스프링부트는 스프링에 부트를 붙이면서 (boot = 부팅하다) 스프링을 좀 더 쉽게 부팅하자 라는 뜻? 이라는 생각이 든다. 실제 스프링과 스프링 부트를 각각 시작해볼때, 스프링 부트가 설정이 훨씬 편했기 때문에 이런 생각이 들었다.

 
공식문서 읽어보니까 스프링 기반의 애플리케이션이지만 상용화 수준까지 훨씬 간단하게 만든다고 써있다.
스프링부트가 스프링보다 편한 몇 가지 이유들은....


스프링에서 pom.xml에서 thymeleaf를 추가해줄때 

```
<dependency>
    <groupId>org.thymeleaf</groupId>
    <artifactId>thymeleaf-spring5</artifactId>
    <version>3.0.11.RELEASE</version>
</dependency>
```

버전 까지 달아줘야한다. 그에 비해 스프링 부트는
```
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-thymeleaf</artifactId>
		</dependency>
```

스프링부트는 버전 관리가 권장 버전으로 자동 설정이 되기 때문에 스프링에 비해 훨씬 편리하다. 심지어 스프링부트 메이븐말고 그래들은 훨씬 짧아진다. bulid.gradle에서 
```
dependencies {
implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
}
```

한 줄만 달아줘도 끝났다. 이런 과정이 모이면
![1](https://user-images.githubusercontent.com/43127088/98907224-d691b380-2501-11eb-9ec4-83fdabd67df1.png)

메이븐 말고 그래들 디펜던시
![2](https://user-images.githubusercontent.com/43127088/98907270-eb6e4700-2501-11eb-9a9e-d5cf0019f32b.png)

스크롤만봐도 차이가 정말 많이난다.


또 스프링부트에는 라이브러리들이 많이 내장되어 있어 별도로 라이브러리를 추가하지 않아도 되는 것들이 많다.

예를들면 로그를 찍기위해 slf4j를 쓸 때, 스프링부트는 spring-boot-starter-web 안에 spring-boot-starter-logging에 구현체가 있다. 하지만 위에 이클립스 pom.xml에서 slf4j 디펜던시 추가 되어있는 것을 보면 확연한 차이가 보인다. 


또 스프링은 Configuration에서 어노테이션을 달며 어떤 처리를 해줄껀지 bean도 등록해주고 기본적으로 설정해줄게 많다. 뭐 예를 들면 thymeleaf를 쓸때
```
@Configuration
public class ThymeleafViewResolverConfig { 
    
    @Value("${thymeleaf.cache}") 
    private boolean isCache;
    
    @Bean
    public SpringResourceTemplateResolver templateResolver() {
        SpringResourceTemplateResolver  templateResolver = new SpringResourceTemplateResolver ();
        templateResolver.setPrefix("classpath:templates/");
        templateResolver.setCharacterEncoding("UTF-8");
        templateResolver.setSuffix(".html");
        templateResolver.setTemplateMode("LEGACYHTML5");
        templateResolver.setCacheable(isCache);
        return templateResolver;
    }
    
    @Bean
    public SpringTemplateEngine templateEngine(MessageSource messageSource) {
        SpringTemplateEngine templateEngine = new SpringTemplateEngine();
        templateEngine.setTemplateResolver(templateResolver());
        templateEngine.setTemplateEngineMessageSource(messageSource);
        templateEngine.addDialect(layoutDialect());
        
        return templateEngine;
    }
    
    @Bean
    public LayoutDialect layoutDialect() {
        return new LayoutDialect();
    }
 
    @Bean
    @Autowired
    public ViewResolver viewResolver(MessageSource messageSource) {
        ThymeleafViewResolver viewResolver = new ThymeleafViewResolver();
        viewResolver.setTemplateEngine(templateEngine(messageSource));
        viewResolver.setCharacterEncoding("UTF-8");
        viewResolver.setOrder(0);
        return viewResolver;
    }
}
```
이에 반해 스프링 부트는

```
implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
```

Configuration 필요없이 그냥  application.yml이나 application.properties에 서만 적용하면 된다. 한 줄이면 끝난다.

그런데 잠깐?

부트로 프로젝트를 만들면 처음에 application.properties 만들어지는데 보통 depth로 인한 가독성이나 중복성때문에 

.properties를 지우고 application.yml으로 만들어서 쓰는 추세다.(들은바로는.. 물론 나도 yml으로 바꿔서 쓰는게 스페이스바 한번눌러서 생기는 depth로 인해 읽기가 쉬웠고 중복이 줄어 훨씬 편했다. )


```
spring.datasource.username= hyungil
spring.datasource.hikari.password= = 1234
// .properties
```

```
  datasource:
    username: hyungil
    password: 1234
 // .yml
```

이런 차이랄까?

또 스프링 부트는 톰캣이 내장되어 있다. 이건 서버 구동 시간이 절반 가까이 단축된다. 

또 내장 서블릿 컨테이너 덕분에 jar 파일로 간단 배포가 가능하다.

java -jar $REPOSITORY?&JAR_NAME& 

정리하자면 스프링보다 스프링부트가 더 편리한 이유는
- 설정, 의존성 관리, 자동으로 권장 버전으로 관리해주는게 편리하다.
- 내장 서버로 인하여 간단하게 배포 서버 구축이 편리하다.
- 스프링 시큐리티, jpa, 테스트관련 의존성 등 다른 스프링 프레임워크 요소를 쉽게 사용 할 수가 있다.

즉, 개발자들이 개발에만 더욱 집중 할 수 있도록 해준다.