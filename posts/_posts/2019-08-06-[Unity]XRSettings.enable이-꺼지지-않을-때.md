---
layout: post
title: XRSettings.enable이 꺼지지 않을 때
categories: devlog
comments: true
description: >
 Unity에서 XRSettings.enable이 꺼지지 않는 문제가 발생
noindex: false
---

유니티에서 PC와 VR을 오가며 자유롭게 스위칭이 되는 프로그램을 만들어야하는 일이 있었다.
그 때, SteamVR 2.0을 사용했는데 XRSettings의 체크박스를 꺼도 다시 켜지는 문제가 발생했다.
알고보니 SteamVR 자체에서 Auto기능으로 이를 다시 켜주고 있었다.
이를 고치는 방법은 2018버전과 2019버전이 상이하다.

## 2018버전 기준

Windows → SteamVR input

- 우측상단에 Advanced Settings 클릭

![enter image description here](https://lh3.googleusercontent.com/nzFiK9vGUd5bt7S-aEWm5D_0bZC5ddhzF6CIWmikChdq0DNPxFZv48P7MZovhNAL0lG41Kila7Q)

- 우측하단에 SteamVR Settings 클릭

![enter image description here](https://lh3.googleusercontent.com/REX78VCBuBF32PRpz4bpZljxWgD6_6uhkA9Z_foS-Wu-9B3B7j193IEWYLWWQKS2Gx-yqk9qFYU)

- Inspector 상의 Auto Enable VR 체크해제

![enter image description here](https://lh3.googleusercontent.com/-Qj-Fz0ab-ieZKpykhhyan-lEtwub8Jr7i9Ki-VOkMlmWcmgv_jHjXz52zMvxSZiTmYOzmMuaUc)


## 2019버전 기준

Edit → Preferences... → SteamVR → Automatically Enable VR 체크해제
