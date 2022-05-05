---
layout: post
title: 모션캡쳐🏃‍♀️-Unity↔Perception Neuron 연동
category: devlog
tags: [unity, motionCapture, perception neuron, axis neuron]
image: /assets/img/blog/unity/2021/PerceptionNeuron/banner.png
description: >
  애니메이션 프로젝트를 진행하며 모션캡쳐 장비로 사용했던 퍼셉션 뉴런을 어떻게 유니티에 연동했는지 순차적으로 알아본다.
comments: true
---


[👉 다운로드: 유니티 연동 참고 문서](https://neuronmocap.com/sites/default/files/downloads/unity_integration.pdf)

[👉 다운로드: 유저매뉴얼](https://neuronmocap.com/system/files/software/Axis%20Neuron%20User%20Manual_V3.8.1.5.pdf)

# 준비

## 프로그램 다운로드

[Axis Neuron]을 다운로드합니다.

[Downloads](https://neuronmocap.com/downloads)

하드웨어에 맞는 소프트웨어를 다운로드합니다.
현재 가지고 있는 장비는 V2이기 때문에 이를 기준으로 서술하겠습니다.

## 장비 셋팅

파우치마다 태그가 달려있어, 태그에 이 파우치가 어떤 부위의 장비를 담고 있는지 표시되어 있습니다. 우선 본 글에서는 전신 트래킹이 아닌 손만 연결해서 테스트하였습니다.

전신을 테스트해보실 분은 손뿐만아니라 다른 부위도 서로 연결해주세요.

![02](/assets/img/blog/unity/2021/PerceptionNeuron/02.jpg)

퍼셉션뉴런V2는 손가락도 트래킹할 수 있습니다. 처음에 장비를 꺼내면 손가락 부분의 칩이 없습니다. 칩 상자를 열어서 손가락 부분에 끼워넣어줍니다.

상자는 자기장의 영향을 받지 않도록 제작한 특수 상자라고 합니다. 칩을 보호하고 싶다면 넣어서 보관합시다.

![03](/assets/img/blog/unity/2021/PerceptionNeuron/03.gif)

![04](/assets/img/blog/unity/2021/PerceptionNeuron/04.gif)

![05](/assets/img/blog/unity/2021/PerceptionNeuron/05.gif)

마지막 [연결부위]와 [hub]를 선으로 연결합니다. 저는 손만 쓰기로 하였으므로 손의 마지막 연결부위와 hub를 연결해주었습니다. *연결선은 파우치와는 다른 비닐팩에 있습니다.*

# Axis Neuron ↔ 퍼셉션 뉴런 연결

퍼셉션 뉴런을 관리할 수 있는 공식지원 소프트웨어인 Axis Neuron과 연동하는 방법을 알아보겠습니다.

## PC와 연결

![06](/assets/img/blog/unity/2021/PerceptionNeuron/06.jpg)

퍼셉션뉴런 hub에 연결할 [케이블]을 꺼냅니다.

![07](/assets/img/blog/unity/2021/PerceptionNeuron/07.jpg)

hub 위와 아래에 케이블을 연결할수 있는데, 우선 [DATA]라고 써진 윗부분과 PC를 연결해줍시다. (Wifi 모드도 한번은 DATA와 연결해주어야 합니다.)

5V/2A라고 써진 아래부분의 포트는 Wifi를 연결하고 난 뒤의 전원용이니 우선은 무시하고 처음 셋팅할 때는 DATA와 연결해주세요.

## Axis Neuron 셋팅

![08](/assets/img/blog/unity/2021/PerceptionNeuron/08.png)
{:.message}
wifi 모드 설정의 대략적인 흐름도. 처음엔 DATA와 연결한뒤 Connect 후 5V/2A라고 써진 부분과 연결한 것을 볼 수 있다.

와이파이 모드로 연결해야지만 편하게 돌아다니며 풀 트래킹을 할 수 있습니다. USB 모드와 큰 차이는 없으므로 Wifi 모드만을 서술하겠습니다.

![09](/assets/img/blog/unity/2021/PerceptionNeuron/09.png)

Axis Neuron을 켜고 Connect를 누릅니다. 혹은 Axis Neuron을 처음 키면 Connect화면이 뜨는데 동일한 창이므로 거기서 진행하셔도 됩니다.

![10](/assets/img/blog/unity/2021/PerceptionNeuron/10.png)

유선모드에서는 Connect를 누르지만 우리는 Wifi 모드를 사용할 것이기 때문에 Setup버튼을 눌러줍시다.

![11](/assets/img/blog/unity/2021/PerceptionNeuron/11.png)

현재 컴퓨터가 사용중인 망과 동일한 Wifi 망을 선택해줍니다. Password를 입력하고 [Apply]해줍니다.

![12](/assets/img/blog/unity/2021/PerceptionNeuron/12.png)

성공했다면 [DATA]와의 연결을 끊고 충전용인 [5V/2A]와 연결해줍니다. 그리고 [보조배터리]를 이용하여 전원을 켭니다. 20초간 기다리면 [알림음]과 함께 연결됐다는 알림을 줍니다.

알림음이 들렸다면 [Finish] 버튼을 눌러줍시다.

![13](/assets/img/blog/unity/2021/PerceptionNeuron/13.png)

다시 장비 리스트를 봅니다. [Connect]를 눌러줍니다.

![14](/assets/img/blog/unity/2021/PerceptionNeuron/14.png)

정상적으로 장비가 연결 되었다면 오른쪽 센서맵에 연결된 장비의 부위가 뜹니다. 저는 왼손을 연결했기 때문에 왼손만 초록불이 들어와 있습니다.

이후엔 장비의 0점을 조정하는 Calibration 단계로 넘어가면 됩니다.

## 장비 Calibration

![15](/assets/img/blog/unity/2021/PerceptionNeuron/15.png)

장비를 연결했다면 뷰포트의 아바타가 움직임을 따라하는 모습이 보일 것입니다. 하지만 조금 이상한 형태로 따라 움직일텐데, 이를 보정하려면 장비 Calibration이 필요합니다.

[Calibrate] 버튼을 누릅니다. 

![16](/assets/img/blog/unity/2021/PerceptionNeuron/16.png)

보정할 포즈를 선택합니다. 주로 Steady post, A pose, T pose를 보정합니다. 좀더 정확한 보정을 원한다면 S pose까지 선택해줍니다. 선택을 마쳤다면 [Next] 버튼을 눌러주세요.

![17](/assets/img/blog/unity/2021/PerceptionNeuron/17.png)

보정을 하다가 이런 창이 뜬다면 사진에 빨갛게 표시된 부분이 너무 많이 흔들려서 보정을 실패한 것입니다. [Retry]를 눌러 다시 보정해주세요.

![18](/assets/img/blog/unity/2021/PerceptionNeuron/18.png)

모든 보정을 마치면 뷰포트에서 정상적으로 작동하는 아바타를 보실 수 있습니다!

만약 예상과 다르게 움직인다면, 아래의 이유 중 하나일 수도 있습니다.

1. 센서의 위치가 틀어져 있을 수 있습니다.
2. 자기장의 영향이 센 곳일 수 있습니다.
3. 보조배터리의 잔량이 부족할 수 있습니다.

위 이유를 모두 확인해도 이상하다면 다시 Calibrate 해주세요.

# 애니메이션 클립 녹화

애니메이션을 녹화해서 저장할 차례입니다. [Record] 버튼을 누릅니다.

![19](/assets/img/blog/unity/2021/PerceptionNeuron/19.png)

Record Settings라는 창이 뜨면 저장할 위치와 이름을 정해주세요. 

[OK]를 누르면 그 순간부터 애니메이션이 녹화됩니다.

![20](/assets/img/blog/unity/2021/PerceptionNeuron/20.png)

완료를 누르면 왼쪽 하단 Working Directory에 들어가게 되고, 이를 더블클릭하면 [Play]버튼을 눌러 볼 수 있습니다.

## 애니메이션 Export

녹화를 완료한 애니메이션을 유니티에서 쓸 수 있게끔 Export 해보겠습니다.

녹화한 파일을 Working Directory에서 더블클릭한 후 [File] → [Export]를 누릅니다. 

![21](/assets/img/blog/unity/2021/PerceptionNeuron/21.png)

저장할 폴더를 선택해주고 [File Type]을 [FBX]로 설정해줍시다. 저는 FBX binary로 설정했습니다.

![22](/assets/img/blog/unity/2021/PerceptionNeuron/22.png)

나머지는 무시하고 [Export] 버튼을 눌러주세요.

## 애니메이션 클립 유니티에 적용

![23](/assets/img/blog/unity/2021/PerceptionNeuron/23.png)

이후, 저장한 FBX를 유니티로 불러옵니다.  
[Rig]탭에서 [Animation Type]을 Humanoid로 꼭 바꿔주세요. [Apply] 버튼을 눌러줍니다.

![24](/assets/img/blog/unity/2021/PerceptionNeuron/24.png)

[Animation] 탭에서 Root Transform Rotation에서 Bake Into Pose 체크해야합니다.

Root Transform Position(Y)에서 Bake INto Pose 체크, Based Upon(at Start)을 Feet으로 변경해줍니다.

## Export 오류 수정(중요!!)

Axis Neuron에서 Export한 애니메이션은 유니티에서 불러오면 오류가 있습니다. T포즈이긴 하지만 각 포인트들이 돌아가 있는 모습을 볼 수 있습니다.

![25](/assets/img/blog/unity/2021/PerceptionNeuron/25.png)
{:.message}
돌아간 손의 모습. 새로 그린 좌표계가 Rotation 0,0,0에 해당됨.

이를 하나씩 보정하려면 아바타 Configure 설정에 들어가서 Bone마다 Rotation을 체크해서 바꿔주어야 합니다. 간혹 0,0,0으로 맞춰주어도 T포즈가 되지 않는 경우가 있습니다. 이럴 경우 각각의 본을 선택하여 로테이션 값을 임의로 수정해주어야 합니다.


# 유니티 리얼타임 트래킹

유니티 리얼타임으로 돌리는 법 또한 간단합니다.

![26](/assets/img/blog/unity/2021/PerceptionNeuron/26.png)

우선 Axis Neuron에서 [File → Settings]에 들어갑니다.

![27](/assets/img/blog/unity/2021/PerceptionNeuron/27.png)

General에 있는 IP를 복사해둡니다.

![28](/assets/img/blog/unity/2021/PerceptionNeuron/28.png)

[Broadcasting]에서 TCP로 되어있는지 확인하고 Advanced BVH와 BVH를 모두 Enable해줍니다. 여기서 나타난 [ServerPort] 중 BVH의 포트를 기억해둡니다.

![29](/assets/img/blog/unity/2021/PerceptionNeuron/29.png)

유니티에서 Neuron Animator Instance 스크립트를 애니메이터가 있는 객체에 붙여주고, [Address] 부분에 복사한 IP를 넣어주고, [Port]에는 BVH ServerPort번호를 넣어줍니다.

Axis Neuron과 유니티를 동시에 켜놓은 뒤, 유니티에서 플레이 버튼을 누르면 Axis Neuron의 아바타가 하는 동작들을 유니티 내의 Neuron Animator Instance를 붙인 아바타가 따라할 것입니다.