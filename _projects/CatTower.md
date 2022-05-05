---
layout: project
title: '고양이 타워'
caption: 추석 게임잼 - 캐주얼 쌓기게임
description: >
  추석 게임잼으로 나흘간 진행한 캐주얼 쌓기게임입니다.
date: 23 Sep 2018
image: 
  path: /assets/img/projects/catTower/CatTower_01.png
accent_color: '#4fb1ba'
accent_image:
  background: '#193747'
theme_color: '#193747'
sitemap: false
---

* this unordered seed list will be replaced by toc as unordered list
{:toc}

<img src="/assets/img/projects/catTower/CatTower_02.png" width="45%"> 
<img src="/assets/img/projects/catTower/CatTower_03.png" width="45%">

# 📽️ 소개 영상

고양이 타워 플레이 영상
{% include youtube.html id="OU4ZUEQ7yDI" %}

# 🚀 프로젝트 소개

**모바일 캐주얼 쌓기 게임**

길쭉하고 네모난 고양이들을 젠가 형식으로 쌓는 쌓기 게임입니다.  
여러 타입의 고양이들이 있으며,  
5마리 이상 쓰러트리지 않고 계속해서 쌓아나가는 형식입니다.

추석 연휴 기간동안 진행한 게임잼의 결과물입니다.

# 개요
- Unity 3d, C# 사용
- 기간: 2018.09.23 ~ 2018.09.26 (4일)
- 인원: **프로그래머 2인**, 3D모델러/사운드 1인

# 참여 내용

## 🔧 Game System

전체적인 게임의 흐름을 담당하는 시스템입니다.
{:.message}

- Enum Switch 상태머신을 이용하여 게임 흐름을 제어합니다.
- 인트로, 본게임, 일시정지, 게임오버의 게임 상태가 있습니다.
- 각 단계별로 매니저들을 참조해 UI와 오디오, 스크립트 제어를 합니다.
- 각 매니저들은 싱글톤으로 처리하였습니다.

## 🔭 Camera System

카메라 및 오브젝트 조작을 담당하는 시스템입니다
{:.message}

- 스와이프 인풋으로 카메라를 90도씩 회전시켰습니다.
- 탑이 높이 쌓일 수록 카메라를 자동으로 조절하였습니다.
- 게임오버 시에 카메라를 줌아웃시켜서 얼마나 높이 쌓았는지 확인할 수 있도록 구현하였습니다.

## 📌 ETC System

각종 매니저 시스템입니다.
{:.message}

- 오디오: UI를 누르거나 효과음이 재생될 때마다 랜덤으로 다른 소리를 재생시켰습니다.
- 점수: 가장 높은 점수를 기록하면 PlayerPrefs를 이용하여 기기에 저장합니다.