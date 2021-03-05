# JPA STUDY

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

## 2.6 애플리케이션 개발

```java
package jpabook.start;

import javax.persistence.*;
import java.util.List;

public class JpaMain {

    public static void main(String[] args) {

        //엔티티 매니저 팩토리 생성
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("jpabook");
        EntityManager em = emf.createEntityManager(); //엔티티 매니저 생성

        EntityTransaction tx = em.getTransaction(); //트랜잭션 기능 획득

        try {

            tx.begin(); //트랜잭션 시작
            logic(em);  //비즈니스 로직
            tx.commit();//트랜잭션 커밋

        } catch (Exception e) {
            e.printStackTrace();
            tx.rollback(); //트랜잭션 롤백
        } finally {
            em.close(); //엔티티 매니저 종료
        }

        emf.close(); //엔티티 매니저 팩토리 종료
    }

    public static void logic(EntityManager em) {

        String id = "id1";
        Member member = new Member();
        member.setId(id);
        member.setUsername("지한");
        member.setAge(2);

        //등록
        em.persist(member);

        //수정
        member.setAge(20);

        //한 건 조회
        Member findMember = em.find(Member.class, id);
        System.out.println("findMember=" + findMember.getUsername() + ", age=" + findMember.getAge());

        //목록 조회
        List<Member> members = em.createQuery("select m from Member m", Member.class).getResultList();
        System.out.println("members.size=" + members.size());

        //삭제
        em.remove(member);

    }
}
```

코드 구성
- 엔티티 매니저 설정
- 트랜잭션 관리
- 비지니스 로직

### 2.6.1 엔티티 매니저 설정

![1](https://user-images.githubusercontent.com/43127088/109125852-e91db900-778f-11eb-80be-15fcd1b0f195.PNG)

**엔티티 매니저 팩토리 생성**

- persistence.xml의 설정 정보를 사용해서 엔티티 매니저 팩토리 생성.
- Persistence 클래스 사용.
- Persistence 클래스 : 엔티티 매니저 팩토리를 생성해서 JPA를 사용할 수 있게 준비.
```java
EntityManagerFactory emf = Persistence.createEntityManagerFactory("jpabook");
```
`META-INF/persistence.xml에서 이름이 "jpabook"인 영속성 유닛을 찾아서 엔티티 매니저 팩토리를 생성.`
  
- 설정 정보 읽기.
- JPA 동작을 위한 기반 객체 만들기.
- JPA 구현체에 따라 데이터베이스 커넥션 풀도 생성.
- 비용이 아주 크다.

따라서 `엔티티 매니저 팩토리는 어플리케이션 전체에 딱 한 번만 생성하고 공유해서 사용해야 한다.`


**엔티티 매니저 생성**

```java
EntityManager em = emf.createEntityManager();
```

- 엔티티 매니저를 사용해서 엔티티를 데이터베이스에 등록/수정/삭제/조회할 수 있다.
- 엔티티 매니저는 데이터베이스 커넥션과 밀접한 관계가 있으므로 스레드간에 공유하거나 재사용하면 안된다.
  
**종료**

사용이 끝난 엔티티 매니저는 반드시 종료
```java
em.close();     // 엔티티 매니저 종료
```
어플리케이션을 종료할 때 엔티티 매니저 팩토리도 종료
```java
emf.close();    // 엔티티 매니저 팩토리 종료
```

### 2.6.2 트랜잭션 관리
JPA를 사용하면 항상 트랜잭션 안에서 데이터를 변경해야 한다.
트랜잭션 없이 데이터를 변경하면 예외가 발생한다.
트랜잭션을 시작하려면 엔티티 매니저에서 트랜잭션 API를 받아와야 한다.

```java
EntityTransaction tx = em.getTransaction(); //트랜잭션 API
try {

     tx.begin(); //트랜잭션 시작
     logic(em);  //비즈니스 로직
     tx.commit();//트랜잭션 커밋

} catch (Exception e) {
      tx.rollback(); //트랜잭션 롤백
}
```
x트랜잭션 API를 사용해서 비즈니스 로직이 정상 작동하면 트랜잭션을 커밋하고 에외가 발생하면 트랜잭션을 롤백한다.

### 2.6.3 비즈니스 로직
회원 엔티티를 하나 생성한 다음 엔티티 매니저를 통해 데이터베이스에 등록, 수정, 삭제, 조회

```java
String id = "id1";
Member member = new Member();
member.setId(id);
member.setUsername("지한");
member.setAge(2);

//등록
em.persist(member);

//수정
member.setAge(20);

//한 건 조회
Member findMember = em.find(Member.class, id);
System.out.println("findMember=" + findMember.getUsername() + ", age=" + findMember.getAge());

//목록 조회
List<Member> members = em.createQuery("select m from Member m", Member.class).getResultList();
System.out.println("members.size=" + members.size());

//삭제
em.remove(member);
```

`수정`

- em.update()를 호출할 것 같은데 없다.
- 단순하게 엔티티의 값만 변경.
- JPA는 어떤 엔티티가 변경되었는지 추적하는 기능을 갖추고 있음.
```sql
UPDATE MEMBER 
    SET AGE = 20, NAME = '지한'
WHERE ID = 'id1'    
```

`삭제`

엔티티 매니저의 remve() 메소드에 삭제하려는 엔티티를 넘겨준다. JPA는 다음 DELETE SQL을 생성해서 실행한다.
```sql
DELETE FROM MEMBER WHERE ID = 'id1'
```

`한 건 조회`
find() 메소드는 조회할 엔티티 타입과 @ID로 데이터베이스 테이블의 기본 키와 매핑한 식별자의 값으로 엔티티 하나를 조회하는 가장 단순한
조회 메소드다. 이 메소드를 호출하면 다음 SELECT SQL을 생성해서 데이터베이스에 결과를 조회한다. 그리고 조회한 결과 값으로 엔티티를
생성해서 반환한다.
```sql
SELECT * FROM MEMBER WHERE ID = `id1`
```

### 2.6.4 JPQL

하나 이상의 회원 목록 조회하는 코드  

```java
//목록 조회
List<Member> members = em.createQuery("select m from Member m", Member.class).getResultList();
System.out.println("members.size=" + members.size());
```

**문제점**

- 엔티티 대상으로 검색해야 함.
- 그러나 테이블이 아닌 엔티티 객체를 대상으로 검색하려면 데이터베이스 모든 데이터를 어플리케이션으로 불러와 엔티티 객체로 변경해서 검색해야 함.

**해결**

JPA는 *JPQL(Java Persistence Query Language)*라는 쿼리 언어로 해결.

**차이점**

JPQL : 엔티티 객체를 대상으로 쿼리. (클래스와 필드)
SQL : 데이터베이스 테이블을 대상으로 쿼리.

**샘플**

`select m from Member m`

- JPQL 표현
- from Member는 MEMBER 테이블이 아닌 Member 회원 엔티티 객체
- JPQL은 데이터베이스 테이블을 전혀 알지 못한다.
- JPA는 JPQL을 분석, 적절한 SQL을 만들어 데이터베이스에 데이터 조회.
- JPQL은 대소문자를 구분.

`SELECT M.ID, M.NAME, M.AGE FROM MEMBER M`



  </div>
</details>

<details>
  <summary>3.영속성 관리</summary>
  <div markdown="1">

## 3. 영속성 관리

매핑한 엔티티를 엔티티 매니저를 통해 어떻게 사용하는지

### JPA가 제공하는 기능
1. 엔티티와 테이블을 매핑하는 설계 부분
2. 매핑한 엔티티를 실제 사용하는 부분

## 엔티티 매니저 팩토리와 엔티티 매니저
데이터베이스를 하나만 사용하는 어플리케이션은 일반적으로 EntityManagerFactory를 하나만 생성.

**EntityManagerFactory를 얻기**
```java
// 비용이 아주 많이 든다.
//엔티티 매니저 팩토리 생성
EntityManagerFactory emf = Persistence.createEntityManagerFactory("jpabook");
```
- 만드는 비용이 상당히 큼.
- 한 개만 만들어서 어플리케이션 전체에서 공유하도록 설계.
- 여러 스레드가 동시에 접근해도 안전, 서로 다른 스레드 간 공유 가능.

### EntityManger 생성
```java
// 엔티티 매니저 생성, 비용이 거의 안든다.
EntityManager em = emf.createEntityManager(); //엔티티 매니저 생성
```
- 여러 스레드가 동시에 접근하면 동시성 문제 발생
- 스레드간 절대 공유하면 안된다.
- 데이터베이스 연결이 필요한 시점까지 커넥션을 얻지 않는다.

![2](https://user-images.githubusercontent.com/43127088/109153722-9d2f3c00-77b0-11eb-9300-29792fe0908b.PNG)

## 3.2 영속성 컨텍스트란?
- 가장 중요한 용어, "영속성 컨텍스트(persistence context)"
- 엔티티를 영구 저장하는 환경

`em.persist(member);`

- 단순하게 회원 엔티티 저장
- 정확하게는 엔티티 매니저를 사용해서 회원 엔티티를 영속성 컨텍스트에 저장
- 논리적인 개념에 가까움
- 엔티티 매니저를 생성할 때 하나 만들어진다.
- 엔티티 매니저를 통해 영속성 컨텍스트에 접근하고 관리할 수 있다.

![1](https://user-images.githubusercontent.com/43127088/109261763-b1704900-7843-11eb-88c5-24e7dd75da11.PNG)

## 3.3 엔티티의 생명주기

### 4가지 상태

**비영속(new/transient)**

영속성 컨텍스트와 전혀 관계가 없는 상태

**영속(managed)**

영속성 컨텍스트에 저장된 상태

**준영속(detached)**

영속성 컨텍스트에 저장되었다가 분리된 상태

**삭제(removed)**

삭제된 상태

### 생명주기
![2](https://user-images.githubusercontent.com/43127088/109262020-2e9bbe00-7844-11eb-9c4d-93dd09d2e3c4.PNG)


### 비영속
- 엔티티 객체를 생성.
- 순수한 객체 상태, 아직 저장하지 않음.
- 영속성 컨텍스트나 데이터베이스와 상관없음.

```java
// 객체를 생성한 상태(비영속)
Member member = new Member();
member.setId(100L);
member.setUsername("회원1");
```

em.persist() 호출 전, 비영속 상태
![3](https://user-images.githubusercontent.com/43127088/109262150-5ab73f00-7844-11eb-89ab-de3b25af40ca.PNG)

### 영속
- 엔티티 매니저를 통해 엔티티를 영속성 컨텍스트에 저장.
- 영속성 컨텍스트가 관리하는 엔티티를 영속 상태.
- 회원 엔티티 : 비영속 상태 => 영속 상태
- 영속상태 = 영속성 컨텍스트에 의해 관리된다는 뜻.
- em.find()나 JPQL를 사용해서 조회한 엔티티도 영속 상태.

```java
// 객체를 저장한 상태(영속)
em.persist(member);
```
![4](https://user-images.githubusercontent.com/43127088/109262217-7f131b80-7844-11eb-86f1-d785b3f3f764.PNG)


### 준영속

- 영속성 컨텍스트가 관리하던 영속 상태의 엔티티를 영속성 컨텍스트가 관리하지 않으면 "준영속 상태".
- em.detach() 호출로 준영속 상태 명시적 호출.
- em.close()를 호출해서 영속성 컨텍스트를 닫음.
- em.clear로 영속성 컨텍스트 초기화

```java
// 회원 엔티티를 영속성 컨텍스트에서 분리, 준영속 상태
em.detach(member);
```

### 삭제

엔티티를 영속성 컨텍스트와 데이타베이스에서 삭제.

```java
// 객체를 삭제한 상태(삭제)
em.remove(member);
```

## 3.4 영속성 컨텍스트의 특징
- 영속성 컨텍스트와 식별자 값
  - 엔티티를 식별자 값(@id로 테이블의 기본 키와 매핑한 값)으로 구분
  - 영속 상태는 식별자 값이 반드시 있어야 한다.
  - 식별자 값이 없으면 예외 발생.
- 영속성 컨텍스트와 데이터베이스 저장
  - JPA는 보통 트랜잭션을 커밋하는 순간 영속성 컨텍스트에 새로 저장된 엔티티를 데이터베이스에 반영
  - 플러시(flush)
- 영속성 컨텍스트가 엔티티를 관리하는 것의 장점
  - 1차 캐시
  - 동일성 보장
  - 트랜잭션을 지원하는 쓰기 지연
  - 변경 감지
  - 지연 로딩
  
### 3.4.1 엔티티 조회
- 영속성 컨텍스트는 내부에 캐시를 가지고 있음 => 1차 캐시
- 영속 상태의 엔티티는 모두 이곳에 저장
  
`영속성 컨텍스트 내부에 Map이 하나 있음
키는 @Id로 매핑한 식별자이고 값은 엔티티 인스턴스`

```java
// 엔티티를 생성한 상태(비영속)
Member member = new Member();
member.setId(100L);
member.setUsername("회원1");

// 엔티티 영속
em.persist(member);
```
![5](https://user-images.githubusercontent.com/43127088/109262495-019bdb00-7845-11eb-9574-1766e4673ae4.PNG)

- 1차 캐시의 키는 식별자 값
- 식별자 값은 데이타베이스 기본 키와 매핑
- 영속성 컨텍스트에 데이터를 저장하고 조회하는 모든 기준은 데이타베이스 기본 키 값.
  
**엔티티 조회**
```java
Member member = em.find(Member.class, 100L);
```

`em.find() 호출 => 1차 캐시에서 엔티티 조회 엔티티가 1차 캐시에 없으면 데이터베이스 조회`

**1차 캐시에서 조회**
- em.find() 호출
  - 우선 1차 캐시에서 식별자 값으로 엔티티 찾음.
  - 찾는 엔티티가 있으면 데이타베이스 조회하지 않고, 메모리에 있는 1차 캐시에서 엔티티를 조회

**1차 캐시에서 조회**
- em.find() 호출
  - 우선 1차 캐시에서 식별자 값으로 엔티티 찾음.
  - 찾는 엔티티가 있으면 데이타베이스 조회하지 않고, 메모리에 있는 1차 캐시에서 엔티티를 조회
  
![7](https://user-images.githubusercontent.com/43127088/109262740-6a835300-7845-11eb-9a9e-eef0b4a10e70.PNG)

**1차 캐시에 있는 엔티티 조회**
```java
Member member = new Member();
member.setId(100L);
member.setUsername("회원1");

// 1차 캐시에 저장됨
em.persist(member);

// 1차 캐시에서 조회
Member findMember = em.find(Member.class, 100L);
```

**데이터베이스에서 조회**
- 엔티티가 1차 캐시에 없으면 엔티티 매니저는 데이터베이스를 조회해서 엔티티를 생성.
- 1차 캐시에 저장한 후에 영속 상태의 엔티티를 반환.
  
![1](https://user-images.githubusercontent.com/43127088/109262836-93a3e380-7845-11eb-8cbe-7ac8f3b16fce.PNG)

**분석**
1. em.find(Member.class, 200L)를 실행.
2. member2가 1차 캐시에 없으므로 데이터베이스에서 조회.
3. 조회한 데이터로 member2 엔티티를 생성해서 1차 캐시에 저장한다.(영속상태)
4. 조회한 엔티티를 반환

**영속 엔티티의 동일성 보장**
```java
Member a = em.find(Member.class, "member1");
Member b = em.find(Member.class, "member1");

System.out.println(a == b); // 동일성 비교
```

- 결과는 "참"
- 둘은 같은 인스턴스
- 영속성 컨텍스트는 성능상 이점과 엔티티의 동일성을 보장한다.
  
### 3.4.2 엔티티 등록

**엔티티 등록 코드**
```java
EntityManager em = emf.createEntityManager();
EntityTransaction transaction = em.getTransaction();

// 엔티티 매니저는 데이터 변경 시 트랜잭션을 시작해야 한다.
transaction.begin();    // 트랜잭션 시작

em.persist(memberA);
em.persist(memberB);
// 여기까지 INSERT SQL을 데이터베이스에 보내지 않는다.

// 커밋하는 순간 데이터베이스에 INSERT SQL을 보낸다.
transaction.commit();   // 트랜잭션 커밋
```
- 엔티티 매니저는 트랜잭션을 커밋하기 직전까지 데이터베이스의 엔티티를 저장하지 않음.
- 내부 쿼리 저장소에 INSERT SQL을 차곡차곡 모아둔다.
- 트랜잭션 커밋할 때 모아둔 쿼리를 데이터베이스에 보낸다.
- 트랜잭션을 지원하는 쓰기 지연
  
**쓰기 지연, 회원 A 영속**
![2](https://user-images.githubusercontent.com/43127088/109263097-090fb400-7846-11eb-9e4f-5d6e8e968d0d.PNG)
**쓰기 지연, 회원 B 영속**
![3](https://user-images.githubusercontent.com/43127088/109263103-0ad97780-7846-11eb-91a4-53bca84b77c2.PNG)
**트랜잭션 커밋, 플러시, 동기화**
![4](https://user-images.githubusercontent.com/43127088/109263105-0b720e00-7846-11eb-98ea-dd0482311123.PNG)

1. 트랜잭션 커밋.
2. 엔티티 매니저 -> 영속성 컨텍스트 플러시.
3. 데이타베이스 동기화 -> 등록, 수정, 삭제한 엔티티를 DB에 반영.
  - 쓰기 지연 SQL 저장소에 모인 쿼리를 데이터베이스에 보낸다.
4. 마지막으로 실제 데이터베이스 트랜잭션 커밋.

`트랜잭션 범위 안에서 실행.
등록 쿼리를 그때 그때 데이타베이스에 전달해도 트랜잭션을 커밋하지 않으면 아무 소용이 없음.`

### 3.4.3 엔티티 수정
- JPA는 엔티티를 수정할 때는 단순히 엔티티를 조회해서 데이터만 변경하면 된다.
- update()라는 메소드 없음.
- 변경 감지 기능을 사용해서 데이타베이스에 자동 반영
  
![5](https://user-images.githubusercontent.com/43127088/109263245-4bd18c00-7846-11eb-8089-7c1ef49262a9.PNG)

`플러시 시점에 스냅샷과 엔티티를 비교`

**수정 순서**
1. 트랜잭션 커밋 -> 엔티티 매니저 내부에서 먼저 플러시 호출
2. 엔티티와 스냅샷을 비교해서 변경된 엔티티 찾는다.
3. 변경된 엔티티가 있으면 수정 쿼리를 생성해서 쓰기 지연 SQL 저장소로 보낸다.
4. 쓰기 지연 저장소의 SQL을 데이터베이스로 보낸다.
5. 데이터베이스 트랜잭션을 커밋

`변경감지는 영속성 컨텍스트가 관리하는 영속 상태의 엔티티에만 적용된다.`

**업데이트 기본 전략**

`JPA의 기본전략은 모든 필드를 업데이트한다.`
- 모든 필드를 사용하면 수정 쿼리가 항상 같음.
- 동일한 쿼리를 보내면 데이터베이스는 이전에 파싱된 쿼리는 재사용.`

**필드가 많거나 저장되는 내용이 큰 경우**
`하이버네이트 확장 기능 사용`

```java
@Entity
@org.hibernate.annotation.DynamicUpdate
@Table(name = "Member")
public class Member {...}   
```

수정된 데이터만 사용해서 동적으로 UPDATE SQL을 생성

### 3.4.4 엔티티 삭제
엔티티를 삭제하려면 먼저 삭제 대상 엔티티를 조회해야 한다.

```java
Member memberA = em.find(Member.class, 100L);  // 삭제 대상 엔티티 조회
em.remove(memberA);     // 엔티티 삭제
```

- 엔티티를 즉시 삭제하는 것이 아님
- 삭제 쿼리를 쓰기 지연 SQL 저장소에 등록
- em.remove(memberA)를 호출하는 순간 영속성 컨텍스트에서 제거

## 3.5 플러시

플러시(flush())는 영속성 컨텍스트의 변경 내용을 데이터베이스에 반영한다.

**플러시 실행**

1. 변경 감지 동작. 모든 엔티티를 스냅샷과 비교.
2. 수정된 엔티티는 수정쿼리를 만들어 쓰기 지연 SQL 저장소 등록.
3. 쓰기 지연 SQL 저장소의 쿼리를 데이터베이스에 전송.
  - 등록, 수정, 삭제 쿼리

**영속성 컨텍스트를 플러시하는 방법 3가지**

1. em.flush() 직접 호출.
2. 트랜잭션 커밋시 플러시가 자동 호출.
3. JPQL (Java Persistence Query Langauge) 쿼리 실행시 플러시가 자동 호출.

**직접 호출**

테스트나 다른 프레임워크와 JPA를 함께 사용할 때를 제외하고 거의 사용하지 않음

**트랜잭션 커밋 시 플러시 자동 호출**
- 트랜잭션 커밋하기 전에 꼭 플러시 호출해서 변경 내용을 데이터베이스에 반영해야 함.
- JPA는 트랜잭션 커밋할 때 플러시를 자동 호출.
  
**JPQL 쿼리 실행 시 플러시 자동 호출**
```java
em.persist(memberA);
em.persist(memberB);
em.persist(memberC);
```

```java
// 중간에 조회
query = em.createQuery("select m from Member m", Member.class);
List<Member> members = query.getResultList();
```

- memberA, memberB, memberC는 데이터베이스에 없음.
- 이런 문제를 해결하기 위해 JPQL 실행 시에 플러시 자동 호출.
- memberA, memberB, memberC는 쿼리 결과에 포함.
  
**플러시 모드 옵션**
- 별도로 설정하지 않으면 AUTO 동작.
- 대부분 AUTO 기본 설정을 그대로 사용.

FlushModeType.AUTO : 커밋이나 쿼리를 실행할 때 플러시(기본값)
FlushModeType.COMMIT : 커밋할 때만 플러시

`플러시는 영속성 컨텍스트의 변경 내용을 데이터베이스에 동기화.
영속성 컨텍스트에 보관된 엔티티를 지우는 것이 아님.

## 3.6 준영속
준영속 상태를 만드는 방법 3가지

 - em.detach(entity) : 특정 엔티티만 준영속 상태로 전환
 - 	em.clear() : 영속성 컨텍스트를 완전히 초기화
 - em.close() : 영속성 컨텍스트를 종료

### 3.6.1 엔티티를 준영속 상태로 전환 : detach()
```java
public void testDetached() {
    ...
    // 회원 엔티티 생성, 비영속 상태
    Member member = new Member();
    member.setId("memberA");
    member.setUsername("회원A");

    // 회원 엔티티 영속 상태
    em.persist(member);

    // 회원 엔티티를 영속성 컨텍스트에서 분리, 준영속 상태
    em.detach(member);

    transaction.commit();   //트랜잭션 커밋
}
```

- em.detach(member)
- 해당 엔티티를 관리하지 말라는 것
- 메소드를 호출하는 순간 1차 캐시부터 쓰기 지연 SQL저장소까지 해당 엔티티를 관리하기 위한 모든 정보가 제거

![1](https://user-images.githubusercontent.com/43127088/109280665-aecf1d00-785e-11eb-8285-4aa640910b99.PNG)
![2](https://user-images.githubusercontent.com/43127088/109280673-b098e080-785e-11eb-96a1-316e544bc452.PNG)

- 영속 상태였다가 더는 형속성 컨텍스트가 관리하지 않는 상태를 준영속 상태.
- 준영속 상태는 영속성 컨텍스트로부터 "분리된 상태".

### 3.6.1 영속성 컨텍스트를 초기화 : clear()

영속성 컨텍스트를 초기화해서 해당 영속성 컨텍스트의 모든 엔티티를 "준영속 상태"로 만든다.

```java
//엔티티 조회, 영속 상태
Member member = em.find(Member.class, "memberA");

em.clear();     //영속성 컨텍스트 초기화

//준영속 상태
member.setUsername("changeName");
```

**영속성 컨텍스트 초기화 전**
![3](https://user-images.githubusercontent.com/43127088/109281063-2d2bbf00-785f-11eb-883a-bd5c3d53b6e6.PNG)

**영속성 컨텍스트 초기화 후**
![4](https://user-images.githubusercontent.com/43127088/109281070-2ef58280-785f-11eb-916c-3783ca9877d6.PNG)
memberA, memberB -> 준영속 상태

### 3.6.1 영속성 컨텍스트 종료 : close()

영속성 컨텍스트를 종료하면 해당 영속성 컨텍스트가 관리하던 영속 상태의 엔티티가 모두 준영속 상태가 된다.

```java
public void closeEntityManager() {

    EntityManagerFactory emf = 
        Persistence.createEntityManagerFactory("jpabook");

    EntityManager em = emf.createEntityManager();
    EntityTransaction transaction = em.getTransaction();

    transaction.begin();    // [트랜잭션] - 시작

    Member memberA = em.find(Member.class, "memberA");
    Member memberB = em.find(Member.class, "memberB");

    transaction.commit();   // [트랜잭션] = 커밋

    em.close();     // 영속성 컨텍스트 닫기(종료)
}
```

**영속성 컨텍스트 제거 전**
![1](https://user-images.githubusercontent.com/43127088/109281699-f904ce00-785f-11eb-9bd1-ed54a2ae9576.PNG)

**영속성 컨텍스트 제거 후**

더는 memberA, memberB를 관리하지 않는다.
![2](https://user-images.githubusercontent.com/43127088/109281702-face9180-785f-11eb-8b1e-611c664cd199.PNG)

개발자가 직접 준영속 상태를 만드는 일은 드물다.

### 3.6.4 준영속 상태의 특징

**거의 비영속 상태에 가깝다.**

1차 캐시, 쓰기 지연, 변경 감지, 지연 로딩을 포함한 영속성 컨텍스트가 제공하는 어떤 기능도 동작하지 않는다.

**식별자 값을 가지고 있다.**

준영속 상태는 이미 한 번 영속 상태였으므로 반드시 식별자 값을 가지고 있다.

**지연 로딩을 할 수 없다.**

지연 로딩시 문제가 발생.

`지연 로딩 - 실제 객체 대신 프록시 객체를 로딩해두고 해당 객체를 실제 사용할 때 영속성 컨텍스트를 통해 데이터를 불러오는 방법

### 3.6.5 병합 : merge()`
- 준영속 상태의 엔티티를 다시 영속 상태로 변경하려면 병합(merge)를 사용하면 된다.
- merge() 메소드는 준영속 상태의 엔티티를 받아서 그 정보로 새로운 영속 상태의 엔티티를 반환.
  
```java
// merge() 메소드 정의
public <T> T merge(T entity);

// 사용 예
Member mergeMember = em.merge(member);
```

**준영속 병합**

![5](https://user-images.githubusercontent.com/43127088/109282092-703a6200-7860-11eb-90b3-d6b6d0e40b62.PNG)

1. merge()를 실행한다.
2. 파라미터로 넘어온 준영속 엔티티의 식별자 값으로 1차 캐시에서 엔티티를 조회
3. 만약 1차 캐시에 엔티티가 없으면 데이터베이스에서 엔티티를 조회하고 1차 캐시에 저장
4. 조회한 영속 엔티티(mergeMember)에 member 엔티티의 값을 채워 넣는다.(member 엔티티의 모든 값을 mergeMember에 밀어넣는다. 이때 mergeMember의 "회원1"이라는 이름이 "회원명 변경"으로 바뀐다.
5. mergeMember를 반환.

**비영속 병합**
```java
Member member = new Member();
Member newMember = em.merge(member);    //비영속 병합
tx.commit();
```

1. 파라미터로 넘어온 엔티티의 식별자 값으로 영속성 컨텍스트를 조회.
2. 엔티티가 없으면 데이터베이스에서 조회.
3. 데이터베이스에서도 발견하지 못하면 새로운 엔티티를 생성해서 병합.

`
병합은 준영속, 비영속을 신경쓰지 않고,
식별자 값으로 엔티티를 조회할 수 있으면 불러서 병합하고,
조회할 수 없으면 새로 생성해서 병합한다.
`

## 3.7 트랜잭션 범위의 영속성 컨텍스트

**스프링 컨테이너의 기본 전략**
![1](https://user-images.githubusercontent.com/43127088/109282573-0b333c00-7861-11eb-919b-fd2b48cdb40f.PNG)

**@Transaction 어노테이션, 트랜잭션 AOP**
![2](https://user-images.githubusercontent.com/43127088/109282579-0c646900-7861-11eb-80c9-dd8eac6edca3.PNG)

**트랜잭션이 같으면 같은 영속성 컨텍스트 사용**
![3](https://user-images.githubusercontent.com/43127088/109282580-0c646900-7861-11eb-854e-392cc0611414.PNG)

**트랜잭션이 다르면 다른 영속성 컨텍스트를 사용**
![4](https://user-images.githubusercontent.com/43127088/109282581-0cfcff80-7861-11eb-90b7-7a139fc2862c.PNG)



  </div>
</details>

<details>
  <summary>4.엔티티 매핑</summary>
  <div markdown="1">

## 4. 엔티티 매핑

대표적인 매핑 어노테이션

XML에 기입해도 되지만 어노테이션 방식이 좀 더 쉽고 직관적

객체와 테이블 매핑 : @Entity, @Table
기본 키 매핑 : @Id
필드와 컬럼 매핑 : @Column
연관관계 매핑 : @ManyToOne, @JoinColumn

### 4.1 Entity

JPA를 사용해서 테이블과 매핑할 클래스는 @Entity 어노테이션을 필수로 붙여야 한다.

@Entity가 붙은 클래스는 JPA가 관리.

**@Entity 적용시 주의사항**

- 기본 생성자는 필수. (파라미터가 없는 public 또는 protected 생성자)
- final 클래스, enum, interface, inner 클래스에는 사용할 수 없다.
- 저장할 필드에 final을 사용하면 안된다

JPA가 엔티티 객체를 생성할 때 기본 생성자를 사용하므로 이 생성자는 반드시 있어야 한다. 자바는 생성자가 하나도 없으면 다음과 같은 기본
셍성자를 자동으로 만든다.

```java
public Member() {}  // 기본 생성자
```

문제는 다음과 같이 생성자를 하나 이상 만들면 자바는 기본 생성자를 자동으로 만들지 않는다. 이때는 기본 생성자를 직접 만들어야 한다.

```java
public Member() {}  // 기본 생성자

public Member(String name) {
    this.name = name;
}
```

### 4.2 @Table

엔티티와 매핑할 테이블을 지정한다.

```java
@Entity
@Table(name="MEMBER")
public class Member {
    ...
}
```

### 4.3 다양한 매핑 사용

```java
package jpabook.start;

import javax.persistence.*;  
import java.util.Date;

@Entity
@Table(name="MEMBER")
public class Member {

    @Id
    @Column(name = "ID")
    private String id;

    @Column(name = "NAME", nullable = false, length = 10) //추가
    private String username;

    private Integer age;

    @Enumerated(EnumType.STRING)
    private RoleType roleType;

    @Temporal(TemporalType.TIMESTAMP)
    private Date createdDate;

    @Temporal(TemporalType.TIMESTAMP)
    private Date lastModifiedDate;

    @Lob
    private String description;

    @Transient
    private String temp;

    //Getter, Setter

    ...
}


package jpabook.start;

public enum RoleType {
    ADMIN, USER
}
```

코드 설명

1. roleType : 자바의 enum을 사용해서 회원 타입을 구분. 자바의 enum을 사용하려면 @Enumerated 어노테이션으로 매핑.
2. createDate, lastModifiedDate : 자바의 날짜 타입은 @Temporal을 사용해서 매핑
3. description : 회원을 설명하는 필드는 길이 제한이 없다. 데이타베이스 VARCHAR 타입 대신에 CLOB 타입으로 저장. 
   @Lob를 사용하면 CLOB, BLOB 타입을 매핑할 수 있다.
   
### 4.4 데이터베이스 스키마 자동 생성

JPA는 데이터베이스 스키마를 자동으로 생성하는 기능을 지원

```java
<property name="hibernate.hbm2ddl.auto" value="create" />
```

어플리케이션 실행 시점에 데이터베이스 테이블을 자동으로 생성한다.

```java
<property name="hibernate.show_sql" value="true" />
```

콘솔에 실행되는 DDL을 출력한다.

- 스키마 자동 생성 기능이 만든 DDL은 운영환경에서 사용할 만큼 완벽하지 않음.
- 개발 환경에서 사용하거나 매핑시 참고하는 용도로 사용.

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

**HBM2DDL 주의사항**

운영 서버에서 create, create-drop, update처럼 DLL을 수정하는 옵션은 절대 사용하면 안된다. 오직 개발 서버나 개발 단계에서만 사용
해야한다. 이 옵션들은 운영 중인 데이터베이스의 테이블이나 컬럼을 삭제할 수 있다.

개발 환경에 따른 추천 전략은 다음과 같다.
- 개발 초기 단계는 create 또는 update
- 초기화 상태로 자동화된 테스트를 진행하는 개발자 환경과 CI 서버는 create 또는 create-drop
- 테스트 서버는 update 또는 validate
- 스테이징과 운영 서버는 validate 또는 none

**이름 매핑 전략**

테이블 명이나 컬럼 명이 생략되면 자바의 카멜케이스 표기법을 언더스코어 표기법으로 매핑한다

하이버네이트는 `org.hibernate.cfg.ImprovedNamingStrategy 클래스를 제공한다.` 이 클래스는
테이블 명이나 컬럼 명이 생략되면 자바의 카멜케이스 표기법을 언더스코어 표기법으로 매핑한다.
```java
<property name="hibernate.ejb.naming_strategy" value="org.hibernate.cfg.ImprovedNamingStrategy" />
```

## 4.5 DDL 생성 기능

제약 조건을 추가할 수 있다.

```java
@Entity
@Table(name="MEMBER")
public class Member {

    @Id
    @Column(name = "ID")
    private String id;

    @Column(name = "NAME", nullable = false, length = 10) //추가
    private String username;
    ...
}
```

- nullable = false : not null 제약 조건 추가
- length = 10 : 크기를 지정

```java
create table MEMBER (
    ...
    NAME varchar(10) not null,
    ...
)
```

단지 DDL을 자동으로 생성할 때만 사용되고, JPA 실행 로직에는 영향을 주지 않는다. 따라서 스키마 자동 생성 기능을 사용하지 않고 직접
DDL을 만든다면 사용할 이유가 없다. 이 기능을 사용하면 애플리케이션 개발자가 엔티티만 보고도 손쉽게 다양한 제약 조건을 파악할 수 있는 장점이 있다.

### 4.6 기본 키 매핑
```java
@Entity
public class Member {

    @Id
    @Column(name = "ID")
    private String id;
```

**JPA가 제공하는 데이터베이스 기본 키 생성 전략**

데이터베이스 벤더마다 기본 키 생성을 지원하는 방식이 다름

**기본키 생성 전략 방식**
직접 할당 : 기본 키를 어플리케이션이 직접 할당

자동 생성 : 대리 키 사용 방식
- INDENTITY : 기본 키 생성을 데이터베이스에 위임.
- SEQUENCE : 데이터베이스 시퀀스를 사용해서 기본 키를 할당.
- TABLE : 키 생성 테이블을 사용한다.

**기본키 생성 방법**
- 기본 키를 직접 할당 : @Id만 사용
- 자동 생성 전략 사용 : @GeneratedValue 추가 및 키 생성 전략 선택.

**키 생성 전략 사용을 위한 속성 추가**
```java
<property name="hibernate.id.new_generator_mappings" value="true" />
```

### 4.6.1 기본 키 직접 할당 전략

기본 키를 직접 할당하려면 다음 코드와 같이 @Id로 매핑하면 된다.
```java
@Id
@Column(name = "id")
private String id;
```

**@Id 적용 가능한 자바 타입**
- 자바 기본형
- 자바 래퍼형
- String
- java.util.Date
- java.sql.Date
- java.math.BigDecimal
- java.math.BigInteger

**기본 키 할당하는 방법**
```java
Board board = new Board();
board.setId("id1");         //기본 키 직접 할당
em.persist(board);
```

### 4.6.2 IDENTITY 전략
- IDENTITY는 기본 키 생성을 데이타베이스에 위임하는 전략.
- 주로 MySQL, PostgreSQL, SQL Server, DB2, H2에서 사용.

**MySQL의 AUTO_INCREMENT 기능**
```java
CREATE TABLE BOARD {
    ID INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    DATA VARCHAR(255)
};

INSERT INTO BOARD(DATA) VALUES('A');
INSERT INTO BOARD(DATA) VALUES('B');
```

Board 테이블 결과

|    ID  | DATA | 
| ----------| ------------------- |
|1|A|
|2|B|

IDENTITY 전략
- 데이터베이스에 값을 저장하고 나서야 기본 키 값을 구할 수 있을 때 사용.
- em.persist() 호출시 INSERT SQL을 즉시 데이터베이스에 전달.
- 식별자를 조회해서 엔티티의 식별자에 할당.
- 쓰기 지연이 동작하지 않는다.

```java
@Entity
public class Professor {
  @Id 
  @GeneratedValue(strategy=GenerationType.IDENTITY)
  private int id;
  private String name;
  private long salary;
  ...
}
```


### 4.6.3 SEQUENCE 전략
유일한 값을 순서대로 생성하는 특별한 데이터베이스 오브젝트
주로 오라클, PostgreSQL, DB2, H2 데이터베이스에서 사용.

**시퀀스 관련 SQL**
```java
CREATE TABLE BOARD (
    ID BIGINT NOT NULL PRIMARY KEY,
    DATA VARCHAR(255)
)

//시퀀스 생성
CREATE SEQUENCE BOARD_SEQ START WITH 1 INCREMENT BY 1;
```

**시퀀스 매핑 코드**
```java
@Entity
@SequenceGenerator(
    name = "BOARD_SEQ_GENERATOR",
    sequenceName = "BOARD_SEQ",
    initialValue = 1,
    allocationSize = 1)
public class Board {

    @Id
    @GeneraedValue(strategy = GenerationType.SEQUNCE,
                    generator = "BOARD_SEQ_GENERATOR")
    private Long id;
}
```

**시퀀스 사용 코드**
```java
private static void logic(EntityManager em) {
    Board board = new Board();
    em.persist(board);
    System.out.println("board.id = " + board.getId());
}
```
시퀀스 사용 코드는 IDENTITY 전략과 가티만 내부 동작 방식은 다르다.

1. 먼저 데이터베이스 시퀀스를 사용해서 식별자를 조회.
2. 조회한 식별자를 엔티티에 할당.
3. 엔티티를 영속성 컨텍스트에 저장.
4. 트랜잭션 커밋.
5. 플러시 - 데이터베이스에 저장.

**주의**
- equenceGenerator.allocationSize 기본값이 50.
- 반드시 1로 설정.

**@SepuenceGenerator**

|    속성  | 기능 | 기본값 |
|----------| ------------------- | ------------------- |
|name|식별자 생성기 이름|필수|
|sequenceName|데이터베이스에 등록되어 있는 시퀀스 이름|hibernate_sequence|
|initialValue|DDL 생성 시에만 사용됨. 시퀀스 DDL을 생성할 때 처음 시작 하는수를 지정한다.|1|
|allocationSize|시퀀스 한 번 호출에 증가하는 수(성능 최척화에 사용됨)|50|
|catalog.schema|데이터베이스 catalog, schema 이름|

매핑할 DDL은 다음과 같다.
```sql
create sequence [sequenceName] 
start with [initialValue] increment by allocationSize
```

jpa 표준 명세서에는 SEQUENCEnAME의 기본 값을 jpa 구현체가 정의하도록 헀다. 
위에서 설명한 기본값은 하이버네이트 기준이다.

### 4.6.4 TABLE 전략
- 키 생성 전용 테이블을 하나 만들고 여기에 이름과 값을 사용할 컬럼을 만들어 데이터베이스 시퀀스를 흉내내는 전략.
- 테이블을 사용하므로 모든 데이터베이스에 적용 할 수 있다.

**TABLE 전략 키 생성 테이블**

```sql
create table MY_SEQUENCES (
    sequence_name varchar(255) not null,
    next_val bigint,
    primary key (sequence_name)
)
```

SEQUENCE_NAME 컬럼을 시퀀스 이름으로 사용하고 NEXT_VAL 컬럼을 시퀀스 값으로 사용한다. 참고로
컬럼의 이름은 변경할 수 있는데 여기서 사용한 것이 기본 값이다.

**TABLE 전략 매핑 코드**
```sql
@Entity
@TableGenerator(
    name = "BOARD_SEQ_GENERATOR",
    table = "MY_SEQUENCES",
    pkColumnValue = "BOARD_SEQ", allocationSize = 1)
public class Board {

    @Id
    @GeneratedValue(strategy = GenerationType.TABLE,
                generator = "BOARD_SEQ_GENERATOR")
    private Long id;
}
```
1. @TableGenerator을 사용해서 테이블 키 생성기를 등록 (BOARE_SEQ_GENERATOR라는 이름의 테이블 키 생성기를 등록하고 방금 생성한
   My_SEQUENCES 테이블을 키 생성용 테이블로 매핑함)
2. TABLE 전략을 사용하기 위해 GenerationType.TABLE을 선택했다.
3. @GeneratedBalue.generator에 방금 만든 테이블 키 생성기를 지정.
4. 이제부터 id 식별자 값은 BOARD_SEQ_GENERATOR 테이블 키 생성기가 할당.

**TABLE 전략 매핑 사용 코드**
```sql
private static void logic(EntityManger em) {
    Board board = new Board();
    em.persist(board);
    System.out.println("board.id = " + board.getId());
}
// 출력 : board.id = 1
```

- MY_SEQUENCES 테이블에 값이 없으면 JPA가 값을 INSERT
- 미리 넣어둘 필요가 없다.

시퀀스 대신에 테이블을 사용한다는 것만 제외하면 SEQUENCE 전략과 동일

**@TableGenerator**

`@TableGenerator` 속성 정리

|    속성  | 기능 | 기본값 |
|----------| ------------------- | ------------------- |
|name|식별자 생성기 이름|필수|
|table|키생성 테이블명|hibernate_sequences|
|table|키생성 테이블명|hibernate_sequences|
|pkCoumnName|시퀀스 컬럼명|sequence_name|
|valueColumnName|시퀀스 값 컬럼명|next_val|
|pkColumnValue|키로 사용할 값 이름|엔티티이름|
|initialValue|초기 값, 마지막으로 생성된 값이 기준이다.|0|
|allocationSize|시퀀스 한 번 호출에 증가하는 수(성능 최적회에 사용됨)|50|
|catalog, schema|데이터베이스 catalog, schema 이름||
|uniqueConstraints(DDL)|유니크 제약 조건을 지정할 수 있다.||

JPA 표준 명세서에는 table, pkColumnName, valueColumnName의 기본값을 JPA 구현체가 정의하도록 했다. 위에서 설명한
기본값은 하이버네이트 기준이다.

`표 4.8 매핑할 DDL,테이블명{table}`
|    {pkColumnName}  | {valueColumnName} |
|----------| ------------------- |
|{pkColumnName}|{initialValue}|

TABLE 전략과 최적화

TABLE 전략은 값을 조회하면서 SELECT 쿼리를 사용하고 다음 값으로 증가시키기 위해 UPDATE 쿼리를 사용한다.
이 전략은 SEQUENCE 전략과 비교해서 데이터베이스와 한 번 더 통신하는 단점이 있다. TABLE 전략을 최적화하려면 @TableGenerator.allocationSize
를 사용하면 된다. 이 값을 사용해서 최적화하는 방법은 SEQUENCE 전략고 같다.

### 4.6.5 AUTO 전략

GenerationType.AUTO는 선택한 데이터베이스 방언에 따라 INDENTITY, SEQUENCE, TABLE 전략 중 하나를 자동으로 선택.

**AUTO 전략 매핑 코드**
```java
@Entity
public class Board {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;
    ...
}
```
@GeneratedValue.strategy의 기본값은 AUTO.

다음과 같이 사용해도 결과는 같다.

```java
import javax.persistence.GeneratedValue;

@Id @GeneratedValue
private Long id;
```
**장점**
- 데이터베이스를 변경해도 코드를 수정할 필요가 없다.
- 키 생성 전략이 확정되지 않은 개발 초기 단계, 프로토타입 개발시 편리.
  
### 4.6.6 기본 키 매핑 정리
영속성 컨텍스트는 엔티티를 식별자 값으로 구분하므로 엔티티를 영속 상태로 만들려면 식별자 값이
반드시 있어야 한다. em.persist()를 호출한 직후에 발생하는 일을 식별자 할당 전략별로 정리하면 다음과 같다.

- 직접 할당 : em.persist()를 호출하기 전에 애플리케이션에서 직접 식별자 값을 할당해야 한다. 만약 식별자 값이 없으면
  예외가 발생한다.
- SEQUENCE : 데이터베이스 시퀀스에서 식별자 값을 획득한 후 영속성 컨텍스트에 저장한다.
- TABLE : 데이터베이스 시퀀스 생성용 테이블에서 식별자 값을 획득한 후 영속성 컨텍스트에 저장한다.
- IDENTITY : 데이터베이스에 엔티티를 저장해서 식별자 값을 획득한 후 영속성 컨텓스트에 저장한다(IDENTITY 전략은 테이블에
  데이터를 저장해야 식별자 값을 획득할 수 있다.)
  
**권장하는 식별자 선택 전략**
데이터베이스 기본 키는 다음 3가지 조건을 모두 만족해야 한다.
1. null값을 허용하지 않는다.
2. 유일해야 한다.
3. 변해선 안 된다.

테이블의 기본 키를 선택하는 전략은 크게 2가지가 있다.
- 자연 키
  - 비지니스에 의미가 있는 키
  - 주민등록번호, 이메일, 전화번호
- 대리 키
  - 비지니스와 관련 없는 임의로 만들어진 키, 대체 키.
  - 오라클 시퀀스, auto_increment, 키생성 테이블


기본 키의 조건을 현재는 물론이고 미래까지 충족하는 자연 키를 찾기는 쉽지 않다.
대리 키는 비즈니스와 무관한 임의의 값이므로 요구사항이 변경되어도 기본 키가 변경되는 일은 드물다. 
대리 키를 기본키로 사용하되 주민등록번호나 이메일처럼 자연 키의 후보가 되는 컬럼들은 필요에 따라 유니크 인덱스를 설정해서
사용하는 것을 권장한다. `그래서 JPA는 모든 엔티티에 일관된 방시으로 대리 키 사용을 권장한다.`

`주의`
기본 키는 변하면 안된다는 기본 원칙으로 인해, 저장된 엔티티의 기본 키 값은 절대 변경하면 안 된다. 이 경우 JPA는 예외를 발생
시키거나 정상 동작하징 ㅏㄶ는다. setId() 같이 식별자를 수정하는 메소드를 외부에 공개하지 않는 것도 문제를 예방하는 
방법이 될 수 있다.

## 4.7 필드와 컬럼 매핑 : 레퍼런스
필드와 컬럼 매핑 

@Column : 컬럼을 매핑한다

@Enumerated : 자바의 enum 타입을 매핑한다.

@Temporal : 날짜 타입을 매핑한다.

@Lob : BLOB, CLOB 타입을 매핑한다.

@Transient : 특정 필드를 데이터베이스에 매핑하지 않는다.

기타

@Access : JPA가 엔티티에 접근하는 방식을 지정한다.

**@Column**

@Column의 속성

- name : 맵핑할 테이블의 컬럼 이름을 지정합니다;
- insertable : 엔티티 저장시 선언된 필드도 같이 저장합니다.
- updateable : 엔티티 수정시 이 필드를 함께 수정합니다.
- table : 지정한 필드를 다른 테이블에 맵핑할 수 있도록 합니다.
- nullable : NULL을 허용할지, 허용하지 않을지 결정합니다.
- unique : 제약조건을 걸 때 사용합니다.
- columnDefinition : DB 컬럼 정보를 직접적으로 지정할 때 사용합니다.
- length : varchar의 길이를 조정합니다. 기본값으로 255가 입력됩니다.
- precsion, scale : BigInteger, BigDecimal 타입에서 사용합니다.  각각 소수점 포함 자리수, 소수의 자리수를 의미합니다.

**@Enumerated**
- value(속성)
  - EnumType.ORDINAL : enum 순서를 데이터베이스에 저장 (기본 값)
  - EnumType : enum 이름을 데이터베이스에 저장

EnumType.ORDINAL은 enum에 정의된 순서대로 ADMIN은 0, USER는 1 값이 데이터베이스에 저장된다.
- 장점 : 데이터베이스에 저장되는 데이터 크기가 작다.
- 단점 : 이미 저장된 enum의 순서를 변경할 수 없다.

EnumType.STRING은 enum 이름 그대로 ADMIN은 'ADMIN', USER는 'USER'라는 문자로 데이터베이스에 저장된ㄷ.
- 장점 : 저장된 enum의 순서가 바뀌거나 enum이 추가되어도 안전하다.
- 단점 : 데이터베이스에 저장되는 데이터 크기가 ORDINAL에 비해서 크다.

`주의`
기본값인 ORDINAL은 주의해서 사용해야 된다.

ADMIN(0번), USER(1번) 사이에 enum이 하나 추가되서 ADMIN(0번). NEW(1번), USER(2번)로 설정되면서 이제부터 USER는 2로
저장되지만 기존에 데이터베이스에 저장된 값은 여전히 1로 남아 있다. 따라서 이런 문제가 발생하지 않는 EnumType.STRING을 권장한다.

**@Temporal**

- 속성
  - TemporalType.DATE : 날짜, 데이터베이스, date 타입과 매핑(예 : 2013-10-11)
  - TemporalType.TIME : 시간, 데이터베이스 time 타입과 매핑(예 : 11:11:11)
  - TemporalType.TIMESTAMP : 날짜와 시간, 뎅터베이스 timestamp 타입과 매핑(예 : 2013-10-11 11:11:11)
  - TemporalType은 필수로 지정해야 한다.
  
ex)
```java
@Temporal(Temporal.TIMESTAMP) // TIMESTAMP 대신에 DATE or TIME가 들어가 수 있다.
private Date timetamp;
```

생성된 DDL

timestamp timestamp;

**@Lob**

속성 정리

@Lob에는  지정할 수 있는 속성이 없다. 대신에 매핑하는 필드 타입이 문자면 CLOB으로 매핑하고 나머지는 BLOG로 매핑한다
- CLOB : String, char[], java.sql.CLOB
- BLOB : byte[], java.sql.BLOB
  
**@Transient**

이 필드는 매핑하지 않는다. 따라서 데이터베이스에 저장하지도 않고 조회하지도 않는다. 객체에 임시로 어떤 값을 보관하고 싶을 때
사용한다.

**@Access**

JPA가 엔티티 데이터에 접근하는 방식을 지정

- 필드 접근
  -AccessType.FIELD로 지정
  - 필드에 직접 접근
  - private이어도 접근
- 프로퍼티 접근
  - AccessType.PROPERTY로 접근
  - 접근자(Getter)를 사용

  </div>
</details>


<details>
  <summary>5.연관관계 매핑 기초</summary>
  <div markdown="1">

## 5. 연관관계 매핑 기초
객체의 참조와 테이블의 외래 키를 매핑할 수 있다.

**방향 : 단방향, 양방향**

- 회원 -> 팀
- 팀 -> 회원
- 회원 -> 팀, 팀 -> 회원

**다중성**

- 다대일, 일대다, 일대일, 다대다
- N:1, 1:N, 1:1, N:N

**연관관계의 주인**

- 객체를 양방향 연관관계로 만들면 연관관계의 주인을 정해야 한다.

### 단방향 연관 관계

**객체 연관관계**

- 객체는 단방향 관계다. 
- 객체의 필드(멤버 변수)로 다른 객체와 연관관계를 맺는다.

**테이블 연관관계**

- 양방향 관계다.
- 테이블은 외래키로 다른 테이블과 연관관계를 맺는다.
- 두 테이블의 외래키를 통해서 서로 조인 할 수가 있다.

**객체 연관관계와 테이블 연관관게의 가장 큰 차이**

참조를 통한 연관관계는 언제나 단방향이다. 객체간에 연관관계를 양방향으로 만들고 싶으면 반대쪽에도 필드를 추가해서 참조를 보관
해야한다. 결국 연관관계를 하나 더 만들어야 한다. 이렇게 양쪽에서 서로 참조하는 것을 양방향 연관관계라 한다. 하지만 정확히
이야기하면 이것은 `양방향 관계가 아니라 서로 다른 단방향 관계 2개다.` 반면에 테이블은 외래 키 하나로 양방향으로 조인할 수 있다.

**객체 연관관계 vs 테이블 연관관계 정리**

- 객체는 참조(주소)로 연관관계를 맺는다.
  - 객체는 참조를 사용해서 연관관계를 탐색할 수 있는데 이것을 `객체 그래프 탐색`이라고 한다.
- 테이블은 외래 키로 연관관계를 맺는다.

### 5.1.3 객체 관계 매핑

**객체 관계 매핑**

```java
@Entity
public class Member {

    @Id
    @Column(name = "MEMBER_ID")
    private Long id;

    private String username;

    //연관 관계 매핑
    @ManyToOne
    @JoinColumn(name="TEAM_ID")
    private Team team;

    //연관관계 설정
    public void setTeam(Team team) {
        this.team = team;
    }

    //Getter Setter
}
```
- @ManyToOne
  - 다대일(N:1) 관계라는 매핑 정보
  - 어노테이션 필수
- @JoinColumn(name="TEAM_ID")
  - 조인컬럼은 외래 키를 매핑할 때 사용
  - name 속성에 매핑할 외래 키 이름을 지정
  - 생략 가능하다.
  
**@JoinColumn 생략**

- 기본 전략 : 필드명 + _ + 참조하는 테이블의 컬러명
- 외래키 = team_TEAM_ID

**@ManyToOne**

다대일 관계에 사용
```java
@OneToMany
private List<Member> members;           // 제네릭 사용

@OneToMany(targetEntity=Member.class)   // 거의 사용 안함
private List members;                   // 타입 알 수 없음
```

### 5.1.4 @JoinColumn

@JounColumn은 외래 키를 매핑할 때 사용한다.

**@JoinColumn 주요 속성**

name
- 매핑할 외래 키 이름
- 필드명 + _ + 참조하는 테이블의 기본 키 컬럼명

referencedColumnName 
- 외래 키가 참조하는 대상 테이블의 컬럼명
- 참조하는 테이블의 기본 키 컬럼명

foreignKey(DDL)
- 외래 키 제약조건을 직접 지정할 수 있다.
- 이 속성은 테이블을 생성할 때만 사용한다.

unique, nullable, insertable, updatable, columnDefinition, table
- @Column의 속성과 같다.

### 5.1.5 @ManyToOne
@ManyToOne 어노테이션은 다대일 관계에서 사용한다.

**@ManyToOne 속성**

optional
- false로 설정하면 연관된 엔티티가 항상 있어야 한다.
- 기본값은 true

fetch 
- 글로벌 패치 전략을 설정한다.
- @ManyToOne - FetchType.EAGER
- @OneToMany = FetchType.LAZY

cascade
- 영속성 전이 기능을 사용한다.

targetEntity 
- 연관된 엔티티의 타입 정보를 설정한다. 이 기능은 거의 사용하지 않는다. 컬렉션을 사용해도 제네릭으로 타입 정보를 알 수 있다.

## 5.2 연관관계 사용

### 5.2.1 저장

```java
public void testSave() {

    //팀1 저장
    Team team1 = new Team("team1", "팀1");
    em.persist(team1);

    //회원1 저장
    Member member1 = new Member(100L, "회원1");
    member1.setTeam(team1);     //연관관계 설정 member1 -> team1
    em.persist(member1);

    //회원2 저장
    Member member2 = new Member(101L, "회원2");
    member2.setTeam(team1);     //연관관계 설정 member2 -> team1
    em.persist(member2);
}
```
JPA에서 엔티티를 저장할때 연관된 모든 엔티티는 `영속 상태`여야 한다.

|MEMBER_ID|	NAME|	TEAM_ID|	TEAM_NAME|
|---------|----|--------------------|----|
|100|	회원1|	team1	|팀1|
|101	|회원2	|team2	|팀1|

### 5.2.2 조회

연관관계가 있는 엔티티를 조회하는 방법은 크게 2가지다.
- 객체 그래프 탐색(객체 연관관계를 사용한 조회)
- 객체지향 쿼리 사용(JPQL)

**객체 그래프 탐색**

```java
Member member = em.find(Member.class, 100L);
Team team = member.getTeam();   //객체 그래프 탐색
System.out.println("팀 이름 = " + team.getName());
```

**객체 지향 쿼리 사용**

객체 지향 쿼리인 JPQL에서 연관관계를 어떻게 사용하는지 알아보자. SQL은 연관된 테이블을 조인해서 검색조건을 사용하면 된다.
JPQL도 조인을 지원한다.

```java
public static void testJPQL(EntityManager em) {
    String jpql1 = "select m from Member m join m.team t where " +
            "t.name = :teamName";

    List<Member> resultList = em.createQuery(jpql1, Member.class)
            .setParameter("teamName", "팀1")
            .getResultList();

    for (Member member : resultList) {
        System.out.println("[query] member.username = " +
                member.getUsername());
    }
}
// 결과: [query] member.username=회원1
// 결과: [query] member.username=회원2
```

`from Member m join m.team t` 부분을 보면 회원이 팀과 관계를 가지고 있는 필드(m.team)을 통해서 Member와 Team을 
조인했다. 그리고 where 절을 보면 조인한 t.name을 검색조건으로 사용해서 팀1에 속한 회원만 검색했다.
다음 실행한 JPQL을 보자. 참고로 :teamName과 같이 :로 시작하는 것은 파라미터를 바인딩 받는 문법이다.

이때 실행되는 SQL은 다음과 같다.

```sql
SELECT M.* FROM MEMBER MEMBER 
INNER JOIN 
    TEAM TEAM ON MEMBER.TEAM_ID = TEAM1_.ID 
WHERE
    TEAM1_.NAME='팀1'
```
실행된 SQL과 JPQL을 비교하면 JPQL은 객체(엔티티)를 대상으로 하고 SQL보다 간결하다.

### 5.2.3 수정

```java
private static void updateRelation(EntityManager em) {

    // 새로운 팀2
    Team team2 = new Team("team2", "팀2");
    em.persist(team2);

    //회원1에 새로운 팀2 설정
    Member member = em.find(Member.class, 100L);
    member.setTeam(team2);
}
```

실행되는 수정 SQL은 다음과 같다.

```sql
UPDATE MEMBER SET TEAM_ID='team2', ... WHERE ID = 'member1' 
```
수정은 em.update() 같은 메소드가 없다. 단순히 불러온 엔티티의 값만 변경해두면 트랜잭션을 커밋할 때 플러시가 일어나면서
변경 감지 기능이 작동한다. 그리고 변경사항을 데이터베이스에 자동으로 반영한다. 이것은 연관관계를 수정할 때도 같은데, 참조하는
대상만 변경하며 나머지는 JPA가 자동으로 처리한다.

### 5.2.4 연관관계 제거

```java
private static void deleteRelation(EntityManager em) {

    Member member1 = em.find(Member.class, "member1");
    member1.setTeam(null);      //연관관계 제거
}
```

### 5.2.5 연관된 엔티티 삭제

연관된 엔티티를 삭제하려면 기존에 있던 연관관계를 먼저 제거하고 삭제해야 한다. 그렇지 않으면 외래 키 제약조건으로 인해,
데이터베이스에서 오류가 발생한다. 팀1에 회원 1,2가 소속되어 있을 시, 팀1을 삭제하려면 연관관계를 먼저 끊는다.

```java
member1.setTeam(null);  // 회원1 연관관계 제거
member2.setTeam(null);  // 회원2 연관관계 제거
em.remove(team);        // 팀 삭제
```

## 5.3 양뱡향 연관관계

![1](https://user-images.githubusercontent.com/43127088/109632843-d0941100-7b8a-11eb-9630-0d5b4de201a5.PNG)

**양방향 객체 연관관계**

회원과 팀이 있을때, 회원과 팀은 다대일 관계라고 하자. 그럼 반대로 팀에서 회원은 일대다 관계다.
일대다 관계는 여러 건과 연관관계를 맺을 수 있으므로 컬렉션을 사용해야 한다. 팀은 `Team.members`를 List 컬렉션으로 추가했다.

- 회원 -> 팀(Member.team)
- 팀 -> 회원(Team.members)

`참고`
JPA는 LIST를 포함해서 Collection, Set, Map 같은 다양한 컬렉션을 지원한다.


**테이블 관계는 어떻게 될까?**

데이터이스 테이블은 외래 키 하나로 양방향으로 조회할 수 있다. 두 테이블의 연관관계는 외래 키 하나만으로 양방향 조회가 가능하므로
처음부터 양방향 관계다. 따라서 데이터베이스에 추가 할 내용이 전혀 없다.

### 5.3.1 양방향 연관관계 매핑

```java
@Entity
public class Team {
    ...
  // 필드 생략

  @OneToMany(mappedBy = "team")
  private List<Member> members = new ArrayList<Member>();
    
  //Getter, Setter ..  
}
```

팀과 회원은 일대다 관계다. 따라서 팀 엔티티에 컬렉션인 `List<Member> members`를 추가했다. 그리고 일대다 관계를 매핑하기
위해 @OneToMany 매핑 정보를 사용했다. mappedBy 속성은 양방향 매핑일 때 사용하는데 반대쪽 매핑의 필드 이름을 값으로 주면 된다.

## 5.4 연관관계 주인
테이블은 왜래 키 하나로 두 테이블의 연관관계를 관리한다. 엔티티를 단반향으로 매핑하면 참조를 하나만 사용하므로
이 참조로 외래 키를 관리하면 된다. 그런데 엔티티를 양방향으로 매핑하면 두 곳에서 서로를 참조한다.
따라서 객체의 연관관계를 관리하는 포인트는 2곳으로 늘어난다.

엔티티를 양방향 연관관계로 설정하면 객체의 참조는 둘인데 외래 키는 하나다. 따라서 둘 사이에 차이가 발생한다. 그럼
둘 중 어떤 관계를 사용해서 외래 키를 관리해야 할까?

이런 차이로 인해 JPA에서는 **두 객체 연관관계 중 하나를 정해서 테이블의 외래키를 관리해야 하는데 이것을 연관관계의 주인이라 한다.**

### 5.4.1 양방향 매핑 규칙 : 연관관계의 주인
- 연관관계의 주인만이 데이타베이스 연관관계와 매핑된다.
- 연관관계의 주인만이 외래키를 관리(등록, 수정, 삭제)할 수 있다.
- 주인이 아닌 쪽은 읽기만 할 수 있다.

연관관계의 주인을 정한다는 것 = 외래 키 관리자를 선택하는 것.

**mappedBy 속성**

- 주인이 아니면 mappedBy 속성을 사용해서 속성의 값으로 연관관계의 주인을 지정
- 주인은 mappedBy 속성을 사용하지 않는다.

그렇다면 `Member.team, Team.members` 둘 중 어떤 것을 연관관계의 주인으로 정해야할까?

**회원 -> 팀(Member.team) 방향**
```java
Class Member {
    @ManyToOne
    @JoinColumn(name="TEAM_ID")
    private Team team;
    ...
        }
```

**팀 -> 회원(Team.members) 방향**
```java
class Team{
    @OneToMany
    private List<Member> members = new ArrayList<Member>();
    ...
```

연관관계의 주인을 정한다는 것은 사실 외래 키 관리자를 선택하는 것이다. 회원 테이블에 있는 TEAM_ID 외래 키를 관리할 관리자를 선택해야 한다.
만약 회원 엔티티에있는 Member.team을 주인으로 선택하면 자기 테이블에 있는 외래 키를 관리하면 된다. 하지만 팀 엔티티에 있는
Team.members를 주인으로 선택하면 물리적으로 전혀 다른 테이블의 외래 키를 관리해야 한다. 왜냐하면 이 경우 Team.members가 있는
Team 엔티티는 TEAM 테이블에 매핑되어 있는데 관리해야할 외래 키는 MEMBER 테이블에 있기 때문이다.

### 5.4.2 연관관계의 주인은 외래 키가 있는 곳
- 연관관계의 주인은 테이블에 외래 키가 있는 곳으로 정해야 한다.
- Team 엔티티는 mappedBy를 통해 주인이 아님을 설정.

```java
class Team {    
    @OneToMany(mappedBy = "team")  // 연관관계 주인인 Member.team
    private List<Member> members = new ArrayList<Member>();
}
```
- 연관관계의 주인만 데이터베이스 연관관계와 매핑, 외래 키를 관리.
- 주인이 아닌 반대편은 읽기만 가능, 외래 키를 변경하지 못한다.
- 항상 '다(N)'쪽이 외래 키를 가진다.
- @ManyToOne은 항상 연관관계의 주인이 되므로 mappedBy를 설정할 수 없다. 따라서 @ManyToOne에는 mappedBy 속성이 없다.

### 양방향 연관관계 저장
```java
public void testSave() {

    //팀1 저장
    Team team1 = new Team("team1", "팀1");
    em.persist(team1);

    //회원1 저장
    Member member1 = new Member("member1", "회원1");
    member1.setTeam(team1);     //연관관계 설정 member1 -> team1
    em.persist(member1);

    //회원2 저장
    Member member2 = new Member("member2", "회원2");
    member2.setTeam(team1);     //연관관계 설정 member2 -> team1
    em.persist(member2);
}
```

**주인이 아닌 곳에 입력된 값은 외래 키에 영향을 주지 않는다. 따라서 이전 코드는 데이터베이스에 저장할 때 무시된다.**
```java
team1.getMembers().add(member1);        //무시(연관관계의 주인이 아님)
team1.getMembers().add(member2);        //무시(연관관계의 주인이 아님)

member1.setTeam(team1);                 //연관관계 설정(연관관계의 주인)
member2.setTeam(team1);                 //연관관계 설정(연관관계의 주인)
```

Member.team은 연관관계의 주인, 엔티티 매니저는 이곳에 입력된 값으로 외래 키 관리

## 5.6 양방향 연관관계 주의점
양방향 연관관계를 설정하고 가장 흔히 하는 실수는 연관관계의 주인에는 값을 입력하지 않고, 주인이 아닌 곳에만 값을 입력하는 것이다.
데이터베이스에 외래 키 값이 정상적으로 저장되지 않으면 이것부터 의심해보자.

```java
public void testSaveNonOwner() {

        //회원1 저장
        Member member1 = new Member("member1", "회원1");
        em.persist(member1);

        //회원2 저장
        Member member2 = new Member("member2", "회원2");
        em.persist(member2);

        Team team1 = new Team("team1", "팀1");

        //주인이 아닌 곳에 연관관계 설정
        team1.getMembers().add(member1);
        team2.getMembers().add(member2);

        em.persist(team1);
}
```

조회한 결과는 다음과 같다,

|    MEMBER_ID  | USERNAME | TEAM_ID| 
| ----------| ------------------- |---|
|member1|회원1|null|
|member2|회원2|null

TEAM_ID 외래 키에 팀의 기본 값이 저장되어 있다.
양방향 연관관계는 연관관계의 주인이 외래 키를 관리한다. 따라서 주인이 아닌 방향은 값을 설정하지 않아도 데이터베이스에 외래 키
값이 정상 입력된다.

### 5.6.1 순수한 객체가지 고려한 양방향 연관관계
그렇다면 정말 연관관계의 주인에만 값을 저장하고 주인이 아닌 곳에는 값을 저장하지 않아도 될까?

사실은 **객체 관점에서 양쪽 방향에 모두 값을 입력해주는 것이 가장 안전하다** 양쪽 방향 모두 값을 입력하지 않으면
JPA를 사용하지 않는 순수한 객체 상태에서 심각한 문제가 발생할 수 있다.

```java
member1.setTime(team1); 회원 -> 팀
```
Member.team에만 연관관게를 설정하고 반대 방향은 연관관계를 설정하지 않는다면 기대했던 결과 값이 나오지 않는다.

양방향은 양쪽다 관계를 설정해야 한다. `회원 -> 팀`을 설정하면 다음 `팀 -> 회원'도 설정해야 한다.

```java
member1.setTeam(team1); // 회원 -> 팀
team1.getMembers().add(member1); // 팀 -> 회원
```
객체까지 고려하면 이렇게 양쪽 다 관계를 맺어야 한다. 이렇게 양쪽에 연관관계를 설정하면 순수한 객체 상태에서도
동작하며, 테이블의 외래 키도 정상 입력된다. 물론 외래 키의 값은 연관관계의 주인인 Member.team 값을 사용한다.

정리하자면, 앞서 이야기한 것처럼 객체까지 고려해서 주인이 아닌 곳에도 값을 입력하자. 
즉, 객체의 양방향 연관관계는 양쪽 모두 관계를 맺어주자.

### 5.6.2 연관관계 편의 메소드
```java
member1.setTeam(team1); // 회원 -> 팀
team1.getMembers().add(member1); // 팀 -> 회원
```
양방향 관계에서 두 코드는 하나인 것처럼 사용하는 것이 안전하다.

Member 클래스의 setTeam() 매소드를 수정해서 코드를 리팩토링해보자.

```java
public class Member {
    private Team team;
    
    public void setTeam(Team team) {
        this.team = team;
        team.getMembers().add(this);
    }
}
```
setTeam() 메소드 하나로 양방향 관계를 모두 설정하도록 변경했다.

### 5.6.3 연관관계 편의 메소드 작성 시 주의사항
```java
member1.setTeam(teamA); // 1
member1.setTeam(teamB); // 2
Member findMember = teamA.getMember(); // member1이 여전히 조회된다.
```

teamB로 변경할 때 tamA -> member1 관계를 제거하지 않는다. 그래서 연관관계를 변경할 때는 기존 팀이 있으면 기존 팀과 회원의 연관관계를
삭제하는 코드를 추가해야 한다. 따라서 기존 관계를 제거하도록 코드를 수정해야 한다.

```java
public void setTeam(Team team) {
    // 기존 팀과 관계를 제거
    if (this.team != null) {
        this.team.getMembers().remove(this);
        }    
    this.team = team;
    team.getMembers().add(this);
}
```

삭제되지 않은 관계2에서 teamA -> member1 관계가 제거되지 않아도 데이터베이스 외래 키를 변경하는 데는문제가 없다. 왜냐하면 teamA -> member1 관계를
설정한 Team.members는 연관관계의 주인이 아니기 떄문이다. 연관관계의 주인인 Member.team의 참조를 member1 -> teamB로
변견했으므로 데이터베이스에 외래 키는 teamB를 참조하도록 정상 반영 된다.

그리고 이후에 새로운 영속성 컨텍스트에서 teamA를 조회해서 teamA.getMembers()를 호출하면 데이터베이스 외래 키에는 관계가
끊어져 있으므로 아무것도 조회되지 않는다. 여기까지만 보면 특별한 문제가 ㅇ벗는 것 같다.

문제는 관계를 변경하고 영속성 컨텍스트가 아직 살아있는 상태에서 teamA의 getMembers()를 호출하면 member1이 반환된다는 점이다. 따라서 변경된 연관관계는
앞서 설명한 것처럼 관계를 제거하는 것이 안전하다.

양방향 매핑은 복잡하다. 비즈니스 로직의 필여에 따라 다르곘지만 우선 단방향 매핑을 사용하고 반대 방향으로 객체 그래프 탐색 기능(JPQL 쿼리
탐색 포함)이 필요할 때 양방향을 사용하도록 코드를 추가해도 된다.

`주의`
양뱡행 매핑 시에는 무한 루프에 빠지지 않게 조심해야 한다. 예를 들어 Member.toString()에서 getTeam()을 호출하고 Team.toString()에서
getMember()를 호출하면 무한 루프에 빠질 수 있다. 이런 문제는 엔티티를 JSON으로 변환할 때 자주 발생하는데 JSON 라이브러리들은 보통
무한루프에 빠지지 않도록 하는 어노테이션이나 기능을 제공한다. 그리고 Lombok을 사용할 때도 자주 발생한다.

  </div>
</details>

<details>
  <summary>6.다양한 연관관계 매핑</summary>
  <div markdown="1">

## 6. 다양한 연관관계 매핑

들어가기 전에

- 연관관계는 사실상 방향이라는 개념이 존재하지 않는다. 외래키 하나로 양쪽을 조인 가능하다.
- 연관관계의 주인은 항상 '다' 즉 [N] 쪽에 설정해줘야 한다.
- 참조용 필드(mappedBy)는 읽기 전용으로, 오로지 참조만 가능하다.
- 객체에서의 양방향은 A->B, B->A 처럼 참조가 2군데인 것이다.

1. 다대일 [N:1]
2. 일대다 [1:N]
3. 일대일 [1:1]
4. 다대다 [N:N]

### 1. 다대일[N:1]

- JPA에서 가장 많이 사용하고, 꼭 알아야 하는 다중성이다.
- 아래 테이블에서 보면 DB설계상 일대다에서 '다' 쪽에 외래키가 존재해야한다. 그렇지 않으면 잘못된 설계이다.
- 테이블에서는 FK가 팀을 찾기 위해 존해하고, 객체에서 Team 필드도 Team을 참조하기 위해 존재한다.

1-1. 다대일 단방향 매핑

- JPA의 @ManyToOne 어노테이션을 사용해서 다대일 관계를 매핑한다.
- @JoinColumn은 외래키를 매핑할 때 사용한다. name은 매핑할  외래 키의 이름이다.

```java
public class Member{
    ...
    @ManyToOne
    @JoinCoulmn(name="TEAM_ID")
    private Team team;
}
```

![다대일단방향](https://user-images.githubusercontent.com/39195377/97150589-b21fa280-17b1-11eb-9128-a9c2abad86eb.PNG)


1-2. 다대일 양방향 매핑

- 다대일 관계에서 단방향 매핑을 진행하고, 양방향 매핑을 진행할때 사용한다.
- 반대쪽에서 일대다 단방향 매핑을 해주면 된다.(객체기준으로, 컬렉션을 추가하자)
- 여기서 중요한건, 반대에서 단방향 매핑을 한다고 해서 DB테이블에 영향을 전혀 주지 않는다.
- 다대일의 관계에서 **다 쪽에서 이미 연관관계 주인**이 되어서 외래키를 관리하고 있다.


![다대일양방향](https://user-images.githubusercontent.com/39195377/97150625-bcda3780-17b1-11eb-81ca-ef9fe1561def.PNG)

- 반대쪽에서 일대다 단방향 매핑. JPA의 @OneToMany 어노테이션을 사용한다.
- 연관관계의 주인이 아니고, 어디에 매핑 되었는지에 대한 정보를 표시하는 (mappedBy="team") 을 꼭 넣어줘야 한다.
- (mappedBy = "team") 에서 team은  Member에서 외래키로 매핑된 필드명이다.

```java
public class Team {
    ...

    @OneToMany(mappedBy = "team")
    private List<Member> members = new ArrayList<>();
    
    ...
}
```

정리

- 외래키가 있는 쪽이 연관관계 주인이다.
- 양쪽을 서로 참조하도록 개발하자.





### 2. 일대다[1:N]

- 일대다 관계에서는 일이 연관관계의 주인이다.
- 일 쪽에서 외래키를 관리하겠다는 의미가 된다
- 결론 먼저 말하자면, 표준스펙에서 지원은 하지만 **실무에서는 사용을 권장하지 않는다.**



2-1. 일대다 단방향 매핑

![일대다단방향](https://user-images.githubusercontent.com/39195377/97150684-d54a5200-17b1-11eb-9c6c-47bb72e2085f.PNG)


- 팀과 멤버가 일대다 관계이다.
- Team이 Members(컬렉션)을 가지는데, Member의 입장에서는 Team을 참조하지 않아도 된다는 설계이다. '객체'의 입장에서 생각해보면 충분히 나올수 있는 설계이다.
- 그러나 DB 테이블 입장에서 보면 무조건 일대다에서 '다' 쪽에 외래키가 들어간다.
- Team에서 members가 바뀌면, DB의 member 테이블에서 업데이트 쿼리가 나가는 상황이 발생한다.



JPA의 @OneToMany와 @JoinColumn()을 이용해 일대다 단방향 매핑을 설정할 수 있다.

- Member는 코드상 연관관계 매핑이 없고, 팀에서만 일대다 단방향 매핑 설정

  ```java
  @Entity
  public class Team {
      ...
      @OneToMany
      @JoinColumn(name = "TEAM_ID")
      private List<Member> members = new ArrayList<>();
  ```



```java
Member member = new Member();
member.setUsername("MemberA");
em.persist(member);

System.out.println("-----멤버 저장");

Team team = new Team();
team.setName("TeamA");
team.getMembers().add(member);
em.persist(team);

System.out.println("-----팀 저장");

tx.commit();
```

- 이렇게 실행하면 쿼리에서 트랜잭션 커밋 시점에 create one-to-many row로 시작되는 주석과 함께 Member 테이블을 업데이트 하는 **쿼리가 나간다.**



★일대다 단방향의 정리

- 일대다 단방향은 일대다의 일이 연관관계 주인이다.
- 테이블 일대다 관계는 항상 다(N)쪽에 외래키가 있다.
- 객체와 테이블의 **패러다임 차이** 때문에 객체의 반대편 테이블의 외래키를 관리하는 특이한 구조다.
- @JoinColumn을 반드시 사용해야 한다. 그렇지 않으면 조인 테이블 방식을 사용한다.



★일대다 단방향 매핑의 문제점과 해결방안

- 일단 업데이트 쿼리가 나간다. 성능상 좋지 않으나 크게 문제되지는 않는다.
- 위 예시로 살펴보면, Team의 엔티티를 수정했는데 Member의 엔티티가 업데이트되는 상황이 발생한다.
- 테이블이 적을때는 크게 문제가 되지 않지만 실무에서는 테이블이 수십개가 엮어서 돌아간다.

- 참고로, 일대다 양방향 매핑은 JPA 공식적으로 존재하지 않는 방법이다.

### 따라서, 다대일 단방향 매핑을 사용하고, 필요시 양방향 설정을 사용하자.





### 3. 일대일[1:1]

- 일대일 관계는 그 반대도 일대일이다.
- 일대일 관계는 특이하게 주 테이블이나 대상 테이블 중에 외래 키를 넣을 테이블을 선택 가능하다.
  - 주 테이블에 외래키 저장
  - 대상 테이블에 외래 키 저장
- 외래키에 데이터베이스 유니크 제약조건이 추가되어야 일대일 관계가 가능하다.



3-1. 일대일 - 주 테이블에 외래키 단방향
![일대일 주테이블 단방향](https://user-images.githubusercontent.com/39195377/97150731-e98e4f00-17b1-11eb-97ed-a73edfec651a.PNG)


- 회원이 딱 하나의 락커를 가지고 있는 상황이다. 반대로 락커도 회원 한명만 할당 받을 수 있다. 이때, 둘의 관계는 일대일 관계이다.
- 이 경우 멤버를 주 테이블로 보고 주 테이블 또는 대상 테이블에 외래키를 지정할 수 있다.
- 다대일[N:1] 단방향 관계와 JPA 어노테이션만 달라지고 거의 유사하다.



3-1-2. 일대일- 주 테이블에 외래키 양방향

- 다대일[N:1] 양방향 매핑처럼 외래키가 있는곳이 연관관계의 주인이다.

- JPA @OneToOne 어노테이션으로 일대일 단방향 관계를 매핑하고, @JoinColumn을 넣어준다.

  - 여기까지만 매핑하면 단방향 관계이고,

    ```java
    @Entity
    public class Member {
        ...
            
        @OneToOne
        @JoinColumn(name = "locker_id")
        private Locker locker;
     
        ...
    }
    ```

- 반대편에 mappedBy를 적용시켜주면 일대일 양방향 관계 매핑이 된다.

  ```java
  @Entity
  public class Locker {
      ...
          
      @OneToOne(mappedBy = "locker")
      private Member member;
  }
  ```

- 마찬가지로, mappedBy는 읽기 전용으로 참조만 가능하다.



3-2-1. 일대일 - 대상 테이블에 외래키 단방향

- 일대일 관계에서 대상 테이블에 외래키를 단방향으로 저장하는 방식은 지원하지 않는다.



3-2-2. 일대일 - 대상 테이블에 외래키 양방향

- 일대일 주 테이블에 외래 키 양방향 매핑을 반대로 뒤집었다고 생각하면 된다. 매핑 방법은 같다.

- 주 테이블은 멤버 테이블이지만, 외래 키를 대상 테이블에서 관리하고 주 테이블의 락커 필드는 읽기 전용이 된다.



일대일 정리

- 주 테이블에 외래키

  - (주 테이블은 많이 접근하는 테이블로 설정하자.)
  - 주 테이블에 외래키를 두고 대상 테이블을 찾는 방식
  - 객체지향 개발자들이 선호하고, JPA 매핑이 편리하다
  - 주 테이블만 조회해도 대상 테이블에 데이터가 있는지 확인 가능하다.
  - 그러나, 값이 없으면(예를들어, member가 locker를 사용하지 않음) NULL을 허용해야한다.

- 대상 테이블에 외래키

  - 대상 테이블에 외래 키가 존재한다.

  - 전통적인 데이터베이스 개발자들이 선호하는 방식이다. NULL을 허용해야하는 문제도 없다.

  - 주 테이블과 대상 테이블을 일대일에서 일대다 관계로 변경할 때 테이블 구조를 유지할 수 있

  - 코드상에서는 주로 멤버 엔티티에서 락커를 많이 엑세스 하는데, 어쩔 수 없이 양방향 매핑을 해야한다.

    - 일대일 - 대상 테이블에 외래 키 단방향 매핑을 JPA에서 지원하지 않으므로, 단방향 매핑만 해서는 멤버 객체를 업데이트 했을 때 락커 테이블에 FK를 업데이트 할 방법이 없다. 따라서 양방향 매핑을 해야 한다.





    ### 4. 다대다[N:N]

    - 결론부터 말하자면, 다대다는 실무에서 사용하지 말아야 한다.
    - 다대다 (@ManyToMany)는 각각 일대다, 다대일 관계로 풀어서 사용하자.
    - 연결 테이블용 엔티티를 추가한다. 연결 테이블을 엔티티로 승격시킨다.
    - JPA가 만들어준 숨겨진 매핑테이블의 존재를 바깥으로 꺼낸다고 생각하자.



![다대다](https://user-images.githubusercontent.com/39195377/97150771-f4e17a80-17b1-11eb-8256-9962af9d1559.PNG)




    위 그림과 같은 다대다 관계를 아래처럼 풀어낼 수 있다.

    

    - Member 엔티티에서 @OneToMany 관계로 변경한다.

      ```java
      @Entity
      public class Member {
          ...
              
          @OneToMany(mappedBy = "member")
          private List<MemberProduct> memberProducts = new ArrayList<>();
      ​
          ...
      }
      ```

      

    - Product도 마찬가지로 @OneToMany 관계로 변경한다.

      ```java
      @Entity
      public class Product {
      
          ...
      
          @OneToMany(mappedBy = "product")
          private List<MemberProduct> members = new ArrayList<>();
          
          ...
      }
      ```

    

    - MemberProduct
      - 연결 테이블을 엔티티르 승격시킨 테이블이다. 그리고 @ManyToOne 매핑을 두개 한다.
      - 여기서 추가 데이터가 들어간다면 아예 의미있는 엔티티 이름으로 변경 될 것이다.

    ```java
    @Entity
    @Getter
    @Setter
    public class MemberProduct {
    
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
    
        @ManyToOne
        @JoinColumn(name = "member_id")
        private Member member;
    
        @ManyToOne
        @JoinColumn(name = "product_id")
        private Product product;
    }
    ```

### 6.4.3 다대다: 매핑의 한계와 극복, 연결 엔티티 사용
@ManyToMany를 사용하면 연결 테이블을 자동으로 처리해주므로 도메인 모델이 단순해지고 여러 가지로 편하다. 하지만
이 매핑을 실무에서 사용하기에는 한계가 있다. 예를 들어 회원이 상품을 주문하면 연결 테이블에 단순히 주문한 회원 아이디와
상품아이디만 담고 끝나지 않는다. 보통은 연결 테이블에 주문 수량 컬럼이나 주문한 날짜 같은 컬럼이 더 필요하다.

연결 테이블에 주문 수량과 주문 날짜 컬럼을 추가하면 더는 @ManyToMany를 사용할 수 없다. 왜냐하면 주문 엔티티나 상품 엔티티에는
추가한 컬럼들을 매핑할 수 없기 때문이다.

결국 연결 엔티티를 만들고 이곳에 추가한 컬럼들을 매핑해야 한다. 그리고 엔티티 간의 관계도 테이블 관계처럼 다대다에서
일대다, 다대일 관계로 풀어야 한다.

**복합 기본 키**

JPA에서 복합 기본 키를 사용하려면 별도의 식별자 클래스를 만들어야 한다. 그리고 엔티티에 @IdClass를 사용해서 식별자 클래스를 지정하면
된다.
- 복합 키는 별도의 식별자 클래스로 만들어야 한다.
- Serializable을 구현해야 한다.
- equals와 hashCode 메소드를 구현해야 한다.
- 기본 생성자가 있어야 한다.
- 식별자 클래스는 public이어야 한다.
- @IdClass를 사용하는 방법 외에 @EmbeddedId를 사용하는 방법도 있다
  
**식별 관계**

회원상품은 회원과 상품의 기본 키를 받아서 자신의 기본 키로 사용한다. 이렇게 부모 테이블의 기본 키를 받아서 자신의 기본 키
+ 외래 키로 사용하는 것을 데이터베이스 용어로 **식별 관계**라 한다.
  
복합 키를 사용하는 방법은 복잡하다. 단순히 컬럼 하나만 기본 키로 사용하는 것과 비교해서 복합 키를 사용하면 ORM 매핑에서
처리할 일이 상당히 많아진다. 복합 키를 위한 식별자 클래스도 만들어야 하고 @IdClass 또는 @EmbeddedId도 사용해야 한다.
그리고 식별자 클래스에 equals, hashCode도 구현해야 한다.

복합 키를 사용하지 않고 간단히 다대다 관계를 구성하는 방법을 알아보자.

### 6.4.4 다대다 : 새로운 기본 키 사용

추천하는 기본 키 생성 전략은 데이터베이스에서 자동으로 생성해주는 대리 키를 Long 값으로 사용하는 것이다.
이것의 장점은 간편하고 거의 영구히 쓸 수 있으며 비즈니스에 의존하지 않는다. 그리고 ORM 매핑 시에 복합키를 만들지 않아도 되므로
간단히 매핑을 완성할 수 있다.

### 6.4.5 다대다 연관관계 정리
다대다 관계를 일대다 다대일 관계로 풀어내기 위해 연결 테이블을 만들 떄 식별자를 어떻게 구성할지 선택해야 한다.
- 식별 관계 : 받아온 식별자를 기본 키 + 외래 키로 사용한다.
- 비식별 관계 : 받아온 식별자는 외래 키로만 사용하고 새로운 식별자를 추가한다.

데이터베이스 설계에서는 1번처럼 부모 테이블의 기본 키를 받아서 자식 테이블의 기본 키 + 외래 키로 사용하는 것을 식별 관계라 하고,
2번 처럼 단순히 외래 키로만 사용하는 것을 비식별 관계라 한다. 객체 입장에서 보면 2번처럼 비식별 관계를 사용하는 것이 복합 키를
위한 식별자 클래스를 만들지 않아도 되므로 단순하고 편리하게 ORM 매핑을 할 수 있다. 이런 이유로 `식별 관계보다는 비식별 관계를 추천한다.`

  </div>
</details>

<details>
  <summary>7.고급 매핑</summary>
  <div markdown="1">

# 7장, 고급 매핑
들어가기 전에..
- 상속 관계 매핑 : 객체의 상속 관계를 데이터베이스에 어떻게 매핑하는지 다룬다.
- @MappedSuperclass : 등록일, 수정일 같이 여러 엔티티에서 공통으로 사용하는 매핑 정보만 상속받고 싶으면 이 
  기능을 사용하면 된다.
- 복합 키와 식별 관계 매핑 : 데이터베이스의 식별자가 하나 이상일 때 매핑하는 방법을 다룬다. 그리고 데이터베이스
  설계에서 이야기하는 식별 관계와 비식별 관계에 대해서도 다룬다.
- 조인 테이블 : 테이블은 외래 키 하나로 연관관계를 맺을 수 있지만 연관관계를 관리하는 연결 테이블을 두는 방법도 있다.
  여기서는 이 연결테이블을 매핑하는 방법을 다룬다.
- 엔티티 하나에 여러 테이블 매핑하기 : 보통 엔티티 하나에 테이블 하나를 매핑하지만 엔티티 하나에 여러 테이블을 매핑
  하는 방법도 있다. 여기서는 이 매핑법을 다룬다.
  
## 7.1 상속관계 매핑
- 객체에서는 상속이라는 개념이 존재하지만, 관계형 데이터베이스는 상속 관계가 존재하지 않는다.
- 슈퍼타입, 서브타입 관계라는 모델링 기법이 상속과 유사하다.
- 상속관계 매핑이라는 것은 객체의 상속 구조와 DB의 슈퍼타입, 서브타입 관계를 매핑하는 것이다.
  
![상속매핑1](https://user-images.githubusercontent.com/39195377/97332065-5fcba800-18bd-11eb-9d46-d22903c57922.PNG)

- 각각의 테이블로 변환 : 각각을 모두 테이블로 구현할 떄는 3가지 방법을 선택할 수 있다.
- 통합 테이블로 변환 : 테이블을 하나만 사용해서 통합한다. JPA에서는 단일 테이블 전략이라 한다.
- 서브타입 테이블로 변환 : 서브 타입마다 하나의 테이블을 만든다. JPA에서는 구현 클래스마다 테이블 전략이라 한ㄷ.

### 7.1.1 조인 전략
`조인 전략`은 엔티티 각각을 모두 테이블로 만들고 자식 테이블이 부모 테이블의 기본 키를 받아서 기본 키 + 외래 키로
사용하는 전략이다. 따라서 조회할 떄 조인을 자주 사용한다.

주의할 점 : 이 전략을 사용할 떄 주의할 점이 있는데 객체는 타입으로 구분할 수 있지만 테이블은 타입의 개념이 없다. 따라서 타입을 구분하는
컬럼을 추가해야 한다. 여기서는 DTYPE 컬럼을 구분 컬럼으로 사용한다.

![조인전략](https://user-images.githubusercontent.com/39195377/97332067-60643e80-18bd-11eb-95ab-d126d8bad349.PNG)

```java
@Entity
@Inheritance(strategy = InheritanceType.XXX) // 상속 구현 전략 선택
public class Item {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private int price;
}
```

**매핑 정보 분석**

1. @Inheritance(strategy = InheritanceType.JOINED) : 상속 매핑은 부모 클래스에 @Inheritance를 
사용해야 한다. 그리고 매핑 전략을 지정해야 하는데 여기서는 조인 전략을 사용하므로 InheritanceType.JOINED를 사용했다.
   
2. @DicriminatorColumn(name = "DTYPE") : 부모 클래스에 구분 컬럼을 지정한다. 이 컬럼으로 저장된 자식 테이블을 구분할
수 있다. 기본값이 DTYPE이므로 @DiscriminatorColumn으로 줄여 사용해도 된다.
   
3. @DiscriminatorVale("M") : 에티티를 저장할떄 구분 컬럼에 입력할 값을 지정한다. 만약 영화 엔티티를 저장하면 구분 컬럼인
DTYPE에 값 M이 저장된다.

기본값으로 자식 테이블의 부모 테이블 ID 컬럼명을 그대로 사용하는데, 만약 자식 테이블의 기본 키 컬럼명을 변경하고
싶으면 `@PrimaryKeyJoinColumn`을 사용하면 된다.

- 실제 실행된 DDL
  - 테이블 4개 생성(item,album,movie,book)
  - 하위 테이블에 외래키 제약조건 생성. 하위 테이블 입장에선 ITEM_ID가 PK 이면서 FK
  
```java
Hibernate: 
    create table Album (
       artist varchar(255),
        id bigint not null,
        primary key (id)
    )
Hibernate: 
    create table Book (
       author varchar(255),
        isbn varchar(255),
        id bigint not null,
        primary key (id)
    )
Hibernate: 
    create table Item (
       DTYPE varchar(31) not null,
        id bigint generated by default as identity,
        name varchar(255),
        price integer not null,
        primary key (id)
    )
Hibernate: 
    create table Movie (
       actor varchar(255),
        director varchar(255),
        id bigint not null,
        primary key (id)
    )
    
    
Hibernate: 
    alter table Album 
       add constraint FKcve1ph6vw9ihye8rbk26h5jm9 
       foreign key (id) 
       references Item
Hibernate: 
    alter table Book 
       add constraint FKbwwc3a7ch631uyv1b5o9tvysi 
       foreign key (id) 
       references Item
Hibernate: 
    alter table Movie 
       add constraint FK5sq6d5agrc34ithpdfs0umo9g 
       foreign key (id) 
       references Item
```  
- 만약 Movie의 객체를 저장하면 다음과 같은 과정이 일어난다.
  - Insert 쿼리가 두개 나간다. Item테이블과 Movie테이블
  - DTYPE은 클래스 이름이 Default로 저장된다.
  
```java
Hibernate: 
    select
        movie0_.id as id2_2_0_,
        movie0_1_.name as name3_2_0_,
        movie0_1_.price as price4_2_0_,
        movie0_.actor as actor1_3_0_,
        movie0_.director as director2_3_0_ 
    from
        Movie movie0_ 
    inner join
        Item movie0_1_ 
            on movie0_.id=movie0_1_.id 
    where
        movie0_.id=?
```

**정리**

- 장점
  - 테이블 정규화
  - 외래키 참조 무결성 제약조건 활용
  - 저장공간 효율화
- 단점
  - 조회시 조인을 많이 사용, 성능 저하 우려
  - 조회 쿼리가 복잡함
  - 데이터 저장시 INSERT SQL을 2번 호출

### 7.1.2 단일 테이블 전략
단일 테이블 전략은 이름 그대로 테이블을 하나만 사용한다. 그리고 구분 컬럼으로 어떤 자식 데이터가 저장되었는지 구분한다
`조회할 때 조인을 사용하지 않으므로 일반적으로 가장 빠르다.`

![1](https://user-images.githubusercontent.com/43127088/109956714-56e55a00-7d27-11eb-9a9c-6e6fdcf8241e.PNG)

이 전략을 사용할 때 주의점은 자식 엔티티가 매핑한 컬럼은 모두 null을 허용해야 한다는 점이다. 예를 들어 엔티티를 저장하면
ITEM 테이블의 AUTHOR, ISEM 컬러만 사용하고 다른 엔티티와 매핑된 ARTIST, DIRECTOR, ACTOR 컬럼은 사용하지
않으므로 null이 입력되기 때문이다.

- 서비스 규모가 크지 않고, 굳이 조인 전략을 선택해서 복잡한 쿼리를 사용할 필요가 없다고 판단될 때 사용한다.
- 한 테이블에 전부다 저장하고, DTYPE으로 구분하는 방법이다.
- INSERT 쿼리도 한 번, SELECT 쿼리도 한 번 이다.
- @Inheritance(strategy = InheritanceType.SINGLE_TABLE)
  - 단일 테이블 전략에서는 @DiscriminatorColumn이 없으면 테이블 구분이 불가능하다. 꼭 사용해야 한다.
  - 따라서 필수로 사용해줘야하는데, @DiscriminatorColumn을 선언하지 않아도 Default로 DTYPE이 생성된다.
  - @DiscriminatorValue를 지정하지 않으면 기본으로 엔티티 이름을 사용한다. (예 Movie, Album, Book)

- 장점
    - 조인이 필요 없으므로 일반적으로 조회 성능이 빠르다.
    - 조회 쿼리가 단순하다.
 - 단점 
   - 자식 엔티티가 매핑한 컬럼을 모두 null을 허용해야 한다.
   - 단일 테이블에 모든 것을 저장하므로 테이블이 커질 수 있다. 그러므로 상황에 따라서는 조회 성능이 오히려 느려질 수 있다.
  
```java
@Entity
@DiscriminatorColumn
@Inheritance(strategy = InheritanceType.SINGLE_TABLE)
public class Item {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private int price;
}

```

- 실제 실행된 DDL :통합 테이블이 하나 생성된다.
```sql
Hibernate: 
    create table Item (
       DTYPE varchar(31) not null,
        id bigint generated by default as identity,
        name varchar(255),
        price integer not null,
        artist varchar(255),
        author varchar(255),
        isbn varchar(255),
        actor varchar(255),
        director varchar(255),
        primary key (id)
    )
```
- 만약 저장, 조회를 실행하면 ?
  - 조인 전략과 다르게 조인하지 않는다. 그냥 Item 테이블을 조회하고, DTYPE을 검색조건으로 추가한다.
```sql
Hibernate: 
    select
        movie0_.id as id2_0_0_,
        movie0_.name as name3_0_0_,
        movie0_.price as price4_0_0_,
        movie0_.actor as actor8_0_0_,
        movie0_.director as director9_0_0_ 
    from
        Item movie0_ 
    where
        movie0_.id=? 
        and movie0_.DTYPE='Movie'
```  

### 7.1.3 구현 클래스마다 테이블 전략

구현 클래스마다 테이블 전략은 아래 그림과 같이 자식 엔티티마다 테이블을 만든다. 그리고 자식 테이블 각각에 필요한
컬럼이 모두 있다.

![2](https://user-images.githubusercontent.com/43127088/109960417-15a37900-7d2c-11eb-9a9b-498e164a3cd4.PNG)

`InheritanceType.TABLE_PER_CLASS`를 선택하면 구현 클래스마다 테이블 전략을 사용한다. 이 전략은 자식 엔티티마다 테이블을 만든다. 
**일반적으로 추천하지 않는 전략이다**

- 장점
  - 서브 타입을 구분해서 처리할 때 효과적이다.
  - not null 제약 조건을 사용하 수 있다.
- 단점
  - 여러 자식이 테이블을 함꼐 조회할 떄 성능이 느리다.(SQL에 UNION을 사용해야 한다.)
  - 자식 테이블을 통합해서 쿼리하기 어렵다.
  
이 전략은 데이터베이스 설계자와 ORM 전문가 둘 다 추천하지 않는 전략이다. 조인이나 단일 테이블 전략을 고려하자.

## 7.2 @MappedSuperclass
부모 클래스는 테이블과 매핑하지 않고 부모 클래스를 상속받는 자식 클래스에게 매핑 정보만 제공하고 싶으면
@MappedSuperclass를 사용하면 된다.

@MappedSuperclass는 비유를 하자면 추상 클래스와 비슷한데 @Entity는 실제 테이블과 매핑되지만 @MappedSuperclass는 
실제 테이블과 매핑되지 않는다. `이것은 단순히 매핑 정보를 상속할 목적으로만 사용된다.`

![맵드슈퍼](https://user-images.githubusercontent.com/39195377/97332061-5fcba800-18bd-11eb-99a3-b5a40e0f6488.PNG)


```java
@Getter
@Setter
@MappedSuperclass
public abstract class BaseEntity {

   private String createdBy;

   private LocalDateTime createdDate;

   private String lastModifiedBy;

   private LocalDateTime lastModifiedDate;
}
```

BaseEntity를 상속

```java
@Entity
public class Member extends BaseEntity {
    ...
}

@Entity
public class Team extends BaseEntity {
    ...
}
```

BaseEntity를 상속받은 엔티티들의 DDL을 살펴보자

```sql
Hibernate: 
    create table Member (
       id bigint generated by default as identity,
        createdBy varchar(255),
        createdDate timestamp,
        lastModifiedBy varchar(255),
        lastModifiedDate timestamp,
        age integer,
        description clob,
        roleType varchar(255),
        name varchar(255),
        locker_id bigint,
        team_id bigint,
        primary key (id)
    )
Hibernate: 
    create table Team (
       id bigint generated by default as identity,
        createdBy varchar(255),
        createdDate timestamp,
        lastModifiedBy varchar(255),
        lastModifiedDate timestamp,
        name varchar(255),
        primary key (id)
    )
```
BaseEntity에는 객체들이 주로 사용하는 공통 매핑 정보를 정의했다. 그리고 자식 엔티티들은 상속을 통해 BaseEntity의 매핑
정보를 물려받았다. 여기서 BaseEntity는 테이블과 매핑할 필요가 없고 자식 엔티티에게 공통으로 사용되는 매핑 정보만
제공하면 된다. 따라서 @MappedSuperclass를 사용했다.

부모로부터 물려받은 매핑 정보를 재정의 하려면 @AttributeOverrides(둘 이상을 재정의할 때)나 @AttributeOverride를 사용하고, 연관관계를 재정의하려면
@AssociationOverrides나 @AssociationOverride를 사용한다.

**@MappedSuperclass**의 특징
- 테이블과 매핑되지 않고 자식 클래스에 엔티티의 매핑 정보를 상속하기 위해 사용한다.
- @MappedSuperclass로 지정한 클래스는엔티티가 아니므로 `em.find()`나 JPQL에서 사용할 수 없다.
- 이 클래스를 직접 생성해서 사용할 일은 거의 없으므로 추상 클래스로 만드는 것을 권장한다.
  
정리

@MappedSuperclass는 테이블과는 관계가 없고 단순히 엔티티가 공통으로 사용하는 매핑 정보를 모아주는 역할을 할 뿐이다. 
ORM에서 이야기하는 진정한 상속 매핑은 이전에 학습한 객체 상속을 데이터베이스의 슈퍼타입 서브타입 관계와 매핑하는 것이다.

@MappedSuperclass를 사용하면 등록일자, 수정일자, 등록자, 수정자 같은 여러 엔티티에서 공통으로 사용하는 속성을 효과적으로
관리 할 수 있다.

`참고`

엔티티(@Entity)는 엔티티(@Entity)이거나 @MappedSuperclass로 지정한 클래스만 상속받을 수 있다.

## 7.3 복합 키와 식별 관계 매핑

### 7.3.1 식별 관계 vs 비식별 관계
데이터베이스 테이블 사이에 관계는 외래 키가 기본 키에 포함되는지 여부에 따라 식별 관계와 비식별 관계로 구분한다.

- 식별 관계
- 비식별 관계

**식별 관계**

식별 관계는 부모 테이블의 기본 키를 내려받아서 자식 테이블의 기본 키 + 외래 키로 사용하는 관계다.

**비식별 관계**

비식별 관계는 부모 테이블의 기본 키를 받아서 자식 테이블의 외래 키로만 사용하는 관계다.

비식별 관계는 외래 키에 NULL을 허용하는지에 따라 `필수적 비식별 관계`와 `선택 적 비식별 관계로`로 나눈다.

- 필수적 비식별 관계
  - 외래 키에 NULL을 허용하지 않는다. 연관관계를 필수적으로 맺어야 한다.
- 선택적 비식별 관계
  - 외래 키에 NULL을 허용한다. 연관관계를 맺을지 선택할 수 있다.
  
데이터베이스 테이블을 설계할 때 식별 관계나 비식별 관게 중 하나를 선택해야 한다. 최근에는 비식별 관계를 주로 사용하고 꼭
필요한 곳에만 식별관계를 사용하는 추세다.

### 7.3.2 식별관계와 비식별 관계를 매핑하는 법, 복합 키 : 비식별 관계 매핑

JPA에서 식별자를 둘 이상 사용하려면 별도의 식별자 클래스를 만들어야 한다.

JPA는 영속성 컨텍스트에 엔티티를 보관할 떄 엔티티의 식별자를키로 사용한다. 그릭 식별자를 구분하기 위해
equals와 hashCode를 사용해서 동승성 비교를 한다. 그런데 식별자 필드가 하나일 떄는 보통 자바의 기본 타입을 사용하므로 문제가
없지만, 식별자 필드가 2개 이상이면 별도의 식별자 클래스를 만들고 그곳에 equals와 hashCode를 구현해야 한다.

JPA는 복합 키를 지원하기 위해 `@IdClass`와 `@EmbeddedId` 2가지 방법을 제공하는데 @IdCLass는 관계형 데이터베이스에
가까운 방법이고, @EmbeddedId는 좀 더 객체지향에 가까운 방법이다. 먼저 @IdClass부터 알아보자.

**@IdClass**

**IdClass**를 사용할 떄 식별자 클래스는 다음 조건을 만족해야 한다.

```java
@Entity
@IdClass(ParentId.class)
public class Parent {

  @Id
  @Column(name = "PARENT_ID1")
  private String id1; // ParentId.id1과 연결

  @Id
  @Column(name = "PARENT_ID2")
  private String id2; // ParentId.id2과 연결
  
  private String name;
  ...

}
```

```java
public class ParentId implements Serializable {
    private String id1; // Parent.id1 매핑
    private String id2; // Parent.id2 매핑
  
    public ParentId() {
    }
    
    public ParentId(String id1, String id2) {
        this.id1 = id1;
        this.id2 = id2;
    }
    
    @Override
    public boolean equals(Object o) {...}

    @Override
    public int hashCode() {...}

}
```
- 식별자 클래스의 속성명과 엔티티에서 사용하는 식별자의 속성명이 같아야 한다.
- Serializable 인터페이스를 구현해야 한다.
- 기본 생성자가 있어야 한다.
- 식별자 클래스는 public어야 한다.

그럼 실제 어떻게 사용될까? 먼저 복합 키를 사용하는 엔티티를 저장해보자.
```java
Parent parent = new Parent();
parent.setId1("myId1"); // 식별자
parent.setId2("myId2"); // 식별자
parent.setName("parentName");
em.persist(parent);
```

저장 코드를 보면 식별자 클래스인 ParentId가 보이지 않는데, em.persist()를 호출하면 영속성 컨텍스트에
엔티티를 등록하기 직전에 내부에서 Parentid1, Parent.id2 값을 사용해서 식별자 클래스인 ParentId를 생성하고 영속성 컨텍
스트의 키로 사용한다.

복합키로 조히해보자.
```java
ParentId parentId = new ParentId("myId1", "myId2");
Parent parent = em.find(Parent.class, parentId);
```
조회 코드를 보면 식별자 클래스인 ParentId를 사용해서 엔티티를 조회한다. 이제 자식 클래스를 추가해보자.

```java
@Entity
public class child {

  @id
  private class child;
  
  @ManyToOne
  @JoinColumns({
          @JoinColumn(name = "PARENT_ID1",
                  referencedColumnName = "PARENT_ID1"),
          @JoinColumn(name = "PARENT_ID2",
                  referencedColumnName = "PARENT_ID2")
  })
  private Parent parentl
}
```
부모 테이블의 기본 키 컬림이 복합 키이므로 자식 테이블의 외래 키도 복합 키다. 따라서 외래 키 매핑 시 여러 컬럼을
매핑해야 하므로 @JoinColumns 어노테이션을 사용하고 각각의 외래 키 칼럼을 @JoinColumn으로 매핑한다.
참고로 예제처럼 @JoinColumn의 name 속성과 referencedColumnName 속성의 값이 같으면 referencedColumnName은 생략해도 된다.

**@EmbeddedId**

@IdClass가 데이터베이스에 맞춘 방법이라면 @EmbeddedId는 좀 더 객체지향적인 방법이다.

```java
@Entity
public class Parent {

  @EmbeddedId
  private ParentId id;
  
  private String name;
  ...
}
```

Parent 엔티티에서 식별자 클래스를 직접 사용하고 @EmbeddedId 어노테이션을 적어주면 된다.

```java
@Embeddable
public class ParentId implements Serializable {
  @Column(name = "PARENT_ID1")
  private String id1;
  @Column(name = "PARENT_ID2")
  private String id1;
  
  //equals and hashCode 구현
}
```

@IdClass와는 다르게 @EmbeddedId를 적용한 식별자 클래스는 식별자 클래스에 기본 키를 직접 매핑한다.

`@EmbeddedId`를 적용한 식별자 클래스는 다음 조건을 만족해야 한다.

- @Embeddable 어노테이션을 붙여주어야 한다.
- Serializable 인터페이스를 구현해야 한다.
- equals, hashCode를 구현해야 한다.
- 기본 생성자가 있어야 한다.
- 식별자 클래스는 public이어야 한다.

엔티티를 저장해보자.

```java
Parent parent = new Parent();
ParentId parentId = new ParentId("myId1", "myId2");
parent.setId(parentId);
parent.setName("parentName");
em.persist(parent);
```
저장하는 코드를 보면 식별자 클래스 parentId를 직접 생성해서 사용한다. 엔티티를 조회해보자.

```java
ParentId parentId = new ParentId("myId1", "myId2");
Parent parent = em.find(Parent.class, parentId);
```
조회 코드도 식별자 클래스 parentId를 직접 사용한다.

**복합 키와 equals(), hashCode()**
```java
ParentId id1 = new ParentId();
id1.setId1("myId1");
id1.setId2("myId2");

ParentId id2 = new ParentId();
id2.setId1("myId1");
id2.setId2("myId2");

id1.equals(id2) -> ?
```

id1과 id2는 인스턴스 둘 다 myId1, myId2라는 같은 값을 갖고 있지만 인스턴스는 다르다. 그렇다면 마지막 줄에 있는
id1.equals(id2)는 참일까 거짓일까?

equals()를 적절히 오버라이딩했다면 참이겠지만 equals()를 적절히 오버라이딩하지 않았다면 결과는 거짓이다.
왜냐하면 `자바의 모든 클래스는 기본으로 Object 클래스를 상속 받는데 이 클래스가 제공하는 기본 equals()는 인스턴스 
참조 값인 비교 == 비교(동일성 비교)를하기 때문이다.`

**IdClass vs @EmbeddedId**

- @EmbeddedId가 @IdClass와 비교해서 더 객체지향적이고 중복도 없어서 좋아보이긴 하지만 특정 상황에 JPQL이 조금 더 길어질 수 있다.
```java
em.createQuery(select p.id.id1, p.id.id2 from Parent p); // @EmbeddedId
em.createQuery(select p.id1, p.id2 from Parent p); // @IdClass
```

`참고`
복합 키에는 @GenerateValue를 사용할 수 없다. 복합 키를 구성하는 여러 칼럼 중 하나에도 사용할 수 없다.

### 7.3.3 복합 키 : 식별 관계 매핑


  </div>
</details>

<details>
  <summary>9. 프록시와 연관관계 관리</summary>
  <div markdown="1">

  </div>
</details>
참고 문헌 :

- [자바 ORM 표준 JPA 프로그래밍 / 김영한](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=62681446)
- [jpa](https://ultrakain.gitbooks.io/jpa/content/chapter1/chapter1.3.html)
- 인프런 인강 자료 / 김영한