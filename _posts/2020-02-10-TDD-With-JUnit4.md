---
layout: post
title: TDD With JUnit4
description: >
  2/10 TDD With JUnit4
tags: [study]
comments: true
---

# 2/10 TDD With JUnit4

## TDD

TDD : Test Driven Development

```
테스트 주도 개발(Test-driven development TDD)은 매우 짧은 개발 사이클을 반복하는 소프트웨어 개발 프로세스 중 하나이다.


우선 개발자는 바라는 향상 또는 새로운 함수를 정의하는 (초기적 결함을 점검하는) 자동화된 테스트 케이스를 작성한다.
그런 후에, 그 케이스를 통과하기 위한 최소한의 양의 코드를 생성한다.
그리고 마지막으로 그 새 코드를 표준에 맞도록 리팩토링한다.
(requirements are turned into very specific test cases, then the software is improved to pass the new tests, only.)


- 위키피디아
```

-> 제품 코드 만들기 전에 테스트 코드 먼저 작성한다.

-> TDD 과정에서 항상 리펙토링을 한다. 



### Testing Level

#### Unit Test

단위 테스트. 

```
유닛 테스트(unit test)는 컴퓨터 프로그래밍에서 소스 코드의 특정 모듈이 의도된 대로 정확히 작동하는지 검증하는 절차다.
즉, 모든 함수와 메소드에 대한 테스트 케이스(Test case)를 작성하는 절차를 말한다.


이를 통해서 언제라도 코드 변경으로 인해 문제가 발생할 경우, 단시간 내에 이를 파악하고 바로 잡을 수 있도록 해준다.
이상적으로, 각 테스트 케이스는 서로 분리되어야 한다.
이를 위해 가짜 객체(Mock object)를 생성하는 것도 좋은 방법이다.

- 위키피디아

Mock object : 의존성을 낮추기 위해 
```

#### Integration Test

통합 테스트. 환경변수, DB등에 의존하는 테스트.

``` 
DB와 통신
Network를 통해 통신(REST API 등)
file system사용 
```

#### System Test

시스템 통합적으로 테스트.

#### Acceptance Test

고객의 요구사항 만족 확인.



## JUnit

Java 에서 가장 유명한 Test.

### Life Cycle

@BeforeClass

@AfterClass

@Before

@After

@Test

```java
// 예제
public class LifecycleTest {
    @BeforeClass
    public static void setUpOnlyOnce() {
        System.out.println("setUpOnlyOnce");
    }

    @Before
    public void setUp() {
        System.out.println("setUp");
    }

    @Test
    public void test1() {
        System.out.println("test1");
    }

    @Test
    public void test2() {
        System.out.println("test2");
    }

    @After
    public void tearDown() {
        System.out.println("tearDown");
    }

    @AfterClass
    public static void tearDownOnlyOnce() {
        System.out.println("tearDownOnlyOnce");
    }
}
```

예제와 같은 경우 테스트가 2개이므로 총 8줄 출력된다. 단, 테스트 2개의 순서는 보장되지 않고, 랜덤하게 테스트 된다. 

##### @BeforeClass -> ( @Before -> @Test -> @After )*n -> @AfterClass

### Assertion

- assertEquals
- assertTrue
- assertFalse
- assertNull
- assertNotNull
- assertSame
- assertNotSame
- assertArrayEquals
- assertThat

```java
//예제

//parameter : description, 기댓값, 실제 값
assertEquals("10,000원으로 계좌 생성 후 잔고 조회", 10_000L, account.getBalance());

//description 생략도 가능
account.deposit(1_000L);
assertEquals(11_000L, account.getBalance());
```

### Matchers

- allOf
- anyOf
- not
- equalTo
- is
- hasToString
- instanceOf, isCompatibleType
- notNullValue, nullValue
- sameInstance
- hasEntry, hasKey, hasValue
- hasItem, hasItems
- hasItemInArray
- closeTo
- greaterThan, greaterThanOrEqualTo, lessThan, lessThanOrEqualTo
- equalToIgnoringCase
- equalToIgnoringWhiteSpace
- containsString, endsWith, startsWith

### Mocking

#### Mockito

1) 생성자 의존성 주입으로 Account Service의 단위테스트 준비

```java
AccountService accountService;
// mock
AccountRepository accountRepository;

@Before
public void setUp() {
    accountRepository = mock(AccountRepository.class)
    accountService = new AccountService(accountRepository)
}    
```

2) Mock사용

```java
@RunWith(MockitoJUnitRunner.class)
public class AccountServiceTest
    
// Mock 생성
    @Mock
    AccountRepository accountRepository;
// Mock 주입
    @InjectMocks
    AccountService accountService;
```

1)과 2)는 동일한 코드가 된다.

```java
// 예제
when(accountRepository.findById()).thenReturn(Optional.of(account));
```

- `thenReturn(T value)` : return value
- `then`, `thenAnswer(Answer answer)` : execute action + return value
- `thenThrow(Throwable... throwables)`: throw exception

##### Mockito.verify()

return 값이 없는 void함수의 경우 사용한다.  호출 횟수를 지정할 수 있다. 

- verification mode
  - times()
  - never()
  - atLeast()
  - atMost()
  - ...

##### 예제 

```java
@Test
public void getAccount() {
    // given(준비 작업)
    final Long accountId = 12L;

    Account account = new Account(10_000, "jordan");
    when(accountRepository.findById(accountId)).thenReturn(Optional.of(account));
    // when(확인하고 싶은 것)
    Account result = accountService.getAccount(accountId);
    // then
    // assertEquals사용도 가능. 단, equals 가 override되어있어야 함
    assertThat(result, is(account));
    verify(accountRepository).findById(accountId);
}
```

### WebMvcTest

예를 들어 3단계 레이어 형태에서 Class / Service / Mapper 중 Class만 따로 Unit test 할 수 있다.

```java
@RunWith(SpringRunner.class)
@WebMvcTest(AccountRestController.class)
public class AccountRestControllerTest
```

##### MockBean

Spring 에 등록된 Bean을 자동으로 MockBean으로 만들어준다.

```java
@MockBean
AccountService accountService;
```

##### SpyBean

진짜 객체를 Mock으로 만든다. Mocking 안 한 객체에 대해서는 실제 객체를 사용한다. 

단, anti-pattern의 증거로, SpyBean사용은 지양한다.

```java
@SpyBean
AccountService accountService;
```

##### 예제

```java
@RunWith(SpringRunner.class)
@WebMvcTest(CategoryRestController.class)
@Import({WebMvcConfiguration.class, ApiSecurityConfiguration.class})
public class CategoryRestControllerTest {
    // 스프링부트에서는 자동으로 사용가능.
    @Autowired
    MockMvc mockMvc;
    @MockBean
    CategoryQuerier categoryQuerier;

    @Test   // magazine/349
    public void list() throws Exception {
        // given
        List<CategorySummary> categories = randomListOf(10, CategorySummary.class);
        // 파라미터가 여러 개 일 경우, eq는 모두 감싸거나, 모두 감싸지 않는다
        given(categoryQuerier.getAllCategories(any(DeviceMeta.class))).willReturn(categories);
        CategorySummary first = categories.get(0);
        // when
        mockMvc.perform(get("/api/categories")
                            .accept(MediaType.APPLICATION_JSON))
               // then
               .andExpect(status().isOk())
               .andExpect(jsonPath("$.length()").value(categories.size()))
               .andExpect(jsonPath("$.[0].categoryId").value(first.getCategoryId()))
               .andExpect(jsonPath("$.[0].name").value(first.getName()))
        ;
        // 통합테스트 문법 bdd
        // then2
        then(categoryQuerier).should(times(1)).getAllCategories(any(DeviceMeta.class));
    }
}
```

### Integration Test

```java
@SuppressWarnings("WeakerAccess")
@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = NONE)
public class CuratorServiceIntegrationTest {
    @Autowired
    CuratorService curatorService;
    @Autowired
    CuratorRepository curatorRepository;

    String memberId;

    /**
     * 매거진/51 큐레이터 승인 안되는 오류
     */
    // @Transactional
    @Test
    public void active() throws Exception {
        // given에서 생성하는 부분을 setup으로 빼도 좋다.
        // given
        memberId = random(String.class);
        Curator curator = Curator.register(memberId, random(String.class));
        curator.deactive(random(String.class));
        curatorRepository.save(curator);
        String modifyMemberId = random(String.class);
        // when
        curatorService.active(memberId, modifyMemberId);
        // then
        Curator after = curatorRepository.findOne(memberId);
        assertThat(after.isActive(), is(true));
        assertThat(after.getModify().getMemberId(), is(modifyMemberId));
    }

    // 위의 transactional을 활성화 하는 경우 
    // 아래 after에 해당하는 부분이 없어도 정상적으로 동작한다.
    @After
    public void tearDown() throws Exception {
        if (memberId != null) {
            curatorRepository.delete(memberId);
        }
    }
}
```

### MyBatis Test

MyBatis test의 경우 로컬에서 임베디드 DB를 띄워서 test 하는 게 좋다. (DB 정합성의 문제)

##### RealDB

```java
@RunWith(SpringRunner.class)
@MybatisTest
@AutoConfigureTestDatabase(replace = AutoConfigureTestDatabase.Replace.NONE)
```

```java
@RunWith(SpringRunner.class)
@WebMvcTest
// WebMvcTest에 Mybatis도 얹어서 test
@AutoConfigureMybatis // Specify instead of @MybatisTest
public class PingTests {

  @Autowired
  private MockMvc mvc;

  @Test
  public void ping() throws Exception {
    this.mvc.perform(get("/ping"))
        .andExpect(status().isOk())
        .andExpect(content().string("OK"));
  }

}
```

