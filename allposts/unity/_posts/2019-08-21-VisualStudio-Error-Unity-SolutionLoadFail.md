---
layout: post
title: Visual Studio 오류 Unity 솔루션 로드 실패
tags: [untiy, visual studio]
description: >
 비주얼 스튜디오를 이용하여 유니티 스크립트를 열 때 솔루션을 제대로 불러오지 못할 때 해결법
comments: true
---

가끔 유니티에서 비주얼 스튜디오를 이용하여 스크립트를 열 때, class나 component들을 읽어오지 못하거나, 자동완성을 지원해주지 않거나, 오류인데도 빨간 밑줄을 쳐주지 않는 경우가 있다. 흔한 visual studio 오류로 보통 껐다가 켰을 때 해결되곤 하는데 그러지 않는 경우도 종종 발생한다.

이것 때문에 퇴근 후 개인프로젝트가 얼마나 막혀있었는지 모른다. 비주얼 스튜디오를 다시 깔아보기도 하고, *.csproj이나 *.sln파일을 지웠다가 다시 생성해보기도 했었지만 문제가 해결되지 않았다.

해결방법은 단순하다.
Unity에서 Edit -> Preferences -> External Tools에서
External Script Editor를 Visual Studio로 설정해주니 솔루션을 잘 찾아왔다.

![](https://lh3.googleusercontent.com/1xiqvAhb5TXm9Iu51fbtrFsgdC_PJx4QxlK4igo4DUz2ecvY9R7dhLMHHAyui9vOKS1bk1m2eiKfyOvvBaglLh5krb7CFJq82LjdqovxSotNuqbgYrti-o4XUizpli2EgHqLrXYW_A=w2400)

이걸로 해결되지 않는다면 에디터를 Mono로 바꿔서 Script를 한 번 열었다가 다시 Visual Studio로 전환하여 열어보라는 설명도 보았다.

막힘없는 개발을 위해 화이팅!
