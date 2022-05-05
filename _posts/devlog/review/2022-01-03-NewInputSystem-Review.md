---
layout: post
title: New Input System (Unite Now 2020) 정리
category: devlog
tags: [review, unityLive]
image: https://user-images.githubusercontent.com/47441135/147895305-de939e96-d511-48cb-a3dc-b32a4a69b9a7.png
description: >
 새로운 Input System을 소개하는 Unite Now 2020 유튜브 내용을 가볍게 정리한 글입니다.
comments: true
---

New Input System이 나왔다길래 얼마나 사용하기 쉬운지 궁금해서 유튜브 영상을 보았다. 전체적으로 툴이 생기면서 깔끔해진 느낌이 들지만, 스크립트에서만 모든 인풋을 조정하려면 조금 고민이 필요해졌다. 자세한 내용은 영상을 보는 것을 추천.

* this unordered seed list will be replaced by toc as unordered list
{:toc}

영상링크: 
[Unite Now: 새로운 Input System](https://youtu.be/n_-8s9IwLKU)

# 사용법

## 설치 및 옵션설정

Window → Package Manager에서 Input System을 Install 후, Import한다.

![Untitled](https://user-images.githubusercontent.com/47441135/147895916-8f67164a-e59f-4185-83bf-3ce9a8349504.png)

Edit → Project Settings → Other Settings에서

Active Input Handling 옵션을 보면 Old버전 New버전, 둘다 가능한 버전 3가지가 있다.

옵션을 알아서 설정하면 된다.

![Untitled 1](https://user-images.githubusercontent.com/47441135/147895692-6495869f-8d50-4da2-86d8-19b95f417158.png)


## 새로운 인풋 만들기

Project 패널에서 Input Actions를 만들 수 있다.

![Untitled 2](https://user-images.githubusercontent.com/47441135/147895701-ca71f855-efd0-407d-a20a-4152376644b5.png)


만들어진 Input Actions를 더블클릭하면 창이 하나 열린다.

![Untitled 3](https://user-images.githubusercontent.com/47441135/147895709-92f22b5e-49e1-40aa-bd28-45e98077dab7.png)

Action Maps를 하나 만들고 Move라는 액션을 하나 추가해준다.

그 후 선택해보면 액션타입과 컨트롤타입을 선택해야하는데, 캐릭터가 움직이는 것을 설정할 것이므로 Value타입, Vector2 타입으로 설정해준다.

![Untitled 4](https://user-images.githubusercontent.com/47441135/147895715-1534a38c-48e4-4fef-8981-ea6ec792ae76.png)

액션 옆에  + 버튼을 눌러보면 바인딩을 추가하거나 2D 벡터 컴포지트를 추가할 수 있는데, 2D벡터 컴포지터를 선택하면 상하좌우가 포함된 2D 벡터가 만들어진다.

![Untitled 5](https://user-images.githubusercontent.com/47441135/147895718-a82266f7-f3f3-428d-a0a8-8a43126e2434.png)

![Untitled 6](https://user-images.githubusercontent.com/47441135/147895726-a07cf3cb-8573-4c2e-bf18-b54040eeb5cb.png)

Up Down Left Right를 클릭해서 바인딩을 추가해준다. WASD를 기준으로 설명하도록 하겠다.

![Untitled 7](https://user-images.githubusercontent.com/47441135/147895733-d6b1fbd9-d2f2-40c1-8c8d-2b44fdc53741.png)

다 되었다면 Save Asset을 누른다.

![Untitled 8](https://user-images.githubusercontent.com/47441135/147895737-0bdd9bdc-48d0-4b5f-8ab3-310d783e0c8f.png)

## 캐릭터에게 적용하기

플레이어 인풋을 조종하고 싶은 캐릭터에게 붙여준다.

![Untitled 9](https://user-images.githubusercontent.com/47441135/147895746-731886a6-ed59-4c01-acff-382c9d9e5f31.png)

Actions에 방금 만든 것을 넣어준다.

![Untitled 10](https://user-images.githubusercontent.com/47441135/147895748-0c66d1b2-d035-4db3-bdae-60002f044a3c.png)

Behavior에서 Send Messages로 설정하면 내가 만든 액션의 이름대로 이벤트 함수처럼 호출할 수 있게된다.

![Untitled 11](https://user-images.githubusercontent.com/47441135/147895751-af234c4b-762f-4a60-b726-808aa6ee6050.png)


# 이전 코드와의 비교

플레이어 컨트롤러 스크립트에서 본래코드와 대체할 코드를 비교해보자.

```csharp
// 본래 코드
if(usingOldInput)
{
	h = Input.GetAxis("Horizontal");
	v = Input.GetAxis("Vertical");
	dir = new Vector3(h, 0, v).normalized;
}
```

```csharp
// 대체할 코드
private void OnMove(InputValue value)
{
	Vector2 inputMovement = value.Get<Vector2>();
	dir = new Vector3(inputMovement.x, 0, inputMovement.y);
}
```

# 후기

위와같이 사용했을 때의 문제점은 코드만 보았을 때 어떤 Axis를 사용하는지 명확하게 드러나지 않는다는 점 같다. 물론 맥락을 읽거나 유니티의 툴을 살펴보면 되지만, 코드만 보고서는 파악이 힘들다. 또한, 다른 유니티 프로젝트에 코드만 이식했을 때의 문제도 있다.
이를 충분히 주의하고 사용한다면 오히려 프로그래밍에 참여하지 않는 개발인력에게는 유용할 것으로 보인다.