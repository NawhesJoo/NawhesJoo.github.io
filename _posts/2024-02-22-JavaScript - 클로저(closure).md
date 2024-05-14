---
title: JavaScript - 클로저 (closure)
description: >-
  
author: NawhesJoo
date: 2024-02-22 20:55:00 +0900
categories: [Language, JavaScript]
tags: [closure, scope]
pin: true
math: true
mermaid: true
image:
  path: /thumbnails/JavaScript.png
  lqip: 
  alt: 
comments: true
---
## 클로저란?

>클로저(closure)는 내부함수가 외부함수의 맥락에 접근할 수 있는 것을 가르킨다.
클로저는 자바스크립트를 이용한 고난이도의 테크닉을 구사하는데 필수적인 개념으로 활용된다.

---

## 내부함수

자바스크립트는 함수 안에서 또 다른 함수를 선언할 수 있다.

```javascript
function outer(){ // 외부함수
	function inner(){ // 내부함수
    	var title = 'coding everyday'; // 내부함수에 정의되어있는 지역변수
     	console.log(title);
    }
  inner();
}
outer();
```

![](https://velog.velcdn.com/images/nawhes_joo/post/5c3af3e3-5222-4db5-b87b-974bbef1e507/image.png)



함수 outer의 내부에는 함수 inner가 정의되어있다. 함수 inner를 내부함수라고 한다.

내부함수는 외부함수의 지역변수에 접근할 수 있다.

---

```javascript
function outer(){ // 외부함수
  	var title = 'coding everyday'; // 외부함수에 정의되어있는 지역변수
	function inner(){ // 내부함수
     	console.log(title);
    }
  inner();
}
outer();
```

![](https://velog.velcdn.com/images/nawhes_joo/post/3d7fd66d-95c4-4581-bedc-5f7e7a6bd2ed/image.png)


> 내부함수 inner에서 title을 호출했을 때 외부함수인 outer의 지역변수에 접근할 수 있음을 보여준다.

---

## 클로저(Closure)

클로저(closure)는 내부함수와 밀접한 관계를 가지고 있는 주제다. 
>내부함수는 외부함수의 지역변수에 접근 할 수 있는데 외부함수의 실행이 끝나서 외부함수가 소멸된 이후에도 내부함수가 외부함수의 변수에 접근 할 수 있다.

이러한 메커니즘을 클로저라고 한다. 

```javascript
function outer(){
    var title = 'coding everybody';  
    return function(){        
        console.log(title);
    }
}
var inner = outer();
inner();
```

![](https://velog.velcdn.com/images/nawhes_joo/post/2e5dfac0-682a-459b-adfb-a2b84a54e17d/image.png)


return은 함수가 종료됐다는 뜻이다.

outer()를 inner에 담고 inner를 호출시켰다.

inner에 담기는 값은 outer의 return 값인데 정상적으로 출력된다.

>외부함수가 소멸된 이후에도 내부함수가 외부함수의 변수에 접근할 수 있는 모습을 볼 수 있다.

---

## private variable

> 어떠한 정보를 아무나 수정하는 것을 방지하기 위한 것.

```javascript
function factory_movie(title){ // 외부 함수
    return { // 객체를 return
        get_title : function (){ // 내부 함수
            return title;
        },
        set_title : function(_title){ // 내부 함수
            title = _title
        }
    }
}
ghost = factory_movie('Ghost in the shell');
matrix = factory_movie('Matrix');

console.log(ghost.get_title());
console.log(matrix.get_title());
```


+ 클로저는 객체의 메소드에서도 사용할 수 있다. 위의 예제는 함수의 리턴값으로 객체를 반환하고 있다. 이 객체는 메소드 get_title과 set_title을 가지고 있다. 이 메소드들은 외부함수인 factory_movie의 인자값으로 전달된 지역변수 title을 사용하고 있다.


![](https://velog.velcdn.com/images/nawhes_joo/post/d9405141-973f-4b1f-8809-7d6cd040cfe8/image.png)


---


```javascript
function factory_movie(title){ // 외부 함수
    return { // 객체를 return
        get_title : function (){ // 내부 함수
            return title;
        },
        set_title : function(_title){ // 내부 함수
            title = _title
        }
    }
}
ghost = factory_movie('Ghost in the shell');
matrix = factory_movie('Matrix');

ghost.set_title('공각기동대');
 
console.log(ghost.get_title());
console.log(matrix.get_title());
```

![](https://velog.velcdn.com/images/nawhes_joo/post/472ae91d-38dc-4a3e-88bb-82b31502045c/image.png)


+ 동일한 외부함수 안에서 만들어진 내부함수나 메소드는 외부함수의 지역변수를 공유한다.  set_title은 외부함수 factory_movie의 지역변수 title의 값을 '공각기동대'로 변경했다. ghost.get_title();의 값이 '공각기동대'인 것은 set_title와 get_title 함수가 title의 값을 공유하고 있다는 의미다.

+ 그런데 똑같은 외부함수 factory_movie를 공유하고 있는 ghost와 matrix의 get_title의 결과는 서로 각각 다르다. 그것은 외부함수가 실행될 때마다 새로운 지역변수를 포함하는 클로저가 생성되기 때문에 ghost와 matrix는 서로 완전히 독립된 개체가 된다.

+ factory_movie의 지역변수 title은 2행(return)에서 정의된 객체의 메소드에서만 접근할 수 있는 값이다. 이 말은 title의 값을 읽고 수정할 수 있는 것은 factory_movie 메소드를 통해서 만들어진 객체 뿐이라는 의미다.
자바스크립트는 기본적으로 Private한 속성을 지원하지 않는데, 클로저의 이러한 특성을 이용해서 Private한 속성을 사용할 수 있게된다.

> Private 속성은 객체의 외부에서는 접근 할 수 없는 외부에 감춰진 속성이나 메소드를 의미한다. 이를 통해서 객체의 내부에서만 사용해야 하는 값이 노출됨으로서 생길 수 있는 오류를 줄일 수 있다. 자바와 같은 언어에서는 이러한 특성을 언어 문법 차원에서 지원하고 있다.

---

## 클로저의 응용

```javascript
var arr = [];
for(var i = 0; i < 5; i++){
	arr[i] = function(){
    	return i;
    }
}
for(var index in arr){
	console.log(arr[index]());
}
```

예상 결과값은 0, 1, 2, 3, 4이다.


![](https://velog.velcdn.com/images/nawhes_joo/post/4bf9b7ec-1560-4977-8320-5845fc86dcac/image.png)


하지만 실행시키면 5가 5번 출력된다.

```javascript
arr[i] = function(){
    	return i;
    }
```

for문에서 정의한 i의 값은 위 코드의 외부변수 값이 아니기 때문이다.

위 코드를 내부함수로 하는 외부함수를 정의하고, 그 외부함수의 지역변수 값을 내부함수가 참조할 수 있도록 수정해보자.

```javascript
var arr = []
for(var i = 0; i < 5; i++){
    arr[i] = function(id) { // 지역변수와 같은 효과를 내는 매개변수
        return function(){ // id라는 매개변수 값으로 i의 값을 받아서 함수 내부로 전달 및 return
            return id; // 내부함수. 외부함수의 지역변수인 id를 사용
        }
    }(i);
}
for(var index in arr) {
    console.log(arr[index]());
}
```

![](https://velog.velcdn.com/images/nawhes_joo/post/83e0c300-6d72-406c-97be-65c71270c6b8/image.png)


처음 기대한 값처럼 0~4가 출력된다.