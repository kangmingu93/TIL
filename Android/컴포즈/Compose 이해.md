# Compose 이해   

https://developer.android.com/jetpack/compose/mental-model?hl=ko   
https://blog.crazzero.com/184   
https://developer.android.com/jetpack/compose/preview?hl=ko

* Jetpack Compose는 Android를 위한 현대적인 선언형 UI 도구 키트입니다. Compose는 프런트엔드 뷰를 명령형으로 변형하지 않고도 앱 UI를 렌더링할 수 있게 하는 선언형 API를 제공하여 앱 UI를 더 쉽게 작성하고 유지관리할 수 있도록 지원합니다. 이 용어에 관해 몇 가지 설명이 필요하며, 앱 디자인에 있어 중요한 함의를 갖습니다.

* Jetpack Compose는 최근 안드로이드 진영에서 밑고 있는 선언형 UI Toolkit인데, Swift에 SwiftUI가 있다면 Android에는 Compose가 있다고 말할수 있다.   
Compose는 프론트엔드 뷰를 명령형(imperatively)으로 변형하지 않고도 앱 UI를 렌더링할 수 있게 하는 선언형(declarative)API를 제공하여 앱 UI를 더 쉽게 작성하고 유지관리할 수 있도록 도와주는데, XML에 익숙한 개발자라면 Compose 스타일에 익숙해지는데 시간이 걸릴 수 있다.   

<br>
<br>

## The Declarative Programming Paradigm (선언형 프로그래밍 패러다임)   

* 기본적으로 Android에서는 사용자와의 상호작용 등을 이유로 인해서 앱의 상태가 변경되면, 현재 데이터를 표시하기 위해서 UI계층 구조를 업데이트 하는데, 여기서 업데이트를 하기 위한 뷰 계층 구조는 트리 형태로 표시할 수 있고, UI를 업데이트하는 가장 일반적인 방법은 findViewById()와 같은 함수를 사용하여 해당 트리를 탐색한 후, button.setText(String), container.addChild(View) 또는 img.setImageBitmap(Bitmap)과 같은 메서드를 호출하여 노드를 변경하는 것이다. 