# Spring 프로젝트 디렉토리 구조

## src/main
* ~/java : 자바코드(컨트롤러, 모델)
* ~/resources : 자바코드에서 사용할 리소스(mapper, sql)

## src/test
* ~/java : 테스트 코드
* ~/resources : 테스트 코드에서 사용할 리소스

## Maven Dependencies
* 라이브러리 관리 도구(maven에서 다운받은 jar 파일)

## src (웹 디렉토리)
* **~/main/webapp : 외부 접근 가능!**
* **~/main/webapp/WEB-INF : 외부 접근 불가!! 컨트롤러를 경유해서 접근 가능**
* ~/main/webapp/resources : js, css, image 등을 관리
* ~/main/webapp/WEB-INF/classes : 컴파일된 클래스
* ~/main/webapp/WEB-INF/spring : 스프링 환경설정 파일(root-context.xml, servlet-context.xml)
* ~/main/webapp/WEB-INF/views : html, jsp 파일

> _WEB-INF 폴더_<br>컴파일된 파일 클래스와 스프링 환경설정 파일(DB 연결 정보)이 존재하기 때문에 외부에서 직접 접속이 차단되어 있다.<br>또한, JSP가 외부 접속으로 수정되는 것을 방지.

## pom.xml
* **maven에서 참조하는 설정 파일**
* maven은 빌드와 관련된 정보를 프로젝트 객체 모델(Project Object Model)이라는 이름으로 정의, 사용.
* 여기서 dependency 태그를 추가하고 설정하고 싶은 라이브러리를 추가. ([maven repository](https://mvnrepository.com/) 에서 원하는 라이브러리 검색한 후, 복사해 추가)



***

참고 블로그<br>
[https://doublesprogramming.tistory.com/16](https://doublesprogramming.tistory.com/16)