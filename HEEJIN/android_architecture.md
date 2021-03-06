# 안드로이드 플랫폼 아키텍처

> 출처 : https://developer.android.com/guide/platform?hl=ko

Android는 다양한 기기와 폼 팩터에 사용할 수 있도록 제작된 Linux 기반의 오픈소스 소프트웨어 스택이다. 다음 다이어그램에서는 Android 플랫폼의 주요 구성 요소를 보여준다. 

![](https://developer.android.com/guide/platform/images/android-stack_2x.png?hl=ko)

___

## Linux 커널

Android 플랫폼의 기반은 Linux 커널이다. ART는 스레딩 및 메모리 관리와 같은 기본 기능을 수행하는데 있어 LInux 커널을 사용한다. 또한 LInux 커널은 모바일 컴퓨터 환경의 기반으로서 Android에 핵심 보안 기능을 제공하며, 이 Linux커널을 통해서 기기 제조업체가 널리 알려진 커널용 하드웨어 드라이버를 개발할 수가 있다.

___

## HAL(하드웨어 추상화 계층)

HAL(하드웨어 추상화 계층)은 상위 수준의 Java API 프레임워크에 기기 하드웨어 기능을 연결하는 표준 인터페이스를 제공한다. HAL은 여러 라이브러리 모듈로 구성되어있고, 카메라, 블루투스 모듈과 같은 특정 유형의 하드웨어 구성 요소를 위한 인터페이스를 구현한다. 프레임워크 API가 기기 하드웨어에 액세스하기 위해 호출을 수행하면 Android 시스템이 해당 하드웨어 구성 요소에 대한 라이브러리 모듈을 로드한다.![ape_fwk_camera2](https://source.android.com/devices/camera/images/ape_fwk_camera2.png)

 위의 그림은 카메라와 관련된 HAL 구성요소들이다. 여기서 HAL는 카메라 드라이버와 상위 수준의 Android 프레임워크 사이에 있으며 앱이 카메라 하드웨어를 제대로 작동할 수 있도록 구현해야 하는 인터페이스를 정의한다. 

___

## Android 런타임

Android 버전 5.0(API 레벨 21) 이상을 실행하는 기기의 경우, 각 앱이 자체 프로세스 내에서 자체 ART 인스턴스로 실행된다. ART는 dex 파일을 실행하여 저용량 메모리 기기에서 여러 가상 머신을 실행하도록 작성되었다. dex 파일은 Android의 최소 메모리 공간에 맞게 최적화되어 있다. Jack과 같은 툴체인을 빌드하고, Java 소스를 Android 플랫폼에서 실행될 수 있는 dex 바이트 코드로 컴파일한다.

> **Jack**
>
> 자바 소스를 Android dex 바이트 코드로 컴파일한 Android 도구 모음이다. Jack을 사용하기 위해서는 다른 방식으로 작업할 필요 없이 표준 makefile 명령어를 사용하여 트리나 프로젝트를 컴파일하면 된다. Android 8.1은 Jack을 사용하는 마지막 버전이다.

ART의 주요 기능 중 몇 가지를 살펴보자면 다음과 같다.

- AOT 및 JIT 컴파일
- 최적화된 가비지 컬렉션(GC)
- 전용 샘플링 프로파일러, 상세 진단 예외 및 크래시 보고, 디버깅 지원 기능

Android 버전 5.0 이전 버전에서는 Dalvik이 Android 런타임이었으나 앱이 ART에서 제대로 실행되면 Davik에서도 제대로 실행되지만, Dalvik에서 앱이 실행된다고 ART에서 실행될것이라는 보장은 없다.

또한 Java API 프레임워크가 사용하는 몇가지 Java 8 언어 기능을 포함하여 대부분의 Java 프로그래밍 언어 기능을 제공하는 핵심 런타임 라이브러리도 포함되어있다.

___

## 네이티브 C/C++ 라이브러리

Android 시스템 구성 요소와 서비스가 C 및 C++로 작성된 네이티브 라이브러리를 필요로 하는 네이티브 코드를 기반으로 빌드되었다. Android 플랫폼은 Java 프레임워크 API를 제공하여 이러한 일부 네이티브 라이브러리의 기능을 앱에 노출한다.

C 또는 C++ 코드가 필요한 앱을 개발하는 경우에는 Android NDK를 사용하여 네이티브 코드에서 직접 이러한 몇몇 네이티브 플랫폼 라이브러리르 액세스할 수 있다.

___

## Java API 프레임 워크

Android OS의 전체 기능 세트는 Java 언어로 작성된 API를 통해 액세스가 가능하다. 이 API는 시스템 구성 요소 및 서비스 재활용을 단순화하여 Android 앱을 제작하는 필요한 빌딩 블록을 구성한다. 빌딩 블록에는 다음이 포함된다.

-  뷰 시스템 - 목록, 그리드, 텍스트 상자, 버튼 및 삽입 가능한 웹 브라우저를 포함하여 앱의 UI를 빌드하는데 사용
- Resource Manager - 현지화된 문자열, 그래픽 및 레이아웃 파일과 같은 리소스에 대한 액세스
- Notification Manager - 모든 앱이 상태 표시줄에 사용자 지정 알림을 표시할 수 있도록 지원
- Activity Manager - 앱의 수명 주기를 관리하고 공통 탐색 백 스택 제공
- 콘텐츠 제공자 - 앱이 주소록 앱과 같은 다른 앱의 데이터에 액세스하거나 자신의 데이터를 공유할 수 있도록 지원

개발자는 Android 시스템 앱이 사용하는 것과 동일한 프레임워크 API에 대한 전체 액세스 권한을 가진다.

___

## 시스템 앱

Android는 이메일, SMS 메시징, 캘린더 등의 주요 앱 세트와 함께 제공된다. 플랫폼에 기본적으로 포함된 앱에는 사용자가 설치하도록 선택하는 앱과 구별되는 특별한 상태가 없다. 따라서 시스템 설정 앱 등의 몇 가지 예외를 제외하고는 타사 앱이 기본 웹 브라우저나, SMS 메시징, 또는 기본 키보드가 될 수도 있다.