---
layout: post
title: 캣트릭스 Unity Webinar 리뷰
category: devlog
tags: [review, unityLive]
image: https://user-images.githubusercontent.com/47441135/100401980-69a02100-309e-11eb-9102-35240c9c4f4b.jpg
description: >
 Unity Korea 유튜브 라이브로 진행된 Webinar의 내용을 정리하고 간단한 소감을 이야기합니다.
comments: true
---

* this unordered seed list will be replaced by toc as unordered list
{:toc}

2020.04.03 진행된 Unity Korea Youtube 채널 라이브 Webinar를 듣고 간략하게 정리해보았다.  
이전에 언리얼에서 진행되었던 Webinar도 시청하였으나 게임이나 소프트웨어 개발자보다는 건축과 애니메이션, 영화 등에 치우쳐 있어서 아쉬웠던 기억이 났다.  
그에 비해 이번 캣트릭스 유니티 Webinar는 나의 관심분야와 딱 맞아 떨어져 즐겁게 들을 수 있었다.  
특히 인디게임 개발자라면 누구나 고민할 법한 최적화와 그래픽 부분을 중점적으로 짚어준 점,  
그리고 대략적인 방법에 대해 훑고 넘어가는 것이 아닌 구체적인 수치를 이야기해주어서 상당히 유익했다.  

코로나19 때문에 웹캠으로 진행되었지만 굉장히 매끄러웠고 만족스러운 웨비나였다.  
라이브 방송은 이곳에 올라와있다.  
[https://youtu.be/9voqMeOGW4A](https://youtu.be/9voqMeOGW4A)

# 모바일 최적화

## 모델링

- Batch

    갤S2: 85 batches  
    갤S3: 178  
    갤S4: 215  
    갤S5: 260  

- 리소스 최적화

    모델링끼리 합쳐도 되는 부분은 합치기  
    맵의 오브젝트를 하나로 merge  

- Polygon Count

    주인공 4000 ~ 6000  
    비주인공 2000 ~ 3000  
    잡캐 800 ~ 1000  

## 화면설정

- Resolution

    SD(480x640): 100%  
    HD(720x1280): 225% → 기준  
    FHD(1080x1920): 400% → 옵션 선택  
    UHD(1440x3840): 900%  

- Frame Rate

    타겟 프레임레이트 30 ~ 45  
    로비 등에서는 20 ~ 30  
    전투는 30 ~ 45  
    기기 성능에 따라 조절할 수도 있다  

## UI

- 아틀라스도 종류별로 구분 (메인끼리, 창끼리..)
- UI창이 화면을 모두 가리면 뒤에 있는 오브젝트는 모두 비활성화하기  
→ 이 과정에서 OnEnable OnDisable 을 잘 처리하자
- 레이어도 너무 많이 쓰지 말자. 모두 Draw call 소비
- ui 이미지 포맷 ASTC
앱플레이어 지원시 ASTC 설정을 켜주어야한다  
안드로이드의 경우는 OpenGL 3.2 이상 혹은 OpenGL ES 3.1 + AEP(Android Extension Pack)이 지원되어야합니다. iOS는 A8 processor를 사용하기 시작하는 기종부터 사용이 가능합니다.  
[https://ozlael.tistory.com/84](https://ozlael.tistory.com/84)
[오즈라엘]

---

# 테크니컬 사이드

## 툰렌더 주의사항

- Outline Pass
원근교정을 사용하여 균일한 외곽선 두께 사용.  
버텍스 컬러페인팅으로 원치않는 부분 외곽선 제거(및 두께조절)  
- Opaque Pass
Ramp 텍스처로 단계별 명암 조절  
림라이트로 양감 조절  
- Shadow Pass
Realtime Planar Shadow를 멀티패스로 구현: 일정한 Y축에 그림자 생성  

## 타일링 텍스처

셀 노이즈 텍스처로 바닥이 불규칙적인 높낮이를 가진 것처럼 표현

![GreyscaleCells](https://user-images.githubusercontent.com/47441135/79189600-1fdf2f00-7e5d-11ea-9781-882bf0f1abdd.png)

## 스캔라인 이펙트

캐릭터가 가리지 않도록 주의

## 스테이지 디졸브 이펙트 셰이더

로비→스테이지 변경 시
타일모양으로 디졸브 쉐이더

## 원형 ProgressBar

[https://github.com/AdultLink/RadialProgressBar](https://github.com/AdultLink/RadialProgressBar)

![https://github.com/AdultLink/RadialProgressBar/blob/master/Screenshots/AllInterfaceExamples.gif?raw=true](https://github.com/AdultLink/RadialProgressBar/blob/master/Screenshots/AllInterfaceExamples.gif?raw=true)

## GrabPass 왜곡(굴절) 셰이더

일렁이는 효과. 텍스쳐로 영역 지정, 가운데를 비워놓은 상태로 스케일값 조절

## 등장 사망 시 디졸브 이펙트 추가

## 모바일 셰이더/파티클 주의사항

- 파티클 이펙트에서 라이트 빼기 (모바일에서 라이트는 사치다!)
- 셰이더도 PC용을 그냥 변환하면 예쁘게 안나온다.

# Q&A

## 서버구축

뒤끝서버: [https://www.thebackend.io/](https://www.thebackend.io/)
