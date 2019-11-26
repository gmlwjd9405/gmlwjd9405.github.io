---
layout: post
title: '[JUnit] JUnit5 사용법 - Parameterized Tests'
subtitle: 'junit5 Parameterized Tests의 기본 사용법을 이해할 수 있다.'
date: 2019-11-27
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
    <artifactId>junit-jupiter-params</artifactId>
    <version>5.4.2</version>
    <scope>test</scope>
</dependency>
```
```xml
testCompile("org.junit.jupiter:junit-jupiter-params:5.4.2")
```

### Parameterized Tests
- **@ParameterizedTest**
    - 이 annotation을 추가하는 것을 제외하고는 다른 테스트와 동일하다.
- e.g.
    ```java
    @ParameterizedTest
    @ValueSource(ints = {1, 3, 5, -3, 15, Integer.MAX_VALUE}) // six numbers
    void isOdd_ShouldReturnTrueForOddNumbers(int number) {
        assertTrue(Numbers.isOdd(number));
    }
    ```
    - 6번의 isOdd 메서드를 실행한다.
    - *a source of arguments*: an int array
    - *a way to access them*: the number parameter
    
#### 1. Simple Value
- **@ValueSource**
    - 해당 annotation 에 지정한 배열을 파라미터 값으로 순서대로 넘겨준다.
    - test method 실행 당 **하나의 인수(argument)** 만을 전달할 때 사용할 수 있다.
    - 리터럴 값의 배열을 테스트 메서드에 전달한다.
    - c.f. *literal values* 종류
        - short, byte, int, long, float, double, char, java.lang.String, java.lang.Class 
- e.g. 하나의 인수(String input)만을 가짐 
    ```java
    public class Strings {
        public static boolean isBlank(String input) {
            return input == null || input.trim().isEmpty();
        }
    }
    ```
    ```java
    @ParameterizedTest
    @ValueSource(strings = {"", "  "})
    void isBlank_ShouldReturnTrueForNullOrBlankStrings(String input) {
        assertTrue(Strings.isBlank(input));
    }
    ```
    
#### 2. Null and Empty Values
- **@NullSource**
    - primitive data 는 널(null) 값을 허용할 수 없으므로 primitive 인수에 `@NullSource` 를 사용할 수 없다.
- **@EmptySource**
    - 이 annotation 을 사용하여 빈 값을 인수에 전달할 수 있다. 
    - String 인수의 경우 ""(empty string), Collection types 과 Arrays 에도 빈 값을 넣을 수 있다. 
- **@NullAndEmptySource**
    - null 값과 empty value 모두 전달하기 위해 해당 annotation 을 사용할 수 있다. 
    - `@EmptySource`와 마찬가지로 문자열, 컬렉션 및 배열에서 동작한다.
    - 몇 가지 빈 문자열 변형을 parameterized test 로 전달하기 위해 **@ValueSource, @NullSource 및 @EmptySource**를 함께 결합할 수 있다.
    ```java
    @ParameterizedTest
    @NullAndEmptySource
    void isBlank_ShouldReturnTrueForNullAndEmptyStrings(String input) {
        assertTrue(Strings.isBlank(input));
    }
    ```
    
#### 3. Enum
- **@EnumSource**
    - enumeration(열겨형) 값의 배열을 테스트 메서드에 전달한다.
    - test method 실행 당 **하나의 인수(argument)** 만을 전달할 때 사용할 수 있다.
    - *names* 속성
        - 리터럴 문자열 외에도 정규 표현식(regular expression)을 *names* 속성에 전달할 수 있다.
        - 기본적으로 *names* 속성은 일치하는 enum 값을 가진다. 
        - mode 속성을 **EXCLUDE** 로 설정하면 제외하는 enum 값을 가질 수 있다. 
- e.g. 모든 월(Month enum)을 확인하여 월 숫자가 1과 12 사이에 있는지 확인하는 경우 
    ```java
    @ParameterizedTest
    @EnumSource(Month.class) // passing all 12 months
    void getValueForAMonth_IsAlwaysBetweenOneAndTwelve(Month month) {
        int monthNumber = month.getValue();
        assertTrue(monthNumber >= 1 && monthNumber <= 12);
    }
    ```
- e.g. 월의 '이름' 속성을 사용하여 특정 개월만 선택하는 경우 (4, 6, 9, 11월의 기간이 30일인지 확인)
    ```java
    @ParameterizedTest
    @EnumSource(value = Month.class, names = {"APRIL", "JUNE", "SEPTEMBER", "NOVEMBER"})
    void someMonths_Are30DaysLong(Month month) {
        final boolean isALeapYear = false;
        assertEquals(30, month.length(isALeapYear));
    }
    ```
- e.g. 2, 4, 6, 9, 11월을 **제외**한 월의 기간이 31일인지 확인하는 경우 
    ```java
    @ParameterizedTest
    @EnumSource(
      value = Month.class,
      names = {"APRIL", "JUNE", "SEPTEMBER", "NOVEMBER", "FEBRUARY"},
      mode = EnumSource.Mode.EXCLUDE)
    void exceptFourMonths_OthersAre31DaysLong(Month month) {
        final boolean isALeapYear = false;
        assertEquals(31, month.length(isALeapYear));
    }
    ```

#### 4. Method
- **@MethodSource**
    - 위의 argument sources들은 복잡한 object를 사용하여 전달하는 것이 어렵거나 불가능하다. 보다 복잡한 인수를 제공하는 방법 중 한 가지는 method를 argument source로 사용하는 것이다.
    - 즉, `@MethodSource`는 test method 실행 당 **복잡한 인수** 를 전달할 때 사용하는 방법이다.
        - test method 호출 당 하나의 인수(argument)를 전달하는 경우는 `<Arguments>` 추상화를 사용하지 않아도 된다. 
    - `@MethodSource` 에 설정하는 이름은 존재하는 메서드 이름이어야 한다.
        - 이름을 설정하지 않으면 JUnit은 test method와 이름이 같은 source method를 찾는다.
    - ***argument source method 조건***
        - `Stream<Arguments>` (혹은 Iterable, Iterator) 또는 `List와 같은 컬렉션`과 유사한 interface 를 반환할 수 있다.
        - 클래스 단위 생명 주기가 아닌 경우, static method 여야 한다.
    - `@MethodSource`를 이용하면 다른 테스트 클래스 간에 인수를 공유하는 것이 유용하다.
        - 이 경우 정규화된 이름 (*FQN#methodName format*)으로 현재 클래스 외부의 source method 를 참조할 수 있다.
- e.g. 기본 사용 예제 - 복잡한 인수(String input, boolean expected)를 가짐
    ```java
    @ParameterizedTest
    @MethodSource("provideStringsForIsBlank") // needs to match an existing method.
    void isBlank_ShouldReturnTrueForNullOrBlankStrings(String input, boolean expected) {
        assertEquals(expected, Strings.isBlank(input));
    }
    ```
    ```java
    // a static method that returns a Stream of Arguments
    private static Stream<Arguments> provideStringsForIsBlank() { // argument source method
        return Stream.of(
          Arguments.of(null, true),
          Arguments.of("", true),
          Arguments.of("  ", true),
          Arguments.of("not blank", false)
        );
    }
    ```
- e.g. 외부 source method 참조 예제 
    ```java
    class StringsUnitTest {
        @ParameterizedTest
        @MethodSource("com.baeldung.parameterized.StringParams#blankStrings") // 클래스 외부의 source method
        void isBlank_ShouldReturnTrueForNullOrBlankStringsExternalSource(String input) {
            assertTrue(Strings.isBlank(input));
        }
    }
    ``` 
    ```java
    public class StringParams {
        static Stream<String> blankStrings() {
            return Stream.of(null, "", "  ");
        }
    }
    ```

--- 

# 관련된 Post
- JUnit5의 기본적인 사용법에 대해 알고 싶으시면 [JUnit5 기본 사용법](https://gmlwjd9405.github.io/2019/11/26/junit5-guide-basic.html)을 참고하시기 바랍니다.


# References
- [Guide to JUnit 5 Parameterized Tests](https://www.baeldung.com/parameterized-tests-junit-5)

