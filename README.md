a# JPA STUDY

## 자바 ORM 표준 JPA 프로그래밍

<details>
  <summary>1.JPA 소개</summary>
  <div markdown="1">

## 1.3 JPA란 무엇인가?

### JPA (Java Persisitence API)는 자바 진영의 ORM 기술 표준.
어플리케이션과 JDBC 사이에서 동작

![1](https://user-images.githubusercontent.com/43127088/108817489-4fbd9e00-75fb-11eb-978a-4dc18aee2248.PNG)

### ORM (Object Relational Mapping)
- 객체와 관계형 데이터베이스를 매핑한다는 뜻.
- ORM 프레임워크 객체와 테이블을 매핑해서 패러다임의 불일치 문제를 개발자 대신 해결해준다.

JPA를 사용해서 객체를 저장하는 코드

![2](https://user-images.githubusercontent.com/43127088/108825478-6b7a7180-7606-11eb-857c-9f2f47798abc.PNG)

`jpa.persist(member)`

JPA를 사용해서 객체를 조회하는 코드

![3](https://user-images.githubusercontent.com/43127088/108825652-a381b480-7606-11eb-98b0-4688404f9e33.PNG)

`Member member = jpa.find(memberId`

ORM 프레임워크는 단순히 SQL을 개발자 대신 생성해서 데이터베이스를 전달해주는 것 뿐 아니라 앞서
다양한 패러다임의 불일치 문제들도 해결해준다. 어떠헤 매핑해야 하는지 매핑 방법만 ORM 프레임워크에 알려주면 된다.

### 1.3.1 JPA 소개
EJB 3.0에서 하이버네이트를 기반으로 새로운 자바 ORM 기술 표준이 만들어졌는데 이것이 바로 JPA

JPA는 자바 ORM 기술에 대한 API 표준 명세다. 쉽게 이야기해서 인터페이스를 모아둔 것이다. 따라서 JPA를 구현한
ORM 프레임워크를 선택해야한다.

![5](https://user-images.githubusercontent.com/43127088/108826317-800b3980-7607-11eb-82f5-a5e1271715e9.PNG)


### 1.3.2 왜 JPA를 사용해야 하는가?

**생산성**
- JPA를 자바 컬렉션에 객체를 저장하듯 JPA에게 저장할 객체를 전달.
- INSERT SQL을 작성하고 JDBC API 사용하는 지루하고 반복적인 일을 JPA가 대신 처리해준다.
- CREATE TABLE같은 DDL문 자동 생성
- 데이터베이스 설계 중심의 패러다임을 객체 설계 중심으로 역전

```java
jpa.persist(member);    // 저장
Member member = jpa.find(memberId);     // 조회
```

**유지 보수**
- 엔티티에 필드 추가시 등록, 수정, 조회 관련 코드 모두 변경
- JPA를 사용하면 이런 과정을 JPA가 대신 처리
- 개발자가 작성해야 할 SQL과 JDBC API 코드를 JPA가 대신 처리해줌으로 유지보수해야 하는 코드 수가 줄어든다.

**패러다임 불일치 해결**
상속, 연관관계, 객체 그래프 탐색, 비교하기 같은 패러다임 불일치 해결

**성능**
```java
String memberId = "helloId"
Member member1 = jpa.find(memberId);
Member member2 = jpa.find(memberId);
```
JDBC API를 사용해서 작성하면 조회할때 마다 SELECT SQL을 사용해서 DB와 두 번 통신했을 것이다.
JPA를 사용하면 회원을 조회하는 SELECT SQL을 한 번만 데이터베이스에 전달하고 두 번쨰는 조회한 회원 객체를 재사용한다.
- 다양한 성능 최적화 기회 제공
- 어플리케이션과 데이터베이스 사이에 존재함으로 여러 최적화 시도 가능

**데이터 접근 추상화와 벤더 독립성**
- 데이터베이스 기술에 종속되지 않도록 한다.
- 데이타베이스를 변경하면 JPA에게 다른 데이터베이스를 사용한다고 알려주면 됨.

![6](https://user-images.githubusercontent.com/43127088/108827293-c8772700-7608-11eb-93ec-152f984f149a.PNG)


**표준**
JPA는 자바 진영의 ORM 기술 표준이다. 앞서 야기했듯이 표준을 사용하면 다른 구현 기술로 손쉽게 변경할 수 있다.

  </div>
</details>


<details>
  <summary>2.JPA 시작</summary>
  <div markdown="1">

## 2.3 JPA 시작
`implementation 'org.springframework.boot:spring-boot-starter-data-jpa`
- spring-boot-starter-aop
- spring-boot-starter-jdbc
    - HikariCP 커넥션 풀 (부트 2.0 기본)
- hibernate + JPA: 하이버네이트 + JPA
- spring-data-jpa: 스프링 데이터 JPA

## 2.4 객체 매핑 시작

```java
spring:
  h2:
    console:
      enabled: true
      path: /h2-console
  datasource:
    driver-class-name: org.h2.Driver
    url: jdbc:h2:mem:testdb
    username: sa

  jpa:
    hibernate:
      ddl-auto: create
    properties:
      hibernate:
        format_sql: true

logging.level:
  org.hibernate.SQL: debug
```
- spring.jpa.hibernate.ddl-auto: create
    - 이 옵션은 애플리케이션 실행 시점에 테이블을 drop 하고, 다시 생성한다.

참고: 모든 로그 출력은 가급적 로거를 통해 남겨야 한다.
- show_sql : 옵션은 System.out 에 하이버네이트 실행 SQL을 남긴다.
- org.hibernate.SQL : 옵션은 logger를 통해 하이버네이트 실행 SQL을 남긴다.

- JPA 표준 속성
    - javax.persistence.jdbc.driver : JDBC 드라이버
    - javax.persistence.jdbc.user : 데이터베이스 접속 아이디
    - javax.persistence.jdbc.password : 데이터베이스 접속 비밀번호
    - javax.persistence.jdbc.url : 데이터베이스 접속 URL
- 하이버네이트 설정
    - hibernate.dialect : 데이터베이스 방언 설정

**데이터베이스 방언**
JPA는 특정 데이터베이스에 종속적이지 않은 기술.다른 데이터베이스로 손쉽게 교체할 수 있다.

**하이버네이트 설정 옵션**
- hibernate.show_sql : 실행한 SQL을 출력.
- hibernate.format_sql : SQL을 보기 좋게 정렬함.
- hibernate.use_sql_comments : 쿼리 출력 시 주석도 함께 출력
- hibernate.id.new_generator_mappings : JPA 표준에 맞는 새로운 키 생성 전략을 사용함.

**하이버네이트 설정**
- create : Session factory가 실행될 때에 스키마를 지우고 다시 생성. 클래스패스에 import.sql 이 존재하면 찾아서, 해당 SQL도 함께 실행함.
- create-drop : create와 같지만 session factory가 내려갈 때 스키마 삭제.
- update : 시작시, 도메인과 스키마 비교하여 필요한 컬럼 추가 등의 작업 실행. 데이터는 삭제하지 않음.
- validate : Session factory 실행시 스키마가 적합한지 검사함. 문제가 있으면 예외 발생.
- 개발시에는 create가, 운영시에는 auto 설정을 빼거나 validate 정도로 두는 것이 좋아 보인다.
  update로 둘 경우에, 개발자들의 스키마가 마구 꼬여서 결국은 drop 해서 새로 만들어야 하는 사태가 발생한다

  </div>
</details>

<details>
  <summary>3.영속성 관리</summary>
  <div markdown="1">

  </div>
</details>

<details>
  <summary>4.엔티티 매핑</summary>
  <div markdown="1">

  </div>
</details>

<details>
  <summary>5.연관관계 매핑 기초</summary>
  <div markdown="1">

  </div>
</details>


참고 문헌 :

- [자바 ORM 표준 JPA 프로그래밍 / 김영한](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=62681446)
- [jpa](https://ultrakain.gitbooks.io/jpa/content/chapter1/chapter1.3.html)
- 인프런 인강 자료 / 김영한