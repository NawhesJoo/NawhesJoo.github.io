---
title: JavaScript - 맵과 셋
description: >-
  
author: NawhesJoo
date: 2024-04-15 20:55:00 +0900
categories: [Language, JavaScript]
tags: [map, set]
pin: false
math: true
mermaid: true
image:
  path: /thumbnails/JavaScript.png
  lqip: 
  alt: 
comments: true
---
## Map

맵(map)은 키가 있는 데이터를 저장한다는 점에서 `객체`와 유사하다. 다만, `맵`은 키에 다양한 자료형을 허용한다는 차이가 있다.

맵에는 다음과 같은 주요 프로퍼티가 있다.
+ `new Map()` - 맵을 만든다.
+ `map.set(key, value)` - `key`를 이용해 `value`를 저장
+ `map.get(key)` - `key`에 해당하는 값을 반환. `key`가 존재하지 않으면 `undefined`를 반환
+ `map.has(key)` - `key`가 존재하면 `true`, 존재하지 않으면 `false`를 반환
+ `map.delete(key)` - `key`에 해당하는 값을 삭제
+ `map.clear()` - 맵 안의 모든 요소를 제거
+ `map.size` - 요소의 개수를 반환

---

예시:

```javascript
let map = new Map();

map.set('1', 'str1');	// 문자형 키
map.set(1, 'num1'); 	// 숫자형 키
map.set(true, 'bool1'); // 불린형 키

alert( map.get(1) );	// 'num1'
alert( map.get('1') );// 'str1'

alert( map.size );	// 3
```

맵은 객체와 달리 키를 문자형으로 반환하지 않는다. 키엔 자료형 제약이 없다.

---

>맵은 키로 객체를 허용한다.

```javascript
let john = { name: "John" };

let visitsCountMap = new Map();

visitsCountMap.set(john, 123);

alert( visitsCountMap.get(john) ); // 123
```

객체를 키로 사용할 수 있다는 점은 `맵`의 가장 중요한 기능 중 하나이다. `객체`에는 문자열 키를 사용할 수 있다. 하지만 객체 키는 사용할 수 없다.

객체형 키를 `객체`에 써보자.

```javascript
let john = { name: "John" };

let visitsCountObj = {};

visitsCountObject[john] = 123;

alert( visitsCountObj["[object Object]"] ); //123
```

`visitsCountObj`는 객체이기 때문에 모든 키를 문자형으로 변환시킨다. 이 과정에서 `john`은 문자형으로 변환되어 `"[object Object]"`가 된다.

---

## 맵의 요소에 반복 작업하기

다음 세 가지 메서드를 사용해 `맵`의 각 요소에 반복 작업을 할 수 있다.

+ `map.keys()` - 각 요소의 키를 모은 반복 가능한(iterable, 이터러블) 객체를 반환한다.
+ `map.values()` - 각 요소의 값을 모은 이터러블 객체를 반환한다.
+ `map.entries()` - 요소의 `[키, 값]`을 한 쌍으로 하는 이터러블 객체를 반환한다. 이 이터러블 객체는 `for..of` 반복문의 기초로 쓰인다.

예시: 

```javascript
let recipeMap = new Map([
  ['cucumber', 500],
  ['tomatoes', 350],
  ['onion',	50]
]);

// 키(vegetable)를 대상으로 순회
for (let vegetable of recipeMap.keys()) {
	alert(vegetable);	// cucumber, tomatoes, onion
}

// 값(amount)을 대상으로 순회
for (let amount of recipeMap.values()) {
	alert(amount);	// 500, 350, 50
}

// [키, 값] 쌍을 대상으로 순회
for (let entry of recipeMap) { // recipeMap.entries()와 동일
	alert(entry);	// cucumber,500 ...
}
```

---

## Object.entries: 객체를 맵으로 바꾸기

각 요소가 키-값 쌍인 배열이나 이터러블 객체를 초기화 용도로 `맵`에 전달해 새로운 `맵`을 만들 수 있다.

```javascript
//각 요소가 [키, 값] 쌍인 배열
let map = new Map([
  ['1', 'str1'],
  [1, 'num1'],
  [true, 'bool1']
  ]);

alert(map.get('1') ); // str1
```

평범한 객체를 가지고 `맵`을 만들고 싶다면 내장 메서드 Object.entries(obj)를 활용해야 한다.
이 메서드는 객체의 키-값 쌍을 요소(`[key, value]`)로 가지는 배열을 반환한다.


```javascript
let obj = {
  name: "John",
  age: 30
};

let map = new Map(Object.entries(obj)
alert( map.get('name') ); // John
```

`Object.entries`를 사용해 객체 `obj`를 배열 `[ ["name", "John"], ["age", 30] ]`로 바꾸고, 이 배열을 이용해 사로운 `맵`을 만들었다.

---

## Object.fromEntries: 맵을 객체로 바꾸기

`Object.fromeEntries`를 사용하여 맵을 객체로 바꿀 수 있다.
이 메서드는 각 요소가 `[키, 값]`쌍인 배열을 객체로 바꿔준다.

```javascript
let prices = Object.fromEntries([
  ['banana', 1],
  ['orange', 2],
  ['meat', 4]
]);

// now prices = { banana: 1, orange: 2, meat: 4 }

alert(prices.orange}; // 2
```

`Object.fromEntries`를 사용해 `맵`을 객체로 바꿔보자.

자료가 `맵`에 저장되어있는데, 서드파티 코드에서 자료를 객체형태로 넘겨받길 원할 때 이 방법을 사용할 수 있다.

```javascript
let map = new Map();
map.set('banana', 1);
map.set('orange', 2);
map.set('meat', 4);

let obj = Object.fromEntries(map.entries()); // 맵을 일반 객체로 변환(*)

// 맵이 겍체가 되었다.

// obj = { banana: 1. orange: 2, meat: 4 }

alert(obj.orange); // 2
```

`map.entries()`를 호출하면 맵의 `[키, 값]`을 요소로 가지는 이터러블을 반환한다. `Object.fromEntries`를 사용하기 위해 딱 맞는 형태이다.

`(*)`로 표시한 줄을 좀 더 짧게 줄이는 것도 가능하다.

```javascript
let obj = Object.fromEntries(map); // .entries()를 생략함
```

`Object.fromEntries`는 인수로 이터러블 객체를 받기 때문에 짧게 줄인 코드도 이전 코드와 동일하게 작동한다. 꼭 배열을 전달해줄 필요는 없다. 그리고 `map`에서의 일반적인 반복은 `map.entries()`를 사용했을 때와 같은 키-값 쌍을 반환한다. 따라서 `map`과 동일한 키-값을 가진 일반 객체를 얻게 된다.

---

## 셋

`셋(set)`은 중복을 허용하지 않는 값을 모아놓은 틀별한 컬렉션이다. 셋에 키가 없는 값이 저장된다.

주요 메서드는 다음과 같다.

+ `new Set(iterable)` - 셋을 만든다. `이터러블` 객체를 전달받으면(대게 배열을 전달받음) 그 안의 값을 복사해 셋에 넣어준다.
+ `set.add(value)` - 값을 추가하고 셋 자신을 반환한다.
+ `set.delete(value)` - 값을 제거한다. 호출 시점에 셋 내에 값이 있어서 제거에 성공하면 `true`, 아니면 `false`를 반환한다.
+ `set.has(value)` - 셋 내에 값이 존재하면 `true`, 아니면 `false`를 반환한다.
+ `set.clear()` - 셋을 비운다.
+ `set.size` - 셋에 몇 개의 값이 있는지 세어준다.

셋 내에 동일한 값(value)이 있다면 `set.add(value)`을 아무리 많이 호출하더라도 아무런 반응이 없을 것이다. 셋 내의 값에 중복이 없는 이유가 바로 이 때문이다.

방문자 방명록을 만든다고 가정해보자. 한 방문자가 여러 번 방문해도 반문자를 중복해서 기록하지 않겠다고 결정을 내린 상황이다. 즉, 한 방문자는 '단 한 번만 기록'되어야 한다.

이때 적합한 자료구조가 바로 `셋`이다.

```javascript
let set = new Set();

let john = { name: "John" };
let pete = { name: "Pete" };
let mary = { name: "Mary" };

// 어떤 고객(john, mary)은 여러 번 방문할 수 있다.
set.add(john);
set.add(pete);
set.add(mary);
set.add(john);
set.add(mary);

// 셋에는 유일무이한 값만 저장된다.
alert( set.size );	// 3

for (let user of set) {
  alert( user.name );	// John, Pete, Mary 순으로 출력
}
```

`셋` 대신 배열을 사용하여 방문자 정보를 저장한 후, 중복 값 여부는 배열 메서드인 `arr.find`를 이용해 확인할 수도 있다. 하지만 `arr.find`는 배열 내 요소 전체를 뒤져 중복 값을 찾기 때문에, 셋보다 성능 면에서 떨어진다. 반면, `셋`은 값의 유일무이함을 확인하는데 최적화되어있다.

---

## 셋의 값에 반복 작업하기

`for..of`나 `forEach`를 사용하면 셋의 값을 대상으로 반복 작업을 수행할 수 있다.

```javascript
let set = new Set(["oranges", "apples", "bananas"]);

for (let value of set) alert( value );

// forEach를 사용해도 동일하게 동작한다.
set.forEach((value, valueAgain, set) => {
  alert( value );
});
```

`forEach`에 쓰인 콜백 함수는 세 개의 인수를 받는데, 첫 번째는 `값`, 두 번째도 같은 값인 `valueAgain`을 받고 있다. 세 번째는 목표하는 객체(셋)이다. 동일한 값이 인수에 두 번 등장하고 있다.

이렇게 구현된 이유는 `맵`과의 호환성 때문이다. `맵`의 `forEach`에 쓰인 콜백이 세 개의 인수를 받을 때를 위해서이다. `맵`을 `셋`으로 혹은 `셋`을 `맵`으로 교체하기가 쉽다.

`셋`에서도 `맵`과 마찬가지로 반복 작업을 위한 메서드가 있다.

+ `set.keys()` - 셋 내의 모든 값을 포함하는 이터러블 객체를 반환한다.
+ `set.values()` - `set.keys`와 동일한 작업을 한다. `맵`과의 호환성을 위해 만들어진 메서드이다.
+ `set.entries()` - 셋 내의 각 값을 이용해 만든 `[value, value]` 배열을 포함하는 이터러블 객체를 반환한다. `맵`과의 호환성을 위해 만들어졌다.

---

## 요약

`맵`은 키가 있는 값이 저장된 컬렉션이다.

주요 메서드와 프로퍼티 : 
+ `new Map([iterable])` - 맵을 만든다. `[key, value]` 쌍이 있는 `iterable`(예: 배열)을 선택적으로 넘길 수 있는데, 이때 넘긴 이터러블 객체는 맵 초기화에 사용된다.
+ `map.set(key, value)` - 키를 이용해 값을 저장한다.
+ `map.get(key)` - 키에 해당하는 값을 반환한다. `key`가 존재하지 않으면 `undefined`를 반환한다.
+ `map.has(key)` - 키가 있으면 `true`, 없으면 `false`를 반환한다.
+ `map.delete(key)` - 키에 해당하는 값을 삭제한다.
+ `map.clear()` - 맵 안의 모든 요소를 제거한다.
+ `map.size` - 요소의 개수를 반환한다.

일반적인 `객체`와의 차이점:

+ 키의 타입에 제약이 없다. 객체도 키가 될 수 있다.
+ `size` 프로퍼티 등의 유용한 케서드나 프로퍼티가 있다.

---

`셋`은 중복이 없는 값을 저장할 때 쓰이는 컬렉션이다.

주요 메서드와 프로퍼티 :

+ `new Set([iterable])` - 셋을 만든다. `iterable` 객체를 선택적으로 전달받을 수 있는데(대개 배열을 전달받음), 이터러블 객체 안의 요소는 셋을 초기화하는데 쓰인다.
+ `set.add(value)` - 값을 추가하고 셋 자신을 반환한다. 셋 내에 이미 `value`가 있는 경우 아무런 작업을 하지 않는다.
+ `set.delete(value)` - 셋 내에 값이 존재하면 `true`, 아니면 `false`를 반환한다.
+ `set.clear()` - 셋을 비운다.
+ `set.size` - 셋에 몇 개의 값이 있는지 세어준다.

`맵`과 `셋`에 반복 작업을 할 땐, 해당 컬렉션에 요소나 값을 추가한 순서대로 반복 작업이 수행된다. 따라서 이 두 컬렉션은 정렬이 되어있지 않다고 할 수 없다. 그렇지만 컬렉션 내 요소나 값을 재정렬하거나 (배열에서 인덱스를 이용해 요소를 가져오는 것처럼) 숫자를 이용해 특정 요소나 값을 가지고 오는 것은 불가능하다.



※ 출처 - 모던 JavasScript (https://ko.javascript.info/map-set#ref-1160)