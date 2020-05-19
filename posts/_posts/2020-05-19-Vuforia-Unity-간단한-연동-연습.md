---
layout: post
title: Vuforia Unity 간단한 연동 연습
categories: devlog
comments: true
description: >
 Unity에서 Vuforia를 사용하여 간단하게 3d 오브젝트를 띄우는 법을 연습해보았습니다.
noindex: false
---

참고: [https://library.vuforia.com/articles/Training/getting-started-with-vuforia-in-unity.html](https://library.vuforia.com/articles/Training/getting-started-with-vuforia-in-unity.html)

환경: Unity 2019.3.0f6/Vuforia Core Sample 8.6.10/Vuforia Engine AR 8.6.10

# 프로젝트 세팅

새 프로젝트를 만들고 Build Setting에서 Platform을 Android로 변경해줍니다.  
여기서 먼저 바꿔주지 않으면 나중에 변환작업이 오래걸리게 되어 지금 바꿔주는게 편합니다.  
Project Settings→XR Settings→Vuforia Augmented Reality에 체크를 켜줍니다.  
유니티 2019.3.0f6 에선 이 설정이 없으므로 설정하지 않으셔도 됩니다.  

Asset Store에서 Vuforia Core Sample 다운로드  
그러면 자동으로 Vuforia Engine AR을 설치해줍니다.  
Window→Package Manager에서 Vuforia Engine AR을 먼저 다운로드하면 오류가 잘 납니다.  
둘이 업데이트 버전이 다른 듯 하네요.  

# 데이터 베이스

## 웹에서 데이터 데이스 생성

Hierarchy에 Vuforia Engine→AR Camera를 생성합니다.  
`Assets\Resources\VuforiaConfiguration`을 클릭하고 Inspector 창을 봅니다.  
Inspector 창에 있는 Databases→Add Database를 누르면 홈페이지로 연결되고 로그인 후   
홈페이지에서 Add Database를 눌러 새로운 데이터 베이스를 생성합니다.  

![뷰포리아00.png](https://lh3.googleusercontent.com/taJhzwBbbFyROCzmPGuDtWGHHJWOCipMUxfJoKoZy2jVP5fL0IN4mZAqHvFW2EBTgnmv0ll_vFeofR3ityrBuDwBCOfckOZ_CF7OUabgCB476Py7gpckx6RhRca1DyScoy6zNMKldg=w2400)

Add Target으로 인식하고 싶은 이미지를 넣어줍니다.  
조건에 맞지 않은 이미지들은 붉은 글씨로 표시를 해주니 다른 이미지를 찾아오면 됩니다.  

Download Database(All) 누르고 Unity Editor에 체크 후 다운로드하면 *.unitypackage로 다운로드됩니다.  

![뷰포리아01.png](https://lh3.googleusercontent.com/pQZW6mPnKQi8iemXgIUm6GnRTYXXz_hCkM2PgZRfy2o4VlyYIGoBPBB1DUopZtkXB4JEoXHXNDLwYvIjeRzrO7NseaORx1fjb1fhXRyUfRQTRLkex1gQqZPZiUQhAy18a5ZMuRagtA=w2400)

참고 페이지에서는 Target을 프린트해서 인식하라고 하는데 그냥 화면에 띄워서 해보려합니다.

## 데이터베이스 유니티 적용

다운로드받은 unityPackage파일을 프로젝트에서 실행하면 알아서 들어갑니다.

Hierarchy에 Vuforia Engine→Image를 생성  
Image의 Inspector를 보면   

![뷰포리아02.png](https://lh3.googleusercontent.com/RdEBHcl-p237Pl6OF5PljjEinp61PNHC3ursTTDmLD4TZ7IFZNnbCdrdJXw4LQ1FKyctRfAJnMoAHEvoL0JKDaVvrmkNwqT0U-Q70Sp4ODWGdbHJy32P65wndkXVO9x85HEDPa7bKA=w2400)

`ImageTargetBehaviour.cs`의 Type을 From Database으로 바꿔줍니다.  
그 밑에 Database도 위에서 다운로드하여 넣어준 것으로 설정합니다.  
ImageTarget을 설정해줍니다.  

# Target에 오브젝트 적용

![뷰포리아03.png](https://lh3.googleusercontent.com/3ezjCYS74wDp92Iy9EaHkTTlIx5Y_yvBMDVpj6Izzmo9iJirb8IhhrKO14y4VTZ9eraeV2g5rWoKaJD3uGx6GbY7qNqoKb0OtnT_wB_DG3phgiGmymiryovc0jU5Kc_4Qt6xEohEOg=w2400)

ImageTarget 하위의 자식으로 띄우고싶은 Obejct를 배치합니다.  
비활성화 상태가 아니어도 괜찮습니다.

apk로 빌드 후, 스마트폰에 설치합니다.  
이미지를 자동으로 인식하여 하위 자식들을 모두 활성화 시켜주는 모습을 볼 수 있습니다.  

# 결론

간단하게 뷰포리아에서 이미지를 인식하여 3d 오브젝트를 띄우는 연습을 해보았습니다.  
뷰포리아 외의 다른 AR api들을 이용해서도 동일한 효과를 낼 수 있으니 다양한 api를 써보도록 합시다.  

감사합니다.  
