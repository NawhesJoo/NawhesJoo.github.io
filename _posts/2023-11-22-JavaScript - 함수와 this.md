---
title: JavaScript - 함수와 this
description: >-
  
author: NawhesJoo
date: 2023-11-22 20:55:00 +0900
categories: [Language, JavaScript]
tags: [function, this]
pin: true
math: true
mermaid: true
image:
  path: /thumbnails/JavaScript.png
  lqip: 
  alt: 
comments: true
---
## this란?

>this는 함수 내에서 함수 호출 맥락(context)를 의미한다. 맥락이라는 것은 상황에 따라서 달라진다는 의미인데 즉 함수를 어떻게 호출하느냐에 따라서 this가 가리키는 대상이 달라진다는 뜻이다. 함수와 객체의 관계가 느슨한 자바스크립트에서 this는 이 둘을 연결시켜주는 실질적인 연결점의 역할을 한다.

### 함수와 this

함수를 호출했을 때 this는 무엇을 가르키는지 살펴보자. this는 전역객체인 window와 같다.

```javascript
function func() {
  if(window === this){	// window가 this라면
    console.log("window === this");
  }
}
func();
```

![](https://velog.velcdn.com/images/nawhes_joo/post/8f437d4d-6fea-4972-9f83-979da98699cd/image.png)

결과

> 어떠한 전역변수나 전역객체, 어떤 메소드에 속해있지 않은 경우에는 window를 가르킨다.
앞에 window.이 생략되어 있다고 생각하면 된다.


---

### 메소드와 this

>객체의 소속인 메소드의 this는 그 객체를 가르킨다.

```javascript
var o = {
  func : function(){
    if(o === this){
      console.log("o === this");
    }
  }
}
o.func();
```

![](https://velog.velcdn.com/images/nawhes_joo/post/5b2a5bfd-3256-4d42-b94a-92028fc4413b/image.png)

결과

어떻게 보면 window 예시와 같다.

this는 속해있는 변수, 객체, 또는 메소드를 가리킨다.

---

### 생성자와 this


~~~javascript
var funcThis = null;

function Func(){
  funcThis = this;
}
var o1 = Func();	// 일반 함수 호출
if(funcThis === window) {
  document.write('window </br>');
}

var o2 = new Func();	// new를 통해 생성자로 호출
if(funcThis === o2){
  document.write('o2 </br>');
}
~~~

![](https://velog.velcdn.com/images/nawhes_joo/post/262e2802-9347-4cac-8861-ebb238178eae/image.png)

결과

---

o1을 실행시키면

~~~javascript
function Func(){
  funcThis = this;
}
~~~

this는 funcThis라는 변수에 할당이 될 것이다.

var이 붙어있지 않기 때문에 전역변수인 

~~~javascript
var funcThis = null;
~~~
를 가르킨다.

그러므로 funcThis의 값은 this의 값이 세팅된다.

~~~javascript
if(funcThis === window)
  ~~~

조건문이 true가 된다.

---

o2를 실행시키면

~~~javascript
var o2 = new Func();
~~~

new로 생성자로 호출하게 되면 자바스크립트는 내부적으로 비어있는 객체를 만들고,

새로 생성되는 객체가 생성자 안에서 this가 된다.

이 생성자가 실행되면

~~~javascript
funcThis = this;
~~~

this는 funcThis가 되고

~~~javascript
var funcThis = null;
~~~

으로 갔다가,

~~~javascript
if(funcThis === o2)
  ~~~

여기로 오게 된다.

생성자를 통해서 만들어진 객체는 o2에 담겨있다.

그러므로 위 조건문은 true가 된다.

> 똑같은 함수지만, 함수로 호출될 때는 this값은 window를, 생성자로 사용될 때는 this 값이 생성될 객체를 가르킨다

---

### apply와 this

함수의 메소드인 apply, call을 이용하면 this의 값을 제어할 수 있다.

#### apply와 call

##### call 
>  	call 메소드는 함수를 호출하면서 특정 개체를 함수의 'this'로 지정한다. 그리고 함수에 필요한 인자를 각각의 매개변수로 전달한다.

~~~javascript
const person1 = {
    firstName: 'John',
    lastName: 'Doe',
};

const person2 = {
    firstName: 'Jane',
    lastName: 'Doe',
};

function greet(message) {
    console.log(`${message}, ${this.firstName} ${this.lastName}!`);
}

greet.call(person1, 'Hello'); // 출력: Hello, John Doe!
greet.call(person2, 'Hi');    // 출력: Hi, Jane Doe!


~~~

위 예제에서 call 메소드를 사용하여 greet 함수를 호출할 때, this를 각각의 person1과 person2로 설정하고 있다.

---

##### apply

>apply 메소드는 call과 유사하지만, 함수에 전달되는 인자를 배열로 받는다.

~~~javascript
const numbers = [1, 2, 3, 4, 5];

function sum() {
    const result = Array.from(arguments).reduce((acc, num) => acc + num, 0);
    console.log(`Sum: ${result}`);
}

sum.apply(null, numbers);
~~~

이 예제에서는 apply 메소드를 사용하여 배열의 요소들을 sum 함수에 전달하고 있다. 

 sum.apply(null, numbers)를 호출하면서 this는 null로 설정되고, 배열 numbers의 각 요소들이 함수에 인자로 전달된다.
 
>공통점: 둘 다 함수를 호출하면서 this 값을 설정할 수 있다.
둘 다 함수에 인자를 전달할 수 있다.

>언제 사용?
만약 함수에 전달할 인자가 배열 형태로 있을 때는 apply를 사용하는 것이 편리
그 외에는 주로 call이 사용되며, 이는 함수에 각각의 인자를 전달할 때 유용

---

### 예제


~~~javascript
var o = {}
var p = {}
function func(){
  switch(this){		// this는 window를 가르킴
    case o :
      document.write('o<br />');
      break;
    case p :
      document.write('p<br />');
      break;
	case window :
      document.write('window<br />');
      break;
  }
}
func();
func.apply(o);
func.apply(p);
~~~

![](https://velog.velcdn.com/images/nawhes_joo/post/0f5494c5-ebc5-4e74-8814-da05efa0461d/image.png)

결과값

---

~~~javascript
func();
~~~
먼저 살펴보자.

this는 window를 가르키므로

~~~javascript
case window :
      document.write('window<br />');
      break;
~~~
위 코드가 실행되어 window가 출력된다.

---

~~~javascript
func.apply(o); 
~~~

.apply 때문에 func()가 실행되면서 this 값이 첫 번째 인자인 o가 된다.

~~~javascript
case o :
      document.write('o<br />');
      break;
~~~

그래서 위 코드가 실행된다.

---

~~~javascript
func.apply(p); 
~~~

p도 마찬가지로 this가 p가 되므로

~~~javascript
case p :
      document.write('p<br />');
      break;
~~~

위 코드가 실행된다.
