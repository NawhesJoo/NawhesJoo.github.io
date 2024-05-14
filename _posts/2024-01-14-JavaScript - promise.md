---
title: JavaScript - promise
description: >-
  
author: NawhesJoo
date: 2024-01-14 20:55:00 +0900
categories: [Language, JavaScript]
tags: [promise]
pin: true
math: true
mermaid: true
image:
  path: /thumbnails/JavaScript.png
  lqip: 
  alt: 
comments: true
---
## promise란?

> 프로미스는 자바스크립트 비동기 처리에 사용되는 객체다. 
>
>여기서 자바스크립트의 비동기 처리란 ‘특정 코드의 실행이 완료될 때까지 기다리지 않고 다음 코드를 먼저 수행하는 자바스크립트의 특성’을 의미한다.

---

### 특징

promise 객체는 아래 코드와 같이 만들 수 있다.

```javascript
let promise = new Promise(function(resolve, reject) {
  // executor (실행자, 실행함수)
});
```


+ new Promise에 전달되는 함수는 executor(실행자, 실행 함수) 라고 부른다.
+ executor는 new Promise가 만들어질 때 자동으로 실행된다.
+ executor의 인수 resolve와 reject는 자바스크립트에서 자체 제공하는 콜백이다.
+ resolve와 reject를 신경 쓰지 않고 executor 안 코드만 작성하면 된다.
+ executor에선 결과를 즉시 얻든 늦게 얻든 상관없이 상황에 따라 인수로 넘겨준 콜백 중 하나를 반드시 호출해야 한다.

+ resolve(value) : 일이 성공적으로 끝난 경우 그 결과를 나타내는 value와 함께 호출
+ reject(error) : 에러 발생 시 에러 객체를 나타내는 error와 함께 호출

> executor는 자동으로 실행되는데 여기서 원하는 일이 처리된다. 
처리가 끝나면 executor는 처리 성공 여부에 따라 resolve나 reject를 호출한다.

---

## promise의 상태(states)

new Promise 생성자가 반환하는 promise 객체는 다음과 같은 내부 프로퍼티를 갖는다.

>+ state — 처음엔 "pending"(보류)이었다 resolve가 호출되면 "fulfilled", reject가 호출되면 "rejected"로 변한다.
+ result — 처음엔 undefined이었다 resolve(value)가 호출되면 value로, reject(error)가 호출되면 error로 변한다.

![](https://velog.velcdn.com/images/nawhes_joo/post/6d4dec0c-2003-4bcc-a4d0-081f24a1a09c/image.png)

따라서 executor는 위 그림과 같이 promise의 상태를 둘 중 하나로 변화시킨다.

---

### Pending(대기)
```javascript
new Promise();
```
new Promise() 메서드를 호출하면 대기(Pending) 상태가 된다.

```javascript
new Promise(function(resolve, reject) {
  // ...
});
```
new Promise() 메서드를 호출할 때 콜백 함수를 선언할 수 있고, 콜백 함수의 인자는 resolve, reject이다.

---

### Fulfilled(이행)

```javascript
new Promise(function(resolve, reject) {
  resolve();
});
```

여기서 콜백 함수의 인자 resolve를 위와 같이 실행하면 이행(Fulfilled)상태가 된다.

```javascript
let promise = new Promise(function(resolve, reject) {
  // 프라미스가 만들어지면 executor 함수는 자동으로 실행

  // 1초 뒤에 일이 성공적으로 끝났다는 신호가 전달되면서 result는 '완료'가 됨
  setTimeout(() => resolve("완료"), 1000);
});
```

위 코드는 promise 생성자와 간단한 executor 함수로 만든 예시이다.

setTimeout을 이용해 executor 함수는 약간의 시간이 걸리도록 구현하였다.

여기서 알 수 있는 것은 두가지이다.

>1. executor는 new Promise에 의해 자동으로 그리고 즉각적으로 호출된다.
2. executor 인자로 resolve와 reject 함수를 받는다. 이 함수들은 자바스크립트 엔진이 미리 정의한 함수이므로 따로 만들 필요가 없다. 하지만 resolve와 reject 둘 중 하나는 반드시 호출해야 한다.

executor '처리’가 시작 된 지 1초 후, resolve("완료")이 호출되고 결과가 만들어집니다. 이때 promise 객체의 상태는 다음과 같이 변한다.

![](https://velog.velcdn.com/images/nawhes_joo/post/e5667c4a-141c-4ecf-a3f4-5581848c122d/image.png)

> 이처럼 일이 성공적으로 처리되었을 때의 프라미스는 'fulfilled promise(약속이 이행된 프라미스)'라고 불린다.

---

### Rejected(실패)

```javascript
let promise = new Promise(function(resolve, reject) {
  // 1초 뒤에 에러와 함께 실행이 종료되었다는 신호를 보냄
  setTimeout(() => reject(new Error("에러 발생!")), 1000);
});
```
1초 후 reject(...)가 호출되면 promise의 상태가 "rejected"로 변한다.

![](https://velog.velcdn.com/images/nawhes_joo/post/0497907f-b7cb-42c4-9f98-cc64befc7ec0/image.png)

---

### 요약

> executor는 보통 시간이 걸리는 일을 수행한다. 일이 끝나면 resolve나 reject 함수를 호출하는데, 이때 프라미스 객체의 상태가 변화한다.
이행(resolved) 혹은 거부(rejected) 상태의 프라미스는 ‘처리된(settled)’ promise라고 부른다. 반대되는 promise로 '대기(pending)'상태가 있다.

#### 성공 or 실패

executor는 resolve나 reject 중 하나를 반드시 호출해야 한다. 이때 변경된 상태는 더 이상 변하지 않는다.

처리가 끝난 프라미스에 resolve와 reject를 호출하면 무시된다.

```javascript
let promise = new Promise(function(resolve, reject) {
  resolve("완료");

  reject(new Error("…")); // 무시됨
  setTimeout(() => resolve("…")); // 무시됨
});
```

이렇게 executor에 의해 처리가 끝난 일은 결과 혹은 에러만 가질 수 있다.

여기에 더하여 resolve나 reject는 인수를 하나만 받고(혹은 아무것도 받지 않음) 그 이외의 인수는 무시한다는 특성도 있다.

---

#### Error

무언가 잘못된 경우, executor는 reject를 호출해야 한다. 이때 인수는 resolve와 마찬가지로 어떤 타입도 가능하지만 Error 객체 또는 Error를 상속받은 객체를 사용할 것을 추천한다.

---

#### resolve·reject 함수 즉시 호출하기

executor는 대개 무언가를 비동기적으로 수행하고, 약간의 시간이 지난 후에 resolve, reject를 호출하는데, 꼭 이렇게 할 필요는 없다. 아래와 같이 resolve나 reject를 즉시 호출할 수도 있다.

```javascript
let promise = new Promise(function(resolve, reject) {
  // 일을 끝마치는 데 시간이 들지 않음
  resolve(123); // 결과(123)를 즉시 resolve에 전달함
});
```

어떤 일을 시작했는데 알고 보니 일이 이미 끝나 저장까지 되어있는 경우, 이렇게 resolve나 reject를 즉시 호출하는 방식을 사용할 수 있다.

이렇게 하면 프라미스는 즉시 이행 상태가 된다.

---

#### state와 result

프라미스 객체의 state, result 프로퍼티는 내부 프로퍼티이므로 직접 접근할 수 없다. .then/.catch/.finally 메서드를 사용하면 접근 가능하다.

---

## then, catch, finally

> promise 객체는 executor와 결과나 에러를 받을 소비 함수를 이어주는 역할을 한다. 소비함수는 .then, .catch, .finally 메서드를 사용해 등록된다.

### then

.then은 promise에서 가장 중요하고 기본이 되는 메소드이다.

```javascript
promise.then(
  function(result) { /* 결과(result) */ },
  function(error) { /* 에러(error) */ }
);
```

.then의 인수들은 promise가 이행되었을 때 실행되는 함수이고
첫 번째 인수는 실행 결과, 두 번째 인수는 에러를 받는다.

---

#### 예시

```javascript
let promise = new Promise(function(resolve, reject) {
  setTimeout(() => resolve("완료!"), 1000);
});

// resolve 함수는 .then의 첫 번째 함수(인수)를 실행
promise.then(
  result => console.log(result), // 1초 후 "완료!"를 출력
  error => console.log(error) // 실행되지 않음
);
```

![](https://velog.velcdn.com/images/nawhes_joo/post/04e4211d-3b99-4d72-81ef-f15baeead948/image.png)

첫 번째 함수가 실행되었다.

---

```javascript
let promise = new Promise(function(resolve, reject) {
  setTimeout(() => reject(new Error("에러 발생!")), 1000);
});

// reject 함수는 .then의 두 번째 함수를 실행
promise.then(
  result => console.log(result), // 실행되지 않음
  error => console.log(error) // 1초 후 "Error: 에러 발생!"을 출력
);
```

![](https://velog.velcdn.com/images/nawhes_joo/post/f78f7d9b-7c10-4590-845d-1c31d2ca51be/image.png)

promise가 거부된 경우에는 두 번째 함수가 실행된다.

---

```javascript
let promise = new Promise(resolve => {
  setTimeout(() => resolve("완료!"), 1000);
});

promise.then(console.log); // 1초 뒤 "완료!" 출력
```

![](https://velog.velcdn.com/images/nawhes_joo/post/570d11ac-6c16-4cea-939d-69ce5c733705/image.png)

작업이 성공적으로 처리된 경우만 다루고 싶다면 .then에 인수를 하나만 전달하면 된다.

---

### catch

```javascript
let promise = new Promise((resolve, reject) => {
  setTimeout(() => reject(new Error("에러 발생!")), 1000);
});

// .catch(f)는 promise.then(null, f)과 동일하게 작동
promise.catch(console.log); // 1초 뒤 "Error: 에러 발생!" 출력
```

![](https://velog.velcdn.com/images/nawhes_joo/post/b287baff-10a2-479b-9773-98aa25fbc5bc/image.png)


에러가 발생한 경우만 다루고 싶다면 .then(null, errorHandlingFunction)같이 null을 첫 번째 인수로 전달하면 된다. 
.catch(errorHandlingFunction)를 써도 되는데, .catch는 .then에 null을 전달하는 것과 동일하게 작동한다.

> .catch(f)는 문법이 간결하다는 점만 빼고 .then(null,f)과 완벽하게 같다.

---

### finally

프라미스가 처리되면(이행이나 거부) f가 항상 실행된다는 점에서 .finally(f) 호출은 .then(f, f)과 유사하다.

>쓸모가 없어진 로딩 인디케이터(loading indicator)를 멈추는 경우같이, 결과가 어떻든 마무리가 필요하면 finally가 유용하다.


```javascript
new Promise((resolve, reject) => {
  /* 시간이 걸리는 어떤 일을 수행하고, 그 후 resolve, reject를 호출함 */
})
  // 성공·실패 여부와 상관없이 프라미스가 처리되면 실행됨
  .finally(() => 로딩 인디케이터 중지)
  .then(result => result와 err 보여줌 => error 보여줌)
```

---

#### 예시

```javascript
new Promise((resolve, reject) => {
  setTimeout(() => resolve("결과"), 2000)
})
  .finally(() => console.log("프라미스가 준비되었습니다."))
  .then(result => console.log(result)); // <-- .then에서 result를 다룰 수 있음
```

![](https://velog.velcdn.com/images/nawhes_joo/post/8dbfa046-b958-48d5-97d3-aa73a12855d9/image.png)

result가 finally를 걸쳐 then까지 전달되는 모습이다.

---

```javascript
new Promise((resolve, reject) => {
  throw new Error("에러 발생!");
})
  .finally(() => console.log("프라미스가 준비되었습니다."))
  .catch(err => console.log(err)); // <-- .catch에서 에러 객체를 다룰 수 있음
```
![](https://velog.velcdn.com/images/nawhes_joo/post/fc39e14e-9580-416d-b690-a1d8fae5fead/image.png)

result가 finally를 걸쳐 catch까지 전달되는 모습이다.


> finally는 promise 결과를 처리하기 위해 만들어 진 게 아니다.
promise 결과는 finally를 통과해서 전달된다. 이런 특징은 아주 유용하게 사용되기도 한다.

---
#### finally와 .then(f,f)

1. finally 핸들러엔 인수가 없다. finally에선 프라미스가 이행되었는지, 거부되었는지 알 수 없다. finally에선 절차를 마무리하는 ‘보편적’ 동작을 수행하기 때문에 성공·실패 여부를 몰라도 된다.

2. finally 핸들러는 자동으로 다음 핸들러에 결과와 에러를 전달한다.

---

## promise와 콜백

| <center>promise</center> |  <center>콜백</center> |
| :- |  :- |
| 프라미스를 이용하면 흐름이 자연스럽다. <br>스크립트를 읽고, 결과에 따라 그다음(.then)에 무엇을 할지에 대한 코드를 작성하면 된다. | 함께 호출할 callback 함수가 준비되어 있어야 한다.<br> 호출하기 이전에 호출 결과로 무엇을 할지 미리 알고 있어야 한다. |
| promise에 원하는 만큼 .then을 호출할 수 있다. | 콜백은 하나만 가능하다. |


---

## 테스트

### resolve, reject

```javascript
let promise = new Promise(function(resolve, reject) {
  console.log('hi');
});
```

promise 안에 동기 작업을 넣었다.

![](https://velog.velcdn.com/images/nawhes_joo/post/59477892-1c71-4186-80e4-6d5c837ac04f/image.png)

console.log는 정상적으로 수행되지만
promise의 상태는 여전히 pending(대기) 상태이다.

```javascript
let promise = new Promise(function(resolve, reject) {
  console.log("Promise 생성");

  // 동기 작업 수행, 그러나 resolve 또는 reject 호출하지 않음
});

promise.then(
  function(result) {
    console.log(result); // 실행되지 않음
  },
  function(error) {
    console.error(error); // 실행되지 않음
  }
);
```

![](https://velog.velcdn.com/images/nawhes_joo/post/c33eee11-15d6-443c-8a1e-ebd333fda2ab/image.png)

then과 catch 또한 실행되지 않는다.

resolve나 reject가 수행되지 않았기 때문이다.

---

이번에는 동기작업을 넣은 후 resolve를 호출해보자.

```javascript
let promise = new Promise(function(resolve, reject) {
  console.log("Promise 생성");

  // 동기 작업 수행
  let result = "동기 작업 완료";

  // resolve를 호출하여 Promise를 이행 상태로 변경
  resolve(result);
});

promise.then(function(result) {
  console.log(result); // "동기 작업 완료" 출력
}).catch(function(error) {
  console.error(error); // 실행되지 않음
});
```

![](https://velog.velcdn.com/images/nawhes_joo/post/79a43647-2500-4bd8-9e78-762873998dcc/image.png)

promise의 상태가 fulfilled(이행) 상태가 되었다.

---

이번엔 반대로 비동기 작업을 넣어보자.

```javascript
let promise = new Promise(function(resolve, reject) {
  console.log("Promise 생성");

  // 비동기 작업 시뮬레이션
  setTimeout(function() {
    console.log("비동기 작업 완료");
    // resolve 또는 reject 호출하지 않음
  }, 1000);
});

promise.then(function(result) {
  console.log(result); // 실행되지 않음
}).catch(function(error) {
  console.error(error); // 실행되지 않음
});

```
![](https://velog.velcdn.com/images/nawhes_joo/post/7ea1ba08-f6e6-4d8e-a12b-38bec18d11ba/image.png)

비동기 작업을 넣었지만 promise의 상태는 여전히 pending(대기)이다.

---

resolve를 호출해보자

```javascript
let promise = new Promise(function(resolve, reject) {
  console.log("Promise 생성");

  // 비동기 작업 시뮬레이션
  setTimeout(function() {
    console.log("비동기 작업 완료");

    // resolve 호출하여 Promise를 이행 상태로 변경
    resolve("비동기 작업 완료");
  }, 1000);
});

promise.then(function(result) {
  console.log(result); // "비동기 작업 완료" 출력
}).catch(function(error) {
  console.error(error); // 실행되지 않음
});
```

![](https://velog.velcdn.com/images/nawhes_joo/post/97b465f6-56ff-4400-a133-f3f46f5ccea9/image.png)

resolve를 호출하니 fulfilled 상태가 되었다.

---

```javascript
let promise = new Promise(function(resolve, reject) {
  console.log("Promise 생성");

  // 비동기 작업 시뮬레이션
  setTimeout(function() {
    console.log("비동기 작업 실패");

    // reject 호출하여 Promise를 거부 상태로 변경
    reject("비동기 작업 실패");
  }, 1000);
});

promise.then(function(result) {
  console.log(result); // 실행되지 않음
}).catch(function(error) {
  console.error(error); // "비동기 작업 실패" 출력
});
```

![](https://velog.velcdn.com/images/nawhes_joo/post/6ccc48bc-3673-473c-a5b0-149b5f986f66/image.png)



> 동기 작업이든, 비동기 작업이든 resolve 또는 reject를 호출하지 않으면 pending(대기) 상태이다.

---

### promise에 promise

```javascript
let outerPromise = new Promise(function(resolve, reject) {
  console.log("Outer Promise 생성");

  // 내부 Promise 생성
  let innerPromise = new Promise(function(innerResolve, innerReject) {
    console.log("Inner Promise 생성");

    // 비동기 작업 시뮬레이션 (성공 상황)
    setTimeout(function() {
      console.log("Inner Promise 성공");

      // 내부 Promise를 이행 상태로 변경
      innerResolve("내부 Promise 성공");
    }, 1000);
  });

  // 내부 Promise의 상태에 따라 외부 Promise도 처리
  innerPromise.then(function(innerResult) {
    console.log(innerResult); // "내부 Promise 성공" 출력
    // 외부 Promise를 이행 상태로 변경
    resolve("Outer Promise 성공");
  }).catch(function(innerError) {
    console.error(innerError); // 실행되지 않음
    // 외부 Promise를 거부 상태로 변경
    reject("Outer Promise 실패");
  });
});

outerPromise.then(function(outerResult) {
  console.log(outerResult); // "Outer Promise 성공" 출력
}).catch(function(outerError) {
  console.error(outerError); // 실행되지 않음
});
```

![](https://velog.velcdn.com/images/nawhes_joo/post/5198761c-0be6-4d97-a301-fd9a925a6ccf/image.png)

내부 promise를 실행한 후에 외부 promise가 실행되는 것을 볼 수 있다.

> promise 안에 promise를 넣을 수 있다.

---

위에서는 외부와 내부 둘 다 resolve를 호출하였다.

아래 코드는 내부만 resolve를 호출하고, 외부에는 reject를 호출한다.

```javascript
let outerPromise = new Promise(function(resolve, reject) {
  console.log("Outer Promise 생성");

  // 내부 Promise 생성
  let innerPromise = new Promise(function(innerResolve, innerReject) {
    console.log("Inner Promise 생성");

    // 비동기 작업 시뮬레이션 (성공 상황)
    setTimeout(function() {
      console.log("Inner Promise 성공");

      // 내부 Promise를 이행 상태로 변경
      innerResolve("내부 Promise 성공");
    }, 1000);
  });

  // 내부 Promise의 상태에 따라 외부 Promise도 처리
  innerPromise.then(function(innerResult) {
    console.log(innerResult); // "내부 Promise 성공" 출력
    // 외부 Promise를 거부 상태로 변경
    reject("Outer Promise 실패");
  }).catch(function(innerError) {
    console.error(innerError); // 실행되지 않음
    // 외부 Promise에는 아무 것도 호출하지 않음
  });
});

outerPromise.then(function(outerResult) {
  console.log(outerResult); // 실행되지 않음
}).catch(function(outerError) {
  console.error(outerError); // "Outer Promise 실패" 출력
});
```

![](https://velog.velcdn.com/images/nawhes_joo/post/ac8fbf27-093b-48a1-8da6-a79612452762/image.png)


내부는 resolve, 외부는 reject가 수행된 것을 확인할 수 있다.

> 내부 promise와 외부 promise는 독립적으로 작동한다.

promise를 찍었을 때 fulfilled(이행) 상태인 이유는
내부에서 resolve를 호출하여 fulfilled(이행) 상태가 되었기 때문에
'변경된 상태는 더 이상 변하지 않는다'라는 promise의 성질에 의해 reject가 아닌 fulfilled 상태인 것이다.



---

