---
title: Flutter - APK 빌드
description: >-
  
author: NawhesJoo
date: 2024-06-29 15:21:00 +0900
categories: [Framework, Flutter]
tags: [Flutter]
pin: true
math: true
mermaid: true
image:
  path: /thumbnails/Flutter.png
  lqip: 
  alt: 
comments: true
---
usb로 휴대폰과 연결할 때 flutter의 과정에 대해 알아보자.

## 빌드

APK 빌드에 대해 알아보자

### 빌드란?

> Android에서의 빌드는 개발자가 소스 코드를 작성한 후 앱 설치 파일 `APK`를 만들기까지의 실행 과정을 의미한다.

![](https://velog.velcdn.com/images/nawhes_joo/post/1b61a9e6-70f5-4359-bbde-a7c827552a15/image.png)

Android는 기본적으로 Linux 커널 위에 여러 소프트웨어 스택이 쌓인 형태로 Linux의 빌드와 동일하다고 생각하면 된다.

+ Linux에서의 빌드는 소스 코드를 기계어로 컴파일 한 후 사용되는 라이브러리와의 `Link`를 수행하여 실제 실행 파일로 만드는 과정을 의미한다.

![](https://velog.velcdn.com/images/nawhes_joo/post/d367b8ad-b776-4a1a-a442-c7e444602d16/image.png)

---

### 빌드 도구

> 빌드 도구는 외부 라이브러리 추가 및 업데이트 등의 설정 시간을 단축시킨다. 또한 테스트 실행 및 호환성 체크까지 진행한다.

Android에서 사용되는 빌드 도구는 `maven`, `gradle` 등이 있으며 구글에서는 `gradle` 의 사용을 권장하고 있다.

`gradle`내부의 빌드 스크립트를 작성하여

+ 앱의 의존성
+ 라이브러리
+ 리소스 파일
+ 빌드 설정

등을 진행한다.

---

### 빌드 진행

> 빌드 프로세스는 `gradle` 빌드 도구가 수행한다. 빌드 프로세스는 앱의 소스 코드와 별도 스크립트를 결합하여 `APK` 파일을 생선한다.

1. 코틀린 컴파일러는 `.kt` 파일을 `.class` 바이트코드파일로 변환한다.

2. `Android SDK`의 `dx` 도구를 사용하여 `.class` 파일들을 `.dex` 파일로 변환한다.

![](https://velog.velcdn.com/images/nawhes_joo/post/479a8876-02f8-4927-98de-4f72867d1891/image.png)

3. Android 리소스 패키징 도구(`aapt`)와 `gradle` 사용하여 리소스 파일 및 외부 라이브러리 모듈을 `.dex`파일과 함께 APK 파일로 패키징합니다.

4. `APK` 파일은 서명되어야 Android 디바이스에서 실행될 수 있다. `APK` 파일에 서명하기 위해서는 디지털 인증서를 사용해야 한다. `APK` 파일에 서명하는 작업은 빌드 과정에서 `gradle`에 설정된 값에 따라 자동으로 수행한다.

![](https://velog.velcdn.com/images/nawhes_joo/post/1e26822f-cc47-432d-8816-ddeaca40a1c3/image.png)

이렇게 서명된 `APK`가 만들어질 수 있다.

---

### APK 설치 및 실행

![](https://velog.velcdn.com/images/nawhes_joo/post/b50c5382-137c-4f5c-8283-ad86dedb3319/image.png)

설치된 `APK`는 안드로이드 런타임과정을 따라 초기 `JIT` 방식을 활용하여 앱을 설치한 후, 이후 자주 사용하는 앱을 `AOT` 방식을 활용하여 컴파일하는 방식으로 진행된다.

---

## 스마트폰과 연결

1. PC에 USB로 스마트폰과 연결한다.

2. 스마트폰 설정 메뉴에서 `빌드 번호`를 검색한다.
![](https://velog.velcdn.com/images/nawhes_joo/post/8433226d-6e34-4f4e-83a3-c4c3705054f5/image.png)

3. 빌드번호를 연속으로 클릭
![](https://velog.velcdn.com/images/nawhes_joo/post/462a95f4-c91d-40a5-b825-e003a37c0810/image.png)


4. 설정에 개발자 옵션이 추가되고, 개발자 옵션에서 `USB 디버깅`을 활성화
![](https://velog.velcdn.com/images/nawhes_joo/post/da43e1bb-ff3c-45cc-8ad7-d3e9c873fd4c/image.png)

5. flutter doctor 명령을 입력하면 연결된 장치 갯수 표시됨
![](https://velog.velcdn.com/images/nawhes_joo/post/2acf3d7b-8af3-41b2-bdf7-ebec612051dc/image.png)

6. 안드로이드 스튜디오에서 연결한 기기를 선택 후 실행버튼 클릭
![](https://velog.velcdn.com/images/nawhes_joo/post/e9e2d168-1f5d-469c-af95-e0e9ade96ee2/image.png)

7. 실제 기기에서 플러터 데모 앱 실행 완료
![](https://velog.velcdn.com/images/nawhes_joo/post/f4651485-7ce9-4da9-b3af-901f97dea4d9/image.png)

---

1. __APK 파일 생성 및 전송__

   + `flutter run` 명령어를 사용하여 Flutter 프로젝트를 빌드하고 APK 파일을 생성한다.
   + 이 APK 파일은 컴퓨터의 파일 시스템에서 생성되며, 이후 ADB(Android Debug Bridge)를 사용하여 USB로 연결된 Android 기기로 전송된다.
 
 2. __ADB를 통한 설치 및 실행__ 
	+ APK 파일이 Android 기기로 전송된 후, ADB를 통해 Android 기기에 설치된다. ADB는 Android SDK의 일부로, 개발자가 Android 기기와 컴퓨터 사이에서 파일 전송 및 명령 실행을 할 수 있도록 지원한다.
	+ 설치가 완료되면, Android 기기에서 APK 파일이 실행되어 Flutter 앱이 시작된다.

3. __Flutter 앱 실행__

	+ Android 기기에서 Flutter 앱이 실행되면, 먼저 Flutter 엔진이 초기화된다. 이 과정에서 Skia 그래픽 엔진과 Dart VM이 활성화된다.
	+ Dart VM이 초기화되면 Flutter 프로젝트의 Dart 코드가 Dart VM에서 실행된다. 이 코드는 Flutter의 위젯을 생성하고, 각 위젯의 상태를 관리하여 UI를 렌더링한다.
	+ Flutter 엔진은 Skia를 사용하여 Dart 코드로 정의된 위젯 트리를 화면에 그린다. Skia 네이티브 그래픽 API를 호출하여 Android 기기의 실제 화면에 UI를 렌더링한다.
