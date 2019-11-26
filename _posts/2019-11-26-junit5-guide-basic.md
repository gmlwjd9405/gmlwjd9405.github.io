---
layout: post
title: '[JUnit] JUnit5 사용법 - 기본'
subtitle: 'junit5의 기본 사용법을 이해할 수 있다.'
date: 2019-11-26
author: heejeong Kwon
cover: '/images/etc/junit5-main.png'
tags: java test junit junit5
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

<span style="background-color: #e1e1e1">계속해서 추가할 예정!</span>  

### Maven Dependencies
```xml
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter-engine</artifactId>
    <version>5.1.0</version>
    <scope>test</scope>
</dependency>
```
```xml
testRuntime("org.junit.jupiter:junit-jupiter-engine:5.1.0")
```

--- 

### 1. 기본 Annotation
#### @BeforeAll and @BeforeEach
- **@BeforeAll**
    - 해당 annotation 이 달린 메서드가 현재 클래스의 모든 테스트 메서드보다 먼저 실행된다.
    - 해당 메서드는 static 이어야 한다.
    - 이전의 `@BeforeClass` 와 동일 
- **@BeforeEach**
    - 해당 annotation 이 달린 메서드가 각 테스트 메서드 전에 실행된다.
    - 이전의 `@Before` 와 동일 

 ```java
@BeforeAll
static void setup() {
    log.info("@BeforeAll - executes once before all test methods in this class");
}
@BeforeEach
void init() {
    log.info("@BeforeEach - executes before each test method in this class");
}
```
   
#### @DisplayName and @Disabled
- **@DisplayName**
    - 테스트 클래스 또는 테스트 메서드의 이름을 정의할 수 있다.
- **@Disable**
    - 테스트 클래스 또는 메서드를 비활성화할 수 있다.
    - 이전의 `@Ignore` 와 동일 

```java
@DisplayName("Single test successful")
@Test
void testSingleSuccessTest() {
    log.info("Success");
}
@Test
@Disabled("Not implemented yet")
void testShowSomething() {
}
```

#### @AfterEach and @AfterAll
- **@AfterAll**
    - 해당 annotation 이 달린 메서드가 현재 클래스의 모든 테스트 메소드보다 이후에 실행된다.
    - 해당 메서드는 static 이어야 한다.
    - 이전의 `@AfterClass` 와 동일 
- **@AfterEach**
    - 해당 annotation 이 달린 메서드가 각 테스트 메서드 이후에 실행된다.
    - 이전의 `@After` 와 동일 

```java
@AfterAll
static void done() {
    log.info("@AfterAll - executed after all test methods.");
}
@AfterEach
void tearDown() {
    log.info("@AfterEach - executed after each test method.");
}
```

### 2. Assertions and Assumptions
#### Assertions
- `org.junit.jupiter.api.Assertions`로 이동
- **assertTrue, assertThat** 등 
    ```java
    @Test
    void lambdaExpressions() {
        assertTrue(Stream.of(1, 2, 3)
          .stream()
          .mapToInt(i -> i)
          .sum() > 5, () -> "Sum should be greater than 5");
    }
    ```
- **assertAll**
    ```java
    @Test
    void groupAssertions() {
        int[] numbers = {0, 1, 2, 3, 4};
        assertAll("numbers",
            () -> assertEquals(numbers[0], 1),
            () -> assertEquals(numbers[3], 3),
            () -> assertEquals(numbers[4], 1)
        );
    }
    ```
    - `assertAll()`을 사용하여 assertions 을 그룹화하여 그룹 내에서 실패한 assertions 을 MultipleFailuresError 와 함께 기록할 수 있다.
    - 즉, 실패의 정확한 위치를 정확히 파악할 수 있기 때문에 보다 복잡한 assertions 을 만들어도 안전하게 사용할 수 있다.

#### Assumptions
- 특정 조건이 충족되는 경우에만 테스트를 실행하는 데 사용된다.
    - 일반적으로 테스트가 제대로 실행되기 위해 필요한 외부 조건에 사용된다.
    - 테스트와 직접적인 관련은 없다.
- assumptions 이 실패하면 `TestAbortedException`이 발생하고 테스트는 수행되지 않는다.
- **assumeTrue(), assumeFalse(), and assumingThat()**
    ```java
    @Test
    void trueAssumption() {
        assumeTrue(5 > 1);
        assertEquals(5 + 2, 7);
    }
    @Test
    void falseAssumption() {
        assumeFalse(5 < 1);
        assertEquals(5 + 2, 7);
    }
    @Test
    void assumptionThat() {
        String someString = "Just a string";
        assumingThat(
            someString.equals("Just a string"),
            () -> assertEquals(2 + 2, 4)
        );
    }
    ```

### 3. Exception Testing
- **assertThrows()**
- 두 가지 예외 테스트 방법
    - 모두 `assertThrows()` 메서드를 사용하여 구현할 수 있다.
    1. 발생한 예외의 세부 사항을 확인
    ```java
    @Test
    void shouldThrowException() {
        Throwable exception = assertThrows(UnsupportedOperationException.class, () -> {
          throw new UnsupportedOperationException("Not supported");
        });
        assertEquals(exception.getMessage(), "Not supported");
    }
    ```
    2. 예외 유형의 유효성을 검사 
    ```java
    @Test
    void assertThrowsException() {
        String str = null;
        assertThrows(IllegalArgumentException.class, () -> {
          Integer.valueOf(str);
        });
    }
    ```


### 4. Test Suites
- **@SelectPackages**
    - Test Suites 를 실행할 때 선택할 패키지 이름 지정
    ```java
    @RunWith(JUnitPlatform.class)
    @SelectPackages("com.baeldung")
    public class AllUnitTest {} 
    ```
- **@SelectClasses**
    - Test Suites 를 실행할 때 선택할 클래스 지정
    ```java
    @RunWith(JUnitPlatform.class)
    @SelectClasses({AssertionTest.class, AssumptionTest.class, ExceptionTest.class})
    public class AllUnitTest {}
    ```
    - 각 테스트는 하나의 패키지에 있지 않아도 된다.


### 5. Dynamic Tests
- **@TestFactory**
    - 해당 annotation 이 달린 메서드는 동적 테스트를 위한 *test factory 메서드*이다.
    - 런타임에 생성된 테스트 케이스를 선언하고 실행할 수 있다. 
    - e.g. 각각 in, out 이라는 두 개의 ArrayList 를 사용하여 단어를 번역한다.
        ```java
        @TestFactory
        public Stream<DynamicTest> translateDynamicTestsFromStream() {
            return in.stream()
              .map(word ->
                  DynamicTest.dynamicTest("Test translate " + word, () -> {
                    int id = in.indexOf(word);
                    assertEquals(out.get(id), translate(word));
                  })
            );
        }
        ```
    - `@TestFactory` 메서드는 private 또는 static 이면 안된다.
    - 테스트 수는 동적이며, ArrayList 크기에 따라 달라진다.
    - `@TestFactory` 메서드는 Stream, Collection, Iterable 또는 Iterator 를 return 해야 한다. 

### 6. ETC
- **@Nested**
    - 해당 annotation 이 달린 클래스는 중첩된 비정적(non-static) 테스트 클래스 임을 나타낸다.
    - 테스트 클래스 안에서 내부 클래스(Inner Class)를 정의해 테스트를 계층화할 수 있다. 
    - 내부 클래스로 정의하기 때문에 부모 클래스의 멤버 필드에 접근할 수 있고, Before/After 와 같은 테스트 생명주기에 관계된 메소드들도 계층에 맞게 동작한다.
- **@Tag**
    - 테스트 필터링을 위한 태그를 선언할 수 있다.
- **@ExtendWith**
    - 사용자 정의 확장명을 등록하는 데 사용된다.
- **@RepeatedTest**
    - `@RepeatedTest(반복횟수)` 를 이용하면 해당 테스트를 반복 실행할 수 있다.
    
--- 

# 관련된 Post
- JUnit5 Parameterized Tests의 기본적인 사용법에 대해 알고 싶으시면 [JUnit5 Parameterized Tests 기본 사용법](https://gmlwjd9405.github.io/2019/11/27/junit5-guide-parameterized-test.html)을 참고하시기 바랍니다.


# References
- [A Guide to JUnit 5](https://www.baeldung.com/junit-5)
- [Guide to Dynamic Tests in Junit 5](https://www.baeldung.com/junit5-dynamic-tests?utm_content=buffer2b72f&utm_medium=social&utm_source=twitter.com&utm_campaign=buffer)
- [https://www.journaldev.com/20834/junit5-tutorial](https://www.journaldev.com/20834/junit5-tutorial)
- [https://reiphiel.tistory.com/entry/junit5-features](https://reiphiel.tistory.com/entry/junit5-features)

