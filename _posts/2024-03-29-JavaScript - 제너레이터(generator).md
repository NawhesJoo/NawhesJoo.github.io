---
title: JavaScript - 제너레이터 (generator)
description: >-
  
author: NawhesJoo
date: 2024-03-29 20:55:00 +0900
categories: [Language, JavaScript]
tags: [generator]
pin: false
math: true
mermaid: true
image:
  path: /thumbnails/JavaScript.png
  lqip: 
  alt: 
comments: true
---
## 제너레이터란?

일반 함수는 하나의 값(혹은 0개의 값)만을 반환합니다.

> 하지만 제너레이터(generator)를 사용하면 여러 개의 값을 필요에 따라 하나씩 변환(yield)할 수 있습니다.

제너레이터와 이터러블 객체를 함께 사용하면 손쉽게 데이터 스트림을 만들 수 있습니다.

---

## 제너레이터 함수

제너레이터를 만들려면 '제너레이터 함수'라 불리는 특별한 문법 구조, `function*` 이 필요합니다.

예시 )

```javascript
function* generateSequence(){
  yield 1;
  yield 2;
  return 3;
}
```

---

제너레이터 함수는 일반 함수와 동작 방식이 다릅니다. 제너레이터 함수를 호출하면 코드가 실행되지 않고, 대신 실행을 처리하는 특별 객체, `제너레이터 객체`가 변환됩니다.

예시)

```javascript
function* generateSequence(){
  yield 1;
  yield 2;
  return 3;
}

// '제너레이터 함수'는 '제너레이터 객체'를 생성합니다.
let generator = generateSequence();
console.log(generator); // [Object Generator]
```

함수 본문 코드는 아직 실행되지 않았습니다.

---

`next()`는 제너레이터의 주요 메서드입니다.

`next()`를 호출하면 가장 가까운 `yield <value>`문을 만날 때까지 실행이 지속됩니다.(value를 생략할 수도 있는데, 이 경우엔 `undefined`가 됩니다)

이후, `yield <value>`문을 만나면 실행이 멈추고 산출하고자 하는 값인 `value`가 바깥 코드에 반환됩니다.


`next()`는 항상 아래 두 프로퍼티를 가진 객체를 반환합니다.
+ `value` : 산출 값
+ `done` : 함수 코드 실행이 끝났으면 `true`, 아니라면 `false`

---

제너레이터를 만들고 첫 번째 산출 값을 받는 예시를 살펴봅시다.

```javascript
function* generateSequence() {
  yield 1;
  yield 2;
  return 3;
}

let generator = generateSequence();

let one = generator.next();

console.log(JSON.stringify(one)); // {value: 1, done: false}
```
![](https://velog.velcdn.com/images/nawhes_joo/post/9e68fb4c-cac8-4415-a2fc-1e49cafcd323/image.png)

첫 번째 값만 받았으므로 함수 실행은 두 번째 줄에서 멈췄습니다.



```javascript
let two = generator.next();

console.log(JSON.stringify(two)); // {value: 2, done: false}
```

![](https://velog.velcdn.com/images/nawhes_joo/post/3ded4e98-934b-4c18-a833-6d042fd4a1e2/image.png)

`generator.next()`를 다시 호출해봅시다. 실행이 재개되고 다음 `yield`를 반환합니다.

---

```javascript
let three = generator.next();

console.log(JSON.stringify(three)); // {value: 3, done: true}
```
![](https://velog.velcdn.com/images/nawhes_joo/post/2652e350-c082-42e1-83da-3999b8cf743e/image.png)

`generator.next()`를 또 호출하면 실행은 `return`문에 다다르고 함수가 종료됩니다.

마지막 결과인 `value : 3`과 `done:trye`를 통해 종료된 것을 확인할 수 있습니다.

---

```javascript
let four = generator.next();

console.log(JSON.stringify(four));
```

![](https://velog.velcdn.com/images/nawhes_joo/post/a259e086-e54e-4bf0-961d-1bfd8f01b67c/image.png)

제너레이터가 종료되었기 때문에 지금 상태에선 `generator.next()`를 호출해도 아무 소용이 없습니다.

`generator.next()`를 여러번 호출해도 객체 `{done: true}`가 반환될 뿐입니다.

---

## 제너레이터와 이터러블

`next()` 메서드를 보면서 짐작했듯이, 제너레이터는 이터러블입니다.

따라서 `for..of` 반복문을 사용해 값을 얻을 수 있습니다.

```javascript
function* generateSequence(){
  yield 1;
  yield 2;
  return 3;
}

let generator = generateSequence();

for(let value of generator){
  console.log(value);
}
```

![](https://velog.velcdn.com/images/nawhes_joo/post/209ef3bc-e9fa-42f8-a49e-94fbc149f130/image.png)

1, 2만 출력되고 3은 출력되지 않습니다.

그 이유는 `for..of` 이터레이션이 `done: true` 일 때 마지막 `value`를 무시하기 때문입니다. 그러므로 `for..of`를 사용했을 때 모든 값이 출력되길 원한다면 `yield`로 값을 반환해야 합니다.

---
```javascript
function* generateSequence(){
  yield 1;
  yield 2;
  yield 3;
}

let generator = generateSequence();

for(let value of generator){
  console.log(value);
}
```

![](https://velog.velcdn.com/images/nawhes_joo/post/7f706cf1-9a0f-4d4e-89ea-f3697d35dc20/image.png)

모든 값이 정상적으로 반환되는 모습

---

제너레이터는 이터러블 객체이므로 제너레이터에도 전개 구문(...)같은 관련 기능을 사용할 수 있습니다.

```javascript
function* generateSequence(){
  yield 1;
  yield 2;
  yield 3;
}

let sequence = [0, ...generateSequence()];

console.log(sequence)
```

![](https://velog.velcdn.com/images/nawhes_joo/post/7af11143-4502-4442-9e4c-59f38c8c7bc0/image.png)


위 예시에서 `...generateSequence()`는 반복 가능한 제너레이터 객체를 배열 요소로 바꿔줍니다.

---

## 이터러블 대신 제너레이터 사용하기

`from..to` 사이에 있는 값을 반환하는 반복 가능 객체, `range`를 만들어 보았습니다.

```javascript
let range = {
  from: 1,
  to: 5,

  // for..of 최초 호출 시, Symbol.iterator가 호출됩니다.
  [Symbol.iterator]() {
    // Symbol.iterator는 이터레이터 객체를 반환합니다.
    // for..of는 반환된 이터레이터 객체만을 대상으로 동작하는데, 이때 다음 값도 정해집니다.
    return {
      current: this.from,
      last: this.to,

      // for..of 반복문에 의해 각 이터레이션마다 next()가 호출됩니다.
      next() {
        // next()는 객체 형태의 값, {done:.., value :...}을 반환해야 합니다.
        if (this.current <= this.last) {
          return { done: false, value: this.current++ };
        } else {
          return { done: true };
        }
      }
    };
  }
};

// 객체 range를 대상으로 하는 이터레이션은 range.from과 range.to 사이의 숫자를 출력합니다.
console.log([...range]); // 1,2,3,4,5
```

`Symbol.iterator` 대신 제너레이터 함수를 사용하면, 제너레이터 함수로 반복이가능합니다.

같은 `range`이지만, 좀 더 압축된 `range`를 살펴봅시다.

```javascript
let range = {
  from: 1,
  to: 5,
  
  *[Symbol.iterator]() { // [Symbol.iterator]: function*()를 짧게 줄임
    for(let value = this.from; value <= this.to; value++){
      yield value;
    }
  }
};

console.log([...range]);
```

![](https://velog.velcdn.com/images/nawhes_joo/post/9189c920-eb00-429d-b18c-fe2ff4584c2e/image.png)

`range[Symbol.iterator]()`는 제너레이터를 반환하고, 제너레이터 메서드는 `for..of`가 동작하는데 필요한 사항(아래 설명)을 충족시키므로 예시가 잘 동작합니다.

+ `.next()` 메서드가 있음
+ 반환 값의 형태는 `{value: ..., done: true/false}` 이어야 함

이렇게 이터러블 객체 대신 제너레이터를 사용할 수 있는 것은 우연이 아닙니다. 제너레이터는 이터레이터를 어떻게 하면 쉽게 구현할지를 염두에 두며 자바스크립트에 추가되었습니다.

제너레이터를 사용해 구현한 예시는 이터러블을 사용해 구현한 기존 예시보다 훨씬 간결합니다. 그리고 동일한 기능을 제공합니다.

---

## 제너레이터 컴포지션



제너레이터 컴포지션(generator composition)은 제너레이터 안에 제너레이터를 '임베딩(embedding, composing)'할 수 있게 해주는 제너레이터의 특별 기능입니다.

먼저, 연속된 숫자를 생성하는 제너레이터 함수를 만들어보겠습니다.

```javascript
function* generateSequence(start, end) {
  for(let i = start; i <= end; i++) yield i;
}
```

그리고 이 함수를 기반으로 좀 더 복잡한 값을 연속해서 생성하는 함수를 만들어봅시다. 

>+ 숫자 0부터 9까지 생성(문자 코드 48부터 57까지)
+ 알파벳 대문자 A부터 Z까지 생성(문자 코드 65부터 90까지)
+ 알파벳 소문자 a부터 z까지 생성(문자코드 97부터 122까지)

이런 규칙을 충족하는 연속 값은 비밀번호를 만들 때 응용할 수 있습니다.(특수 문자도 추가 가능)

일반 함수로는 여러 개의 함수를 만들고 그 호출 결과를 어딘가에 저장한 후 다시 그 결과들을 조합해야 원하는 기능을 구현할 수 있습니다.

하지만 제너레이터의 특수 문법 `yield*`를 사용하면 제너레이터를 다른 제너레이터에 '끼워 넣을 수' 있습니다.

**컴포지션을 적용한 제너레이터**

```javascript
function* generateSequence(start, end) {
  for(let i = start; i <= end; i++) yield i;
}

function* generatePasswordCodes() {
  
  // 0~9
  yield* generateSequence(48, 57);
  
  // A~Z
  yield* generateSequence(65, 90);
  
  // a~z
  yield* generateSequence(97, 122);
  
}

let str = '';

for(let code of generatePasswordCodes()) {
  str += String.fromCharCode(code);
}

console.log(str);
```

![](https://velog.velcdn.com/images/nawhes_joo/post/6a31efb7-673c-48ae-b68f-df99b27b1c64/image.png)

순서대로 0~9, A~Z, a~z가 출력되는 모습

`yield*` 지시자는 실행을 다른 제너레이터에 위임합니다(delegate). 여기서 '위임'은 `yield* gen`이 제너레이터 `gen`을 대상으로 반복을 수행하고, 산출 값들을 바깥으로 전달한다는 것을 의미합니다. 마치 외부 제너레이터에 의해 값이 선출된 것처럼 말이죠.

중첩 제너레이터의 코드를 직접 써줘도 결과는 같습니다.

```javascript
function* generateSequence(start, end) {
  for (let i = start; i <= end; i++) yield i;
}

function* generateAlphaNum() {

  // yield* generateSequence(48, 57);
  for (let i = 48; i <= 57; i++) yield i;

  // yield* generateSequence(65, 90);
  for (let i = 65; i <= 90; i++) yield i;

  // yield* generateSequence(97, 122);
  for (let i = 97; i <= 122; i++) yield i;

}

let str = '';

for(let code of generateAlphaNum()) {
  str += String.fromCharCode(code);
}

console.log(str); // 0..9A..Za..z
```

![](https://velog.velcdn.com/images/nawhes_joo/post/5b59a974-dd8c-42db-8f06-a7c3cbcbad03/image.png)

같은 결과가 나오는 모습을 확인할 수 있습니다.

제너레이터 컴포지션을 사용하면 한 제너레이터의 흐름을 자연스럽게 다른 제너레이터에 삽입할 수 있습니다. 제너레이터 컴포지션을 사용하면 중간 결과 저장 용도의 추가 메모리가 필요하지 않습니다.

---

## 'yield'를 사용해 제너레이터 안/밖으로 정보 교환하기

제너레이터는 값을 생성해주는 특수 문법을 가진 이터러블 객체와 유사했습니다. 그러나 사실 제너레이터는 더 강력하고 유연한 기능을 제공합니다.

`yield`가 양방향 길과 같은 역할을 하기 때문입니다. `yield`는 결과를 바깥으로 전달할 뿐만 아니라 값을 제너레이터 안으로 전달하기까지 합니다.

값은 안/밖으로 전달하려면 `generator/next(arg)`를 호출해야 합니다. 이때 인수 `arg`는 `yield`의 결과가 됩니다.

```javascript
function* gen() {
  // 질문을 제너레이터 밖 코드로 던지고 답을 기다립니다.
  let result = yield "2 + 2 = "; // (*)
  
  console.log(result);
}

let generator = gen();

let question = generator.next().value;  // <-- yield는 value를 반환합니다.

generator.next(4); // --> 결과를 제너레이터 안으로 전달합니다.
```

![](https://velog.velcdn.com/images/nawhes_joo/post/546e452d-410f-4020-8a73-d4dbec70a129/image.png)

1. `generator.next()`를 처음 호출할 땐 항상 인수가 없어야 합니다. 인수가 넘어오더라도 무시되어야 합니다.
`generator.next()`를 호출하면 실행이 시작되고 첫 번째 `yield "2+2=?"`의 결과가 반환됩니다. 이 시점에는 제너레이터가 `(*)`로 표시한 줄에서 실행을 잠시 멈춥니다.

2. 그 후, 위 그림에서 보듯이 `yield`의 결과가 제너레이터를 호출하는 외부 코드에 있는 변수, `question`에 할당됩니다.

3. 마지막 줄, `generator.next(4)`에서 제너레이터가 다시 시작되고 `4`는 `result`에 할당됩니다(`let result = 4`)

외부 코드에선 `next(4)`를 즉시 호출하지 않고 있습니다. 제너레이터가 기다려주기 때문에 호출을 나중에 해도 문제가 되지 않습니다.

예시 :
```javascript
// 일정 시간이 지난 후에 제너레이터가 다시 시작됨
setTimeout(() => generator.next(4), 1000);
```

---


일반 함수와 다르게 제너레이터와 외부 호출 코드는 `next/yield`를 이용해 결과를 전달 및 교환합니다.

예시 : 
```javascript
function* gen() {
  let ask1 = yield "2 + 2 = ?";
  
  console.log(ask1); // 4
  
  let ask2 = yield "3 * 3 = ?";
  
  console.log(ask2); // 9
}

let generator = gen();

console.log( generator.next().value ); // "2 + 2 = ?"

console.log( generator.next(4).value ); // "3 + 3 = ?"

console.log( generator.next(9).done ); // true
```

![](https://velog.velcdn.com/images/nawhes_joo/post/88eda2f7-7abc-48cf-8952-016b2f538635/image.png)

실행 흐름을 나타낸 그림은 다음과 같습니다.

![](https://velog.velcdn.com/images/nawhes_joo/post/80c99e73-3b4b-4ada-926b-56296477ef4a/image.png)


1. 제너레이터 객체가 만들어지고 첫 번째 `.next()`가 호출되면, 그 실행이 시작되고 첫 번째 `yield`에 다다릅니다.

2. 산출 값은 바깥 코드로 반환됩니다.

3. 두 번째 `.next(4)`는 첫 번째 `yield`의 결과가 될 `4`를 제너레이터 안으로 전달합니다. 그리고 다시 실행이 이어집니다.

4. 실행 흐름이 두 번째 `yield`에 다다르고, 산출값(`"3 * 3 = ?"`)이 제너레이터 호출 결과가 됩니다.

5. 세 번째 `next(9)`는 두 번째 `yield`의 결과가 될 `9`를 제너레이터 안으로 전달합니다. 그리고 다시 실행이 이어지는데, `done: true`이므로 제너레이터 함수는 종료됩니다.

첫 번째 `next()`를 제외한 각 `next(value)`는 현재 `yield`의 결과가 될 값을 제너레이터 안에 전달합니다. 그리고 다음 `yield`의 결과로 되돌아옵니다.

---

## generator.throw

`yield`의 결과가 될 값을 제너레이터 안에 전달하기도 합니다.

그런데 외부 코드가 에러를 만들거나 던질 수 있습니다. 에러는 결과의 한 종류이기 때문에 이는 자연스러운 현상입니다.

에러를 `yield` 안으로 전달하려면 `generator.throw(err)`를 호출해야 합니다. `generator.throw(err)`를 호출하게 되면 `err`는 `yield`가 있는 줄로 던져집니다.