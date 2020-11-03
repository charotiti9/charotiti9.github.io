---
layout: post
title: 유니티 TextMesh Pro(텍스트메시 프로) 한글영문 같이 사용하기
categories: devlog
comments: true
description: >
 TextMesh Pro를 설치하고, 사용하는 방법을 알아봅니다.
noindex: false
---

프로젝트를 하다가 다양한 텍스트 효과를 주고 싶을 때가 있습니다. 하이퍼링크 기능, 3D 텍스트, 하이라이팅 기능, 밑줄기능 등등... 기존 UGUI text에서는 제공하지 않는 기능들을 TextMesh Pro가 지원해줍니다.

# 설치법

Package Manager에서 TextMesh Pro를 다운로드 받아줍니다.

<a href="https://lh3.googleusercontent.com/giS2I68nsbI8Q4oTeaoWzDrTTq4WwOO1HIqI-rJAmu8_044jhVuHJK-HOGdC55_X_DFvaO-KXvn-aEgNO5IKyFoANELla51F3CFPK_2TlKKkjDGePI7Tc97gxPdKkf0vD-893ttEpQ=w2400?source=screenshot.guru"> <img src="https://lh3.googleusercontent.com/giS2I68nsbI8Q4oTeaoWzDrTTq4WwOO1HIqI-rJAmu8_044jhVuHJK-HOGdC55_X_DFvaO-KXvn-aEgNO5IKyFoANELla51F3CFPK_2TlKKkjDGePI7Tc97gxPdKkf0vD-893ttEpQ=w600-h315-p-k" /> </a>  
<br/>

다 다운받으면 Essential Resources와 Example and Extras를 다운받을거냐고 물어보는 창이 하나 뜰텐데, 둘 다 받아줍니다.

Essential Resources는 TMPro에 사용되는 리소스들을 제공합니다. 가지고 놀다가 뭔가 잘못된거 같을 때도 유용합니다.  
Example and Extras는 용량이 늘어나지만 추가 기능을 제공해줍니다.  

<a href="https://lh3.googleusercontent.com/DOvt3NabIlQe0Oz90e4k2F97mOq05OPGIw2oC7F-Hmkg2Nmg0AtN6Fs79NOStLVo9nCjRo9iUEnNVmC1jBm0fFMniHcheeyrYmu2JsID10bvhGqHOhCiqfwlqruBtEcQYN3ElMt7jw=w2400?source=screenshot.guru"> <img src="https://lh3.googleusercontent.com/DOvt3NabIlQe0Oz90e4k2F97mOq05OPGIw2oC7F-Hmkg2Nmg0AtN6Fs79NOStLVo9nCjRo9iUEnNVmC1jBm0fFMniHcheeyrYmu2JsID10bvhGqHOhCiqfwlqruBtEcQYN3ElMt7jw=w582-h315-p-k" /> </a>


# 폰트 만들기

Window/TextMeshPro/Font Asset Creator를 누르면 창이 하나 뜹니다.

<a href="https://lh3.googleusercontent.com/ItZxRgmyrORwftj3JmLtg3MIhgzXNG_pCkE6SlbULW8Jbum_o28j0oNz4dBmduM0s6p57t67WEdlwKDJG-PQRjaZYCDEALFAe1HJO3WOlJvFOvjAHHCfjhjt9V2pgCsreqr_qSVcRw=w2400?source=screenshot.guru"> <img src="https://lh3.googleusercontent.com/ItZxRgmyrORwftj3JmLtg3MIhgzXNG_pCkE6SlbULW8Jbum_o28j0oNz4dBmduM0s6p57t67WEdlwKDJG-PQRjaZYCDEALFAe1HJO3WOlJvFOvjAHHCfjhjt9V2pgCsreqr_qSVcRw=w581-h315-p-k" /> </a>
<br/>   

Source Font File에 폰트 파일을 넣어주고 원하는 해상도를 설정합니다.  
Character Set을 Custom Range로 바꿔줍니다. 

<a href="https://lh3.googleusercontent.com/RMxnMcEP88_c3iYz1e8vcMCQQDI0bSZiRzEHS7s73INDvPHSUWbFne4cP2C7C7j_JHZmrYgq7q8b7HL6o9DdXJiyKi3HPD24vrVoGhbP2rmuX9Ph79aGTVQBENUH-jxGvA2HJMF59A=w2400?source=screenshot.guru"> <img src="https://lh3.googleusercontent.com/RMxnMcEP88_c3iYz1e8vcMCQQDI0bSZiRzEHS7s73INDvPHSUWbFne4cP2C7C7j_JHZmrYgq7q8b7HL6o9DdXJiyKi3HPD24vrVoGhbP2rmuX9Ph79aGTVQBENUH-jxGvA2HJMF59A=w523-h228-p-k" /> </a>
<br/>

이제 Character Sequence에 범위를 지정해주면 됩니다.

영어 `32-126`  
한글 `44032-55203`  
한글자모 `12593-12643`  
특수문자 `8200-9900`  

글꼴에 따라 지원하지 않는 문자는 missing이 난다고 하니 참고해주세요.  
출처: [https://justice-dev.tistory.com/30?category=719399](https://justice-dev.tistory.com/30?category=719399)

저같은 경우에는 영어와 한글 모두 지원하도록 만들고 싶어서 영어와 한글을 지원하는 폰트를 찾아 아래와 같이 사용하였습니다.

`32-126,44032-55203,12593-12643,8200-9900`

![](https://lh3.googleusercontent.com/L_2vW1hHWPQtYFTA1vnCf5CU3SHqMat5O8Y8yniaOT07Ad_nYpekLqacjUczTzJ-F2XSkuN0DePs94xp2edNj-D9cJLABZ8b1R0BhlkhMf-ZWVzN0D3DsOjKwakRXSwB803sK-AhSQ=w2400)
<br/>

다른 방법으로는, Character Set을 Custom Character로 바꾸고, 문자열을 복사하여 폰트를 뽑는 방식도 많이 사용합니다.  
가갸거겨... 복사 붙여넣기 해서 사용하는 방식인데 필요 한글문자열은 이곳에서 다운받을 수 있습니다: [https://phlm7th.tistory.com/45](https://phlm7th.tistory.com/45)
  
<br/>
<br/>
  
이제 Generate Font Atlas하여 SDF파일을 만들어줍니다.

![](https://lh3.googleusercontent.com/dhj6KKXI56jxs74VsBw5RpUCLQ_5J4DpVmQCV1Zw3kbuplbPKexGP8ls6POO4oHOEbknfLvqqnphgK8tBpmxStTMQKewSvVzfyYTZDXIsbgEkvDlA1VMTmzCPY_nGHhfB73eMd9IjQ=w2400)
<br/>

다 만들어지면 Save나 Save As...를 눌러 저장하는데,  
여기서 중요한 것은 폴더의 경로입니다.

![](https://lh3.googleusercontent.com/pi0-rmh2DiJPtH4aU-Bt5PWVFu_sea_8m-iLkE5pN-5I-4nHW5btXNlgGJrMfgucI1Q5mUXwC1JrK6VPmwu1QLka9LAAsjrGz8bPNBq8qQsGGem681wzU1JSgtGxII1sfhyjP6Mg5A=w2400)
<br/>

완성된 SDF파일을 사용하여 `<font="Name of font asset">` 리치텍스트를 사용할 예정이라면, 지정된 경로에 저장해야합니다.

기본경로는 Resources/Fonts & Materials이고, 바꿔줄 수도 있습니다.  
Project Settings/TextMesh Pro/Settings에서 Resources/이하의 파일명을 지정할 수 있습니다.

![](https://lh3.googleusercontent.com/ujkHOx467mFzi2N8VpyLyGb4FydycgOT4qlXEvUHEulvN7FWE4AcCZg0BEVdXHu8xO4rTYw9fSoj4Iqk1GULsSmYjXRBHF8Z16Imx95FvaeXr4BdqAMIu1dndCgQcvA6PWWa90J6oA=w2400)
<br/>

# 리치 텍스트

Text Mesh Pro는 다양한 tag들을 지원합니다.

`<b>` `<i>` `<color>` 등등이 있는데 주로 `<tag>`로 시작해서 `</tag>` 형식으로 끝납니다.  
value값이 있는 tag라면 `<tag=value>`로 시작해서 `</tag>`로 끝납니다.  

이는 아래의 링크를 참고하거나, TextMesh Pro/Examples & Extras/Scenes 쪽에 있는 예제씬을 참고하면 쉽게 알 수 있습니다.  
[TextMesh Pro Documentation-Rich Text](http://digitalnativestudios.com/textmeshpro/docs/rich-text/)

# 간단한 코드 사용법

```cs
using TMPro;

public class TextMeshPro_Example : MonoBehaviour
{
  public TMP_Text t;
  string GetText()
  {
    return t.text;
  }
  void SetText(string s)
  {
    t.text = s;
  }
}
```

# 마무리

Rich Text에 관해서는 다룬 포스팅들이 많고, 참고하면 바로 알 수 있으니 따로 자세히 적진 않겠습니다.

요새 프로젝트에서 UGUI의 text보다 TextMesh Pro를 더 많이 사용하게 되어 정리해보았습니다. 확실히 쓰다보면 편한 감이 있네요. 3D 텍스트는 아직 다뤄보지 못했지만 조만간 다뤄볼 생각입니다.

