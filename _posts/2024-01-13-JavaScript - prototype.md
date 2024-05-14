---
title: JavaScript - prototype
description: >-
  
author: NawhesJoo
date: 2024-01-14 20:55:00 +0900
categories: [Language, JavaScript]
tags: [prototype]
pin: true
math: true
mermaid: true
image:
  path: /thumbnails/JavaScript.png
  lqip: 
  alt: 
comments: true
---
## Prototype이란?

Prototype이란 한국어로는 원형정도로 번역된다.

Prototype은 말 그대로 객체의 원형이라고 할 수 있다.

함수는 객체다. 그러므로 생성자로 사용될 함수도 객체다.

객체는 prototype이라는 프로퍼티는 그 용도가 약속되어 있는 특수한 프로퍼티다.

> prototype에 저장된 속성들은 생성자를 통해서 객체가 만들어질 때 그 객체에 연결된다.


---

## 예시

```javascript
function Ultra(){}
Ultra.prototype.ultraProp = true;

function Super(){}
Super.prototype = new Ultra();

function Sub(){}
Sub.prototype = new Super();

var o = new Sub();
console.log(o.ultraProp);
```


o의 ultraProp이라는 프로퍼티에 접근했을 때 어떤 결과가 나올까?

![](https://velog.velcdn.com/images/nawhes_joo/post/14447170-f470-453e-81b2-f6df4097c510/image.png)

true가 나온다.

Sub라는 객체 자체는 ultraProp을 가지고 있지 않다.

이 true는 어떻게 나온 걸까?

Sub의 부모인 Super, Super의 부모인 Ultra가 가지고 있다.

> Sub는 Super를 상속받고, Super는 Ultra를 상속받는다.

Sub에는 ultraProp 정의되어있지 않다.

자바스크립트는 내부적으로 Sub의 부모인 Super에서 ultraProp를 가지고 있는지 확인한다.

Super에도 ultraProp를 가지고 있지 않다면 그 부모인 Ultra에서 ultraProp를 찾는다.

---

### 생성자

생성자는 기본적으로 함수이다. 함수를 호출할 때 앞에 new를 붙여주면,

이 new로 인해서 단순한 함수가 아니라 생성자라는 함수가 된다.

그렇게해서 실행된 결과는 새로운 객체를 만들어서, 그 객체를 return하기 때문에 o라는 변수 안에는 생성자를 통해서 만들어진 객체가 들어가게 된다.

```javascript
var o = {}
```

이렇게 하지 않는 이유는 어떠한 객체를 만들 때, 그 객체가 가지고 있어야 하는 어떤 메소드 또는 데이터인 프로퍼티값을 기본적으로 가지고 주어주기를 바라는 것이다.

왜냐면 그 객체 안에 로직, 데이터가 담겨있기 때문이다.

---

### 예시 분석

어떤 메소드, 프로퍼티를 가지고 있는 객체의 원형은 어딘가에 저장되어 있다.

그 '어딘가'는 prototype이라는 property에 저장되어 있는 것이다.

```javascript
function Ultra(){}
Ultra.prototype.ultraProp = true;

function Super(){}
Super.prototype = new Ultra();

function Sub(){}
Sub.prototype = new Super();

var o = new Sub();
console.log(o.ultraProp);
```

Sub라고 하는 함수는 객체이기 때문에 property를 가질 수 있다.

그중에 prototype이라는 특수한 property 안에 객체를 저장한다.

> 나중에 new를 이용해서 생성자를 호출하게 되면 자바스크립트는 생성자 함수의 prototype에 저장되어 있는 객체를 꺼내서 return해준다.

---

```javascript
Sub.prototype = new Super();
```

위 코드는 Super 생성자가 만든 객체가 new Super()에 들어가는 것이다.

```javascript
Super.prototype = new Ultra();
```

또한 Super 생성자의 prototype 객체는 Ultra로 인해서 만들어진 객체가 담기는 것이다.

```javascript
Ultra.prototype.ultraProp = true;
```

Ultra의 prototype값은 ultraProp라는 값을 가지고 있다.

ultraProp의 값은 true라고 저장되어 있다.

위 값이 new Ultra()로 인해 Super의 prototype에 저장되고,
또 new Super()로 인해 Sub의 prototype에 저장된다.

그래서 new Sub()라고 정의한 o의 o.ultraProp을 출력하면 true가 나오는 것이다.

> 이러한 개념을 prototype chain 이라고 한다.

---

## prototype chain

```javascript
function Ultra(){}
Ultra.prototype.ultraProp = true;

function Super(){}
Super.prototype = new Ultra();

function Sub(){}
Sub.prototype = new Super();

var o = new Sub();
o.ultraProp = 1;
console.log(o.ultraProp);
```

o.ultraProp을 1이라고 정의하고 호출해보자.

![](https://velog.velcdn.com/images/nawhes_joo/post/f512820b-4357-4e63-b7b1-0c1ebc7b2e70/image.png)

1이 출력된다.

o.ultraProp을 출력하였다.

o가 ultraProp의 값을 가지고 있는지 먼저 확인한다.

o.ultraProp을 1로 정의하였기 때문에 1이 호출된다.

---

```javascript
function Ultra(){}
Ultra.prototype.ultraProp = true;

function Super(){}
Super.prototype = new Ultra();

function Sub(){}
Sub.prototype = new Super();
Sub.prototype.ultraProp = 2;

var o = new Sub();
console.log(o.ultraProp);
```

위 코드처럼 수정해보자.

![](https://velog.velcdn.com/images/nawhes_joo/post/c1c1431e-352b-4ab0-b101-1de18e522917/image.png)

2가 출력된다.

o라는 객체에서 ultraProp가 있는지 확인한다.

없으므로 바로 상위인 Sub에서 ultraProp을 찾게 된다.

---

```javascript
function Ultra(){}
Ultra.prototype.ultraProp = true;

function Super(){}
Super.prototype = new Ultra();

function Sub(){}
var s = new Super();
s.ultraProp = 3;
Sub.prototype = s;

var o = new Sub();
console.log(o.ultraProp);
```
위 코드처럼 수정해보자.

![](https://velog.velcdn.com/images/nawhes_joo/post/9867690d-e820-42f6-b028-98e069d14a8a/image.png)

3이 출력된다.

위 코드도 마찬가지이다.

new Super()를 s에 담고 s의 ultraProp을 3으로 정의하였기 때문에 3이 출력된다.

> 객체에 값이 존재하지 않다면 상위 객체에서 값을 찾는다. 이를 반복한다.