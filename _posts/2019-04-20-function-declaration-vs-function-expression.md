---
layout: post
title: '[JavaScript] 함수선언문과 함수표현식의 차이'
subtitle: '자바스크립트의 함수선언문과 함수표현식의 차이에 대해 이해한다.'
date: 2019-04-20
author: heejeong Kwon
cover: '/images/javascript/javascript-main.png'
tags: javascript function
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

[Edwith 강의](https://www.edwith.org/boostcourse-web/lecture/16695/) 참고

## Goal
> - 자바스크립트의 함수의 개념과 특징에 대해 이해한다.
> - 함수선언문과 함수표현식의 차이를 이해한다.

## 함수
- 함수는 여러 개의 인자를 받아서, 그 결과를 출력한다. 
- 파라미터의 개수와 인자의 개수가 일치하지 않아도 오류가 발생하지 않는다.
    - 만약, 파라미터 1개가 정의된 함수를 부를 때, 인자의 개수를 0개만 넣어 실행하면 이미 정의된 파라미터(매개변수)는 undefined이라는 값을 갖게 된다. 
    - 이는 변수는 초기화됐지만, 값이 할당되지 않았기 때문이다. 
- 자바스크립트에서는 **함수도 객체**이다.
    - 따라서 다른 객체와 마찬가지로 넘기거나 할당할 수 있다.
    - 함수를 *객체 프로퍼티*에 할당할 수도 있다.
        ```js
        const o = {}
        o.f = testFunc
        o.f() // test!!
        ```
    - 함수를 *객체 배열 요소*로 할당할 수도 있다.
        ```js
        const arr = [1, 2, 3]
        arr[1] = testFunc
        arr[1]() // test!!
        ```

### 함수 호출 vs 함수 참조
- **함수 호출**
    - 함수 식별자 뒤에 괄호를 쓰면 함수 본문(Body)을 실행한다
    - 함수를 호출한 표현식은 반환값이 된다.
    - `testFunc()` // test!! (함수 호출)
    ```js
    // 함수의 호출.
    function printName(firstname) {
          var myname = "HEEE";
          return myname + " " +  firstname;
    }
    ``` 
- **함수 참조** 
    - 함수 식별자 뒤에 괄호를 쓰지 않으면 함수는 실행되지 않는다.
    - `testFunc` // function testFunc() // (함수 참조)
    - `const f = testFunc`와 같이 함수를 변수에 할당하면 `f()`과 같이 다른 이름으로 함수를 호출할 수 있다.

### 원시값 매개변수 vs 객체 매개변수
- 함수를 호출하면 함수 매개변수는 변수 자체가 아니라 그 값을 전달받는다.
    - 따라서 넘겨받은 **원시값** 매개변수를 함수 내에서 변경하더라도 밖에서는 변경되지 않는다.
    - 하지만 넘겨받은 매개변수가 **객체**이고, 이 객체 자체를 변경하면 그 객체는 함수 밖에서도 바뀐 점이 반영된다. 
- 원시값과 객체의 차이?
    - 원시값은 불변이므로 수정할 수 없다.
    - 원시값을 담은 변수는 수정할 수 있지만(다른 값으로 바꿀 수 있지만) 원시값 자체는 바뀌지 않는다.
    - 반면 객체는 바뀔 수 있다.

### 반환값과 undefined
- 함수는 어떤 타입의 값이라도 반환할 수 있다.
- 자바스크립트 함수는 반드시 return값이 존재하며, 없을 때는 기본 반환값인 **'undefined'**가 반환된다.
    - 자바스크립트에서는 void 타입이 없다. 
- Ex. 반환값: undefined
  ```js
  function printName(firstname) {
      var myname = "HEEE";
      var result = myname + " " +  firstname;
  }
  ```

### arguments 객체
- 함수 호출 시에 넘겨진 실제 인자값을 가지는 객체 
- 함수가 실행되면 그 안에는 arguments라는 **특별한 지역변수**가 자동으로 생성된다.
    - arguments의 타입은 객체이다.
- 자바스크립트 함수는 선언한 파라미터보다 더 많은 인자를 보낼 수도 있다.
    - 이때 넘어온 인자를 arguments로 하나씩 접근할 수 있다.
    - arguments는 배열의 형태(array-like)를 가지고 있다.
- arguments는 배열 타입(array)은 아니다.
    - 따라서 배열의 메서드를 사용할 수 없다.
    ```js
    function a() {
          console.log(arguments);
    }
    a(1, 2, 3); // { '0':1, '1':2, '2':3 }
    ```

- 자바스크립트의 가변인자를 받아서 처리하는 함수를 만드는 상황에서 arguments 속성을 유용하게 사용할 수 있다.(메서드에 넘겨 받을 인자의 개수를 모를 때)
    ```js
    function a() {
        if(arguments.length < 3) { // 인자의 개수가 중요한 경우 
            console.log("errer");
            return;
        }
        otherMethod(arguments[1]); // 다른 메서드에 가변인자를 넘겨주는 경우 
    }
    ```

- arguments 사용 시 주의
    - arguments도 남용하면 변경에 약한 코드가 된다.
    - arguments를 함부로 수정해서도 안된다. 수정이 된다 하더라도 수정을 해서 해당 값을 바꾸려고 하는 것을 좋지 않다.
 

## 함수선언문과 함수표현식의 차이 
### 함수선언문 (Function Declaration)
- 일반적인 프로그래밍 언어에서의 함수 선언과 비슷한 형식 
```js
function 함수명() {
    구현 로직
}
```
```js
// 함수의 호출.
function printName(firstname) {
    var myname = "HEEE";
    return myname + " " +  firstname;
}
``` 

### 함수표현식 (Function Expression)
- 변수값에 함수 표현을 담아 놓은 형태 
    - 유연한 자바스크립트 언어의 특징을 활용한 선언 방식 
- 함수표현식은 익명 함수표현식과 기명 함수표현식으로 나눌 수 있다. 
    - 일반적으로 함수표현식이라고 부르면 앞에 익명이 생략된 형태라고 볼 수 있다.
    - 익명 함수표현식: 함수에 식별자가 주어지지 않는다.
    - 기명 함수표현식: 함수의 식별자가 존재한다.
- 함수표현식의 장점
    - 클로져로 사용
    - 콜백으로 사용(다른 함수의 인자로 넘길 수 있음)

```js
var test1 = function() { // (익명) 함수표현식
  return '익명 함수표현식';
}

var test2 = function test2() { // 기명 함수표현식 
  return '기명 함수표현식';
}
```

### 함수선언문과 함수표현식의 차이
- 함수선언문은 호이스팅에 영향을 받지만, 함수표현식은 호이스팅에 영향을 받지 않는다.
    - 함수선언문은 코드를 구현한 위치와 관계없이 자바스크립트의 특징인 호이스팅에 따라 브라우저가 자바스크립트를 해석할 때 맨 위로 끌어 올려진다.
    - 함수표현식은 함수선언문과 달리 선언과 호출 순서에 따라서 정상적으로 함수가 실행되지 않을 수 있다.

- 함수표현식 Error 예시 
  ```js
  /* 정상 */
  function printName(firstname) { // 함수선언문
      var inner = function() { // 함수표현식
          return "inner value";
      }

      var result = inner();
      console.log("name is " + result);
  }

  printName(); // "name is inner value"
  ```
  ```js
  /* 오류 */
  function printName(firstname) { // 함수선언문
      console.log(inner); // "undefined": 선언은 되어 있지만 값이 할당되어있지 않은 경우
      var result = inner(); // ERROR!!
      console.log("name is " + result);

      var inner = function() { // 함수표현식 
          return "inner value";
      }
  }
  printName(); // TypeError: inner is not a function

  /** --- JS Parser 내부의 호이스팅(Hoisting)의 결과 --- */
  /* 오류 */
  function printName(firstname) { // 함수선언문
      var inner; // Hoisting - 변수값을 끌어올린다. (선언은 되어 있지만 값이 할당되어있지 않은 경우)
      console.log(inner); // "undefined"
      var result = inner(); // ERROR!!
      console.log("name is " + result);

      inner = function() { // 함수표현식 
          return "inner value";
      }
  }
  printName(); // TypeError: inner is not a function
  ```
    - **Q.** "printName이 is not defined" 이라고 오류가 나오지 않고, function이 아니라는 TypeError가 나오는 이유?
    - **A.** printName이 실행되는 순간 ([Hoisting]()에 의해) inner는 'undefined'으로 지정되기 때문 
    - inner가 undefined라는 것은 즉, 아직은 함수로 인식이 되지 않고 있다는 것을 의미한다.

- 함수표현식보다 함수선언문을 더 자주 사용하지만, 어떤 코딩컨벤션에서는 함수표현식을 권장하기도 한다.
    - 즉, 어떤 컨벤션을 갖던지 한가지만 정해서 사용하는 게 좋다.


# 관련된 Post
- JavaScript의 변수, 연산자, 타입에 대해 알고 싶으시면 [자바스크립트의 변수, 연산자, 타입의 종류](https://gmlwjd9405.github.io/2019/04/18/javascript-variable-and-type.html)를 참고하시기 바랍니다
- JavaScript의 비교, 반복, 문자열에 대해 알고 싶으시면 [자바스크립트의 비교, 반복, 문자열 이해하기](https://gmlwjd9405.github.io/2019/04/19/javascript-if-switch-for.html)를 참고하시기 바랍니다.
- JavaScript의 호이스팅에 대해 알고 싶으시면 [자바스크립트의 호이스팅이란](https://gmlwjd9405.github.io/2019/04/22/javascript-hoisting.html)을 참고하시기 바랍니다.


# Reference
> - [https://www.edwith.org/boostcourse-web/lecture/16695/](https://www.edwith.org/boostcourse-web/lecture/16695/)
> - [https://joshua1988.github.io/web-development/javascript/function-expressions-vs-declarations/](https://joshua1988.github.io/web-development/javascript/function-expressions-vs-declarations/)
> - [장기효 (캡틴판교) - Vue.js 중급 강좌, 웹앱 제작으로 배워보는 Vue.js, ES6, Vuex](https://www.inflearn.com/course/vue-pwa-vue-js-%EC%A4%91%EA%B8%89/)
