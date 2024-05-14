---
title: JavaScript - 렉시컬 스코프(lexical scope)와 클로저(closure)
description: >-
  
author: NawhesJoo
date: 2024-05-07 20:55:00 +0900
categories: [Language, JavaScript]
tags: [scope, lexical scope, static scope, closure]
pin: true
math: true
mermaid: true
image:
  path: /thumbnails/JavaScript.png
  lqip: 
  alt: 
comments: true
---


> `클로저`는 함수와 함수가 선언된 어휘적 환경의 조합이다. 클로저를 이해하려면 자바스크립트가 어떻게 변수의 유효범위를 지정하는지 `렉시컬 스코프(Lexical Scope)를 먼저 이해`해야 한다.

## 정적 스코프(static scope), 렉시컬 스코프(Lexical Scope)란?

`렉시컬 스코프`는 한 마디로 `함수를 어디에 선언하였는지에 따라 상위 스코프가 결정`되는 것을 말한다.

`자바스크립트를 포함한 대부분의 프로그래밍 언어는 렉시컬 스코프를 따르며`, 이를 정적 스코프(Static Scope)라고 부르기도 한다.

```javascript
function init(){
  var name = "이름"
  function displayName(){
    console.log(name);
  }
  displayName();
}
init();
```

![](https://velog.velcdn.com/images/nawhes_joo/post/f0792525-cf29-43d3-90da-c9024e044df6/image.png)


MDN에 나와있는 예제이다.

displayName()은 init() 안에 정의된 내부 함수이며 init() 함수 본문에서만 사용할 수 있다.

다만, 여기서 주의할 점은 displayName() 내부엔 자신만의 지역 변수가 없다는 점이다. 그런데 함수 내부에서 외부 함수의 변수에 접근할 수 있기 때문에 displayName() 역시 부모 함수 init()에서 선언된 변수 name에 접근할 수 있다.

상위스코프가 무엇인지 알려면 displayName 함수가 어디에 선언되었는지를 봐야하는데, 위 코드에서는 displayName 함수가 init 함수 안에서 선언되었으므로 `상위 스코프는 지역 스코프`가 된다.

그래서 init 함수 내의 선언된 name이라는 변수를 참조할 수 있는 것이다.

![](https://velog.velcdn.com/images/nawhes_joo/post/456eeb1b-00be-426d-8749-d8e823a3be32/image.png)


init()에 선언된 name이라는 변수가 존재하지 않고 상위스코프(전역)에 선언되었다면 전역에 선언된 name을 참조할 수 있다.

---

## 스코프(Scope)란?

`스코프`는 `변수나 함수가 유효할 수 있는 범위`를 뜻하는데,
그렇기 때문에 자바스크립트 엔진이 `식별자를 검색할 때 사용하는 규칙`으로 작용한다.

---

## 클로저(Closure)

사전적인 의미로는 `폐쇄`라는 뜻을 갖고 있다.
자바스크립트의 클로저도 이 폐쇄와 유사한 의미를 가지고 있다.

한 마디로 `클로저`란 함수가 선언될(생성될) 때 그 당시에 `주변의 환경과 함께 갇히는 것`을 말한다.

또 다른 말로, 함수가 속한 `렉시컬 스코프를 기억`하며, `함수가 렉시컬 스코프 밖에서 실행될 때도 이 스코프에 접근할 수 있게 해주는 기능`이다.

```javascript
function sayHello(){
  const a = 'Hello';
  const b = 'World';
  
  function sumString(){
    console.log(a + '' + b);
  }
  
  return sumString;
}

const myFunc = sayHello();

myFunc();	// 'Hello World'
```

위 예제를 살펴보면, myFunc라는 변수는 sayHello 함수를 호출하고 있다.

그래서 myFunc를 실행하게 되면 문제 없이 'Hello World'가 잘 출력된다. 여기서 살펴볼 점은 myFunc의 부분은 변수 a와 b가 담겨있는 sayHello 함수 스코프의 바깥에 있음에도 불구하고 a와 b를 합친 Hello World를 잘 출력한다는 것인데, 그 이유가 바로 클로저(Closure) 때문이다.

모든 자바스크립트 함수는 선언될(생성될) 당시에 클로저가 형성되어 주변 환경, 즉 `렉시컬 스코프를 기억`할 수 있다.

---

### 클로저의 활용

다음과 같은 상황에서 클로저가 유용하게 사용될 수 있다.

- 정보 은닉 (접근 권한 제어)

클로저 기법을 사용하는 이유는 `전역변수를 사용하지 않으려는 목적`이 강한 듯 하다.
누구나 접근 가능하고, 변경까지 할 수 있는 전역변수를 사용하는 것은 매우 위험하기 때문이다.
하지만 `클로저`를 사용하면 `전역변수를 사용하지 않으면서도 클로저 함수 내부의 변수에 계속 접근`할 수 있기 때문에 이 문제가 해결된다.


- 커링(Currying)

`커링`은 f(a,b,c)처럼 `여러 개의 인자를 한 번의 호출로 처리하던 함수를 f(a)(b)(c) 처럼 분리하여 인자를 하나씩만 받는 여러개의 함수로 만드는 것`이다.

즉, 함수 하나가 n개의 인자를 받는 대신 n개의 함수를 만들어서 각각의 함수가 인자를 받도록 하는 것이다.
예제코드를 보면 커링을 구현할 때도 클로저가 들어간다.

```javascript
// 일반 함수
function fn(x,y){
  return x + y;
}
console.log(fn(1,2));	// 3

// 일반 함수를 커링 함수로 변환
function curried_fn(x){
  return function(y){
    return x + y
  }
}

const curried_fn2 = x => y => x + y;

console.log(
  curried_fn(1)(2),	// 3
  curried_fn2(1)(2)	// 3
);
```

※ 커링을 사용하는 이유

위 예제에서 일반함수의 경우 2개의 인자를 모두 전달받아야 해당 함수가 실행되는데 반해, 커링함수의 경우 첫번째 인자를 통해 하나의 함수를 실행하고, 이어 두번째 인자를 통해 또 하나의 함수를 독립적으로 실행한다. 따라서 인자를 담고있는 함수의 재사용이 가능해지고, 어떤 함수를 특정 애플리케이션 지점에서 다른 지점으로 값을 전달해야 하는 경우에도 유용하게 사용할 수 있다.


- 부분적용함수

위 커링함수를 조금 다른 방식으로 사용해볼 수도 있다.
`부분적용함수`란 n개의 인자를 받는 함수에 `미리 m개의 인자만 넘겨 기억`시켰다가, `나중에 (n-m)개의 인자`를 넘기면 비로소 원래 함수의 실행결과를 얻을 수 있도록 하는 함수를 말한다.
다시 말해, 하나의 고정값을 가지고 다른 인자들의 값을 변환시켜야 하는 경우에 이 부분적용함수를 사용할 수 있다.
