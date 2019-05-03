---
layout: post
title: '[JavaScript] 호이스팅(Hoisting)이란'
subtitle: '자바스크립트의 호이스팅(Hoisting)에 대해 이해한다.'
date: 2019-04-22
author: heejeong Kwon
cover: '/images/javascript/javascript-main.png'
tags: javascript function hoisting
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

[Edwith 강의](https://www.edwith.org/boostcourse-web/lecture/16695/) 참고

## Goal
> - 호이스팅(Hoisting)이란 무엇인지 이해한다.
> - 함수선언문과 함수표현식에서의 호이스팅 차이를 이해한다.
>   - let/const와 var 변수 선언에서의 호이스팅 예시 
> - 같은 이름의 var 변수 선언과 함수 선언에서의 호이스팅에 대해 이해한다.

## 호이스팅(Hoisting)의 개념 
<span style="background-color: #e1e1e1">함수 안에 있는 선언들을 모두 끌어올려서 해당 함수 유효 범위의 최상단에 선언하는 것</span>을 말한다.

### 호이스팅이란
- 자바스크립트 함수는 실행되기 전에 함수 안에 필요한 변수값들을 모두 모아서 **유효 범위의 최상단**에 선언한다.
    - 자바스크립트 Parser가 함수 실행 전 해당 함수를 한 번 훑는다.
    - 함수 안에 존재하는 변수/함수선언에 대한 정보를 기억하고 있다가 실행시킨다.
    - 유효 범위: 함수 블록 `{}` 안에서 유효
- 즉, 함수 내에서 아래쪽에 존재하는 내용 중 필요한 값들을 끌어올리는 것이다. 
    - 실제로 코드가 끌어올려지는 건 아니며, 자바스크립트 Parser 내부적으로 끌어올려서 처리하는 것이다.
    - 실제 메모리에서는 변화가 없다.

### 호이스팅의 대상
- **var 변수 선언**과 **함수선언문**에서만 호이스팅이 일어난다.
    - var 변수/함수의 **선언**만 위로 끌어 올려지며, **할당**은 끌어 올려지지 않는다.
    - *let/const 변수 선언과 함수표현식*에서는 호이스팅이 발생하지 않는다. 
- 간단한 예시 (var 변수 vs let/const 변수)
    ```js
    console.log("hello");
    var myname = "HEEE"; // var 변수 
    let myname2 = "HEEE2"; // let 변수 
    ```
    ```js
    /** --- JS Parser 내부의 호이스팅(Hoisting)의 결과 - 위와 동일 --- */
    var myname; // [Hoisting] "선언"
    console.log("hello");
    myname = "HEEE"; // "할당"
    let myname2 = "HEEE2"; // [Hoisting] 발생 X
    ```
- 간단한 예시 (함수선언문 vs 함수표현식)
    ```js
    foo();
    foo2();

    function foo() { // 함수선언문
            console.log("hello");
    }
    var foo2 = function() { // 함수표현식
            console.log("hello2");
    }
    ```
    ```js
    /** --- JS Parser 내부의 호이스팅(Hoisting)의 결과 - 위와 동일 --- */
    var foo2; // [Hoisting] 함수표현식의 변수값 "선언"

    function foo() { // [Hoisting] 함수선언문
            console.log("hello");
    }

    foo();
    foo2(); // ERROR!! 

    foo2 = function() { 
            console.log("hello2");
    }
    ```
- 호이스팅은 함수선언문과 함수표현식에서 서로 다르게 동작하기 때문에 주의해야 한다.
    - 변수에 할당된 함수표현식은 끌어 올려지지 않기 때문에 이때는 변수의 스코프 규칙을 그대로 따른다.


## 함수선언문과 함수표현식에서의 호이스팅

> 들어가기 전: [함수선언문과 함수표현식의 차이](https://gmlwjd9405.github.io/2019/04/20/function-declaration-vs-function-expression.html)는 다음 POST를 참고하자. 

### 함수선언문에서의 호이스팅 
- 함수선언문은 코드를 구현한 위치와 관계없이 자바스크립트의 특징인 호이스팅에 따라 브라우저가 자바스크립트를 해석할 때 맨 위로 끌어 올려진다.

```js
/* 정상 출력 */
function printName(firstname) { // 함수선언문 
    var result = inner(); // "선언 및 할당"
    console.log(typeof inner); // > "function"
    console.log("name is " + result); // > "name is inner value"

    function inner() { // 함수선언문 
        return "inner value";
    }
}

printName(); // 함수 호출 
```
```js
/** --- JS Parser 내부의 호이스팅(Hoisting)의 결과 - 위와 동일 --- */
/* 정상 출력 */
function printName(firstname) { 
    var result; // [Hoisting] var 변수 "선언"

    function inner() { // [Hoisting] 함수선언문
        return "inner value";
    }

    result = inner(); // "할당"
    console.log(typeof inner); // > "function"
    console.log("name is " + result); // > "name is inner value"
}

printName(); 
```
  - 즉, 해당 예제에서는 함수선언문이 아래에 있어도 printName 함수 내에서 inner를 function으로 인식하기 때문에 오류가 발생하지 않는다.


### 함수표현식에서의 호이스팅 
- 함수표현식은 함수선언문과 달리 선언과 호출 순서에 따라서 정상적으로 함수가 실행되지 않을 수 있다.
    - 함수표현식에서는 선언과 할당의 분리가 발생한다.

1. 함수표현식의 선언이 호출보다 위에 있는 경우 - 정상 출력 
    ```js
    /* 정상 */
    function printName(firstname) { // 함수선언문
        var inner = function() { // 함수표현식 
            return "inner value";
        }
        
        var result = inner(); // 함수 "호출"
        console.log("name is " + result);
    }

    printName(); // > "name is inner value"
    ```
    ```js
    /* 정상 */
    /** --- JS Parser 내부의 호이스팅(Hoisting)의 결과 - 위와 동일 --- */
    function printName(firstname) { 
        var inner; // [Hoisting] 함수표현식의 변수값 "선언"
        var result; // [Hoisting] var 변수값 "선언"

        inner = function() { // 함수표현식 "할당"
            return "inner value";
        }
        
        result = inner(); // 함수 "호출"
        console.log("name is " + result);
    }

    printName(); // > "name is inner value"
    ```
2. 함수표현식의 선언이 호출보다 아래에 있는 경우 (*var 변수*에 할당) - ***TypeError***
    ```js
    /* 오류 */
    function printName(firstname) { // 함수선언문
        console.log(inner); // > "undefined": 선언은 되어 있지만 값이 할당되어있지 않은 경우
        var result = inner(); // ERROR!!
        console.log("name is " + result);

        var inner = function() { // 함수표현식 
            return "inner value";
        }
    }
    printName(); // > TypeError: inner is not a function
    ```
    ```js
    /** --- JS Parser 내부의 호이스팅(Hoisting)의 결과 --- */
    /* 오류 */
    function printName(firstname) { 
        var inner; // [Hoisting] 함수표현식의 변수값 "선언"

        console.log(inner); // > "undefined"
        var result = inner(); // ERROR!!
        console.log("name is " + result);

        inner = function() { 
            return "inner value";
        }
    }
    printName(); // > TypeError: inner is not a function
    ```
      - **Q.** printName에서 "inner is not defined" 이라고 오류가 나오지 않고, "inner is not a function"이라는 TypeError가 나오는 이유?
      - **A.** printName이 실행되는 순간 (Hoisting에 의해) inner는 'undefined'으로 지정되기 때문 
      - inner가 undefined라는 것은 즉, 아직은 함수로 인식이 되지 않고 있다는 것을 의미한다.
3. 함수표현식의 선언이 호출보다 아래에 있는 경우 (*const/let 변수*에 할당) - ***ReferenceError***
    ```js
    /* 오류 */
    function printName(firstname) { // 함수선언문
        console.log(inner); // ERROR!!
        let result = inner();  
        console.log("name is " + result);

        let inner = function() { // 함수표현식 
            return "inner value";
        }
    }
    printName(); // > ReferenceError: inner is not defined
    ```
      - let/const의 경우, 호이스팅이 일어나지 않기 때문에 위의 예시 그대로 이해하면 된다.
      - `console.log(inner);`에서 inner에 대한 선언이 되어있지 않기 때문에 이때는 "inner is not defined" 오류가 발생한다.

- 함수표현식보다 함수선언문을 더 자주 사용하지만, 어떤 코딩컨벤션에서는 함수표현식을 권장하기도 한다.
    - 즉, 어떤 컨벤션을 갖던지 한가지만 정해서 사용하는 게 좋다.


## 호이스팅 우선순위
### 같은 이름의 var 변수 선언과 함수 선언에서의 호이스팅 
- 변수 선언이 함수 선언보다 위로 끌어올려진다.
    ```js
    var myName = "hi";

    function myName() {
        console.log("yuddomack");
    }
    function yourName() {
        console.log("everyone");
    }

    var yourName = "bye";

    console.log(typeof myName);
    console.log(typeof yourName);
    ```
    ```js
    /** --- JS Parser 내부의 호이스팅(Hoisting)의 결과 --- */
    // 1. [Hoisting] 변수값 선언 
    var myName; 
    var yourName; 

    // 2. [Hoisting] 함수선언문
    function myName() {
        console.log("yuddomack");
    }
    function yourName() {
        console.log("everyone");
    }

    // 3. 변수값 할당
    myName = "hi";
    yourName = "bye";

    console.log(typeof myName); // > "string"
    console.log(typeof yourName); // > "string"
    ```
- 값이 할당되어 있지 않은 변수와 값이 할당되어 있는 변수에서의 호이스팅 
    ```js
    var myName = "Heee"; // 값 할당 
    var yourName; // 값 할당 X

    function myName() { // 같은 이름의 함수 선언
        console.log("myName Function");
    }
    function yourName() { // 같은 이름의 함수 선언
        console.log("yourName Function");
    }

    console.log(typeof myName); // > "string"
    console.log(typeof yourName); // > "function"
    ```
    - 값이 할당되어 있지 않은 변수의 경우, 함수선언문이 변수를 덮어쓴다.
    - 값이 할당되어 있는 변수의 경우, 변수가 함수선언문을 덮어쓴다.


<mark>TIP</mark> 호이스팅 사용 시 주의 
- 코드의 가독성과 유지보수를 위해 호이스팅이 일어나지 않도록 한다.
    - 호이스팅을 제대로 모르더라도 함수와 변수를 가급적 코드 상단부에서 선언하면, 호이스팅으로 인한 스코프 꼬임 현상은 방지할 수 있다.
    - let/const를 사용한다.
- var를 쓰면 혼란스럽고 쓸모없는 코드가 생길 수 있다. 그럼 왜 var와 호이스팅을 이해해야 할까?
    - ES6를 어디에서든 쓸 수 있으려면 아직 시간이 더 필요하므로 ES5로 트랜스컴파일을 해야한다. 
    - 따라서 아직은 var가 어떻게 동작하는지 이해하고 있어야 한다.

# 관련된 Post
- JavaScript의 변수, 연산자, 타입에 대해 알고 싶으시면 [자바스크립트의 변수, 연산자, 타입의 종류](https://gmlwjd9405.github.io/2019/04/18/javascript-variable-and-type.html)를 참고하시기 바랍니다
- JavaScript의 비교, 반복, 문자열에 대해 알고 싶으시면 [자바스크립트의 비교, 반복, 문자열 이해하기](https://gmlwjd9405.github.io/2019/04/19/javascript-if-switch-for.html)를 참고하시기 바랍니다.
- JavaScript의 함수선언문과 함수표현식의 차이에 대해 알고 싶으시면 [함수표현식 vs 함수선언문](https://gmlwjd9405.github.io/2019/04/20/function-declaration-vs-function-expression.html)를 참고하시기 바랍니다



# Reference
> - [https://www.edwith.org/boostcourse-web/lecture/16695/](https://www.edwith.org/boostcourse-web/lecture/16695/)
> - [https://yuddomack.tistory.com/entry/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%ED%98%B8%EC%9D%B4%EC%8A%A4%ED%8C%85Hoisting#recentComments](https://yuddomack.tistory.com/entry/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%ED%98%B8%EC%9D%B4%EC%8A%A4%ED%8C%85Hoisting#recentComments)
