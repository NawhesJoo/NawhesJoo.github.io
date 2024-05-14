---
title: JavaScript - 스코프 체인(scope chain)
description: >-
  
author: NawhesJoo
date: 2024-05-08 20:55:00 +0900
categories: [Language, JavaScript]
tags: [scope, scope chain, execution context]
pin: true
math: true
mermaid: true
image:
  path: /thumbnails/JavaScript.png
  lqip: 
  alt: 
comments: true
---
## 스코프 체인(scope chain)이란?

스코프 체인(scope chain)은 일종의 리스트로서 전역 객체와 중첩된 함수의 스코프의 레퍼런스를 차례로 저장하고, 의미 그대로 각각의 `스코프가 어떻게 연결(chain)되고 있는지 보여주는 것`을 말한다.

하지만 스코프 체인(scope chain)을 이해하기 위해서 먼저 자바스크립트의 `실행 컨텍스트(Execution context)`를 알아야 한다.

---

## 실행 컨텍스트란 무엇인가?

실행 컨텍스트(Execution context)는 `우리가 작성한 코드가 실행되는 환경`을 말하며, scope, hoising, this, function, closure 등의 동작원리를 담고 있는 자바스크립트의 핵심원리를 말한다.

그리고 이 실행 컨텍스트에는 두 개의 실행 컨텍스트가 존재한다.

---
### 글로벌 실행 컨텍스트

코드가 실행되기 전에 생성이 되며, 함수 내에 없는 코드는 모두 `전역 실행 컨텍스트 안에 존재`한다.

그렇기 때문에, 자바스크립트 엔진은 일부 자바스크립트 코드를 실행할 때마다 글로벌 실행 컨텍스트(Global Execution Context)를 작성한다.

글로벌 실행 컨텍스트의 특징으로는 무조건 `하나의 전역 실행 컨텍스트만이 존재`하며, 어플리케이션이 종료될 때(웹 페이지에서 나가거나 브라우저를 닫을 때)까지 유지하는 것이다.

---


### 함수 실행 컨텍스트

전역 실행 컨텍스트가 생성된 후, 함수가 실행(ex 호출) 될 때마다 `새로운 실행 컨텍스트가 작성`된다.

---

## 실행 컨텍스트에서 스코프 체인(scope chain)은 어떻게 작동하는가?

실행 컨텍스트는 `LIFO(Last in, First out) 구조의 스택`으로, 코드 실행 중에 생성된 `모든 실행 컨텍스트를 저장`하는 데 사용된다.

실행 컨텍스트가 실행되면, 엔진이 `스코프 체인`을 통해 `렉시컬 스코프를 먼저 파악`한다.

그러고 나서, 함수가 중첩 상태일 때 하위 함수 내에서 상위 함수의 스코프와 전역 스코프까지 참조할 수 있는데 이것을 `스코프 체인을 통해 탐색`하는 것이다.

```javascript
var v = '전역 변수';

function a() {
  // function a Execution Context(EC)
  var v = '지역 변수';
  
  function b(){
    // function b Execution Context
    console.log(v);
  }
  
  b();
}

// Global Execution Context(GEC)
a();
```

위 코드의 예제를 보면 먼저 글로벌 실행 컨텍스트(GEC)가 실행되고 스택에 쌓인다.

그런 다음 함수 호출 순으로 실행 컨텍스트 스택에 쌓이게 되고, 가장 나중에 호출된 b() 함수가 실행 컨텍스트 안에서부터 탐색을 시작한다.

![](https://velog.velcdn.com/images/nawhes_joo/post/51ecc995-90d2-44ed-ae7d-206f35f44aab/image.png)


출처 : https://medium.com/@pvivek4/scope-and-execution-context-in-javascript-3b71e76cd193

그러면, b() 함수 안에서 변수 v를 탐색하기 시작하는데, 만약 변수 v가 없으면 b()함수를 감싸고 있는 외부 함수 a() 함수를 탐색하기 시작한다.




이때 a()함수 안에 변수 v가 존재하면 안에 있는 v를 참조하게 되고, 만약 없다면 마지막으로 전역 객체를 탐색하여 v를 찾아낸다.


결국 찾지 못한다면, v가 없다고 Uncaught ReferenceError : v is not defined 라는 에러가 발생한다.

![](https://velog.velcdn.com/images/nawhes_joo/post/149756de-f3cc-4a8c-ac24-d877dda1e832/image.png)


반대로 찾았다면, 결과값은 a() 안에 변수 v가 존재하기 때문에 `지역 변수`라는 값이 출력된다.

![](https://velog.velcdn.com/images/nawhes_joo/post/f2244875-153f-4946-95ea-97d46d83d000/image.png)

하지만 만약, a() 함수 안에 v를 제거한다면 전역 객체에 있는 변수 v의 값 `전역 변수`가 출력된다.


![](https://velog.velcdn.com/images/nawhes_joo/post/38cd6136-9713-44c2-8990-d97d9b859fc1/image.png)

이러한 과정들이 스코프에 담긴 순서대로 탐색하는 `스코프 체인`이라고 보면 된다.

---

## 스코프 체인을 개발자 도구로 확인하기

스코프 체인(scope chain)은 함수의 감춰진 프로퍼티인 [[Scope]]로 참조할 수 있다.

`console.dir()`을 사용하면, 개발자 도구로 쉽게 확인이 가능하다.

![](https://velog.velcdn.com/images/nawhes_joo/post/9ae161fc-4741-4f15-a5d5-044515861914/image.png)


b() 함수에 `[[Scopes]] 속성이 존재`한다. 이것이 바로 스코프 체인(scope chain)이다.

자기 자신의 스코프(scope)를 제외한 자신과 가장 가까운 변수 객체의 스코프 순으로 접근하는 것을 눈으로 확인할 수 있다.

>정리하자면, 자기 자신의 스코프(scope)를 제외한 자신과 가장 가까운 변수 객체의 모든 스코프들을 스코프 체인이라 할 수 있다.

출처 : https://ljtaek2.tistory.com/140