---
layout: post
title: 유니티 TextMesh Pro(텍스트메시 프로) 리치텍스트 매크로
categories: devlog
comments: true
description: >
 Unity의 TextMesh Pro에서 리치텍스트를 매크로로 만들어 사용하는 법을 설명합니다.
noindex: false
---

리치 텍스트를 매크로화 할 수 있는 기능을 발견하여 써둡니다.

Project Settings/TextMesh Pro/Settings에서 밑에 보면 Default Style Sheet라는 파일이 보입니다.  
그 파일을 클릭하여 경로로 이동하거나, TextMesh Pro/Resources/Style Sheets 경로로 갑니다.

![](https://lh3.googleusercontent.com/7BPMh7wviN_V6erGTJSyT3CDRmItum2iqfvBwBNvkmH23Tra-dnLwxNrBWqgilHL4FLQFuXhNaAb1WqKRlPVnEo7TmpPzk5oWVKShm8-VGl6F9FPUsVr6x3U8sWE0VtnzTgATLmzag=w2400)
</br>

TextMesh Pro/Resources/Style Sheets에 가면 Default Style Sheet가 하나 있습니다. 

![](https://lh3.googleusercontent.com/onrhNSgO1DhRNKud5gcZUfrS8QWQdfcRY8p5HCc0Q8XZjLyzmX1o6oFxfvga437lwA_xUoSIxG70ecOuT9buEdyQFoknkKXN52DMtsRUTFbLtsNAqbT0WeGSazuVL_D6GbBgzCN1WA=w2400)
</br>

이를 열어보면 기본으로 설정되어있는 매크로들이 나옵니다.  
예를들어 H1은 사이즈를 키우고, 볼드를 먹인 뒤, 색을 변경하는 것을 볼 수 있습니다. <style="H1"> </style>로 사용합니다.

![](https://lh3.googleusercontent.com/zr8jbR1XVYlB-BwTBz7ZOqxkOwuYSYdZpwYFhXRQABoXsIQkNIzmwFvH_1KAl-YHVUZKCAuzYvLiSk0uV4gUfnh7wIqouUQ90Akie9o1A0JtGrY_ArTW4WQpyHphuX9gHTGsxXgqsw=w2400)
</br>

[+] 버튼을 눌러 매크로를 만들 수 있습니다.  
Name을 입력하면 HashCode가 자동으로 생성됩니다.

![](https://lh3.googleusercontent.com/jNtLlefk47_uFLcCY86oh7gQbAibkd3HxDelJ3lBWfL2IGPzzw-BZhb4dZ0fsP8btd1RF9u8go-iuDEyL9nCBbgwpxrAd6Ij_hYDyKSEAK5NHwr9vFmqLE3nR_cTrH7d607VqyTbug=w2400)
</br>

아예 스타일 시트를 새롭게 하나 만들 수도 있습니다. Project에서 Create/TextMeshPro/Style Sheet를 누르면 새로운 파일이 생성됩니다.

![](https://lh3.googleusercontent.com/Mm3_n12_Obb_I_iJ7plFgcE5lHeBkQLRe7FtjmFV9wyK0Ju2pV6KO8jCPXXwhS6tdWgRAGAKKUVoGyVzau6uBmllQl4sZFTlx_6nHrmnBHDtjwFlBVkSaVNyllpXadcuTBaBGcPN-w=w2400)

빈 스타일 시트가 생성되며, 이를 다시 Project Settings/TextMesh Pro/Settings의  Default Style Sheet로 설정해주면 됩니다.

