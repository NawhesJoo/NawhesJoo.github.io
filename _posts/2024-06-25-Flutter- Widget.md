---
title: Flutter - Widget(위젯)
description: >-
  
author: NawhesJoo
date: 2024-06-25 15:21:00 +0900
categories: [Framework, Flutter]
tags: [Flutter]
pin: false
math: true
mermaid: true
image:
  path: /thumbnails/Flutter.png
  lqip: 
  alt: 
comments: true
---

기본 위젯 4개만 알면 기초 끝이라고 한다.

우선 시작하기 전, analysis_options.yaml 파일을 수정하자

## 세팅

![](https://velog.velcdn.com/images/nawhes_joo/post/7ccf6e80-6a5e-4ac9-82ab-e9ac967c9def/image.png)


analysis_options.yaml 파일을 열어보면, 이렇게 세팅되어 있을 것이다.

rules 란에 아래 코드를 추가하자.

```yaml
rules:
  prefer_typing_uninitialized_variables : false	# 앞에 스페이스바 2개
  prefer_const_constructors_in_immutables : false
  prefer_const_constructors : false
  avoid_print : false
```

각각 어떤 규칙인지 알아보자.

1. `prefer_typing_uninitialized_variables`
	+ 설명 : 초기화되지 않은 변수에 명시적으로 타입을 지정하도록 권장한다.
    + 예시
    ```dart
    // 권장되지 않음
    var foo;
    
    // 권장됨
    String foo;
    ```
    
2. `prefer_const_constructors_in_immutables`
	+ 설명 : 불변 클래스(immutable class)에서 생성자를 상수 생성자로 만들도록 권장한다.
    + 예시
    
    ```dart
    class MyWidget extends StatelessWidget {
    	// 권장되지 않음
        MyWidget();
        
        // 권장됨
        const MyWidget();
    }
    ```
    
3. `prefer_const_constructors`

	+ 설명 : 가능한 곳에서는 항상 상수 생성자를 사용하도록 권장한다.
    + 예시
    
    ```dart
    // 권장되지 않음
    var foo = MyClass();
    
    // 권장됨
    var foo = const MyClass();
    ```
    
4. `avoid_print`
	+ 설명 : `print()` 함수 대신에 로깅(logging) 패키지를 사용하도록 권장한다.
    		`print()`는 디버깅 목적으로만 사용되며, 프로덕션 코드에서는 적절한 로깅 프레임워크를 사용하는 것이 좋다.
            
	+ 예시
    ```dart
    // 권장되지 않음
    print('Hello, World!');
    
    // 권장됨
    final logger = Logger('MyLogger');
    logger.info('Hello, World!');
    ```

모두 false로 설정하면 Dart 분석기가 위의 네 가지 규칙을 검사하지 않게 된다.

---

## main

![](https://velog.velcdn.com/images/nawhes_joo/post/f45cb255-d6d3-4911-b0d7-c3d00aea4d0f/image.png)

main.dart를 열어보면, 길게 세팅되어 있다.

구글이 이해하기 쉽도록 쉬운 프로젝트 하나를 생성해주었다.

void main() 밑은 싹~ 다 지우자.

![](https://velog.velcdn.com/images/nawhes_joo/post/b9f5b4fe-e174-44cc-8d6c-984784ff9567/image.png)

stless를 입력한 뒤 tab 키를 누르면

![](https://velog.velcdn.com/images/nawhes_joo/post/da9a1a6b-0756-4fee-90f6-7e83869a0506/image.png)

이런 식으로 자동완성이 된다.


MyApp을 입력해주면 아래와 같이 될 것이다.

```dart
import 'package:flutter/material.dart'; // 다른 파일 or package import

void main() {
  runApp(const MyApp());	// runApp : app 구동. MyApp 자리에 앱 메인페이지 입력
}

class MyApp extends StatelessWidget {	// 앱 메인페이지. 기본적으로 채워야하는 문법
  const MyApp({super.key});
  @override
  Widget build(BuildContext context) {

    return MaterialApp( // 실질적 코드 작성
      home: 
    );

  }
}
```

이러면 메인페이지 세팅은 끝이다.

앱 디자인을 넣는 법을 배워보자.

---

## Design

우리가 꼭 알아야할 위젯은 4가지이다.

1. 글자 (Text)
2. 아이콘(Icon)
3. 이미지(Image)
4. 박스(Container)

이 4가지만 알아도 웬만한 건 다 만들 수 있다.

Flutter에서 디자인하는 방법은, Widget 짜집기하는 식으로 디자인한다고 보면 된다.

글자를 넣고 싶으면 글자 위젯, 박스를 넣고 싶으면 박스 위젯을 넣어서 짜집기 하면 된다.

Widget의 개념이 매우 중요하다.

### Text

우선 글자 먼저 넣어보자.

```dart
Text('글자')
```

```dart
return MaterialApp(
	home: Text('Hello, World!')
);
```

위 코드처럼 수정한 후 

![](https://velog.velcdn.com/images/nawhes_joo/post/84f4a333-5ee4-4e1c-a55f-d87ce4b86691/image.png)


Chrome으로 main.dart를 실행해보자.

사실은 앱이지만 웹 브라우저로 미리보기를 띄우는 것이다.

![](https://velog.velcdn.com/images/nawhes_joo/post/764c49c6-73b2-4667-b6a9-f274237bc2a9/image.png)

크롬창에서 Hello, World!가 출력된다.

---

### Icon

>```dart
Icon(Icons.아이콘 이름)
```

```dart
return MaterialApp(
	home: Icon(Icons.star)
);
```

Icons.star를 집어넣어보자.

![](https://velog.velcdn.com/images/nawhes_joo/post/dfb3ea03-036b-413c-9888-75d57d19ecae/image.png)

별 아이콘 하나가 생기는 것을 확인할 수 있다.

---

```dart
return MaterialApp(
	home: Icon(Icons.shop)
);
```

이번엔 star 대신 shop을 입력하였다.

![](https://velog.velcdn.com/images/nawhes_joo/post/1b999915-4d48-46b4-a578-9aefb602895e/image.png)

별 대신 상점 아이콘이 출력되는 것을 확인할 수 있다.

>아이콘 이름은
>
https://fontawesomeicons.com/materialdesign/icons?search=list
>
이 사이트에서 검색해서 확인할 수 있다.

---

### Image

>```dart
Image.asset('경로')
>```

이미지를 넣고 싶으면 프로젝트 폴더 안에 존재해야 한다.

#### 이미지 등록

![](https://velog.velcdn.com/images/nawhes_joo/post/86e14784-04d2-4ebb-9d68-270b2e886d75/image.png)

프로젝트 우클릭 - New - Directory를 클릭하여 이미지 보관용 assets 폴더를 만들고 이미지를 넣는다.

![](https://velog.velcdn.com/images/nawhes_joo/post/e54c19fd-3763-4c6b-b505-4dae8ea31c20/image.png)

예시로 Flutter 이미지를 넣어두었다.

---

#### yaml 수정

그 다음, `pubspec.yaml` 파일을 열어보자.

> pubspec.yaml이란?
앱을 만들 때 필요한 모든 자료들을 쭉~ 정리한 파일이라고 보면 된다. 외부 패키지, 라이브러리 등등

![](https://velog.velcdn.com/images/nawhes_joo/post/3eaa9080-2e8b-4a6f-9980-08effb354c2d/image.png)

pubspec.yaml 파일에서 flutter를 찾아보자.

flutter를 찾았으면

```yaml
flutter:
  assets:
    - assets/
```

위 처럼 수정해주자.

assets 안에 있는 모든 이미지 파일을 가져다 쓸 수 있다.

---

#### 호출

이제 main.dart에서 호출해보자.

```dart
return MaterialApp(
  home: Image.asset('flutter.png')
);
```

assets 폴더에 이미지를 추가하고, pubspec.yaml 파일을 정상적으로 수정했다면

Image.asset('경로')에서 경로 위치에 이미지 파일명만 입력하면 된다.

(정확한 경로 명은 'assets/flutter.png'이다.)


![](https://velog.velcdn.com/images/nawhes_joo/post/fe7bac57-5881-4b78-8069-259b2eea7134/image.png)

---

### Container / SizedBox

> Container( 스타일(스타일 명 : 값) ) / SizedBox()


```dart
return MaterialApp(
	home: Container( width: 50, height: 50, color: Colors.blue )
);
```

위 코드처럼 Container()를 입력하고 테스트해보자.

![](https://velog.velcdn.com/images/nawhes_joo/post/0835040a-16f9-43d4-b099-e467c4eabb32/image.png)

엄청난 크기의 파란색 박스가 생성되었다.

50을 입력했는데 왜 이렇게 크게 나올까??

> 50을 입력했지만, 단위를 입력하지 않았다. 플러터의 사이즈의 단위는 LP이다.
> 50LP == 1.2cm

1.2cm라고 해도 너무 크다.

그 이유는 무엇일까?

`어디부터 50을 차지할지 몰라서 그렇다`

어디부터 차지할지는 부모가 정한다.


```dart
return MaterialApp(
  home: Center( // 정중앙
    child: Container( width: 50, height: 50, color: Colors.blue ) // 자식
  )
);
```

위 코드처럼 수정해보자.

Center를 통해 기준점을 중앙으로 설정해주고, child 안에 자식(Container)를 작성해주면

![](https://velog.velcdn.com/images/nawhes_joo/post/2a91a930-e613-4098-b7f8-e36ab8837752/image.png)

50LP * 50LP의 파란색 박스가 생성된다.

출처 - [코딩애플](https://www.youtube.com/@codingapple)

