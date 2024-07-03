---
title: Flutter - Flutter 개발 환경 세팅
description: >-
  
author: NawhesJoo
date: 2024-06-24 15:21:00 +0900
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
## 구축 방법 종류

Flutter 개발환경을 구축하는 방법에는 여러가지가 있다.

1. Flutter 공식 문서에서 제공하는 설치 가이드 (Flutter SDK)

2. 명령줄 도구를 이용한 설치(CLI)

3. 패키지 매니저를 통한 설치

4. IDE 통합 개발환경 설치 (Visual Studio Code, Android Studio)

5. Docker를 이용한 설치

Flutter 공식 문서에서 설치하는 방법과 IDE 통합 개발환경 설치하는 방법만 간단히 비교해보자.

## 비교

### 장단점

#### Flutter SDK 수동 설치

##### 장점

1. __제어 및 유연성__:
   + 개발 환경을 세밀하게 제어할 수 있다.
   + 필요한 도구와 설정을 직접 관리할 수 있어, `커스터마이징이 용이`하다.

2. __환경 독립성__    
	+ IDE에 종속되지 않으므로, 다양한 텍스트 에디터나 IDE와 함께 사용할 수 있다.
    + 특정 버전의 Flutter SDK를 유지하거나 `여러 버전을 동시에 관리`하기 쉽다.
    
3. __경량성__
	+ 불필요한 플러그인이나 확장 기능을 설치할 필요 없이, `최소한의 도구`만 설치하여 사용 가능
    
##### 단점

1. __설정 복잡성__
	
    + 환경 변수를 직접 설정해야 하므로, 설정 과정이 다소 복잡할 수 있다.
    + 의존성 설치 및 관리가 `수동`으로 이루어져야 한다.
    
2. __개발 생산성__
	+ IDE의 자동화된 기능(자동 완성, 디버깅 등)을 완전히 활용하기 어렵다.
    + 빌드 및 실행 등의 작업이 `수동`으로 이루어져 시간이 더 소요될 수 있다.
    
---

#### IDE를 통한 설치(Visual Studio Code, Android Studio)

##### 장점

1. __설정 간편성__

	+ 플러그인을 통해 Flutter SDK를 쉽게 설치하고 설정할 수 있다.
    + 환경 변수를 자동으로 설정해주므로, 개발환경 구성이 간편하다.
2. __개발 생산성__   
	+ 자동 완성, 코드 포맷팅, 상태 검사 등 다양한 개발 도구를 제공하여 `생산성`을 높인다.
    + `통합된 UI`로 다양한 작업(프로젝트 생성, 빌드, 실행 등)을 쉽게 수행할 수 있다.
    
3. __통합 기능__
	+ Git과 같은 버전 관리 도구와 쉽게 통합할 수 있다.
    + 프로젝트 관리를 위한 다양한 확장 기능을 지원한다.
    
##### 단점

1. __의존성__

	+ 특정 IDE에 종속될 수 있어, 다른 환경에서 작업할 때 `비효율적`일 수 있다.
    + 플러그인이나 IDE 버전에 따라 `호환성 문제`가 발생할 수 있다.
    
2. __리소스 사용__    
	+ IDE가 더 많은 시스템 자원을 사용하므로, 시스템 사양이 낮을 경우 `성능 저하`가 발생할 수 있다.
    + 불필요한 기능이나 플러그인이 설치되어 있을 경우, IDE가 무거워질 수 있다.
    
---

### 개발할 때의 차이점

#### Flutter SDK 수동 설치

1. __프로젝트 생성 및 관리__
	
    + `명령줄(CLI)`을 통해 프로젝트를 생성하고 관리한다. ( 예 : 'flutter create my_project' )
    + IDE가 아닌 텍스트 에디터( 예 : Sublime Text, Vim )에서 코드를 작성할 수 있다.
    
2. __빌드 및 실행__
	+ `명령줄(CLI)`를 통해 앱을 빌드하고 실행한다. ( 예 : 'flutter run' )
    + 디버깅도 명령줄 도구를 통해 `수동`으로 진행한다.
    
3. __의존성 관리__
	+ 직접 `flutter doctor`를 실행하여 필요한 의존성을 설치하고 관리한다.
  
---

#### IDE를 통한 설치  

1. __프로젝트 생성 및 관리__

	+ IDE의 GUI를 통해 `쉽게` 프로젝트를 생성하고 관리할 수 있다.
    + 프로젝트 설정, 디펜던시 관리, 코드 작성 등이 `통합된 인터페이스`에서 이루어진다.
    
2. __빌드 및 실행__
	+ IDE내에서 `버튼 클릭만으로` 앱을 빌드하고 실행할 수 있다. ~~딸깍~~
    + `디버깅 도구`를 사용하여 브레이크포인트 설정, 변수 검사 등이 가능하다.
    
3. __의존성 관리__    
	+ IDE가 `의존성 문제를 자동으로 감지`하고, 필요한 경우 설치를 안내해준다.
    
---

### 요약

+ __Flutter SDK 수동 설치__는 더 많은 `제어와 유연성`을 제공하지만, 설정과 관리가 복잡할 수 있으며, 개발 생산성이 다소 떨어질 수 있다.

+ __IDE를 통한 설치__는 `설정이 간편`하고, 다양한 `자동화 도구`와 `통합 기능`을 제공하여 개발 생산성을 크게 향상시킬 수 있다. 다만, 특정 IDE에 종속될 수 있고, 시스템 자원을 더 많이 사용할 수 있다.


<br><br/>
이렇게 장단점이 뚜렷하다. 이 장점들을 한 번에 다 사용할 순 없을까? 

가능하다. 쓰까서 쓰면된다.

---

## 개발환경 구축

### Flutter SDK 설치

1. [Flutter 웹사이트](https://flutter.dev)에 접속한 뒤 우측 상단의 'Get Start' 클릭

![](https://velog.velcdn.com/images/nawhes_joo/post/02db61b6-1054-453c-8b45-ada176a6620d/image.png)

>플러터를 구동하여 개발할 수 있는 운영체제(32비트 운영체제는 정상적으로 동작하지 않음)
+ 윈도우 7 SP1 이상(64비트)
+ 맥OS
+ 리눅스


2. PC에 설치된 운영체제에 맞는 항목 선택

![](https://velog.velcdn.com/images/nawhes_joo/post/44b9ea32-d815-49c8-863b-f81a6e3941d4/image.png)

![](https://velog.velcdn.com/images/nawhes_joo/post/e6ff59fa-2e87-4e46-acd1-48dd94f3b7fd/image.png)

일단 최신버전을 다운받았다.

3. 다운로드 받은 파일을 C:/src 폴더에 압축해제

![](https://velog.velcdn.com/images/nawhes_joo/post/41c8adf7-30c7-4748-9e3d-f50f53f5c72b/image.png)

> 쓰기 권한, 업데이트 및 설치 문제 등의 이유 때문에 C:/ProgramFiles 와 같은 높은 권한이 필요한 위치는 피하는 것이 좋다.

---

### 환경 변수 등록

+ Flutter 명령 파일은 압축을 푼 폴더 아래 bin 폴더에 위치
+ 이 파일이 들어있는 경로를 운영체제의 환경변수에 등록하면 어디에서나 플러터 명령을 사용 가능

1. '검색'에 '환경 변수'를 입력하고 시스템 환경 변수 편집'을 클릭

	![](https://velog.velcdn.com/images/nawhes_joo/post/23937593-1316-4400-b5c3-50b871696689/image.png)

2. '시스템 속성' 창이 뜨면 '고급' 탭에서 '환경 변수(N)'을 클릭

![](https://velog.velcdn.com/images/nawhes_joo/post/f7c416a0-86ea-4edd-9032-0f303bef85b7/image.png)

3. 환경 변수 창에서 'Path'를 선택한 후 '편집' 클릭

![](https://velog.velcdn.com/images/nawhes_joo/post/2ea9dec5-064d-452b-a143-73fef22f940d/image.png)

4. '찾아보기'를 클릭하고 Flutter SDK의 bin 폴더 위치 ( C:/src/flutter/bin )를 선택하고 '확인' 클릭

	![](https://velog.velcdn.com/images/nawhes_joo/post/d1d0fc4f-982a-4137-aa56-71a7a4badd8b/image.png)

![](https://velog.velcdn.com/images/nawhes_joo/post/0c879275-cbd6-4ea5-9ed3-63a78513405f/image.png)

5. 터미널을 실행시킨 후 `flutter --version`을 입력

![](https://velog.velcdn.com/images/nawhes_joo/post/dad952e0-8a46-483f-8fbd-5aac23c7f1ac/image.png)

6. `flutter doctor`를 통해 SDK가 정상적으로 설치되었는지 확인

![](https://velog.velcdn.com/images/nawhes_joo/post/4f695338-566a-4d99-8445-29c41da22323/image.png)

여러 항목들이 나오는데 SDK 설치 확인은 첫 번째 [v] Flutter 녹색 체크를 보면 된다.

나머지 사항은 개개인의 PC 환경에 따라 다르게 나올 수 있다.

위처럼 출력되면 성공

---

### 앱 개발 도구 설치

+ 플러터 개발에 사용할 도구로 다음과 같은 IDE(통합 개발 환경)을 지원
  + `Android Studio` : [https://developer.android.com/studio](https://developer.android.com/studio)
  + `Visual Studio Code` : [https://code.visualstudio.com/](https://developer.android.com/studio)

+ 두 가지 IDE 모두 훌륭하지만 플러터는 구글이 만들었기 때문에 자사에서 개발한 안드로이드 스튜디오를 사용하는 것이 편의성 등에서 더 나음
+ 설치 환경을 구성할 때에도 안드로이드 스튜디오가 더 간단함

안드로이드 스튜디오로 진행할 것이다.

---

#### Android Studio 설치

1. [Android Studio 다운로드](https://developer.android.com/studio)<br>
    1-1 [Android Studio 구버전 다운로드](https://developer.android.com/studio/archive) 
    ![](https://velog.velcdn.com/images/nawhes_joo/post/ea67497b-6886-4d3c-858d-be064cac3b24/image.png)
    우측 상단 언어 한국어 -> ENGLISH로 변경
    
    + Giraffe 버전이 플러터 개발을 위한 최신기능과 안정성을 제공한다고 함
    
---

![](https://velog.velcdn.com/images/nawhes_joo/post/2b551630-dd61-4bf6-bf6e-8fbbd1dc1ee9/image.png)

2. 다운로드한 설치 파일을 실행하여 설치 진행

3. Flutter, Dart 플러그인 설치

![](https://velog.velcdn.com/images/nawhes_joo/post/02d04ffe-d0d2-45e7-8698-6ea22490e5b5/image.png)

설치가 완료되면 Restart IDE를 클릭하여 재실행해주자.


![](https://velog.velcdn.com/images/nawhes_joo/post/7cc4aa26-a3f8-4cb2-a90c-adf50b34460b/image.png)

Installed를 들어가보면 Flutter와 Dart가 설치된 것을 확인할 수 있다.

4. Flutter Project 생성하기

![](https://velog.velcdn.com/images/nawhes_joo/post/99c39d7a-3bf6-4873-9b0c-7bb8083c8665/image.png)

New Flutter Project를 클릭

![](https://velog.velcdn.com/images/nawhes_joo/post/41de945e-046f-409d-85fa-1b6791898756/image.png)

![](https://velog.velcdn.com/images/nawhes_joo/post/8eec0a4e-85a2-400f-ab6d-8c2e44051db1/image.png)

Project name : 프로젝트 명
Project location : 프로젝트 위치
Description : 설명
Organization : 조직
Android language : 안드로이드 언어 설정
Platforms : 플랫폼

목적에 맞게 작성하자.

![](https://velog.velcdn.com/images/nawhes_joo/post/51e3e7f7-309a-4b32-95b3-13aec5639547/image.png)


작성 후 Create를 누르면 프로젝트가 생성된다.

5. flutter doctor

> flutter doctor는 플러터 환경 구성을 검사하는 명령어이다.

좌측 아래 터미널을 클릭하여 flutter doctor를 입력한다.

![](https://velog.velcdn.com/images/nawhes_joo/post/5720f7cd-17e4-4500-b3d7-5d1029cd7732/image.png)

위와 같은 오류가 발생한다.

+ 에러 1) cmdline-tools component is missing
+ 에러 2) Android license status unknown
  + 이 경고는 toolchain이 연결되지 않았다는 오류, 라이센스를 허용하지 않았다는 뜻이다.
  
  ---
  5-1. SDK Manager에서 Command-line Tools 설치
  
![](https://velog.velcdn.com/images/nawhes_joo/post/977630c4-0f2a-4702-8815-77aa111ba732/image.png)

우측 상단에 톱니바퀴 버튼을 누르고 SDK Manager를 클릭한다.

![](https://velog.velcdn.com/images/nawhes_joo/post/564139a4-422d-42f1-9fca-7769abf58bfb/image.png)

Android SDK Command-line Tools (latest)를 클릭 후 Apply 버튼을 눌러 설치 후 OK 버튼 클릭.

5-2. 환경 변수 설정

![](https://velog.velcdn.com/images/nawhes_joo/post/b8e171a4-1ca6-496f-a29c-491e13587b56/image.png)

시스템 변수 섹션에서 새로 만들기 클릭.

![](https://velog.velcdn.com/images/nawhes_joo/post/e5d8a84b-e013-4fe0-9578-46d2523034a1/image.png)

변수 이름 : ANDROID_HOME
변수 값 : C:\Users\username\Appdata\Local\Android\Sdk

AppData는 기본적으로 숨김폴더이다. 보이지 않는다면 보기 설정에서 숨김폴더 보이도록 변경하면 된다.

![](https://velog.velcdn.com/images/nawhes_joo/post/486564b4-1a2e-4d92-8e51-503e81f6b8c7/image.png)

시스템 변수 섹션에서 Path 변수를 선택하고 편집을 클릭한다.

새로 만들기 or 찾아보기를 클릭하여

```
C:\User\username\Appdata\Local\Android\Sdk\platform-tools
C:\User\username\Appdata\Local\Android\Sdk\cmdline-tools\latest\bin
```

두 가지를 추가한다.

5-3 Android SDK 라이센스 동의

```
flutter doctor --android-licenses
```

Android Studio 하단의 Terminal에 위 명령어를 입력한다.

![](https://velog.velcdn.com/images/nawhes_joo/post/4ec2f2a4-e4f1-4473-a2f8-1bf5f7fb11e7/image.png)

위 사진처럼 뜬다면 Y를 눌러 라이센스를 검토할 수 있다.

![](https://velog.velcdn.com/images/nawhes_joo/post/4fd24354-c93e-4200-aadc-2a897444eb64/image.png)

Y를 누르니 위와 같이 라이센스를 보여준다.

Y를 한 번 더 눌러서 동의해주자.

5개의 라이센스를 동의한 후 

```
flutter doctor
```
명령어를 입력하면

![](https://velog.velcdn.com/images/nawhes_joo/post/948c338b-57d4-4602-b307-62a0766e84df/image.png)


오류가 해결된 것을 확인할 수 있다.

VSCode는 사용하지 않을 것이니 굳이 해결하지 않아도 된다.

---

#### Wear OS 개발 환경 설정

Android Studio에서 watch 앱을 개발하기 위해서 Wear OS를 필요로 하다.

(추후 공개)

---

### 테스트

#### 가상 기기

![](https://velog.velcdn.com/images/nawhes_joo/post/15b32ec0-adb5-4289-87b4-6e32a52ab116/image.png)

우측 Device Manager를 누른 후 Create Visual Device를 클릭

![](https://velog.velcdn.com/images/nawhes_joo/post/80b37c5d-9d15-46a9-ad9e-5b82b6e9bc5d/image.png)

원하는 기기를 선택한 후 Next

![](https://velog.velcdn.com/images/nawhes_joo/post/b806d8ce-c158-4be8-8a91-42dadd58b17d/image.png)

API 35가 기본으로 설정되어있다.

Tiramisu를 설치&선택 후 Next

![](https://velog.velcdn.com/images/nawhes_joo/post/432fe8ca-9b5c-4cad-8dca-5903cf67082a/image.png)

따로 건드릴 건 없다.

Finish~

![](https://velog.velcdn.com/images/nawhes_joo/post/cd7634d4-27a5-46ac-b56a-dce01c7cbb0a/image.png)


상단에 no device selected를 누른 후 방금 생성한 device를 클릭

만약 보이지 않는다면 Refresh를 누르면 된다.

![](https://velog.velcdn.com/images/nawhes_joo/post/c00b0f95-6838-4487-be8a-cae620dc14ab/image.png)

우측 Running Device를 클릭하면 Virtual Device가 보인다.

위 사진처럼 에뮬레이터가 안드로이드 스튜디오 화면 내에서 실행이 된다.

이렇게 사용해도 되지만 불편하다면 따로 뺄 수 있다.

![](https://velog.velcdn.com/images/nawhes_joo/post/8a79c7ef-2689-42ab-8442-91d52cbf4cf9/image.png)

Settings - Emulator에 들어가 Launch in the Running Devices tool window 체크해제 후 OK를 누르고 Android Studio를 재실행

![](https://velog.velcdn.com/images/nawhes_joo/post/3a5d3569-239d-4f2f-9696-7a562a08953e/image.png)

이렇게 밖으로 뺴낼 수 있다.


![](https://velog.velcdn.com/images/nawhes_joo/post/384d7583-deae-427e-9ecb-6ba80df7661d/image.png)

상단에 초록색 화살표를 클릭하면 기본으로 세팅된 어플이 실행된다.


![](https://velog.velcdn.com/images/nawhes_joo/post/0d3f54c6-5e87-45de-a2d8-35c5ea2451cf/image.png)

프로젝트 생성 시 기본으로 생성된 어플을 실행시킬 수 있다.