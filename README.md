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

영속성 컨텍스트는 JPA에서 가장 중요한 개념중 하나로,  **논리적인 개념, 눈에 보이지 않으며 Entity를 영구히 저장하는 환경** 이라는 뜻이다.

영속성 컨텍스트를 이해하기전에 먼저 EntityManagerFactory와 EntityManager를 간단하게 이해하고 넘어가자.



- EntityManagerFactory는 고객이 요청이 올 때마다 (쓰레드가 하나 생성될 때마다) EntityManager를 생성한다.
- EntityManager는 내부적으로 DB connection pool을 사용해서 DB에 접근한다.
- EntityManagerFactory
  - JPA 는 EntityManagerFactory 를 만들어야 한다.
  - application loading 시점에 DB 당 딱 하나만 생성되어야 한다.
  -  WAS 가 종료되는 시점에 EntityManagerFactory 를 닫는다. 그래야 내부적으로 Connection pooling 에 대한 Resource 가 Release 된다.

- EntityManager
  - 실제 Transaction 단위를 수행할 때마다 생성한다.
  - 즉, 고객의 요청이 올 때마다 사용했다가 닫는다.
  - thread 간에 공유하면 안된다. (사용하고 버려야 한다.)
- EntityTransaction
  - Data 를 “변경”하는 모든 작업은 반드시 Transaction 안에서 이루어져야 한다.
  - 단순한 조회의 경우는 상관없음.
  -

엔티티의 생명주기는 크게 4가지로 나뉜다.

1. 비영속(new/ transient ) 상태 : 영속성 컨텍스트와 전혀 관계가 없는 새로운 상태

   ![비영속상태](https://user-images.githubusercontent.com/39195377/97031972-06513980-159c-11eb-81c4-fde00dc09533.PNG)

   -객체를 단순히 '생성만' 한 상태로, 영속성 컨텍스트와는 전혀 관계가 없다.

   ```java
   Member member = new Member();
   member.setId("member1");
   member.setUsername("회원1");
   ```



2. 영속(managed) 상태 : 영속성 컨텍스트에 **관리** 되는 상태

![영속상태](https://user-images.githubusercontent.com/39195377/97031995-0cdfb100-159c-11eb-8bf9-e03d3a4b43eb.PNG)


-영속성 컨텍스트에 저장되어있는 상태

-persist는 DB에 쿼리를 날려 DB에 저장하는 작업이 아닌, 객체(Entity)를 영속성 컨텍스트에 저장하는 작업

   ```java
   // 객체를 생성한 상태 (비영속)
   Member member = new Member();
   member.setId("member1");
   member.setUsername("회원1");
   EntityManager entityManager = entityManagerFactory.createEntityManager();
   entityManager.getTransaction().begin();
   // 객체를 저장한 상태 (영속)
   entityManager.persist(member);
   ```



3. 준영속(detached) 상태 : 영속성 컨텍스트에 있다가 빠져나온(분리)된 상태

   -영속성 컨텍스트에서 지운 상태

   ```java
   // 회원 엔티티를 영속성 컨텍스트에서 분리, 준영속 상태
   entityManager.detach(member);
   ```



4. 삭제(removed) 상태 : 삭제된 상태

   -실제로 DB에서 삭제를 요청한 상태

   ```java
   // 객체를 삭제한 상태
   entityManager.remove(member);
   ```



★1차캐시 : 영속성 컨텍스트에는 1차캐시라는것이 존재하는데, 1차캐시를 영속성 컨텍스트라고 이해해도 좋다.

![1차캐시](https://user-images.githubusercontent.com/39195377/97031946-00f3ef00-159c-11eb-9a44-cdbbc0414d51.PNG)

```java
//엔티티를 생성한 상태(비영속) 
Member member = new Member(); 
member.setId("member1"); 
member.setUsername("회원1");   
//엔티티를 영속(1차캐시에 저장)
em.persist(member);
```



**영속성 컨텍스트는 아래와 같은 메커니즘을 가지고 동작한다. Member라는 객체로 예를 들겠다.**

1. 먼저 JPA에서 member라는 객체를 조회하면, DB에 select 쿼리문을 날려서 조회를 하기 전에 1차캐시에 조회하려던 member가 저장되어있는지 확인한다.
2. 만약 1차캐시에 **이미 저장되어있다면** 조회 쿼리를 날리지 않고 1차 캐시에 있는 데이터를 가져온다.
3. 만약 1차 캐시에 데이터가 존재하지 않는다면 조회 쿼리를 날린 후 DB에서 조회를 하고 1차캐시에 저장한 다음에 값을 반환한다.

그러나 사실 1차 캐시는 큰 성능 이점을 가지고 있지는 않다.

EntityManger는 트랜잭션 단위로 만들고, 해당 DB 트랜잭션이 종료될때 같이 종료된다. 즉 1차 캐시도 모두 소멸되기 때문에 아주 짧은 찰나의 순간에만 성능 이점을 가진다.



★영속성 컨텍스트는 동일성을 보장한다.

```java
Member a = entityManager.find(Member.class, "member1");
Member b = entityManager.find(Member.class, "member1");
System.out.println(a == b); // 동일성 비교 true
```

- 영속 Entity 동일성(==비교) 를 보장해준다.
- member1 이라는 Entity를 2번 조회하면, select 조회 쿼리가 한번만 나가고, 그 이후에 조회되는것은 1차캐시에 가져오기때문에 조회쿼리가 나가지 않는다.



★엔티티 등록시, 트랜잭션을 지원하는 쓰기지연

```java
transaction.begin(); // Transaction 시작
entityManager.persist(memberA);
entityManager.persist(memberB);
// 이때까지 INSERT SQL을 DB에 보내지 않는다.
// 커밋하는 순간 DB에 INSERT SQL을 보낸다.
transaction.commit(); // Transaction 커밋 

```

![쓰기지연](https://user-images.githubusercontent.com/39195377/97031981-09e4c080-159c-11eb-95e1-bdf569314fb8.PNG)

쓰기지연은 아래와 같은 메커니즘으로 실행된다.

1. entityManager.persist(memberA) 가 실행되면, memberA를 1차캐시에 저장한다.
2. 1)과 동시에 JPA가 Entity를 분석하여 insert 쿼리를 만든다.
3. insert 쿼리를 바로 실행하지 않고 쓰기 지연 SQL 저장소에 쌓아둔다.
4. memberB도 동일하게 이루어진다.
5. tansaction.commit() 호출과 동시에 쓰기지연 SQL 저장소에 쌓여있는 쿼리들을 실행한다.



★JPA는 엔티티 '수정'시 변경 감지(Dirty Checking)을 지원한다.

```java
transaction.begin(); // Transaction 시작

// 영속 엔티티 조회
Member memberA = em.find(Member.class, "memberA");

// 영속 엔티티 데이터 수정
memberA.setUsername("hi");
memberA.setAge(10);

transaction.commit(); // Transaction 커밋
```

위 코드는 이미 생성된 entity의 정보를 변경하는 과정이다. 이 과정을 보면 아래와 같은 의문이 생길 수 있다.

```java
 em.update(member) 또는 em.persist(member) 
     //이런 코드가 있어야 하지 않을까???
```

아니다. 필요없다. Entity 데이터만 수정하고 commit하면 알아서 DB에 반영된다.

즉, 데이터를 set하면 해당 데이터의 변경을 감지하여 자동으로 UPDATE 쿼리가 실행된다.

**(실무에서는 set 사용을 지양해야 한다.)**

![변경감지](https://user-images.githubusercontent.com/39195377/97031963-04877600-159c-11eb-830d-96fa1dbff703.PNG)

변경 감지는 아래와같은 매커니즘으로 동작한다.

1. commit()을 하면 , <u>**flush()**</u> 가 일어날때 1차캐시에 있는 스냅샷과 일일이 비교한다.
2. 변경사항이 있으면 UPDATE 쿼리를 만들어서 쓰기 지연 SQL에 쌓아둔다.
3. UPDATE 쿼리 실행 후 commit() 한다

<u>**flush()**</u> : 영속성 컨텍스트에 저장된 데이터들을 DB에 반영하는 작업 (commit할때 자동으로 발생) 이다. <u>여기서 주의해야 할 점은, flush()는 영속성 컨텍스트의 값을 비우는 작업이 아니고, 단순히 DB에 반영하는 작업이다.</u>



JPA에서 flush는 아래 세 가지 상황에 발생한다.

1. entityManager.flush() 로 직접 호출
2. 트랜잭션 커밋
3. JPQL 쿼리를 실행

3번의 경우(JPQL 쿼리 실행시 자동으로 flush)의 이유는 아래와 같다.

```java
entityManager.persist(memberA);
entityManager.persist(memberB);
entityManager.persist(memberC);

//바로 아래에 JPQL을 실행한다고 가정해보자. 
//여기선 간단하게 member를 조회하는 JPQL을 실행한다고 가정
query = entityManager.createQuery("select m from member m", Member.class);
List<Member> members = query.getResultList();
```

만약 위의 코드와 같이 memberA~memberC까지 영속성 컨텍스트에 저장했다고 가정하자.

아직 트랜잭션 커밋이 일어나기 전이라 memberA~memberC는 DB에 저장되기 전 상태이다.

따라서 JPQL 실행시 아무런 값도 반환되지 않는다.

이러한 문제를 방지하기 위해 JPA는 기본적으로 JPQL 실행시 자동으로 flush를 실행한다.

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