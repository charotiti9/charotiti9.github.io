---
layout: post
title: 안드로이드 스튜디오에서 플러그인을 만들어보자
category: devlog
tags: [study]
image: /assets/img/blog/study/AndroidPlugin.png
description: >
 안드로이드 스튜디오에서 플러그인을 만들고, 이를 유니티에서 어떻게 호출하는지 간단하게 정리했습니다.
comments: true
---
참고: 안드로이드 스튜디오 헤지혹 버전으로 진행했습니다.

# 프로젝트 셋팅

새 프로젝트를 만듭니다.
바로 모듈을 만들수가 없어서 Empty Activity로 만들어줍니다.
어차피 app은 지울거라서 아무거로나 만들어도 상관없습니다.

![](/assets/img/blog/study/Pasted%20image%2020240222152427.png)

File>New>New Module... 을 선택해줍니다.

![](/assets/img/blog/study/Pasted%20image%2020240222152530.png)

Android Library를 선택하고 이름을 지어줍니다.
Minimum SDK에서 정한 버전이 유니티에서도 Minimum SDK로 설정해야하니 주의.

![](/assets/img/blog/study/Pasted%20image%2020240222152701.png)

기존 앱이 거슬리니까 지워줍니다
File > Project Structure... 을 클릭해서 
![](/assets/img/blog/study/Pasted%20image%2020240222152906.png)

Modulse 탭에서 app을 우클릭하여 Remove Module 해줍니다. 

![](/assets/img/blog/study/Pasted%20image%2020240222152951.png)

# 유니티 사용할 수 있게 셋팅

아래의 경로로 들어가봅니다.

`C:\Program Files\Unity\Hub\Editor\2021.3.22f1\Editor\Data\PlaybackEngines\AndroidPlayer\Variations`

이 아래에 il2cpp와  mono 파일이 있을텐데 본인의 유니티 프로젝트 설정에 맞게 선택합니다.  
저는 il2cpp를 사용하기 때문에 il2cpp폴더로 들어갔습니다.

`~\il2cpp\Release\Classes`

이 안에 classes.jar 파일이 있습니다. 이걸 복사해줍니다.

![](/assets/img/blog/study/Pasted%20image%2020240222153541.png)


다시 안드로이드 스튜디오로 돌아옵니다.
Project 를 누릅니다.

![](/assets/img/blog/study/Pasted%20image%2020240222153059.png)

그러면 이렇게 뷰가 바뀌는데, 만들 모듈의 libs쪽에 아까 복사한 classes.jar를 넣어줄 겁니다.

![](/assets/img/blog/study/Pasted%20image%2020240222153222.png)

이렇게 들어간 것을 확인해주세요.

![](/assets/img/blog/study/Pasted%20image%2020240222153702.png)


다시 Android로 뷰를 바꾸고 Gradle Script > build.gradle (Module: ~)을 열어주세요.

![](/assets/img/blog/study/Pasted%20image%2020240222153740.png)

아래의 코드를 추가해주세요.

```
dependencies {  

    // ...
   
	compileOnly files('libs\\classes.jar')  
}
```

그리고 우측 상단에 Sync Now를 눌러주세요.

![](/assets/img/blog/study/Pasted%20image%2020240222153953.png)

그럼 아까 Project 뷰에서 classes.jar가 폴더처럼 바뀝니다.

![](/assets/img/blog/study/Pasted%20image%2020240222154022.png)


# 모듈 빌드하기
자바 클래스 하나를 만듭니다.

![](/assets/img/blog/study/Pasted%20image%2020240222154534.png)

Build > Make Module ~을 누릅니다.

![](/assets/img/blog/study/Pasted%20image%2020240222154649.png)

다음에 경로에 저장됩니다.

`~\프로젝트명\모듈명\build\outputs\aar`

# 유니티에서 사용하기

Plugins>Android안에 aar 파일을 넣습니다.

![](/assets/img/blog/study/Pasted%20image%2020240222154959.png)

다음의 코드로 사용이 가능합니다

클래스 불러오기

`new AndroidJavaClass("com.example.모듈명.클래스명");`

클래스의 함수 호출

`불러온 클래스.Call("함수명");`

반대로 유니티의 함수를 안드로이드 코드에서 호출하고 싶다면 이렇게 한다.

`UnityPlayer.UnitySendMessage("게임오브젝트명", "함수명", "인자");`

