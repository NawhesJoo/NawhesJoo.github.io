---
title: Flutter - 가로새로 배치와 Scaffold
description: >-
  
author: NawhesJoo
date: 2024-07-14 15:21:00 +0900
categories: [Framework, Flutter]
tags: [Flutter, Scaffold, Row, Column, align]
pin: false
math: true
mermaid: true
image:
  path: /thumbnails/Flutter.png
  lqip: 
  alt: 
comments: true
---
## MaterialApp()

![](https://velog.velcdn.com/images/nawhes_joo/post/4f178b93-34f5-47f2-a0d7-8f30cc562be2/image.png)

`MaterialApp()`이라는 위젯이다.

이 위젯을 써놓고 시작하면, 구글이 제공하는 Material 테마를 이용하여 앱을 만들 수 있다.

+ 구글이 제공하는 유용한 UI들을 이용할 수 있다. (구글 스타일)

+ 아이폰 기본 앱 같은 스타일을 이용하고 싶으면 `Cuptertino()`를 이용하면 된다.

`커스터마이징`을 하고 싶으면 MaterialApp()을 사용해야 한다.

---

## Scaffold

Scaffold는 상중하로 나누어주는 위젯이다.


```dart
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {

    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(),						// 상단에 들어갈 위젯
        body: Container(),						// 중간에 들어갈 위젯
        bottomNavigationBar: BottomAppBar(),	// 하단에 들어갈 위젯
      )
    );
  }
}
```

appBar : 상단
body : 중간(내용 영역)
BottomAppBar : 하단

Container 넣어도 되고, 미리 만들어진 위젯을 넣어도 된다.

```dart
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {

    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(),
        body: Container(),
        bottomNavigationBar: BottomAppBar( child: Text('dsdfs')),	
      )
    );
  }
}
```

BottomAppBar에 child: Text('dsdfs')) 를 입력하면

![](https://velog.velcdn.com/images/nawhes_joo/post/0122be37-6f78-4bfb-9dcb-6a6b2ee680a7/image.png)

하단에 입력한 텍스트가 나온다.

이렇게 간단한 하단바를 생성할 수 있다.

---

## 배치

### 가로로 배치하는 법 (Row)

```dart
    return MaterialApp(
      home: Scaffold(
        body: Container(
          child: Icon(Icons.star),
        ),
      )
    );
```

상단과 하단은 제외하고 body에 아이콘을 배치해보자.

![](https://velog.velcdn.com/images/nawhes_joo/post/6bd05e20-c04b-4262-a37b-4a8077df03a4/image.png)

위 사진을 보면 왼쪽 위에 별 아이콘이 생긴다.

이 이유는 Scaffold가 기준점을 `왼쪽 위`로 잡아주기 때문이다.

---

이제 여러 위젯을 가로로 배치해보자.


```dart
    return MaterialApp(
      home: Scaffold(
        body: Row(
          children: [
            Icon(Icons.star),
            Icon(Icons.star),
          ],
        ),
      )
    );
```
   body에 Container가 아닌 Row 위젯을 사용하고, child가 아닌 children이라는 파라미터를 사용해보자.
   
children 파라미터는 Array 형태로 입력하면 된다.
   
![](https://velog.velcdn.com/images/nawhes_joo/post/1ea5ddcb-e014-456f-b6dc-420bfde1011a/image.png)
  
   
왼쪽 위 기준으로 별 아이콘 두개를 가로로 배치할 수 있게 된다.

![](https://velog.velcdn.com/images/nawhes_joo/post/db597521-4606-48b8-8ca7-49b9dbb22bbf/image.png)

위 사진과 같이 밑줄이 거슬린다.

lint 때문이라고 한다.

![](https://velog.velcdn.com/images/nawhes_joo/post/a982bfa8-fe80-4a9f-bbdf-2179c8d300ca/image.png)

const를 추가하면 해결되지만, const 사용하기 싫은 사람은

analysis_options.yaml 파일을 열어서 rules 안에 아래 코드를 추가하자.

```dart
prefer_const_literals_to_create_immutables : false

```

![](https://velog.velcdn.com/images/nawhes_joo/post/13bb9274-9267-4435-a9f5-f1b965fe9568/image.png)

밑줄이 깔끔하게 사라진다.

---

### 세로로 배치하는 법 (Column)

세로로 배치하는 방법은 Row 대신 Column을 사용하면 된다.

```dart
    return MaterialApp(
      home: Scaffold(
        body: Column(
          children: [
            Icon(Icons.star),
            Icon(Icons.star),
            Icon(Icons.star),
          ],
        ),
      )
    );
```

Row 대신 Column을 사용하고, children 파라미터를 사용하며 Array 형식으로 별 아이콘 3개를 입력하였다.

![](https://velog.velcdn.com/images/nawhes_joo/post/fece5311-5a6a-4aff-a79e-1cb1dde61e11/image.png)

저장하면 세로로 별 아이콘 3개가 생성되는 것을 확인할 수 있다.

---

## 정렬

### mainAxisAlignment

Row와 Column을 이용하여 가로, 세로로 배치하는 방법을 배워보았다.

Scaffold가 왼쪽 위를 기준으로 잡기 때문에 왼쪽 위에 배치된다. 이를 정렬해보자.

```dart
    return MaterialApp(
      home: Scaffold(
        body: Row(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Icon(Icons.star),
            Icon(Icons.star),
            Icon(Icons.star),
          ],
        ),
      )
    );
```

`mainAxisAlignment`를 사용해보자.

> mainAxisAlignment는 Row의 가로축을 정렬


![](https://velog.velcdn.com/images/nawhes_joo/post/1b8f65f4-516d-42d9-be22-0a46af7857a7/image.png)

MainAxisAlignment.center를 사용하여 중간에 정렬되는 것을 확인할 수 있다.

![](https://velog.velcdn.com/images/nawhes_joo/post/62edc578-7ff5-4449-9831-b848e57de14b/image.png)

자동완성을 보면 더 많은 정렬이 있다.


![](https://velog.velcdn.com/images/nawhes_joo/post/fa0ed462-5779-4e9b-b9f6-b8aa79222379/image.png)

위 사진은 MainAxisAlignment.spaceEvenly이다. 

CSS의 display : flex와 유사하다.

---

```dart
    return MaterialApp(
      home: Scaffold(
        body: Column(
          mainAxisAlignment: MainAxisAlignment.spaceEvenly,
          children: [
            Icon(Icons.star),
            Icon(Icons.star),
            Icon(Icons.star),
          ],
        ),
      )
    );
```

이번엔 Row를 Column으로 변경해보자.

![](https://velog.velcdn.com/images/nawhes_joo/post/4d6e3370-11ef-494a-9016-4d7e04795796/image.png)

이번엔 세로축을 기준으로 정렬한다.

> Row의 mainAxisAlignment은 가로축, Column의 mainAxisAlign은 세로축을 의미한다.
crossAxisAlignment는 mainAxisAlignment의 반대인 Row - 세로축, Column - 가로축을 의미한다.

---

## 활용

```dart
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar( title: Text('앱임')),
        body: Text('안녕'),
        bottomNavigationBar: BottomAppBar(
          child: SizedBox(
            height: 70,
            child: Row(
              mainAxisAlignment: MainAxisAlignment.spaceEvenly,
              children: [
                Icon(Icons.phone),
                Icon(Icons.message),
                Icon(Icons.contact_page),
             ],
            ),
          ),
        ),
      )
    );
```

위처럼 appBar, body, bottomNavigationBar를 모두 이용하여

![](https://velog.velcdn.com/images/nawhes_joo/post/774647af-b3f1-435d-a2b5-b11ad6d55c99/image.png)

이렇게 디자인을 할 수 있다.

출처 - [코딩애플](https://www.youtube.com/@codingapple)