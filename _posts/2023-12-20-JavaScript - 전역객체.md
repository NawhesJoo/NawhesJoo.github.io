---
title: JavaScript - 전역객체
description: >-
  
author: NawhesJoo
date: 2023-12-20 20:55:00 +0900
categories: [Language, JavaScript]
tags: [global, object]
pin: true
math: true
mermaid: true
image:
  path: /thumbnails/JavaScript.png
  lqip: 
  alt: 
comments: true
---
## 전역객체란?

> 전역객체(Global object)는 특수한 객체다. 모든 객체는 이 전역객체의 프로퍼티다.

```javascript
function func(){
  console.log('Hello?');
}
func();
window.func(); // window = 객체, func() : 속성(메소드)
```

![](https://velog.velcdn.com/images/nawhes_joo/post/2d27624f-e623-4f5c-8984-e6af5e90171b/image.png)

결과값

func();와 window.func();는 모두 실행이 된다. 모든 전역변수와 함수는 사실 window 객체의 프로퍼티다. 객체를 명시하지 않으면 암시적으로 window의 프로퍼티로 간주된다.

---

```javascript
var o = {'func':function(){
  console.log('Hello?');
}}
o.func();
window.o.func();
```

![](https://velog.velcdn.com/images/nawhes_joo/post/87bbbbdb-bc79-4a33-ac0f-5e17b891c15b/image.png)

결과값

자바스크립트에서 모든 객체는 기본적으로 전역객체의 프로퍼티임을 알 수 있다.

---

## 전역객체 API

ECMAScript에서는 전역객체의 API를 정의해두었다. 그 외의 API는 호스트 환경에서 필요에 따라서 추가로 정의하고 있다. 이를테면 웹브라우저 자바스크립트에서는 alert()라는 전역객체의 메소드가 존재하지만 node.js에는 존재하지 않는다. 또한 전역객체의 이름도 호스트환경에 따라서 다른데, 웹브라우저에서 전역객체는 window이지만 node.js에서는 global이다.

