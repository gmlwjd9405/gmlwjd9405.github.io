---
layout: post
title: '[JavaScript] 자바스크립트의 비교문/분기문/반복문, 문자열'
subtitle: '자바스크립트의 비교문/분기문/반복문, 문자열에 대해 이해한다.'
date: 2019-04-19 
author: heejeong Kwon
cover: '/images/javascript/javascript-main.png'
tags: javascript 
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

[Edwith 강의](https://www.edwith.org/boostcourse-web/lecture/16694/) 참고

## Goal
> - 자바스크립트의 비교문/분기문/반복문에 대해 이해한다
> - 자바스크립트의 문자열 처리에 대해 이해한다.


### [비교문](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Control_flow_and_error_handling#%EC%A1%B0%EA%B1%B4%EB%AC%B8)
- if, else if, else 문을 통해서 다양한 비교문을 사용할 수 있다. 

```js
if(true) console.log(true)
else console.log(false)

// 삼항 연산자
var a = true;
var result = (a) ? "ok" : "not ok";
console.log(result); // ok 

a = "";
var result = (a) ? "ok" : "not ok";
console.log(result); // not ok
```

### [분기문](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Control_flow_and_error_handling#switch%EB%AC%B8)
- 로직을 분기하기 위해서 if 문 이외에도 switch 문을 통해서도 해결할 수 있다. 

### [반복문](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Loops_and_iteration#for_%EB%AC%B8)
- for 문이나 while문을 사용해서 반복문을 구현할 수 있다. 

```js
function howMany(selectObject) {
  var numberSelected = 0;
  for (var i = 0; i < selectObject.options.length; i++) { // 비효율적 (배열의 길이를 계속 계산)
    if (selectObject.options[i].selected) {
      numberSelected++;
    }
  }
  return numberSelected;
}
```

- 배열의 경우 `forEach`와 같은 메서드도 있고, `for-of`를 통한 탐색도 자주 사용된다. 
  - `for-in`은 객체를 탐색할 때 사용한다. 
 
- for 문의 성능 개선
  1. 배열의 길이 한 번만 계산 
      - `for (var i = 0; len = selectObject.options.length; i < len; i++)`
  2. Reverse Iteration
      - `for (var len = selectObject.options.length; i = len; i > len; i--)`
      - 시작 포인트를 배열의 길이로 하고, 포인트 값을 감소하면서 반복문을 도는 형태 

- <mark>TIP</mark> 반대로 반복문을 동작시키는(reverse iteration) 경우가 실제 브라우저에서 얼마나 성능차이가 있을까? 
  - 현대의 자바스크립트 엔진은 최적화를 통해 반복문을 최대한 빠르게 처리하는 과정을 거쳐왔기 때문에 실제로 실험을 해보면 그 차이가 미미하다.
  - 따라서 for문을 무조건 반대로 구현할 필요는 없다. 
  - 이런 상황이외에도 자바스크립트의 구현 방법에 따라 (for 가 빠를까 while 빠를까? 등) 성능차이는 그리 크지 않다. 
  - 따라서 일반적으로는 **코드의 가독성**에 좀 더 우선 집중하는 게 좋다. 

### 문자열 처리
- 자바스크립트의 문자와 문자열은 같은 타입으로 모두 문자열이다. 
  ```js
  typeof "abc";  // string
  typeof "a";    // string
  typeof 'a';    // string. single quote도 사용가능.
  ``` 

- 문자열에 다양한 메서드가 있다.
 ```js
 "ab:cd".split(":"); // ["ab","cd"]
 "ab:cd".replace(":", "$"); // "ab$cd"
 " abcde  ".trim();  // "abcde"
 ```
  - 문자열은 내부적으로 객체로 변환되기 때문에 어떤 객체 내에 있는 메서드를 사용할 수 있는 것이다. 

- 정규표현식으로도 문자열을 처리할 수 있다.

# 관련된 Post
- JavaScript의 변수, 연산자, 타입에 대해 알고 싶으시면 [자바스크립트의 변수, 연산자, 타입의 종류](https://gmlwjd9405.github.io/2019/04/18/javascript-variable-and-type.html)를 참고하시기 바랍니다
- JavaScript의 함수선언문과 함수표현식의 차이에 대해 알고 싶으시면 [함수표현식 vs 함수선언문](https://gmlwjd9405.github.io/2019/04/20/function-declaration-vs-function-expression.html)를 참고하시기 바랍니다.
- JavaScript의 호이스팅에 대해 알고 싶으시면 [자바스크립트의 호이스팅이란](https://gmlwjd9405.github.io/2019/04/22/javascript-hoisting.html)을 참고하시기 바랍니다.

# Reference
> - [https://www.edwith.org/boostcourse-web/lecture/16693/](https://www.edwith.org/boostcourse-web/lecture/16694/)
> - [https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Control_flow_and_error_handling#조건문](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Control_flow_and_error_handling#%EC%A1%B0%EA%B1%B4%EB%AC%B8)
> - [https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Control_flow_and_error_handling#switch문](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Control_flow_and_error_handling#switch%EB%AC%B8)
> - [https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Loops_and_iteration#for_문](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Loops_and_iteration#for_%EB%AC%B8)
