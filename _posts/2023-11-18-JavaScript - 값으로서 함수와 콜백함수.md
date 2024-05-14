---
title: JavaScript - 값으로서 함수와 콜백함수
description: >-
  
author: NawhesJoo
date: 2023-11-18 20:55:00 +0900
categories: [Language, JavaScript]
tags: [function, cfn]
pin: true
math: true
mermaid: true
image:
  path: /thumbnails/JavaScript.png
  lqip: 
  alt: 
comments: true
---
## 함수의 용도

### 값으로서 함수

> JavaScript에서는 함수도 객체다. 다시 말해 일종의 값이다. 거의 모든 언어가 함수를 가지고 있다. JavaScript의 함수가 다른 언어의 함수와 다른 점은 함수가 값이 될 수 있다는 점이다.

---

#### 메소드

```javascript
function a() {}
```

위의 예제에서 함수 a는 변수 a에 담겨진 값이다. 또한 함수는 객체의 값으로 포함될 수 있다. 이렇게 객체의 속성 값으로 담겨진 함수를 메소드(method)라고 부른다.

```javascript
a = {
  	b:function(){
    }
};

```
> b : key, function() : value

b(key)는 변수와 같은 역할을 한다. 
객체 안에서 어떤 변수의 역할을 하는 것을 '속성(property)'이라고 한다.
그 속성에 저장되어있는 값이 함수라고 한다면, 그 함수를 이러한 맥락에서 메소드(method)라고 부른다. 

```javascript
function a(){}; // 함수

a = {
  b:function(){ // 메소드
  }
};
```

---

```javascript
function cal(func, num){ // func와 num을 인자로 전달
  return func(num)
}
function increase(num){
  return num+1			// 인자로 전달된 값에 +1을 한 값을 return,
}
function decrease(num){
  return num-1			// 인자로 전달된 값에 -1을 한 값을 return
}
alert(cal(increase, 1));
alert(cal(decrease, 1));
```

3개의 함수를 정의하였다.

첫 번쨰 인자인 func라고 하는 함수를 호출, 두 번째 인자인 num을 호출된 func 함수의 인자로 전달한다.


cal(increase,1)을 살펴보자

function cal(func, num)에서
func와 increase를 대입하면

```javascript
function cal(func, num){
  var func = increase(num){
    return num+1
  }
}
```
num에 1을 넣으면 return값은 +1을 계산한 값인

![](https://velog.velcdn.com/images/nawhes_joo/post/9b45a8d6-f005-4453-ab82-df5ffaa21054/image.png)

2라는 값이 알림창에 발생할 것이다.

decrease도 마찬가지로

![](https://velog.velcdn.com/images/nawhes_joo/post/9187cfea-0cac-4f65-9653-c3337e9f4a21/image.png)

0이 출력된다.

---

#### return 값

> 함수는 함수의 return 값으로도 사용할 수 있다.

```javascript
function cal(mode){
  var funcs = {
    'plus' : function(left, right) {return left + right},
    'minus' : function(left, right) {return left - right}
  }
  return funcs[mode]
}
alert(cal('plus')(2,1));
alert(cal('minus')(2,1));
```
위 예제에서
cal이라는 함수를 호출할 때, 첫 번째 인자로 'plus'라는 값을 주게 되면,
var funcs = ~ 라는 객체에 [mode]의 값이 'plus'가 되고, 
'plus'에 해당되는 속성은 'plus'라는 키 값이다.
'plus'의 실제내용은 value값인 functino(left,right) {return left+right}인 것이다.

정리하자면 cal('plus')로 인해 return funcs[mode] 로직은 

```javascript
'plus' : function(left, right) {return left+plus}
```

이 부분을 외부로 return하는 것이다.
cal('plus')(2,1)의 인자인 2와 1이 'plus'의 value 값인 function의 left와 right 인자에 각각 들어간다.

left는 2가, right는 1이 되어, left + right 값인

![](https://velog.velcdn.com/images/nawhes_joo/post/5a24ba77-f5e5-4bdd-a59a-b386b2ce23aa/image.png)

3이 return 된다.

마찬가지로 

```javascript
alert(cal('minus')(2,1));
```

의 출력값은

![](https://velog.velcdn.com/images/nawhes_joo/post/0ed0cc08-e605-4e40-94d5-63c02de8386c/image.png)

1이 된다.

---

#### 배열

```javascript
var process = {
  function(input) {return input + 10;},	// i가 0일 때
  function(input) {return input * input;}, // i가 1일 때
  function(input) {return input / 2;}	// i가 2일 때
};
var input = 1;
for(var i = 0; i < process.length; i++){ // i의 값은 0,1,2
  input = process[i](input);
}
alert(input);
```

process[i] 부분은, i값이 0일 때 1번째 함수, 1일 때 2번째, 2일 때는 3번째 함수를 의미한다.

process[i]는 input의 값을 인자로 전달한다.

input의 값을 1로 지정하였다.


```javascript
// i = 0, input = 1
function(input) {
  return input + 10;	// return 값은 11
}
```

```javascript
// i = 1, input = 11
function(input) {	
  return input * input; // return 값은 121
}
```

```javascript
// i = 2, input = 121
function(input){
  return input / 2; // return 값은 60.5
}
```

---

> 함수는 변수, 함수의 매개변수, return값으로 사용될 수 있다.
이러한 용도로 사용될 수 있는 데이터를 프로그래밍에서는 
first-class citizen 또는 first-class object 같은
first-class ~~ 라고 불린다.

---

## 콜백

### 처리의 위임

> 값으로 사용될 수 있는 특성을 이용하면 함수의 인자로 함수로 전달할 수 있다. 값으로 전달된 함수는 호출될 수 있기 때문에 이를 이용하면 함수의 동작을 완전히 바꿀 수 있다. 인자로 전달된 함수 sortNumber의 구현에 따라서 sort의 동작방법이 완전히 바뀌게 된다.

```javascript
function sortNumber(a,b){
  return b-a;
}
var numbers = [20,10,9,8,7,6,5,4,3,2,1];
alert(numbers.sort(sortNumber)); // array, [20,10,9,8,7,6,5,4,3,2,1]
```

numbers : 배열객체, sort : 메소드
>+ 배열이나 sort와 같은 객체, 메소드는 JavaScript가 기본적으로 가지고 있으면서 제공하는 기능이다. 이를 내장객체, 내장메소드 또는 빌트인 객체, 빌트인 메소드라고 한다
+ 반대로 직접 만드는 객체, 메소드, 함수는 사용자 정의 객체, 사용자 정의 메소드, 사용자 정의 함수라고 한다.

```javascript
var numbers = [20,10,9,8,7,6,5,4,3,2,1];
console.log(numbers.sort());
```

vscode로 위 함수를 실행시켜보자.

![](https://velog.velcdn.com/images/nawhes_joo/post/25ca2a75-a4a9-4899-94dd-41f471911bbd/image.png)


1, 2, 3 ... 10, 20 이렇게 정렬 되지 않고 위 사진처럼 출력된다.

숫자가 작은 순으로 출력될 수 있도록 정렬기준을 정해보자.

```javascript
var numbers = [20,10,9,8,7,6,5,4,3,2,1];
var sortfunc = function(a, b){
    console.log(a, b);
    if(a > b) {
        return 1;
    } else if (a < b){
        return -1;
    } else {
        return 0;
    }
}
console.log(numbers.sort(sortfunc));
```

위 코드처럼 수정한다.

![](https://velog.velcdn.com/images/nawhes_joo/post/d7ffd7c6-a9b1-4a73-ad08-126ad1d91c94/image.png)

a와 b가 출력이 되고, a와 b를 비교한다.

비교하여 return 값을 양수, 음수, 0으로 return 한다.

```javascript
var numbers = [20,10,9,8,7,6,5,4,3,2,1];
var sortfunc = function(a, b){
    return a - b;
}
console.log(numbers.sort(sortfunc));
```

양수, 음수, 0을 return 할 것이니 위 코드처럼 수정해도 같은 기능을 수행한다.

![](https://velog.velcdn.com/images/nawhes_joo/post/8ee554a4-a474-4482-90fa-95d63b4ab6e3/image.png)

반대로 큰 수부터 정렬하려면

```javascript
var numbers = [20,10,9,8,7,6,5,4,3,2,1];
var sortfunc = function(a, b){
    return b - a;
}
console.log(numbers.sort(sortfunc));
```

![](https://velog.velcdn.com/images/nawhes_joo/post/caa0cf5a-fc8c-413f-9029-f4917b97039d/image.png)

이렇게 수정할 수 있다.

여기서 sortfunc함수가 '콜백함수'가 된다.

> 이 콜백함수를 수신받는 sort라는 메소드가 콜백함수를 인자로 받아, 내부적으로 호출하는 것을 통해 sort가 동작하는 방법을 변경할 수 있게 된 것이다.

즉, 값으로서 함수를 사용할 수 있기 때문에 이 sort 함수에 값(콜백함수)을 전달하는 것을 통해 완전히 바꿀 수 있다.

---

### 비동기 처리

>콜백은 비동기처리에서도 유용하게 사용한다. 시간이 오래걸리는 작업이 있을 때 이 작업이 완료된 후에 처리해야 할 일을 콜백으로 지정하면 해당 작업이 끝났을 때 미리 등록한 작업을 실행하도록 할 수 있다.

+ 동기와 비동기

>동기란 여러 태스크가 순차적으로 동작하는 것을 의미한다.
반대로 비동기란 여러 태스크가 한 번에 동작하는 것을 의미한다.
 
 ---
>비동기처리를 자바스크르립트에서 어떤 경우에 사용할 수 있나? -> Ajax

Ajax : Asynchronous Javascript And Xml
Asyncrhonous : 비동기

간단한 예시를 들자면

동기를 사용한다면 웹에서 메뉴가 많은 항목을 클릭하면 로딩하는 동안 웹이 freeze된다.

비동기를 사용한다면 로딩하는 동안에 다른 탭을 본다던지 스크롤 등을 할 수 있다.

---

아래부터는 영상 캡쳐로 대체한다.

![](https://velog.velcdn.com/images/nawhes_joo/post/f9832a7f-b37f-47a7-b6af-13b46c8d4134/image.png)

```javascript
<script src="~~"
```

를 통해 jquery를 include하고 있다.

jquery가 가지고 있는 기능 중에 Ajax 통신을 하기 위한 $.get이라는 기능이 있다.

$는 jquery의 특수한 객체이다. 그 객체가 가지고 있는 get이라는 메소드를 사용하여 호출한다.

.get은 첫 번째 인자로 URL을 전달하도록 약속되어있다.
두 번째 인자로는 함수를 전달한다. 이 함수는 서버와 통신이 끝났을 때 결과로 호출되도록 함수를 적는다.

Ajax를 위한 처리는 누가 call하던 행위 자체는 동일하다. 그 동일한 부분은 get이라는 메소드가 알아서 처리한다.

>서버에서 정보를 가져온 후에 처리하는 행위는 다르다.(비즈니스 로직) 
동일하지 않기 때문에 사용자에게 위임을 해야 한다. 
이 위임을 하는 기법이 바로 '콜백함수'를 통해서 인자를 전달받는 것이다.

함수를 매개변수 값으로 전달하는 것을 통해서 get이라는 메소드가 동작하는 방법을 완전히 바꿀 수 있다.