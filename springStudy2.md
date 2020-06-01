# Spring 프로젝트 구조

## IntelliJ의 프로젝트 구조
| Eclipse | IntelliJ |
| ------- | -------- |
| Workspace | Project |
| Project | Module |

* IntelliJ에서는 **한 프로젝트** 내에 **여러개의 모듈**로 구성되어 있다.

<br><br>

## 지난 프로젝트로 디렉토리 구조 살펴보기!

![Screen Shot 2020-06-01 at 10 41 17 PM](https://user-images.githubusercontent.com/41534832/83414817-10419700-a459-11ea-9339-baeac6414e7d.png)

1. controller : DispatcherServlet에서 전달된 요청을 처리
2. index.jsp : 뷰(.jsp)
3. applicationContext.xml : 스프링 컨테이너 설정 파일
4. dispatcher-servlet.xml : 클라이언트의 요청을 최초로 받아서, 이를 컨트롤러에게 전달
5. web.xml : DispatcherServlet 맵핑, 스프링 설정 파일 위치를 정의

<br>

## src/main
* ~/java : 자바 클래스(컨트롤러, 모델, DAO, VO 등)
* ~/resources : 자바코드에서 사용할 리소스(mapper, sql, config.xml)와 설정 파일

## src/test
* ~/java : 테스트 코드
* ~/resources : 테스트 코드에서 사용할 리소스

## web/WEB-INF
* ~/views : 뷰. jsp, html의 영역
* ~/web.xml : 배포서술자(Deployment Descriptor)라고 하며, 해당 파일 내에 정의된 설정내용 구성.

> _WEB-INF 폴더_<br>컴파일된 파일 클래스와 스프링 환경설정 파일(DB 연결 정보)이 존재하기 때문에 외부에서 직접 접속이 차단되어 있다.<br>또한, JSP가 외부 접속으로 수정되는 것을 방지.

## pom.xml
* maven에서 참조하는 설정 파일
* maven은 빌드와 관련된 정보를 프로젝트 객체 모델(**Project Object Model**)이라는 이름으로 정의, 사용.
* 여기서 dependency 태그를 추가하고 설정하고 싶은 라이브러리를 추가. ([maven repository](https://mvnrepository.com/) 에서 원하는 라이브러리 검색한 후, 복사해 추가)
* 의존성 관리와 배포를 가능하게 해줌

참고 블로그<br>
[https://doublesprogramming.tistory.com/16](https://doublesprogramming.tistory.com/16)

<br><br>

> ### _+ web.xml_
> 프론트 컨트롤러(DispatcherServlet)
> - HTTPServlet 클래스에서 파생된 Servlet
> - 매핑된 URL의 모든 HTTP 요청을 처리
> - 각 DispatcherServlet은 자기 자신의 웹 어플리케이션용 IoC 컨테이너인 WebApplicationContext가 생성
> - WebApplicationContext는 Spring MVC 웹 어플리케이션에 필요한 Spring 빈의 인스턴스를 생성, 관리
> - 생성된 Spring 빈은 웹 어플리케이션에서 정의한 컨트롤러, HTTP 요청을 컨트롤러와 매핑시켜주는 HandlerMapping과 ViewResolver도 포함한다

<br>

***

<br>

# 간단한 실습(전 개념 정리!)

### IoC(Inversion of Control)
- IoC : 객체가 사용하는 **의존 객체를 직접 생성하지 않고, 주입받아 사용**하는 방법
- 스프링 IoC 컨테이너 : 빈 설정 소스로부터 빈 정의를 읽어들이고, **빈 객체 생성, 의존 관계 설정, 빈 제공**
- 빈 : 스프링 IoC 컨테이너가 관리하는 객체


### ApplicationContext
- BeanFactory 인터페이스를 상속받는 인터페이스
- BeanFactory의 IoC 컨테이너로서의 기능 +a

### 빈(Bean)
- 빈의 등록, 주입 등은 **어노테이션**을 활용
    - 등록 : @Component, @Repository, @Service, @Controller, @Configuration
    - 주입 : @Autowired