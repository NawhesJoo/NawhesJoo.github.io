---
title: JavaScript - 정규표현식(RegExp)
description: >-
  
author: NawhesJoo
date: 2024-05-27 17:21:00 +0900
categories: [Language, JavaScript]
tags: [RegExp]
pin: false
math: true
mermaid: true
image:
  path: /thumbnails/JavaScript.png
  lqip: 
  alt: 
comments: true
---
## 정규표현식이란?

> 정규표현식(regular expression)은 문자열에서 특정한 문자를 찾아내는 도구다. 이 도구를 이용하면 수십줄이 필요한 작업을 한 줄로 끝낼 수 있다.

---

## 정규표현식 생성

정규표현식은 두가지 단계로 이루어진다. 하나는 `컴파일(compile)`, 다른 하나는 `실행(execute)`이다.

우선 컴파일부터 알아보자.

### 컴파일

컴파일은 검출하고자 하는 패턴을 만드는 일이다. 우선 정규표현식 객체를 만들어야 한다. 객체를 만드는 방법은 두 가지가 있다. a라는 텍스트를 찾아내는 정규식을 만들어보자.

#### 정규표현식 리터럴

```javascript
var pattern = /a/;
```

#### 정규표현식 객체 생성자

```javascript
var pattern = new RegExp('a');
```

두가지 모두 같은 결과를 만들지만 각자가 장단점이 있다.

---

### 정규표현식 메소드 실행

정규표현식을 컴파일해서 객체를 만들었다면 이제 문자열에서 원하는 문자를 찾아내야 한다.

#### RegExp.exec()

```javascript
console.log(pattern.exec('abcdef')); // ['a']
```

![](https://velog.velcdn.com/images/nawhes_joo/post/3ce0002f-6ffc-4de9-a822-cd4981885719/image.png)

실행결과는 문자열 a를 값으로 하는 배열을 리턴한다.

```javascript
console.log(pattern.exec('bcdefg')); // null
```

![](https://velog.velcdn.com/images/nawhes_joo/post/490c1355-1620-4c18-b49f-5d8bc2b965e9/image.png)

인자 'bcdefg'에는 a가 없기 때문에 null을 리턴한다.

---

#### RegExp.test()

```javascript
console.log(pattern.test('abcdef')); // true
console.log(pattern.test('bcdefg')); // false
```

![](https://velog.velcdn.com/images/nawhes_joo/post/586eb0ee-5847-4910-ab5e-6d34e2f2121a/image.png)


test는 인자 안에 패턴에 해당되는 문자열이 있으면 true, 없으면 false를 리턴한다.

---

### 문자열 메소드 실행

문자열 객체의 몇몇 메소드는 정규표현식을 사용할 수 있다.

#### String.match()

```javascript
console.log('abcdef'.match(pattern));
console.log('bcdefg'.match(pattern));
```

![](https://velog.velcdn.com/images/nawhes_joo/post/143dcce9-da9e-4152-b231-6400d872076d/image.png)

RegExp.exec()와 비슷하다.

---

#### String.replace()

```javascript
console.log('abcdef'.replace(pattern, 'A'));
```

![](https://velog.velcdn.com/images/nawhes_joo/post/c19292b2-1bbe-40bc-9ec1-7762f1aff89f/image.png)


문자열에서 패턴을 검색하여 이를 변경한 후에 변경된 값을 리턴한다.

---

### 옵션

정규표현식 패턴을 만들 때 옵션을 설정할 수 있다. 옵션에 따라서 검출되는 데이터가 달라진다.

#### i

```javascript
var xi = /a/;
console.log('Abcde'.match(xi)); // null

var oi = /a/i;
console.log('Abcde'.match(oi)); // ['A']
```

![](https://velog.velcdn.com/images/nawhes_joo/post/f871c3ed-de2b-403b-b402-04cb7e5caac3/image.png)

i를 붙이면 대소문자를 구분하지 않는다.

---

#### g

```javascript
var xg = /a/;
console.log('abcdea'.match(xg)); // ['a']

var og = /a/g;
console.log('abcdea'.match(og)); // ['a', 'a']
```

![](https://velog.velcdn.com/images/nawhes_joo/post/541f2fdd-fa91-4a97-98ae-523a234545f2/image.png)

g를 붙이면 검색된 모든 결과를 리턴한다.

---

### 캡처

```javascript
var pattern = /(\w+)\s(\w+)/;
var str = "Hello world";
var result = str.replace(pattern, "$2, $1");
console.log(result);
```

![](https://velog.velcdn.com/images/nawhes_joo/post/bd09312b-4a48-4c51-84f8-a95fda1d4847/image.png)

괄호안의 패턴은 마치 변수처럼 재사용할 수 있다. 이 때 기호 $를 사용하는데 위 예시는 Hello와 world의 순서를 역전시킨다.

```javascript
var result1 = str.replace(pattern, "$2 $1"); // 콤마 제거
var result2 = str.replace(pattern, "$1$2");	 // 공백 제거
var result3 = str.replace(pattern, "$1 $2 $2$1 $1+$2"); // 짬뽕
```

![](https://velog.velcdn.com/images/nawhes_joo/post/4510f3fa-db98-4fc0-8e3c-7d3b56f9b7c8/image.png)

순서뿐만 아니라 `$1`과 `$2` 사이에 구분자, 공백 등을 추가할 수 있다.

---

### 치환

```javascript
var urlPattern = /\b(?:https?):\/\/[a-z0-9-+&@#\/%?=~_|!:,.;]*/gim;
var content = '블로그 : https://nawhes.techblog.doubleuem.com/ 입니다. 네이버 : http://naver.com 입니다. ';
var result = content.replace(urlPattern, function(url){
    return '<a href="'+url+'">'+url+'</a>';
});
console.log(result);
```

```javascript
블로그 : <a href="https://nawhes.techblog.doubleuem.com/">https://nawhes.techblog.doubleuem.com/</a> 입니다. 네이버 : <a href="http://naver.com">http://naver.com</a> 입니다. 
```

replace를 통해 URL을 링크 html 태그로 교체하였다.


#### 분석

1. `/.../` : 정규 표현식 리터럴
	정규표현식을 감싸는 슬래시는 자바스크립트에서 정규표현식 리터럴을 정의

2.  `\b` : 단어 경계 (word boundary)
	`\b`는 단어 경계를 나타낸다. 단어 경계는 `\w` (단어 문자: 알파벳, 숫자, 밑줄)와 `\W` (비단어 문자: 공백, 특수 문자 등) 사이의 위치를 의미한다. 즉, URL이 단어의 시작 부분에 위치해야 함을 나타낸다.
    
3. `(?...)` : 캡처하지 않는 그룹 (non-capturing group)
	`(?...)`는 캡처하지 않는 그룹을 나타낸다. 이는 그룹화를 위해 사용되지만, 해당 그룹의 매칭 결과를 메모리에 저장하지 않는다. 위 코드의 경우 그룹 안의 내용은 `https` 또는 `http`가 된다.
    
4. `https?` : http 또는 https
	`s?`는 `s`가 0번 또는 1번 나타날 수 있음을 의미한다.
    
5. `:\/\/` : 콜론과 슬래시 두 개
	`:\/\/`sms `://`를 문자 그대로 매칭한다. 슬래시(`/`)와 콜론(`:`)은 정규표현식에서 특수 문자가 아니므로 그대로 사용된다.
    
6. `[a-z0-9-+&@#\/%?=~_|!:,.;]*` : URL의 나머지 부분
	이 부분은 URL의 나머지 부분을 매칭한다. 대괄호(`[]`) 안의 내용은 문자 클래스를 나타내며, 이 클래스 안의 어떤 문자도 매칭될 수 있다. 아스테리스크(`*`)는 앞의 요소가 0번 이상 반복될 수 있음을 의미한다.
    
	문자 클래스 내의 문자들 : 
    + `a-z` : 소문자 알파벳
    + `0-9` : 숫자
    + `-` : gkdlvms
    + `+&@#` : 각 문자들
    + `\/` : 슬래시(여기서 슬래시는 이스케이프되어 문자 그대로의 슬래시를 의미)
    + `%?=~_|!:,.;` : 각 문자들
    
7. `*` : 0번 이상 반복
	문자 클래스 뒤의 `*`는 해당 클래스에 정의된 문자가 0번 이상 반복될 수 있음을 의미
    
8. `gim` : 플래그
	정규표현식의 끝에 붙는 플래그는 검색 방식을 조정한다.
    + `g`(global) : 전체 문자열에서 모든 매칭을 찾는다.
    + `i`(ignore case) : 대소문자를 구분하지 않는다.
    + `m`(multiline) 여러 줄에 걸쳐 매칭을 수행한다. 여기서는 큰 의미가 없지만, 줄바꿈 문자가 있을 때 각 줄을 개별 문자열로 처리한다.

#### 전체 요약

+ `http` 또는 `https`로 시작
+ `://`로 이어짐
+ URL의 나머지 부분은 소문자 알파벳, 숫자, 하이픈, 플러스 등 하나 이상의 문자로 이루어짐

이 정규표현식은 대소문자를 구분하지 않고, 전체 문자열에서 모든 매칭을 찾으며, 여러 줄에서 작동한다.

---

## 메타문자와 이스케이프

### 메타문자

> 메타문자는 정규표현식에서 특별한 의미를 가지는 문자들이다. 이 문자는 문자 그대로 매칭되지 않고, 특정 패턴이나 동작을 정의하는 데 사용된다.

+ `.` : 임의의 한 문자 (줄바꿈을 제외한 모든 문자). 
  + `a.b` : a와 b사이에 어떤 문자가 와도 매칭 ('a1b', 'a-b')

+ `^` : 문자열의 시작
  + `^abc` : 문자열이 'abc'로 시작할 때 매칭

+ `$` : 문자열의 끝
   + `abc$` : 문자열이 'abc'로 끝날 때 매칭

+ `*` : 앞의 요소가 0번 이상 반복됨
   + `a*` : 'a'가 0번 이상 반복될 때 매칭 ('', 'a', 'aaa')

+ `+` : 앞의 요소가 1번 이상 반복됨
  + `a+` : 'a'가 1번 이상 반복될 때 매칭 ('a', 'aaa')

+ `?` : 앞의 요소가 0번 또는 1번 나타남
  + `a?` : 'a'가 0번 또는 1번 나타날 때 매칭 ('', 'a')

+ `{n,m}` : 앞의 요소가 n번 이상 m번 이하 반복됨
  + `a{2,4}` : 'a'가 2번 이상, 4번 이하 반복될 때 매칭 ('aa', 'aaa', 'aaaa')

+ `[]` : 문자 클래스. 대괄호 안의 문자 중 하나와 매칭
  + `[abc]` : 'a', 'b', 'c' 중 하나와 매칭

+ `|` : 논리적 OR
  + `a|b` : 'a' 또는 'b'와 매칭

+ `()` : 그룹화 및 캡처
  + `(abc)` : 'abc'라는 문자열과 매칭, 캡처 그룹으로 사용.

+ `\` : 이스케이프 문자 (특수문자나 메타문자를 그대로 매칭할 때 사용)

---

### 이스케이프

> 이스케이프는 메타문자나 특수 문자를 문자 그대로 매칭하기 위해 사용하는 방법이다.
백슬레시`(\)`는 이스케이프 문자로 사용된다. 메타문자를 이스케이프하면, 그것이 특별한 의미를 가지지 않고 `문자 그대로` 매칭된다.

+ `\.` : 문자 그대로의 점(.)

+ `\*` : 문자 그대로의 별표(*)

+ `\?` : 문자 그대로의 물음표(?)

+ `\\` : 문자 그대로의 백슬래시

---

추가로, 정규표현식 내에서 특수 문자나 이스케이프 시퀀스를 사용하여 특정 문자나 패턴을 나타낼 수 있다.

+ `\d` : 숫자 문자 (0-9)

+ `\D` : 숫자가 아닌 문자

+ `\w` : 단어 문자 (알파벳, 숫자, 밑줄)

+ `\W` : 단어 문자가 아닌 문자

+ `\s` : 공백 문자 (스페이스, 탭, 줄바꿈 등)

+ `\S` : 공백 문자가 아닌 문자
