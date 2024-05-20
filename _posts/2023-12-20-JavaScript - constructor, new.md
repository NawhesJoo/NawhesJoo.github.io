---
title: JavaScript - 생성자와 new
description: >-
  
author: NawhesJoo
date: 2023-12-20 20:55:00 +0900
categories: [Language, JavaScript]
tags: [constructor, new]
pin: false
math: true
mermaid: true
image:
  path: /thumbnails/JavaScript.png
  lqip: 
  alt: 
comments: true
---
## 객체

> 객체란 서로 연관된 변수와 함수를 그룹핑한 그릇이라고 할 수 있다. 객체 내의 변수를 프로퍼티(property) 함수를 메소드(method)라고 부른다.

```javascript
var person = {} // Object
person.name = 'egoing'; // 프로퍼티(변수)
person.introduce = function(){	// 메소드
	return 'My name is ' + this.name;
}

document.write(person.introcue());
```

object 라고 하는 그릇 안에 name이라는 변수를 담고, 그 변수의 내용은 문자열 'egoing'이 되는 것이다. (프로퍼티 / 속성)

또다른 introduce라는 변수를 담는다. 이 변수안에 담긴 값은 함수이다. (메소드)

---

객체를 만드는 과정에 분산되어 있다. 중간에 다른 코드가 들어온다면 가독성이 좋지 않을 뿐더러 내용이 변질될 수 있다.
객체를 정의할 때 값을 셋팅하도록 코드를 바꿔보자.

```javascript
var person = {
	'name' : 'egoing',
  'introduce' : function(){
  	return 'My name is ' + this.name;
  }
}
```

이렇게 변경하면 가독성이 좋고, 내용이 변질될 위험성도 줄어든다.

---

## 생성자

> 생성자(constructor)는 객체를 만드는 역할을 하는 함수다. 자바스크립트에서 함수는 재사용 가능한 로직의 묶음이 아니라 객체를 만드는 창조자라고 할 수 있다.

```javascript
function Person(){
  var p = new Person();
  p.name = 'egoing';
  p.introduce = function(){
  	return 'My name is ' + this.name;
  }
}
document.write(p.introduce())
```

![](https://velog.velcdn.com/images/nawhes_joo/post/f30f5ccc-aa02-46a9-9a38-87ff1463baaf/image.png)

return값이 없으므로 p0에는 아무것도 존재하지 않는다.

![](https://velog.velcdn.com/images/nawhes_joo/post/03e9b0f2-4559-4314-b0e0-342670b7f1d1/image.png)

p를 new Person()이라고 정의했다.

p에는 Person{}이 담겨있다.

new를 앞에 붙이고 함수를 호출하게 되면, 이 맥락에서 함수는 함수가 아닌 객체의 생성자이다.

이 생성자는 new로 인해서 비어있는 객체를 만들고, 그 객체를 return하기 때문에
p에는 비어있는 객체가 만들어진 것이다.

> 함수에 new를 붙이면 그 return값은 객체가 된다.

---

```javascript
function Person(){}
var p1 = new Person();	// 객체 생성
p1.name = 'egoing';	// name 지정
p1.introduce = function(){ // introduce 함수 정의
  return 'My name is ' + this.name;
}
document.write(p1.introduce()+"<br />");

var p2 = new Person();	// 아래도 같음
p2.name = 'sehwan';
p2.introduce = function(){
  return 'My name is ' + this.name;
}
document.write(p2.introduce());
```

똑같은 역할을 하고 있는 메소드임에도 불구하고 중복된다.

개선된 것이 없다.

---

```javascript
function Person(name){
  this.name = name;
  this.introduce = function(){
    return 'My name is ' + this.name;
  }
}
var p1 = new person('egoing');	// person : 생성자, 'egoing' : 인자
document.write(p1.introduce()+"<br />");

var p2 = new Person('sehwan');
document.write(p2.introduce());
```

![](https://velog.velcdn.com/images/nawhes_joo/post/63bb975f-65dc-4337-b602-9535e5bcefff/image.png)

생성자를 활용하여 매우 간결해졌다.

생성자 내에서 이 객체의 프로퍼티를 정의하고 있다. 이러한 작업을 초기화(init / initialize)라고 한다.
이를 통해서 코드의 재사용성이 대폭 높아졌다.