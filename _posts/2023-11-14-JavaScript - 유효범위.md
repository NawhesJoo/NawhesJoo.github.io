---
title: JavaScript - 유효범위
description: >-
  
author: NawhesJoo
date: 2023-11-14 20:55:00 +0900
categories: [Language, JavaScript]
tags: [scope]
pin: true
math: true
mermaid: true
image:
  path: /thumbnails/JavaScript.png
  lqip: 
  alt: 
comments: true
---

## 유효범위

유효범위(scope)는 변수의 수명을 의미한다.

## 전역변수와 지역변수
### 전역변수란?
> 함수 밖에서 변수를 선언하면 그 변수는 전역변수가 된다. 전역변수는 애플리케이션 전역에서 접근이 가능한 변수이다.

#### 전역변수 예시

```javascript
var vscope = 'global';
function fscope(){
  alert(vscope);
}
fscope();
```

![](https://velog.velcdn.com/images/nawhes_joo/post/b8b196f7-cbed-42be-986e-a31ddee78471/image.png)

결과값

---

### 지역변수란?
> 함수 안에서 변수를 선언하면 그 변수는 지역변수가 된다. 지역변수는 함수 { } 안에서만 접근이 가능한 변수이다.

#### 지역변수 예시

```javascript
var vscope = 'global';
function fscope(){
  var vscope = 'local';
  alert(vscope);
}
fscope();
```

![](https://velog.velcdn.com/images/nawhes_joo/post/a230a877-a9bc-43bc-ab69-fa7ac9f81bca/image.png)
결과값

가까운 쪽에 정의되어 있는 vscope
즉, 함수 내에서 정의되어 있는 vscope을 가르킨다.

그러므로 local이 출력된다.

---

### 비교 예시

#### 전역변수

```javascript
var vscope = 'global';
function fscope(){
  alert(vscope);
}
function fscope2(){
  alert(vscope);
}
fscope();
fscope2();
```

결과값

![](https://velog.velcdn.com/images/nawhes_joo/post/b25a85ac-d88b-4973-baf5-19775c6801c9/image.png)

![](https://velog.velcdn.com/images/nawhes_joo/post/21a7431c-e794-442e-bc58-c30be138d070/image.png)

global이 두 번 출력된다.

함수 내에서 변수를 따로 정의하지 않았기 때문에
전역변수로 정의한 'global'이 출력된다.

---

#### 지역변수

```javascript
var vscope = 'global';
function fscope(){
  var vscope = 'local';
  var lv = 'local variables';
  alert(lv);
}
fscope();
alert(lv);
```
![](https://velog.velcdn.com/images/nawhes_joo/post/e384ae85-8b74-45fc-be3e-35bf3dcae501/image.png)

함수 내의 alert(lv)는 정상적으로 출력되지만

![](https://velog.velcdn.com/images/nawhes_joo/post/4b20574a-8e97-4c50-b6ce-813b0f473d02/image.png)

함수 밖에서 alert(lv)는 정의되지 않았기 때문에 출력되지 않는다.

---

#### var 사용 유무 차이
```javascript
var vscope = 'global';
function fscope(){
  var vscope = 'local'; 
}
fscope();
alert(vscope);
```
![](https://velog.velcdn.com/images/nawhes_joo/post/186212dd-2dcf-4186-b1f8-1a80cbebd1b4/image.png)

local 변수를 만들 때 var 키워드를 사용하게 되면 local 변수가 된다.




```javascript
var vscope = 'global';
function fscope(){
  vscope = 'local'; 
}
fscope();
alert(vscope);
```

![](https://velog.velcdn.com/images/nawhes_joo/post/015a5dc9-ae4e-4163-815c-d25906847b17/image.png)

하지만 var을 사용하지 않으면 함수가 실행될 때 전역변수가 된다.

![](https://velog.velcdn.com/images/nawhes_joo/post/dadf9e02-912d-4703-a0a7-6f661e965e06/image.png)

---

#### var vscope + vscope

```javascript
var vscope = 'global';
function fscope(){
  var vscope = 'local';
  vscope = 'local'; 
}
fscope();
alert(vscope);
```

![](https://velog.velcdn.com/images/nawhes_joo/post/60a5b085-3cdc-4d6f-b4f2-52e5c291e7e2/image.png)

vscope는 지역변수가 있는지 먼저 체크한다.
지역변수가 존재하면 지역변수를 할당한다.
var vscope가 없다면 전역변수를 할당한다.

---

> 전역변수의 사용이 분명하지 않다면 지역변수를 사용하자

---

## 유효범위의 효용
> 아래 두 개의 예제는 변수 i를 지역변수로 사용했을 때와 전역변수로 사용했을 때의 차이점을 보여준다. 전역변수는 각기 다른 로직에서 같은 이름의 변수값을 변형시켜서 의도하지 않은 문제를 발생시킨다.

```javascript
function a (){
  var i = 0;
}
for(var i = 0; i < 5; i++){
  a();
  document.write(i);
}
```
![](https://velog.velcdn.com/images/nawhes_joo/post/5b25690c-b270-4f2d-afca-43cb3468007b/image.png)

정상적으로 출력된다.


```javascript
function a (){
  i = 0;
}
for(var i = 0; i < 5; i++){
  a();
  document.write(i);
}
```
![](https://velog.velcdn.com/images/nawhes_joo/post/cd60a5a3-4b08-4e47-b2b3-10200c2c74bb/image.png)
하지만 var를 제거하니 실행이 되지 않는다.

for문 안에 있는 i는 전역변수가 된다.

a(); 때문에 전역변수가 된 i는 0이 된다.
그래서 반복문이 무한반복이 된다.

---

## 전역변수의 사용

프로그램을 작성할 때, 내가 만든 코드로만 만들지 않음. 다른 사람과 협엽 또는 오픈소스를 이용하는 경우가 있다. 
만약 다른 사람이 전역변수를 사용한다면 내가 만든 이름과 충돌할 가능성이 있다.

나만의 전역변수를 만들고, 다른 변수들을 그 전역변수의 소속으로 만들면 충돌할 가능성은 현저히 낮아진다.

```javascript
function(){
  var MYAPP = {} // 객체
  MYAPP.calculator = { // 객체의 속성
    'left' : null,	// 속성의 객체
    'right' : null
  }
  MYAPP.coordinate = { // 객체의 속성
    'left' : null,	// 속성의 객체
    'right' : null
  }
 
  MYAPP.calculator.left = 10;
  MYAPP.calculator.right = 20;
  function sum(){
    return MYAPP.calculator.left + MYAPP.calculator.right;
  }
  document.right(sum());
}()
```
이렇게 함수를 생성하여 호출할 수 있다.

```javascript
(function(){	
  var MYAPP = {} // 객체
  MYAPP.calculator = { // 객체의 속성
    'left' : null,	// 속성의 객체
    'right' : null
  }
  MYAPP.coordinate = { // 객체의 속성
    'left' : null,	// 속성의 객체
    'right' : null
  }
 
  MYAPP.calculator.left = 10;
  MYAPP.calculator.right = 20;
  function sum(){
    return MYAPP.calculator.left + MYAPP.calculator.right;
  }
  document.right(sum());
}())
```
이렇게 함수 전체를 괄호로 묶어주면 MYAPP은 전역변수가 아닌 함수의 지역변수가 된다.
이를 익명함수라고 한다.


> calculator.left : 닷 노테이션 ('유효한 변수 식별자’인 경우에만 동작)
calculator['left'] : 브라켓 노테이션 (키에 어떤 문자열이 있던지 상관없이 동작)

---

## 유효범위의 대상(함수)

> 자바스크립트는 함수에 대한 유효범위만을 제공한다. 많은 언어들이 블록(대체로{,})에 대한 유효범위를 제공하는 것과 다른 점이다.

```java
for(int i = 0; i < 10; i++){
	String name = "Sehwan";
}
Syste.out.println(name);
```
위 코드는 자바 함수이다.
자바에서는 for문 안에서 name을 정의하면 지역변수가 된다.

물론 System.out.println으로 출력도 되지 않는다.

```javascript
for(var i = 0; i < 1; i++){
  var name = 'sehwan';
}
alert(name);
```
위 코드는 자바스크립트 함수이다. 
자바스크립트는 for문과 if문 중괄호 안에서 선언된 변수는 지역변수로서의 의미를 갖지 않는다.

---

## 정적 유효 범위

> 자바스크립트는 함수가 선언된 시점의 유효범위를 갖는다. 이러한 유효범위의 방식을 유효범위
(static scoping) 혹은 랙시컬(lexical scoping)이라고 한다.

```javascript
var i = 5;		// 전역변수

function a(){
  var i = 10;
  b();
}

function b(){
  document.write(i);
}

a();
```

> document.write(i)의 i는 무엇을 가르킬까?


+ 우선 i는 함수 b 내에서 i를 찾게된다.
함수 b에는 지역변수가 존재하지 않는다.
그럼 전역변수를 찾게 된다.

+ 함수 b를 호출하는 함수는 함수 a이다.

+ 그렇다면, a가 가지고 있는 지역변수 i = 10으로 접근하게 될까?
아니면 함수 b가 정의된 시점에서 전역변수인 i = 5로 접근하게 될까?

> 답은 5가 사용된다.

함수 b를 선언하였을 때 시점을 기준으로 한다.

> 사용될 때 X, 정의될 때 O

이를 정적 유효범위라고 한다.


출처 : 생활코딩 유튜브