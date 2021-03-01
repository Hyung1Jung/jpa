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
  
**Transient**
이 필드는 매핑하지 않는다. 따라서 데이터베이스에 저장하지도 않고 조회하지도 않는다. 객체에 임시로 어떤 값을 보관하고 싶을 때
사용한다.

**Access**
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

  </div>
</details>

참고 문헌 :

- [자바 ORM 표준 JPA 프로그래밍 / 김영한](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=62681446)
- [jpa](https://ultrakain.gitbooks.io/jpa/content/chapter1/chapter1.3.html)
- 인프런 인강 자료 / 김영한