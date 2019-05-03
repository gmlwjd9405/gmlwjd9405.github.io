---
layout: post
title: '[JavaScript] 자바스크립트의 변수, 연산자, 타입의 종류'
subtitle: '자바스크립트의 변수, 연산자 및 타입의 종류와 개념을 이해한다.'
date: 2019-04-18 
author: heejeong Kwon
cover: '/images/javascript/javascript-main.png'
tags: javascript 
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

[Edwith 강의](https://www.edwith.org/boostcourse-web/lecture/16693/) 참고

## Goal
> - 자바스크립트의 변수의 종류를 확인한다. 
>   - var과 let, const의 차이에 대해 이해한다. 
> - 자바스크립트의 연산자의 종류 및 개념을 이해한다. 
> - 자바스크립트의 타입의 종류를 확인한다. 
>   - 자바스크립트에서 type 체크 방법에 대해 이해한다. 
>   - 자바스크립트에서 null 체크 방법에 대해 이해한다. 

### 자바스크립트의 버전
- 자바스크립트 버전은 ECMAScript(줄여서 ES, 이크마스크립트)의 버전에 따라서 결정되고, 이를 자바스크립트 실행 엔진이 반영한다.
  - 자바스크립트 엔진은 작성한 JS 코드를 한 줄씩 해석하면서 실행을 준비한다.
- ES5, ES6(ES2015).. 이런 식으로 버전을 일컫는다.
- 2018년을 중심으로 ES6를 지원하는 브라우저가 많아서 몇 년간 ES6 문법이 표준으로 쓰이고 있다. 
- ES6는 ES5문법을 포함하고 있어 하위호환성 문제가 없다. 
  - 다만 feature별로 지원하지 않는 브라우저가 있을 수 있어 주의해야 한다.
- [Bable](https://babeljs.io/)
    - 구 버전 브라우저 중에서는 ES6의 기능을 지원하지 않는 브라우저가 있으므로 **transpiling**이 필요
    - ES6의 문법을 각 브라우저의 호환 가능한 ES5로 변환하는 컴파일러
    - Babel 온라인 에디터: [https://babeljs.io/repl/](https://babeljs.io/repl/)

## 변수
- 변수는 var, let, const로 선언할 수 있다.
- 어떤 것을 사용하는가에 의해서 scope, 즉 변수의 유효범위가 달라진다.
- ES6 이전 버전에서는 var를 사용해서 변수를 선언할 수 있다. 
  - var로 선언한 변수는 끌어올린다는 뜻의 **호이스팅(hoisting)** 이라는 매커니즘을 따른다.

```js
var a = 2;
var a = "aaa";
var a = 'aaa';
var a = true;
var a = [];
var a = {};
var a = undefined;
```

### 변수의 Scope
- `{}`에 상관없이 스코프가 설정된다.

```javascript
var sum = 0;
for(var i = 1; i <= 5; i++) {
    sum = sum + i;
}
console.log(sum); // 15
console.log(i); // 6
```

### ES6 새로운 변수 선언 방식 - const, let
- 블록 단위 `{}`로 변수의 범위가 제한되었다.
  ```js
  let sum = 0;
  for (let i=1; i<=5; i++) {
      sum = sum + i;
  }
  console.log(sum); // 10
  console.log(i); // Uncaught ReferenceError: i is not defined
  ```
- `const`: 한번 선언한 값에 대해서 변경할 수 없다.(상수 개념)
  ```js
  /* 예시 */
  const a = 10;
  a = 20; // Uncaught TypeError: Assignment to constant variable

  /* [주의!] 하지만, 객체나 배열의 내부는 변경할 수 있다. */
  const a = {};
  a.num = 10;
  console.log(a); // {num: 10}

  const b = [];
  b.push(20);
  console.log(b); // [20]
  ```
- `let`: 한번 선언한 값에 대해서 다시 선언할 수 없다. 변경은 가능 
  - 메모리에 할당하면 다시 할당하지 못함
- 간단한 scope 예시
  ```js
  function f() {
      {
        let x;
        {
            // 새로운 블록 안에 새로운 x의 스코프가 생김
            const x = "sneaky";
            x = "foo"; // 위에 이미 const로 x를 선언했으므로 다시 값을 대입하면 Error
        }
        // 이전 블록 범위로 돌아왔기 때문에 'let x'에 해당하는 메모리에 값을 대입
        x = "bar";
        let x = "inner"; // SyntaxError: Identifier 'x' has already been declared
      }
  }
  ```

## 연산자
- 연산자 우선순위를 표현하기 위해서는 ()를 사용하면 된다. 
- 수학 연산자, 논리 연산자, 관계 연산자, 삼항 연산자 등이 있다. 

### 수학 연산자 
- +, -, *, /, %(나머지) 등이 있다. 

### 논리 연산자
- `&&` 연산자
  - 모든 값이 true인지 확인하지만, 첫 번째가 이미 false라면 그 이후의 값은 확인하지 않는다.
  - 모든 값이 true인 경우 마지막 값이 할당된다.
- `||` 연산자
  - 첫 번째가 true인 경우 그 이후의 값은 확인하지 않고 첫 번째 값이 할당된다. 

```js
// or 연산자 활용
const name = "myname";
const result = name || "default";
console.log(result); // myname 
const result2 = name && "test";
console.log(result2); // test (&&의 경우, 뒤의 값이 할당된다.)

var name = "";
var result = name || "default";
console.log(result); // default 
``` 

### 삼항 연산자
- 간단한 비교와 값 할당은 삼항연산자를 사용할 수 있다.

```js
const data = 11;
const result = (data > 10) ? "ok" : "fail";
console.log(result); // ok 
```

### 비교 연산자
- 비교는 == 보다는 **===** 를 사용한다.
  - === 의 경우는 Type까지 확인하는 연산자이다. 
  - == 의 경우 임의적으로 Type을 바꿔서 비교하기 때문에 원하는 결과와 다른 값이 나올 수 있다. 
- == 로 인한 다양한 오류 상황 예시 
  ```js
  0 == false; // true
  "" == false; // true 
  null == false; // false (null은 객체)
  undefined == false; // false 
  0 == "0"; // true
  null == undefined; // true 
  ```

<mark>TIP</mark> 자바스크립트의 null 체크에서의 ! 사용 시 주의 
- 0, null, undefined, "", {}, [] 
  ```js
  var target;

  target = 0;
  console.log(!target); // true
  target = null;
  console.log(!target); // true
  target = undefined;
  console.log(!target); // true
  target = "";
  console.log(!target); // true
  target = {};
  console.log(!target); // false
  target = [];
  console.log(!target); // false 
  ```

## 자바스크립트의 타입 
- undefined, null, boolean, number, string, object, function, array, Date, RegExp 등

### 기본형과 참조형의 종류
- Primitive
  - Number
  - String
  - Boolean
  - null
  - undefined
- Reference: 기본형 데이터의 집합이라고 할 수 있다. 
  - Object
    - Array
    - Function
    - RegExp

### 기본형과 참조형의 차이점
- 기본형: 값을 그대로 할당
- 참조형: 값이 저장된 주소값을 할당(참조)

### 자바스크립트 Type 체크 
- 컴파일 단계가 없는 JavaScript의 Type은 선언할 때가 아니고, **실행 타임** (Dynamic Type)에 결정된다.
  - 함수 안에서의 파라미터나 변수는 실행될 때 그 타입이 결정된다. 
- 타입을 체크하는 또렷한 방법은 없다. 
  - 정확하게는 `toString.call()` 함수를 이용해서 그 결과를 매칭하곤 하는데, 문자, 숫자와 같은 자바스크립트 기본 타입은 `typeof` 키워드를 사용해서 체크할 수 있습니다. 
  - 배열은 타입을 체크하는 isArray함수가 표준으로 생겼다. (IE와 같은 구 브라우저를 사용해야 한다면 지원범위를 살펴보고 사용) 
- 예시 
  ```js
  toString.call("test"); // "[object String]"
  
  const target = "test";
  typeof target; // "string"
  ``` 

# 관련된 Post
- JavaScript의 비교, 반복, 문자열에 대해 알고 싶으시면 [자바스크립트의 비교, 반복, 문자열 이해하기](https://gmlwjd9405.github.io/2019/04/19/javascript-if-switch-for.html)를 참고하시기 바랍니다.
- JavaScript의 함수선언문과 함수표현식의 차이에 대해 알고 싶으시면 [함수표현식 vs 함수선언문](https://gmlwjd9405.github.io/2019/04/20/function-declaration-vs-function-expression.html)를 참고하시기 바랍니다.
- JavaScript의 호이스팅에 대해 알고 싶으시면 [자바스크립트의 호이스팅이란](https://gmlwjd9405.github.io/2019/04/22/javascript-hoisting.html)을 참고하시기 바랍니다.
# Reference
> - [https://www.edwith.org/boostcourse-web/lecture/16693/](https://www.edwith.org/boostcourse-web/lecture/16693/)
