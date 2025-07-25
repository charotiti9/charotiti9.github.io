---
layout: post
title: Xcode에서 ios 플러그인을 만들어보자
category: devlog
tags: [study]
image: /assets/img/blog/study/xcode.png
description: >
 Xcode에서 ios플러그인을 만들고, 이를 유니티에서 어떻게 호출하는지 간단하게 정리했습니다.
comments: true
---
참고: Xcode 15.3 버전에서 진행했습니다.

# 프로젝트 셋팅

![](/assets/img/blog/study/Pasted%20image%2020240325175225.png)

- Xcode를 열고 새로운 프로젝트를 생성합니다.
- Framework & Library 중에서 Static Library를 선택합니다.
- 프로젝트 이름과 기타 옵션을 설정하고 프로젝트를 생성합니다.

# iOS 코드

![](/assets/img/blog/study/Pasted%20image%2020240325102748.png)

- 프로젝트에 새로운 Objective-C 클래스를 추가합니다.
- .m 으로 되어있는 확장자를 .mm으로 바꾸어 Object-C++ 클래스로 변경합니다.

```object-C++
extern "C"{
    const char * getSimpleString(){
        return "Simple String from iOS Plugin";
    }
}
```

위의 코드를 입력합니다.

# 플러그인 빌드

![](/assets/img/blog/study/Pasted%20image%2020240405172657.png)

빌드 타겟을 Any iOS Device로 설정


![](/assets/img/blog/study/Pasted%20image%2020240325182159.png)

- Product' > 'Scheme' > 'Edit Scheme'을 선택하고, 'Build Configuration'을 'Release'로 설정
  (프레임워크를 공개 배포할 경우 Release 모드에서 빌드하는 것이 일반적입니다.)

![](/assets/img/blog/study/Pasted%20image%2020240325141947.png)

- 'Product' 메뉴에서 'Build'를 선택하여 프로젝트를 빌드합니다.

![](/assets/img/blog/study/Pasted%20image%2020240325182249.png)

- Product > Show Build Folder in Finder
- 그러면 파인더가 나오고 Build>Products>Release-프로젝트폴더가 있고
- 그 안에.a 파일과 .h 파일이 있습니다. (.h 파일은 include 폴더 안에 있음)

![](/assets/img/blog/study/Pasted%20image%2020240405150325.png)

- include는 그냥 폴더 째로, .a와 함께 유니티의 Plugins>iOS 폴더 안에 배치시킵니다.

# 유니티에서의 활용

```cs
using System.Runtime.InteropServices;
using UnityEngine;

public class PluginTester : MonoBehaviour
{
    [DllImport("__Internal")]
    private static extern IntPtr getSimpleString();

    void Start()
    {
        string str = Marshal.PtrToStirngAnsi(getSimpleString());
        Debug.Log(str);
    }
}

```

유니티 코드는 이런 식으로 해주면 됩니다.
- DLLImport("Internal") 을 이용하여 불러올 것
- 중요한 점은 const char * 를 string으로 변형해주는 코드.
  이게 없으면 오류가 납니다.

# 유니티에서의 iOS 빌드

![](/assets/img/blog/study/Pasted%20image%2020240405160110.png)

- 플랫폼을 iOS로 해서 빌드해줍니다.

![](/assets/img/blog/study/Pasted%20image%2020240405160240.png)

Unity가 빌드를 완료하면, 지정한 위치에 Xcode 프로젝트 폴더가 생성됩니다.
이 폴더를 찾아 `.xcodeproj` 파일을 더블 클릭하여 Xcode에서 엽니다.

![](/assets/img/blog/study/Pasted%20image%2020240405160349.png) 

Xcode의 프로젝트 설정에서 개발자 계정을 설정합니다. 
`Signing & Capabilities` 탭에서 `Team`을 선택하거나 추가합니다.

  위의 그림처럼 두가지 에러가 뜨면
1. Unable to process request - PLA Update available 
   : 이 에러는 Apple 개발자 프로그램 라이선스 계약(Program License Agreement, PLA)에 업데이트가 있고, 그것을 검토하고 동의해야 함을 나타냅니다.
2. 2. No profiles for com.DefaultCompany.PluginTest were found
   : 이 에러는 Xcode가 지정된 앱 ID에 대한 유효한 프로비저닝 프로파일을 찾을 수 없음을 나타냅니다. 

1을 해결하니 자동으로 2도 해결이 되었습니다.

![](/assets/img/blog/study/Pasted%20image%2020240408142922.png)

- 이렇게 'Apple Development: 내 이름' 키체인 로그인을 하라고 하면 
  이 컴퓨터에 '로그인한 계정의 처음으로 입력했던 비밀번호'를 쳐봅니다. 
  (이거 때문에 많이 헤맸음...)

# 테스트하기

![](/assets/img/blog/study/Pasted%20image%2020240408143010.png)

- 이제 USB로 iPhone을 연결합니다
- 위에 타겟을 연결한 iPhone으로 변경해줍니다
- 플레이 버튼을 누릅니다




  
