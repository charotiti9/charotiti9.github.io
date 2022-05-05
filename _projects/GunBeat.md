---
layout: project
title: 'Gun Beat'
caption: Steam 출시 VR 리듬게임
description: >
  2019년 1월 14일에 Steam에 출시한 VR 리듬게임입니다.
date: 10 Dec 2018
image: 
  path: /assets/img/projects/gunBeat/GunBeat03.gif
links:
  - title: Steam Link
    url: https://store.steampowered.com/app/1000990/Gun_Beat/
accent_color: '#4fb1ba'
accent_image:
  background: '#193747'
theme_color: '#193747'
sitemap: false
---

* this unordered seed list will be replaced by toc as unordered list
{:toc}

<img src="/assets/img/projects/gunBeat/GunBeat02.gif" width="45%"> 
<img src="/assets/img/projects/gunBeat/GunBeat01.gif" width="45%">

# 📽️ 소개 영상

건비트 트레일러
{% include youtube.html id="k7E1TrTfluc" %}

# 🏆 수상 이력

<img src="/assets/img/projects/gunBeat/GunBeat_prize01.png" width="45%"> 
<img src="/assets/img/projects/gunBeat/GunBeat_prize02.png" width="45%">

- VR·AR 그랜드 챌린지
: 한국전자통신연구원 원장상

- 한국전파진흥협회 인재개발교육원
: 최우수 프로젝트 상

# 🚀 프로젝트 소개

**VR 리듬 슈팅게임**

양 손에 총을 들고, BPM 비트에 맞추어 적을 쏩니다.

적이 쏘는 총알의 궤적을 피해 곡이 끝날 때까지 점수를 계산하여 C, B, A, S 등급을 줍니다.

![steamImage](/assets/img/projects/gunBeat/GunBeat_Steam.png)

Steam에 Gun Beat라는 이름으로 2019년 1월에 출시하였습니다.
👉 [*Steam: Gun Beat 링크*](https://store.steampowered.com/app/1000990/Gun_Beat/)

세 달 뒤,  100만원가량의 수익을 창출하였습니다.

# 개요

- Unity 3d, C# 사용
- 기간: 2018.06.18 ~ 2018.12.10 (6개월)
- 출시일: 2019.01.14
- 인원: **프로그래머 1인**, 기획자 1인

# 참여 내용

## 🔧 Game System

전체적인 게임의 흐름을 담당하는 시스템입니다.
{:.message}

- Enum Switch 상태머신을 이용하여 게임 흐름을 제어합니다.
- 인트로, 튜토리얼, 본게임, 게임오버의 게임 상태가 있습니다.
- 각 단계별로 매니저들을 참조해 UI와 오디오, 스크립트 제어를 합니다.
- 각 매니저들은 싱글톤으로 처리하였습니다.

## 🎼 Rhythm System

BPM을 입력하면 박자를 세주는 메트로놈 시스템입니다.
{:.message}

- 정확한 박자를 세기 위해 AudioSetting.dspTime을 이용하였습니다.
	<details markdown="1">
	<summary>코드보기(클릭! 👈)</summary>

        ```csharp
        using System;
        using System.Collections;
        using System.Collections.Generic;
        using UnityEngine;
        
        // 비트를 생성하고
        // true/false 반환
        public class Metronome : MonoBehaviour
        {
            // bpm 설정
            public double bpm;
            public float perfectTiming = 0.05f;
            public float normalTiming = 0.10f;
        
            double nextTick = 0.0F;
            double startTick;
            double timePerTick;
        
            // 중복방지
            bool ticked = false;
        
            // 반환할 bool값
            [HideInInspector]
            public static bool isBeat;
            [HideInInspector]
            public bool isPerfect;
        
            private void OnEnable()
            {
                startTick = AudioSettings.dspTime;
                nextTick = 0;
                // 다음 tick은 처음 시작했을 때에서 60/bpm
                nextTick = startTick + (60.0 / bpm);
            }
        
            void Start()
            {
                // 처음 시작할 때의 시간
                // dspTime = 오디오 시스템의 현재 시각(Time.time보다 정확)
                // ex. 48000Hz
                startTick = AudioSettings.dspTime;
                // 다음틱
                nextTick = 0;
                // 다음 tick은 처음 시작했을 때에서 60/bpm
                nextTick = startTick + (60.0 / bpm);
            }
        
            void FixedUpdate()
            {
                timePerTick = 60.0f / bpm;
                // 현재 오디오 시간
                double dspTime = AudioSettings.dspTime;
        
                // 다음 틱이 현재 오디오시간보다 같거나 크다면
                while (dspTime >= nextTick)
                {
                    // Update의 OnTick으로 갈 수 있도록한다
                    ticked = false;
                    // 다음 틱에는 TimePerTick을 더하여 저장해준다
                    nextTick += timePerTick;
                }
        
                // 한번만 실행
                // 만약 다음 틱이 오디오의 현재 시각보다 같거나 크다면
                if (!ticked && nextTick >= AudioSettings.dspTime)
                {
                    ticked = true;
                    //OnTick을 실행시켜라
                    OnTick();
                }
                
            }
        
            // Tick일 때 처리할 명령들
            void OnTick()
            {
                StartCoroutine(BeatOn());
            }
        
            private IEnumerator BeatOn()
            {
                isBeat = true;
        
                isPerfect = true;
                yield return new WaitForSecondsRealtime(perfectTiming);
                isPerfect = false;
                yield return new WaitForSecondsRealtime(normalTiming);
        
                isBeat = false;
            }
        }
        ```
        
	</details>
- 판정시스템: 코루틴을 이용하여 Perfect, Good, Bad, Miss 4단계로 판정하였습니다.
- 콤보 시스템: 콤보를 합산하여 Best Score를 기록하여 Rank를 만들었습니다.
- 피버 시스템: 일정 콤보 수를 넘어가면 피버타임이 시작되도록 했습니다.
                     피버타임은 코루틴을 이용하여 조절하였습니다.

## 🙋 Player System

플레이어 인풋을 담당하는 시스템입니다.
{:.message}

- 슈팅: 레이캐스트를 사용하여 판정하였습니다.
- 컨트롤러의 방향에 따라서 크로스헤어 위치와 크기를 제어하였습니다.
- 멀티플랫폼 인풋: Oculus와 Vive 둘 모두 사용 가능하도록 멀티플랫폼 작업을 진행했습니다.

## 😈 Enemy System

적들의 생성 및 행동패턴을 제어하는 시스템입니다.
{:.message}

- Object Pool을 이용하여 생성로직을 구축하였습니다.
- Enum/Switch 문을 이용하여 Idle, Move, Attack, Defeat, Win 상태를 제어했습니다.