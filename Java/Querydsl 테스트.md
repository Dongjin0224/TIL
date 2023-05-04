# @DataJpaTest
JPA에 관련된 요소들만 테스트하기 위한 어노테이션.

JPA 테스트에 관련된 설정들만 적용해준다.

메모리상에 내부 데이터베이스를 생성하고 @Entity 클래스들을 등록하고 JPARepository 설정들을 해준다. (실제 DB로도 테스트 가능하지만 권장하지 않는다.)

각 테스트마다 테스트가 완료되면 관련한 설정들은 롤백됨. (@Transactional 을 기본적으로 내장하고 있으므로, 매 테스트 코드가 종료되면 자동으로 롤백.)

## 설정 방법
```java
@DataJpaTest
@ActiveProfiles("test") // application-test.yml 파일을 만들었음
@Import(TestConfig.class)
class RepositoryTest {

  @Autowired
  EntityDvrp0010Repository repository;
  @Autowired
  private EntityManager entityManager;
  @Autowired
  private JPAQueryFactory jpaQueryFactory;
  
  //////
```
CustomRepositry를 사용하는 패턴에서 테스트 방법.

만약 ```@Import(TestConfig.class)```가 없다면 JpaQueryFactory가 빈등록이 되지 않아 테스트가 정상적으로 작동하지 않는다.

```java
@TestConfiguration
public class TestConfig {
  @PersistenceContext
  private EntityManager entityManager;

  @Bean
  public JPAQueryFactory jpaQueryFactory() {
    return new JPAQueryFactory(entityManager);
  }
}
```
이렇게 테스트에서 사용할 JPAQueryFactory를 빈으로 등록.

이후에 ```@Import``` 어노테이션을 사용하면 Querydsl 테스트 가능.

테스트 클래스에서 ```@ActiveProfiles```  어노테이션을 사용하여 특정 프로파일을 활성화하고 있는지 확인.


