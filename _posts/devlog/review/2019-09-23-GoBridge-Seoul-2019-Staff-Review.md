---
layout: post
title: GoBridge Seoul 2019 스텝 참가후기
category: devlog
tags: [review, study]
image: https://user-images.githubusercontent.com/47441135/100401759-bd5e3a80-309d-11eb-9e5d-1b68139ea274.png
description: >
 GoBridge Seoul 2019에 준비스텝으로 참여했던 후기
comments: true
---

* this unordered seed list will be replaced by toc as unordered list
{:toc}

준비스텝 및 일반참가자로 GoBridge Seoul 2019에 참석했다. 최근 회사를 다니며 오프라인 행사를 참여하는 것이 더욱 어려워졌는데, 마침 좋은 시기에 여는 오프라인 행사에 참석할 수 있게 되었다. 페이스북을 통해 소식을 듣게 되었고 용기를 내어 스텝으로 참여해보기로 했다. 이에 대한 후기를 적는다. 


# 참가 전

<img src="https://user-images.githubusercontent.com/47441135/79192464-b6aeea00-7e63-11ea-9020-db7d31de84e0.jpg" width="60%" height="60%">

참가 신청할 당시에는 새로운 언어에 대한 관심이 굉장히 많았다. (사실 지금은 자신의 한계를 깨닫고 기본기에 좀 더 집중하자는 마음이 크다.) C, C++, C#만 공부했던 나는 요새 뜨는 언어들(Python, Kotlin, Go... 등등)을 공부해보고 싶은 마음이 컸다. 마침 Go를 공부하는 워크샵이 있고, 취지도 좋아서 당장 참가하게 되었다.

참가 신청서를 쓰는데 준비스텝으로도 참가할 수 있다는 사실을 발견했다. 이전에 한빛 주니어 개발자 세미나에 참가했을 때 이런 오프라인 행사에서 스텝으로도 일해보고 사람들을 만나면 동기부여가 된다고 강사님의 말씀이 떠올랐다. 그리고 용기를 내어 스텝으로 일해보기로 했다.


## 워크샵 준비

<img src="https://user-images.githubusercontent.com/47441135/79193086-e0b4dc00-7e64-11ea-8b69-1fa00e5acc11.jpg" width="40%">

행사 전까지는 매 주말 시간이 되는 날엔 회의에 참석하여 어떤 식으로 행사를 이끌어갈지, 필요한 건 무엇인지 이야기했다. 스텝은 처음이어서 처음엔 다른 분들이 하는 일이나 말을 살펴보는 작업을 했다. 후반부에는 나도 의견을 많이 내보았다.  

<img src="https://user-images.githubusercontent.com/47441135/79193138-faeeba00-7e64-11ea-80ff-a36be72e159e.jpg" width="55%"> <img src="https://user-images.githubusercontent.com/47441135/79202758-06e27800-7e75-11ea-846e-f547a8f4d9bc.jpg" width="31%"> 

그리고 포스터를 담당하여 만들었는데, 조금 작지만 유광으로 뽑아서 실물이 굉장히 멋지게 나왔다. 하지만 금융건물이다보니 건물 내부까지도 보안이 철저해서 딱히 포스터를 많이 붙이지 못했다. 그 점은 조금 아쉬웠다. 행사 당일, 스텝와 코치들은 조금 일찍 모였다. 뱅크샐러드 사무실에 처음 가봤는데 확실히 요즘 핫한 어플을 운영하는 회사답게 젊은 분위기가 가득했다. 전체적으로 초록색이 많아서 산뜻한 느낌이 들었고, 빈백과 안마기가 편안한 분위기였다. 

빠르게 다과를 준비하고, 취소자들이 꽤 있어서 조편성을 잠시 다듬었다. 그 후 조끼리 어느 공간을 사용할지 정하고, 참가자들을 맞을 준비를 했다. 확실히 다양하고 많은 사람들이 한자리에 모이다보니, 공지된 시간보다 일찍 오시는 분부터 행사 시작 후 오시는 분까지 다양했다. 행사장소 앞에 앉아 참가자들 안내를 하는데 참석선물인 컵을 받으시고 모두 좋아하셔서 기분이 좋았다.


# 워크샵 참가

<img src="https://user-images.githubusercontent.com/47441135/79202814-1bbf0b80-7e75-11ea-8850-3f5e8f23de70.jpg" width="60%">

우리 조는 어느정도 코딩을 접해본 분들이 모인 조였다. 가볍게 자기소개를 하고 변수부터 차근차근 나가는데 거의 아는 내용이었지만 문법을 익혀나가는 과정 중 Go언어가 어떤 언어인지 대강 윤곽이 그려졌다. 하지만 여전히 이해가 안가는 것이 있다. 변수 작성이나 세미콜론, 조건문 등은 여러 방식으로 쓸 수 있도록 문법적으로 열려있었는데, 중괄호에는 너무나 딱딱한 문법을 사용한다는 사실이다. 특히 사용하지 않는 변수나 함수, 패키지 등은 오류처리한다는 사실에 너무나도 놀랐다. 그리고 어째서 중괄호는 함수명이나 조건식 옆에 붙어있어야 하는걸까?

이러한 의문들은 둘째 날에 조금 적응이 되자 그냥 그런가보다 하고 넘어가게 되었다. 둘째 날엔 오전에 일찍 나와서 go언어의 포인터, 가변배열, 맵, 구조체 등 심화부분을 실험해보았다. 그래도 다른 언어들을 공부한 적이 있어서 빠르게 습득할 수 있었다. struct 내부에 slice로 변수를 선언하고 이를 map으로 저장하여 key값으로 불러오고, 포인터로 저장하는 실험을 함으로써 총체적으로 이해하려 했다.  이후 고루틴에 대해 개념만 익히고 시간이 없어서 실험은 해보지는 못했다. 동시성과 병렬성의  차이를 명확히 몰랐는데 교재에 자세히 적혀 있고 코치님께서 도와주셔서 어떤 개념인지 정확히 이해할 수 있게 되었다.

둘째 날 세션은 1시간 20분 정도여서 많은 걸 배우지는 못했다. 반복문에 대해 배웠는데, 신기하게도 go에는 반복문이 for문 하나뿐이다. 하지만 for 하나로도 while과 foreach가 가능하도록 만들었다. 물론 문법적으로 변화가 크지만 익숙해지면 못쓸 것도 아니었다. 언더바로 index를 대신하여 range를 지정하면 foreach와 동일하게 작동했고, 아예 조건문을 적지 않고 무한루프 내부에서 if()와 break로 빠져나온다면 while과 동일하게 작동한다. 

전체적으로 신기한 문법들이 많았다. 실습 때엔 Todo List를 웹서버를 이용하여 만들어보았다. 서버쪽에서 go가 어떤 식으로 사용되는지도 궁금했고, 개인적으로 서버단에 대한 흥미도 있어서 신청했다. 코치님께서 정말 자세히 설명해주셔서 대충은 이해할 수 있었다. 서버에 대한 흥미도를 기르고, 친밀도를 올릴 수 있는 좋은 시간이었다. 짧은 시간이었지만 이 실습을 고르길 잘했다는 생각이 들었다.


# 워크샵 뒷풀이

뒷풀이 및 네트워킹은 15명 정도가 참가하여 근처 치킨집에서 이루어졌다. ios 개발, 게임 개발, 안드로이드 개발, 웹 개발 등 다양한 환경에서 일하는 개발자분들과 공감가는 이야기와 회사 이야기 등을 나누며 새로운 것을 배우고 유대감을 쌓았다. 오프라인 행사는 시야가 넓어지는 기분이다. 내가 모르는 것들을 알고, 혹은 내가 알고 있었다고 생각하는 것들에 대한 틀을 깨주는 기회가 된다. 이런 행사가 있다는 것에 항상 고마울 따름이다.


# 스텝으로서의 소감

가장 좋았던 점은 행사 전반에 대한 이해도가 높아진다는 것이다. 첫날 세션이 끝나고 스텝과 코치가 모여 각 조가 대체적으로 어떤 분위기였는지, 어느 부분을 힘들어했는지 공유했다. 대부분 초보자 분들이라서 힘들어 했다는 의견이 있었고, 회의실에 들어가서 강의하셨던 코치 분은 조용해서 졸음이 오는 사람들이 많았다는 의견도 나왔다. 이를 다음날에 어떻게 수정하여 반영할지에 대해 잠시 토의를 하고 다음날을 위해 뒷정리를 하고 해산하였다. 이런 일련의 과정들이 쌓여서 나도 나중에 이런 행사를 주도해볼 수도 있지 않을까 하는 생각도 하게 되었다.

스텝과 코치로 만나 좀 더 네트워킹을 할 수 있는 시간이 많다는 점도 좋았다. 각지에서 열심히 살고 있는 다양한 사람들을 만나 이야기하는 것은 언제나 동기부여가 되고 힘이 된다. 후속스터디도 있다고 하니 시간이 난다면 꼭 참여해야겠다. 이번이 2회차이니 내년도에도 참가하게 된다면 코치로서 한 번 참가해보고 싶다.

코치님들께 감사하고 다음에도 이런 좋은 오프라인 행사에 참여할 수 있게 되기를 바란다.
