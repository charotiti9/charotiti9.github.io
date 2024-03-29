---
layout: post
title: 유니티 2020 7월 라이브
category: devlog
tags: [review, unityLive]
image: https://user-images.githubusercontent.com/47441135/100402355-66f1fb80-309f-11eb-9c6b-a8c1e2d520d0.jpg
description: >
 Unity Korea 유튜브 라이브, 유니티 2020 변경된 내용 훑어보기 
comments: true
---

7월에 진행한 2020 라이브 방송을 보며 가볍게 정리한 내용을 공유합니다. 집중을 위해 영상을 보며 작성한 내용입니다. 영상 시청 후에 아주 살짝 가공하기는 했지만 자세한 내용까지 적혀 있진 않습니다.

살펴보시고 관심이 있는 부분이 있다면, 전체 영상에서 해당 내용이 나오는 시간대를 링크해드렸으니 클릭하시면 바로 볼 수 있습니다.
  
* this unordered seed list will be replaced by toc as unordered list
{:toc}
  
전체적으로 2020에 바뀐 부분을 훑고 넘어가주는 방송이었습니다. 큼지막한 것을 소개해주고 이후 자세한 내용은 찾아보도록 유도해 준 것이 밸런스가 잘 맞다는 생각이 들었습니다.  
특히 놀란 것은 애니메이션 리깅입니다. 대규모 팀에서 일하는 것이 아니라면 애니메이터도 아트도 없는 경우도 있는데 그럴 때 굉장히 유용하게 쓰이지 않을까 합니다. 갑자기 문득 생각이 드는게, 분명히 유니티는 협업을 위한 방향성을 추구하는데 사회에서는 '와! 이제 혼자서도 다 할 수 있겠다!'로 받아들여지는 것 같아서 고맙기도 하고 씁쓸하기도 합니다.  
괜한 사족이 붙었네요. '뜯어봐야겠다'고 생각이 든 예제는 역시 ML-Agent와 Unity-Chan HDRP네요. ML-Agent는 예전에도 살짝 본 적도 있고, 당장 어딘가에 적용할 곳은 없지만 익혀둬야 할 느낌이고, Unity-Chan HDRP는 보고 배울 점이 아주 많을 것 같고 당장 적용시킬 수 있을거란 생각이 듭니다.  
Bolt는 친한 친구에게 추천해주고 싶어요. 자신만의 게임을 만들고 싶다는데 어떻게 접근해야할지 모르겠다는 친구였거든요. 언리얼을 추천해주기엔 너무 무거웠는데 유니티가 딱 알맞게 Bolt를 무료로 제공해주네요.
  
  
영상링크: 
[유니티 라이브 2020 7월(Unity Korea Live, July)](https://youtu.be/CulhcKKJ-CY)

# Animation Rigging

영상 시작 시간 ([16:30](https://youtu.be/CulhcKKJ-CY?t=989))

참고 링크:
[고급 애니메이션 리깅: 캐릭터와 프랍 상호작용 - Unity Technologies Blog](https://blogs.unity3d.com/kr/2019/09/05/advanced-animation-rigging-character-and-props-interaction/)

## 특징
![](https://lh3.googleusercontent.com/RN9P8I-kkMVa9LfdgeQi8-Ig0GOObsAxknVbMrYkLVDBXY146_Q7FjTNzPNzwy6ivJPfvT6LSgJ2hGeT4-ZFh1bf0221dYnpsGkIda2Fmxo3FMh966Z_5oygzS9OkIY3QtABDnoSfA=w2400)

- 아이템에 따라 애니메이션 모션 수가 많아지는 문제를 해결할 수 있음  
    기본적으로 만들어진 애니메이션 하나로 리깅을 조금씩 바꿔서 적용할 수 있다  
    ex) 아이템의 몸체를 잡고 있는 애니메이션이 있다면 핸들을 잡도록 변경하여 사용

- 런타임 기반. 플레이가 될 때 그 장면으로 프리뷰하고 키프레임을 잡을 수 있다. 플레이 도중 움직이면 그대로 반영된다.  

## 활용처
![](https://lh3.googleusercontent.com/OuZsnm3Yt9RWSjQhZFntkeX99xvFPXv9PVm0Sp4u7dmwmtLmzaEx1-ZB3NDunv6iQJCpetTPEHZu6wijl5gyuhtio3yp7YKNWUhoclaqhFHmIh5f9uz_fnBp0swJ-zWXMaZzJ4wMaQ=w2400)

### Multi Parent
- 허리에서 손으로, 여러개의 부모를 자유롭게 사용할 수 있음

### Override Transform
- 기본적으로 있는 모션에 새로운 모션을 만들어 덮어쓸 수 있음
- float으로 mix가 가능하도록 만들어졌다

### Multi-Aim
- 타겟팅을 따라 자유롭게 이동, 코드가 아닌 애니메이션으로 편리하게 활용 가능하도록 바뀌었다

### Multi-Pivot Point
- 여러개의 피봇포인트를 사용 가능
- 애니메이션과 동시에 사용 가능
- 프로그래밍, 애니메이션, 타임라인 모두에서 적용 가능  


## 2019 → 2020 업데이트
자동화 기능이 강화되었다
- Animation Rigging→Bone Renderer Setup: 유니티에서 본을 시각화
- Animation Rigging → Bone Setup: 기본적인 셋팅 자동화  


# Prefab System
영상 시작 시간 ([39:00](https://youtu.be/CulhcKKJ-CY?t=2341))

![](https://lh3.googleusercontent.com/5l26BzqexPoyK_W3YFnNbe8Z8H_cX_83OEPEKNuvKr7phGHZ_w9QzmAgtrvxR5fQYyOrIPMC_caT-NW3EufgzvwcqM1EIBGs3QJOohZZWorRpYkDV8ZhavRgilopLS_XTg55e2BbEA=w2400)  
기존 프리팹 에디터를 유지하면서도, 씬을 보며 프리팹이 수정가능하도록 바뀌었다

![](https://lh3.googleusercontent.com/XY-lhVtboANTSZlF1ITMRWet18vSH6USBjv7373vc6n9CoBLZpzlgk_xz8JgIgfJutCfU45SV1sXsO_Pyjn8q7aulYIsyVr3iFt_NYNXeNt4WSGVW7QjHskTpjjNtb2NeZOEkB2OnA=w2400)  
기존 프리팹 에디터로도 열 수 있고, 씬과 함께 보려면 인스펙터 창에서 Open을 통해 들어가야 한다.

# Lightmap

영상 시작 시간 ([40:00](https://youtu.be/CulhcKKJ-CY?t=2402))

## Lighting Settings 관리

![](https://lh3.googleusercontent.com/ymZ4YeAdBAUd9KP2Xq2z1qkPodVPjRAqwH42zN-OiO-h2AhWxRZd2du-0VW_JV8ShHd28g5t7LQc8Cvjvikk_RxZ9UoXP_KxJpVXjIEA4nZ_KAy5WwOwMNKK_l6__Pk1xVE7N6m0sA=w2400)  
Lighting Settings을 프로필로 관리 가능하도록 변경되었다

이전에는 한 Scene에 Lighting Settings이 종속되어 있어 다양한 씬에 적용이 불가능 했었는데, 이제 한 Scene에서만 셋팅하면 동일한 셋팅값으로 여러 Scene에서 적용 가능하다

## Lightmapper
![](https://lh3.googleusercontent.com/26goSdv5dgqHiAX3KsEL3Hgs-oxrRvNKt3arnPbn2aORzRT07MOwoeaXDxhZUZbBCzVMgBNfqBEoFpGbvcU4viAi-tniiMJFsoFKIWhV_6KTDofe6FbsMgQLEbtqIpfQrzbjaE7HKw=w2400)  
Lightmapper → Enlighten은 곧 삭제 예정이기 때문에 Progressive를 쓰는 것을 권장

## Lightmap Parameters

객체 Mesh Renderer에서 Lightmap Parameters 중 기본으로 제공하는 것 외에 새로 만들 수 있다  

![](https://lh3.googleusercontent.com/T2g2ag403kmGehC6_wTbEqn-oOLOm_K1sLN5UZJDOyXbMKFvNV3ow4SpdGFBTTwYzX94ve174l6JJj5vTHkh_2HSP80wC246hltfvmV3j6-O7tUqeaEjAc6Geea6tnIkE6fGV983Pg=w2400)

Baked Tag를 같은 번호로 지정하면, 그 Scene에 존재하는 객체들 중 같은 번호를 가진 객체들은 하나의 Lighting Texture로 bake한다  
마찬가지로 프로필로 관리한다

# Bolt

영상 시작 시간 ([1:10:15](https://youtu.be/CulhcKKJ-CY?t=4217))

참고 링크:
[Bolt visual scripting is now included in all Unity plans](https://blogs.unity3d.com/kr/2020/07/22/bolt-visual-scripting-is-now-included-in-all-unity-plans/)

### 알려진 정보

- Package Manager 추가예정
- Bolt2 까지 지원예정
- Custom Node 기능 지원: BluePrint(언리얼) = Custom Node인 셈
- Unity API명이 그대로 노출이 되기 때문에 이해하기 쉽다

## 사용용도

프로토타입 제작  
or  
Custom Node로 시스템 개발 후 후속 콘텐츠 제작

## 간단한 사용법

- 객체 생성 후 Flow Machine 컴포넌트 추가  
![](https://lh3.googleusercontent.com/pI7wi7jhJIX6mnQ1YLPf3dYYWuIdSuqbgbw0IMTYgGGky0z3CMKzTyKCCSKtiaqSHO7FKW3YxcG6rbaOnniqXJbWE2WPxdUC2gXLoT7lYC_gW3XkOSQNkzzalAXPGOzMavnlz4boiw=w2400)

- Macro → New로 새로운 플로우 생성 ex) AutoRotate

- 업데이트 이벤트에서 노드를 뽑아 Rotate 검색 후 float값 입력  
![](https://lh3.googleusercontent.com/uL1s7U1wJszHVa5NQUiwyICkFLZ1DMid0uc-PY27T46hwGCumns6I8hYmNmjn3D6j8Rciv0IZmTfoUWd1iviuB-kTevrZyUr8SauiU0US4UbouFyR9B9ufsky6T5l52FLyJHeCSmsw=w2400)

- 분기(if문)은 branch  
![](https://lh3.googleusercontent.com/HYM6Ug4EukxAsz_ITokjOMjq5a9OnuJeVN2VRaJa4RlzZ4R7EAQkm4rCwcw0J6u3iTPBa9CET-5lzQZAheertltAKh7OnV1eig3_z5zWwwjmLzBPufn04_OGyFYFYO-v7_Qk4RjPpg=w2400)

스페이스가 눌리면 돌지 않도록 만듦 

### 커스텀 노드

Tools→Bolt→Build Unit Options  눌러서 업데이트  
만든 스크립트명을 검색하면 추가 가능  
자세한 사항은 샘플 프로젝트나 유니티런 참고

# ML-Agents Package v1.0

영상 시작 시간 ([1:29:40](https://youtu.be/CulhcKKJ-CY?t=5382))

참고 링크:
[ML-Agents Unity 패키지 v1.0 출시 - Unity Technologies Blog](https://blogs.unity3d.com/kr/2020/05/12/announcing-ml-agents-unity-package-v1-0/)

![](https://lh3.googleusercontent.com/EfICHilcLiSygnYXSZ9S7qDCLbM4wQH3fJfhjpccZHZ-IosHZPRpTvBkKju5yvTg2yWJ1MD75rNBnzjbL9zDW6ZGSJyv0SHpo90N-wfWDzfvjl5xWnqLJGR7pkq4COK_S7f88Q5d9w=w2400)  
![](https://lh3.googleusercontent.com/jMd0ZHuA4xPZdZpRWJLcWAbVp05ub48m8XFdmjqbjwNOjYmwQZUBIlT3IbaNFuVyhLJE-TuYr2NNlqEZURDd6iUjSncqZPL-aw2ZjyEBQfYsWgVRI9h9ghkIVsJVHTBsS6QghrGf-A=w2400)  
Joint로 연결된 관절의 움직임을 학습

![](https://lh3.googleusercontent.com/PLC3P83B1gvPKfWHEQibXY50A4R0ByEcc5Aq5CYS-Qebg5ijYpGVKpX971xeo4mgeAJDa8qzZ4ybfsIs5UzdfNBqNQsY_nyfIjaCcRp7jJpBkEcIZ5Bb8TgsFFOdoGXui1FTy8Ua2Q=w2400)  
옛날에 봤을 때보다 훨씬 귀여워진 모습  
내가 유니티를 아는데 다음 버전에서는 여기에 얼굴 표정도 생길듯

![](https://lh3.googleusercontent.com/RXt9iCMo7gaZbHvYhi7BNrTiOpgsuR36lOUmuU8dyOTgQhMZkaq21czNdwy5kMJ09O7rlPaYt7aGKlOE2EYrzSDlE36XwdGW7nqgPFswXVnjD8zSXRogNRxTutyCeI-hwzhJ-TLQuw=w2400)  
- 공의 위치
- 공의 속도
- 면의 기울기
값을 가져온다

![](https://lh3.googleusercontent.com/_RmATp3r8mUV4DJBVMuoM0xgv736m7qsO6Q99NjGehgYoTDRN7HrT_MpNKeiFmvBuut0WBCdu4r89DqsqSA2k2GowUiyujfascZ0K5ZoKyMEj05NeyTuaDQTX7OaYfbcOEy9LhxnRA=w2400)  
공을 떨어트렸을 땐, 마이너스 리워드를 준다

![](https://lh3.googleusercontent.com/oRZ3EKg4FLMO7y7X7i2XGl6xVRryHeG1HJwEj46SBamqVzeTSOIFoEX9n-38cvdpVxnwB82FIGU7UjtNKCZThnhehBOIfmhttedpyB4xvr5NohD5kAlNXVq99p4n-1kcK98837r1yA=w2400)  
아니면 리워드를 준다  
리워드는 지속적으로 값이 들어오니 0.1f만 준다


# AR Foundation - MARS

영상 시작 시간 ([1:48:16](https://youtu.be/CulhcKKJ-CY?t=6496))

참고링크: 
[MARS 프로젝트 - Unity Technologies Blog](https://blogs.unity3d.com/kr/2019/10/02/labs-spotlight-project-mars/)

개발하고 빌드하고 폰에 넣어서 테스트하고  
→ 시간을 단축하기 위해, 유니티 안에서 테스트 할 수 있도록 지원해준다

![](https://lh3.googleusercontent.com/CLQh8jhmwEnAUSsysTHlF5TiVTK1lNtOPmZ0JI7vWrOTtK5XDiHUaEi8rp6WerysvLU4Cn2jkE_BcNgAzC-HBLK1WoL-H0R3mPdHssvPqybu-vzk9ho1hwibOk5MnfawLHxNHLDE9Q=w2400)  
가상환경에서 바닥을 인식하고 가상 공간에서 테스트 할 수 있도록 한다


# 그 외,

## Unity-Chan HDRP

영상 시작 시간 ([57:00](https://youtu.be/CulhcKKJ-CY?t=3439))

참고링크: 
[★遂に登場！HDRPとURPで使えるユニティちゃんアセット](https://unity-chan.com/contents/news/3878/)

## Unite now

영상 시작 시간 ([1:25:45](https://youtu.be/CulhcKKJ-CY?t=5145))

참고링크: 
[Unite Now 2020: An online conference for real-time development | Unity](https://resources.unity.com/unitenow)

온라인으로 유나이트 진행중  
8, 9월달에 한글로 번역한 영상 공개 예정  
AI AR이 가장 인기 많았다고 한다

