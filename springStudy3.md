# 트랜잭션

### 정의
* 여러 개의 DML 명령문을 **하나의 논리적인 작업 단위**로 묶어서 관리하는 것
* **ALL 또는 Nothing** 방식으로 작업을 처리함으로써 작업의 일관성 유지]
> 트랜잭션 처리 = 어떤 한 작업 묶음 속에서 문제가 발생했을 경우, 원상복구 시키는 것
> _데이터 무결성 유지_

### 트랜잭션의 특징(ACID)

* Atomicity(원자성) : 트랜잭션은 자기의 연산을 **전부 또는 전무** 실행만이 있지 일부 실행으로 트랜잭션의 기능을 갖는 것은 아니다.
* Consistency(일관성) : 트랜잭션이 그 실행을 성공적으로 완료하면 **언제나 일관성있는 데이터베이스 상태**로 변환한다. 즉, 트랜잭션 실행의 결과로 데이터베이스 상태가 모순되지 않는다.
* Isolation(격리성) : 트랜잭션이 실행 중에 있는 연산의 중간 결과는 **다른 트랜잭션이 접근할 수 없다**.
* Durability(영속성) : 트랜잭션이 일단 그 실행을 성공적으로 완료하면 그 결과는 영속적이다. 따라서 시스템은 어떤 경우에도 완료된 **결과의 영속성을 보장**해야 한다.

### 트랜잭션의 상태

* 활동(Active) : 트랜잭션이 Begin_Trans에서부터 실행을 시작하였거나 실행 중인 상태
* 부분 완료(Partially Committed) : 트랜잭션이 마지막 명령문을 실행한 직후의 상태
* 실패(Failed) : 정상적 실행을 더 이상 계속할 수 없어서 중단한 상태
* 철회(Aborted) : 트랜잭션이 실행에 실패하여 Rollback 연산을 수행한 상태
* 완료(Committed) : 트랜잭션이 실행을 성공적으로 완료하여 Commit 연산을 수행한 상태

[참고 블로그](https://blog.naver.com/sillllver/90179069288)

<br>

## 트랜잭션 설정 방법

### 1. 선언적 트랜잭션

![Screen Shot 2020-06-01 at 11 20 15 PM](https://user-images.githubusercontent.com/41534832/83418387-7846ac00-a45e-11ea-9caf-3be7dfefeb12.png)

<br>
XML(pom.xml)에 트랜잭션에 대한 설정을 함으로써 트랜잭션을 적용할 범위와 대상을 선언

![Screen Shot 2020-06-01 at 11 42 15 PM](https://user-images.githubusercontent.com/41534832/83420397-8b0eb000-a461-11ea-83e0-babdd00b806b.png)

<br>
AOP namespace 사용.(Transaction을 위한 AOP 설정)

![Screen Shot 2020-06-01 at 11 21 20 PM](https://user-images.githubusercontent.com/41534832/83418478-9f9d7900-a45e-11ea-8168-5fa8e1e5b0f3.png)

<br>
TX namespace 사용. (선언적 Transaction 설정)

![Screen Shot 2020-06-01 at 11 48 06 PM](https://user-images.githubusercontent.com/41534832/83420975-5c450980-a462-11ea-8551-2653e810f12a.png)


#### 선언적 트랜잭션 - \<tx:method\> 태그의 속성

| 속성 이름 | 설명 |
| ------- | --- |
| name | 트랜잭션이 적용될 메소드 이름을 명시. '*' 사용 설정이 가능함 |
| propagation | 트랜잭션의 전파 규칙을 설정 |
| isolation | 트랜잭션의 격리 레벨을 설정 |
| read-only | 읽기 전용 여부 설정 |
| **no-rollback-for** | 트랜잭션을 롤백하지 않을 예외 타입을 설정 | 
| **rollback-for** | 트랜잭션을 롤백할 예외 타입을 설정 |
| timeout | 트랜잭션의 타임아웃 시간을 초 단위로 설정 |


#### 선언적 트랜잭션 - Propagation 속성에 설정 가능한 값

| 속성 값 | 설명 |
| ----- | --- |
| REQUIRED(default) | 메서드를 수행하는 데 트랜잭션이 필 요하다는 것을 의미 |
| MANDATORY | REQUIRED와 달리, 진행 중인 트랜잭 션이 존재하지 않을 경우 예외를 발생 |
| REQUIRES_NEW | 항상 새로운 트랜잭션을 시작 |
| SUPPORTS | 메서드가 트랜잭션을 필요로 하지는 않지만, 기존 트랜잭션이 존재할 경우 트랜잭션을 사용한다는 것을 의미 |
| NOT_SUPPORTED | 메서드가 트랜잭션을 필요로 하지 않 음을 의미 |
| NEVER | 메서드가 트랜잭션을 필요로 하지 않으며, 만약 진행 중인 트랜잭션이 존재하면 예외를 발생 |
| NESTED | 기존 트랜잭션이 존재하면, 기존 트랜 잭션에 중첩된 트랜잭션에서 메서드
를 실행 |

<br><br>

### 2. Transactional Annotation 사용

> #### Annotation
> : @를 이용한 주석. 자바코드에 주석을 달아 특별한 의미를 부여한 것.  
> 본질적 목적 = 소스코드에 메타 데이터를 표현

> > Meta Annotation을 이용해 커스텀 Annotation을 생성<br><br>
> > @Retention - Annotation 범위. 어떤 시점까지 영향을 미치는지 결정<br>
> > @Documented - 문서에도 Annotation의 정보가 표현<br>
> > @Target - 적용할 위치<br>
> > @Inherited - 부모 클래스에서 Annotation을 상속<br>
> > @Repeatable - 반복적으로 선언<br>

<br>

XML(pom.xml)에 <tx:annotation-driven transaction-manager = “transactionManager”/> 추가
![Screen Shot 2020-06-02 at 12 13 09 AM](https://user-images.githubusercontent.com/41534832/83423421-e80c6500-a465-11ea-96e5-bdb61a036d2b.png)


![Screen Shot 2020-06-02 at 12 16 24 AM](https://user-images.githubusercontent.com/41534832/83423694-505b4680-a466-11ea-95eb-4c654c6873ef.png)

<br><br>

***

<br><br>


# 보안

### 스프링 시큐리티(Spring Security)

- 스프링 기반의 어플리케이션에서 보안을 위해 **인증**과 **권한 부여**를 사용하여 **접근을 제어**하는 프레임워크
- 커스터마이징 가능
- filter 기반으로 동작하기 때문에 Spring MVC와 분리되어 관리 및 동작
- 허용되지 않은 페이지에 사용자가 접근할 경우 스프링 시큐리티는 페이지 호출 전에 인증이 되어있는지를 체크하고 페이지에 접근할 수 있는 권한이 있는지 체크 -> 허용 / 차단
- 크리덴셜 기반 인증 사용

<br>

### 인증 & 인가 & 권한

1. 인증(Authentication)
    - 현재 유저가 누구인지 확인 ex)로그인
2. 인가(Authorization)
    - 현재 유저가 어떤 서비스/페이지에 접근할 수 있는 권한이 있는지 검사
3. 권한
    - 인증된 주체가 어플리케이션의 동작을 수행할 수 있도록 허락되었는지를 결정<br>

![Screen Shot 2020-06-04 at 7 12 33 PM](https://user-images.githubusercontent.com/41534832/83744640-63089200-a697-11ea-9623-eee83a9593ee.png)

> ### 인증의 종류<br>
> 1. *크리덴셜 기반 인증 : 사용자명과 비밀번호를 이용한 방식
> 2. 이중 인증 : 한번에 2가지 방식으로 인증을 받는 것<br>
>   ex) 금융, 은행 웹 어플리케이션을 이용해 온라인 거래를 할 때. 로그인 + 보안인증서
> 3. 하드웨어 인증 : 웹의 영역 밖...<br>
>   ex) 지문인식, 키 삽입

> _*크리덴셜(Credential:자격) 기반 인증_ : 우리가 웹에서 사용하는 대부분의 인증 방식은 크리덴션 기반의 인증 방식입니다. 즉 권한을 부여받는데 1차례의 인증과정이 필요하며 대개 사용자명과 비밀번호를 입력받아 입력한 비밀번호가 저장된 비밀번호와 일치하는지 확인합니다. 일반적으로 스프링 시큐리티에서는 아이디를 프린시플(principle), 비밀번호를 크리덴셜(credential)이라고 부르기도 합니다.

<br>

### 스프링 시큐리티 구조

스프링 시큐리티는 **세션-쿠키** 방식으로 인증한다!

![Screen Shot 2020-06-04 at 7 20 27 PM](https://user-images.githubusercontent.com/41534832/83745403-77995a00-a698-11ea-8556-9110a1ff2595.png)

1. 유저가 로그인을 시도(HTTP Request)
2. AuthenticationFilter 에서부터 User DB까지 타고 들어감(그림 상 3, 4, 5)
3. DB에 있는 유저라면, UserDetails로 꺼내서 유저의 session 생성(그림 상 6)
4. 스프링 시큐리티의 인메모리 세션장소인 SecurityContextHolder에 저장(그림 상 7, 8, 9, 10)
5. 유저에세 session ID와 함께 응답을 내려줌
6. 이후 요청에서는 요청 쿠키에서 jsessionid를 찾아서, 검증 후 유효하면 Authentication을 쥐어줌!

<br>

### 필터

- 필터들이 어플리케이션에 대한 **모든 요청을 감싸서 처리**함
- 스프링 시큐리티에서 여러 개의 필터들이 **체인형태를 이루면서 동작** -> 10개의 스프링 시큐리티 필터가 자동으로 설정됨

<br>

### DelegatingFilterProxy

- 스프링 시큐리티가 모든 어플리케이션 요청을 감싸게 해서 모든 요청에 보안이 적용되게 하는 서블릿 필터
- @MVC에서 보았던 DispatcherServlet처럼 클라이언트의 요청을 가로채고, 이를 해당 빈으로 전달. 권한이 부여된 요청만 자원에 접근할 수 있다.

<br><br><img src="./img/security.png" width="70%">

[참고 블로그](http://egloos.zum.com/springmvc/v/504862)

- web.xml에 추가 -> 어플리케이션의 모든 요청을 스프링 시큐리티가 감싸서 처리
```xml
<filter>
<filter-name>springSecurityFilterChain</filter-name>
<filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
</filter>

<filter-mapping>
<filter-name>springSecurityFilterChain</filter-name>
<url-pattern>/*</url-pattern>
</filter-mapping>
```

<br>

### **스프링 시큐리티를 쓰는 이유!!**

1. 모든 URL을 가로채어 인증을 요구
2. 로그인 폼을 생성해준다
3. CRSF 공격을 막아준다 (CRSF : Cross-site Request Frogery 사이트간 요청 위조)
4. Session Fixation을 막아준다 (Session Fixation : 하나로 유효한 유저 세션을 탈취하여 인증을 우회하는 수법)
5. 요청 헤더 보안
6. Servlet API 메소드 제공