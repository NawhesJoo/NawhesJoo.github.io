---
title: JavaScript - 커링(currying)
description: >-
  
author: NawhesJoo
date: 2024-05-20 14:28:00 +0900
categories: [Language, JavaScript]
tags: [currying]
pin: true
math: true
mermaid: true
image:
  path: /thumbnails/JavaScript.png
  lqip: 
  alt: 
comments: true
---
## 커링이란?



![](https://velog.velcdn.com/images/nawhes_joo/post/ea31e609-3444-4a30-b01a-654f69c13a4f/image.png)

출처 - [hustle-dev 벨로그](https://velog.io/@hustle-dev/Javascript-%EC%BB%A4%EB%A7%81%EC%97%90-%EB%8C%80%ED%95%B4-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90)

>커링(Currying)은 함수의 `재사용성을 높이기 위해 함수 자체를 return하는 함수`이다.
>
>쉽게 말하면, `'함수를 반환하는 함수'`이다.

함수를 하나만 사용할 때는 필요한 모든 파라미터를 한 번에 넣어야 한다. 커링을 사용하면 함수를 분리할 수 있으므로 파라미터도 나눠 전달할 수 있다.

+ 커링과 같이 함수 자체를 인자로 받거나 반환하는 함수를 `고차 함수`라고 부르기도 한다.

커링이 왜 사용될까??

* 함수의 재활용을 위해서
	- 원하는 함수들을 조합해서 사용할 수 있다.

* 하나 이상의 인수의 함수를, 하나의 인수를 받는 함수로 축소할 수 있다.
	- 그래서 더 가벼운 함수 제작이 가능하다.

---

## 커링

### 일반적인 커링

```javascript
function add(a,b){
  return a+b;
}
```

다음 add 함수를 재활용 하려면?

```javascript
function addTwo(a){
  return add(a,2)
}
```

이렇게 2+@를 해주는 addTwo 함수를 만들 수 있다.

만약 add3, add4, add5... 등의 여러 함수가 필요하다고 하자.

~~~javascript
function addTwo(a){
  return add(a,2);
}
function addThree(a){
  return add(a,3);
}
function addFour(a){
  return add(a,4);
}
~~~

분명 add 함수를 재활용하긴 하지만, 뭔가 이상하다.

여기에 커링을 적용해보자.

```javascript
function add(a,b){
    return console.log(a+b);
}

function addX(x){
    return function(a){
      return add(a, x);
    }
  }
  
  const addTwo = addX(2);
  const addThree = addX(3);
  const addFour = addX(4);

  addTwo(2); // 2 + 2 = 4
  addThree(5); // 3 + 5 = 8
  addFour(1); // 4 + 1 = 5
```

위 코드를 하나씩 살펴보자.

```javascript
function addX(x){
  return function(a){
    return add(a, x);
  }
}
```

위 코드에서 addX 함수는 하나의 인수 x를 받아들이고, 내부에서 또 다른 함수를 반환한다. 이 내부 함수는 a라는 인수를 받아들이고, add(a, x)를 호출한다. 이때 x는 addX가 호출될 때의 값으로 유지된다. 이 구조는 클로저를 형성한다.

> 클로저 : 클로저는 함수가 자신이 선언될 때의 환경(스코프)을 기억하는 기능이다. 따라서 내부 함수는 addX가 호출될 때의 x 값을 `기억`하고 있다.
{: .prompt-info }

```javascript
const addTwo = addX(2);   // 클로저 함수 생성, x = 2
const addThree = addX(3); // 클로저 함수 생성, x = 3
const addFour = addX(4);  // 클로저 함수 생성, x = 4
```

이 코드들은 각각 'addX' 함수에 2, 3, 4를 인수로 전달하여 새로운 함수를 반환받는다.

예를 들어, `addTwo`는 `addX(2)`를 호출하여 `x`가 2인 클로저를 반환받는다.

```javascript
addTwo(2);  // 2 + 2 = 4
addThree(5);  // 3 + 5 = 8
addFour(1); // 4 + 1 = 5
```

`addTwo(2)`를  호출하면 내부 함수는 `a`가 `2`이고 `x`가 `2`인 상황에서 `add(2, 2)`를 호출한다.

`addThree`와 `addFour`도 동일한 방식으로 작동한다.

> `addTwo`, `addThree`, `addFour` 함수들은 클로저로서 `addX`가 호출될 때의 `x` 값을 기억하며, 호출될 때마다 해당 값을 사용하여 `add` 함수를 호출한다.
{: .prompt-tip }


![](https://velog.velcdn.com/images/nawhes_joo/post/e74fc8d1-8f55-463c-a0c8-8ac182374dec/image.png)

다음과 같이 addX 함수를 이용해서 여러 파생 함수들을 만들 수 있다.

정리하자면

+ 인자 x를 받아서 익명함수를 반환한다. 그 함수는 function(a){ return add(a,x); } 의 형태이다.

+ 반환받은 값인 함수는 인자 a를 요구한다. a를 넣어주면 add(a,x)의 실행 결과를 반환한다.

이처럼 함수 실행을 위한 인자를 한 번에 받지 않고, 여러 차례에 나눠서 받을 수 있다.

이 예시를 화살표 함수로 나타내면,

```javascript
function addX(x){
  return function(a){
    return add(a, x);
  }
}

const addX = x => a => add(a,x);
```

이렇게 더 간단해진다.

---

### 함수를 인자로 받는 커링함수

자바스크립트에서는 함수도 값이다.

그러므로 함수의 인자로 또 다른 함수도 받을 수 있다.

```javascript
function curry(fn){
  return function(a){
    return function(b){
      return fn(a,b);
    }
  }
}
```

이제 이 함수를 어떻게 써야할까?

정확히 위에서 선언했던 addX와 동일하게 만들 수 있다.

```javascript
function add(a,b){
  return a+b;
}

const addX = curry(add);
const addTwo = addX(2);
console.log(addTwo(5));
```

![](https://velog.velcdn.com/images/nawhes_joo/post/b6104e4a-88cf-4f09-9e6b-8354f14cb8b4/image.png)

함수 자체를 인자로 받아서 활용할 수 있는 것을 알게 되면, 코드를 작성하는 스타일이 더 다양해진다.

---


### 객체 데이터를 가져오는 커링

커링을 사용하지 않은 경우 :

```javascript
const todos = [
  { id: 3, content: 'HTML', completed: false },
  { id: 2, content: 'CSS', completed: true },
  { id: 1, content: 'Javascript', completed: false}
];

const getTodosIdArr = todos => todos.map(todo => todo.id);
const getTodosContentArr = todos => todos.map(todo => todo.content);
const getTodosCompletedArr = todos => todos.map(todo => todo.completed)

console.log(getTodosIdArr(todos));			// [ 3, 2, 1 ]
console.log(getTodosContentArr(todos));		// [ 'HTML', 'CSS', 'Javascript' ]
console.log(getTodosCompletedArr(todos));	// [ false, true, false ]
```

일반적인 경우 코드를 작성하면 위와 같이 작성하게 된다. 여기에 커링을 적용해보자.

커링을 사용한 경우 :
```javascript
const todos = [
    { id: 3, content: 'HTML', completed: false },
    { id: 2, content: 'CSS', completed: true },
    { id: 1, content: 'Javascript', completed: false }
];

const get = property => object => object[property];

const getId = get('id'); // object => object['id'];
const getContent = get('content'); // object => object['content'];
const getCompleted = get('completed'); // object => object['completed']

const getTodosIdArr = todos => todos.map(getId);
const getTodosContentArr = todos => todos.map(getContent);
const getTodosCompletedArr = todos => todos.map(getCompleted);

console.log(getTodosIdArr(todos)); // [ 3, 2, 1 ]
console.log(getTodosContentArr(todos)); // [ 'HTML', 'CSS', 'Javascript' ]
console.log(getTodosCompletedArr(todos)); // [ false, true, false ]
```

> 커링을 사용하는 경우 인자의 순서가 중요하다.
> 앞에 존재하는 인자일 수록 변동 가능성이 적고, 뒤에 있는 인자일 수록 변동 가능성이 높기 때문에 이 순서를 고려하여 설계하는 것이 중요하다고 한다.

---

## 커링 응용

function 키워드 없이 화살표 함수로 표현하여 방정식 4x^(x+2)를 수행할 함수를 만들어보자.

```javascript
const add = (a,b) => a + b;
const multiply = (a,b) => a * b;

const addX = x => a => add(a, x);
const addTwo = addX(2);

const multiplyX = x => a => multiply(a, x);
const multiplyFour = multiplyX(4);
```

이렇게 커링을 이용해 addTwo, multiplyFour 함수를 만들었다.

```javascript
const compose = fn => fn2 => x => fn2(x) * fn(x);
const equation = compose(addTwo)(multiplyFour);
equation(10) // (4 * 10) * (10 + 2) = 40 * 12 = 480
```

equation 함수에 넣는 값이 x가 되어 4x(x+2)의 다양한 값에 대한 결과를 받을 수 있다.

만약 5x(x+2)의 결과를 반환하는 함수가 필요하다면?

```javascript
const multiplyFive = multiplyX(5);
const equaion2 = compose(addTwo)(multiplyFive);
equation2(10) // (5 * 10) * (10 + 2) = 600
```

이렇게 표현할 수 있다.

마지막으로 위에서 선언한 equation 함수를 생성하면서 중복되는 부분을 제거하면,

```javascript
const addTwo = addX(2);
const addFour = addX(4);
const multiplyFour = multiplyX(4);
const multiplyFive = multiplyX(5);

const compose = fn => fn2 => x => fn2(x) * fn(x);
const composeAddTwo = compose(addTwo);

const equation1 = composeAddTwo(multiplyFour); // 4x(x+2)
const equation2 = composeAddTwo(multiplyFive); // 5x(x+2)
const equation3 = composeAddTwo(addFour) // (x+4)(x+2)
```

위와 같이 인자로 받은 함수들을 교체해주면서, 유사한 함수들을 여러개 만들 수 있다.

참고 - [Go devlog](https://gobae.tistory.com/139), [hustle-dev velog](https://velog.io/@hustle-dev/Javascript-%EC%BB%A4%EB%A7%81%EC%97%90-%EB%8C%80%ED%95%B4-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90#%EC%BB%A4%EB%A7%81%EC%9D%98-%ED%99%9C%EC%9A%A9)
