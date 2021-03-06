---
layout: post
title: 구글포토 임베드 링크 만들기
description: >
 구글 포토를 사용하여 어딘가에 사진을 임베드 하고 싶은 사람들을 위해 작성해보았습니다.  
 ~~무제한용량(조건있음)인~~ (수정)2021년 6월부터 구글 포토의 무제한용량 정책이 변경됩니다. 
date: 2020-06-05
modified: 2020-11-18
tags: [google, imbed]
comments: true
sitemap: false
---

* this unordered seed list will be replaced by toc as unordered list
{:toc}

깃허브 페이지를 사용하거나 각종 페이지에 사진을 임베드할 때 imgur나 Github Issue란을 사용하는 걸 많이 보았습니다. 물론 이것도 좋지만 저는 기왕이면 구글 포토를 이용해서 저장소를 하나로 관리하고 싶었습니다.

지금부터 이야기 할 구글포토 임베드 링크 생성은 간단하지만 굳이 사용할만한 것은 아닙니다. 하지만 어딘가 저처럼 구글포토를 굳이 사용하겠다는 사람이 있을 수 있으므로 글로 적어두려 합니다. 이 글 또한 구글 포토로 작성 중이니 혹시나 이 글의 사진들이 보이지 않는다면 제 글이 틀린 것이 되겠네요. 아직 사진의 만료기간은 체크가 안되있으므로 참고바랍니다.

※구글 포토?  
**2021년 6월부터 구글 포토의 무제한용량 정책이 구글 드라이브 용량을 사용하도록 변경됩니다.**  
~~구글 포토는 사진을 불러오면 이를 최대 16MP 및 1080p의 고화질 사진과 동영상으로 가공하여 무제한으로 저장할 수 있도록 서비스를 제공합니다. 사진을 전문으로 하시는 분들 외엔 스마트폰으로만 사진을 찍는 분들이 많으실텐데, 스마트폰 사진은 대부분 원본사진이 1600만 픽셀을 넘지 않아서 사진과 동영상은 구글포토를 통해 백업하고 관리하면 편합니다.~~

※이미지 주소 복사는 사용하면 안되는 이유  
![](https://lh3.googleusercontent.com/nBhAYMXdP6G52WLARP_RWV9ZNXpuaqhQCH_oiVhN-Ll9qYJt78VYZHDqSh3E458OjoPgxhgiuPjEv8aDINCxjL1slIkY5cZzKKXWvD60MY9gdGnvtMmQ-sFWyxDhuLC3WPRIMG5TGg=w2400)

이런 사진을 보게됩니다.  
[이미지 주소 복사] 등을 이용하면 구글 계정 단에서 계정별로 접근을 막아버립니다.  
예를들어 같은 구글계정을 사용하는 크롬 등에서 보면 본계정에서는 잘 보이나, 로그아웃을 하면 제대로 보이지 않습니다.

---

# 방법1. 사이트 사용

[Embed Google Photos - Add Images from Google Photos in your Website](https://www.labnol.org/embed/google/photos/)

이 사이트를 사용하면 Google 계정에 로그인하지 않은 사람을 포함하여 누구나 액세스 할 수있는 미등록 링크가 생성됩니다.

이 사이트는 이미지 호스팅 서비스가 아니며, 다른 사이트에 이미지가 저장되지도 않습니다.  
내부적으로 임베드 앱은 구글포토 링크의 페이지를 다운로드하고 Open Graph 태그를 추출하여 이미지와 기본 사진 앨범의 직접 링크를 판별한다고 합니다.

방법은 간단합니다. 구글 포토에서 알맞은 사진을 클릭한 뒤 [공유] 버튼을 누릅니다.

![](https://lh3.googleusercontent.com/htkqkhgBhV3LORos_SgC1X1Ru-NUZgXYhRBLSm_P2Yc432vJ8hIK7b-fgU2zon5Nc7GL9phlzqMXb3269B21C5E00fvjS4n2NXrlwVD5WCEKFSLlBBSfowLXQs1rfLgOO6LEJ3uOFw=w2400)

[링크 만들기]를 통해 공유 링크를 생성합니다.

아래 사이트에 접속합니다.

[Embed Google Photos - Add Images from Google Photos in your Website](https://www.labnol.org/embed/google/photos/)

주소를 입력하고 로봇이 아닙니다에 체크한 뒤, [Generate] 버튼을 눌러줍니다.

![](https://lh3.googleusercontent.com/HZhfBkJ3Gwagub-QFlXZB3IXcYLYcB6XVpVlwgCo_RzvnaR-raI8bqTMPkq_iiNmvy7hRTASyzD2PkJz28nV2gJk32Q_Xq8fTCzR4f994XB6jdx-gfS_o3dkmYCy0EH-ldgyG3FIZQ=w2400)

생성된 링크를 복사하여 사용합니다.  
이 화면에서 나가도 상관없습니다. 다른 사진 링크를 생성하고 싶다면 [Embed Another] 버튼을 누릅니다.

![](https://lh3.googleusercontent.com/gCsswYMSA9siia-RjTu32u-lumZ9P3MqskpNXfhZOY3eBCuzuU7JtEf2fv-XtkPqvM7HpttWIkXKE8MahfnQ0sFGnx6jobD1YY40Z297Df7gLDpk38kIdUvoGuYf536zE803wkgKvw=w2400)

사이트에서 제공하는 동영상 튜토리얼도 첨부하겠습니다.

<style>.embed-container { position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; } .embed-container iframe, .embed-container object, .embed-container embed { position: absolute; top: 0; left: 0; width: 100%; height: 100%; }</style><div class='embed-container'><iframe src='https://www.youtube.com/embed//InKJcbOgf9k' frameborder='0' allowfullscreen></iframe></div>

# ~~방법2. 확장 프로그램 사용~~

**확인결과 정상작동하지 않습니다. 구글 계정을 통해 접속하도록 되어있습니다.**

~~크롬 브라우저에서만 사용할 수 있습니다.~~  
~~하단의 확장프로그램을 설치합니다.~~  
[Google Photos Direct Link](https://chrome.google.com/webstore/detail/google-photos-direct-link/dgnaaplaoheafdckphgmpiaodckbcafg)

~~구글 앨범 아카이브를 엽니다.~~  
[Sign in - Google Accounts](https://get.google.com/albumarchive)

~~이미지를 클릭하면 클립보드에 이미지가 복사되었다는 팝업메세지가 뜹니다.~~  
~~이 상태로 Ctrl+V를 누르면 복사된 링크를 붙여넣기 할 수 있습니다.~~ 

---

# 이미지 옵션

## 크기 조절

특정 크기의 이미지를 얻을 수도 있습니다. 실제 크기보다 큰 크기를 요청하면 본래 크기의 이미지로 나타납니다.  
주소 뒤의 '=w2400' 부분을 바꾸면 됩니다.

원본
[https://lh3.googleusercontent.com/acHAu1BMPE4ZuqekT-LqxqPF-poKFwGGf7CqUr4lRXIfwS01pQfdtQlJQDv4vj4H7zP3366u1kK4UWG4IGo7MPukutGEf0Ri5ZslwYxSdU8UqWFZpqqxSNlF1ngKyZsprc9K06t4vw=w2400](https://lh3.googleusercontent.com/acHAu1BMPE4ZuqekT-LqxqPF-poKFwGGf7CqUr4lRXIfwS01pQfdtQlJQDv4vj4H7zP3366u1kK4UWG4IGo7MPukutGEf0Ri5ZslwYxSdU8UqWFZpqqxSNlF1ngKyZsprc9K06t4vw=w2400)

1. s400: 이미지에서 너비와 높이 중 더 큰 치수를 가져와 픽셀 수를 조정합니다.  
[https://lh3.googleusercontent.com/acHAu1BMPE4ZuqekT-LqxqPF-poKFwGGf7CqUr4lRXIfwS01pQfdtQlJQDv4vj4H7zP3366u1kK4UWG4IGo7MPukutGEf0Ri5ZslwYxSdU8UqWFZpqqxSNlF1ngKyZsprc9K06t4vw=s400](https://lh3.googleusercontent.com/acHAu1BMPE4ZuqekT-LqxqPF-poKFwGGf7CqUr4lRXIfwS01pQfdtQlJQDv4vj4H7zP3366u1kK4UWG4IGo7MPukutGEf0Ri5ZslwYxSdU8UqWFZpqqxSNlF1ngKyZsprc9K06t4vw=s400)
2. w400: 이미지 너비의 픽셀 수를 조정합니다.  
[https://lh3.googleusercontent.com/acHAu1BMPE4ZuqekT-LqxqPF-poKFwGGf7CqUr4lRXIfwS01pQfdtQlJQDv4vj4H7zP3366u1kK4UWG4IGo7MPukutGEf0Ri5ZslwYxSdU8UqWFZpqqxSNlF1ngKyZsprc9K06t4vw=w400](https://lh3.googleusercontent.com/acHAu1BMPE4ZuqekT-LqxqPF-poKFwGGf7CqUr4lRXIfwS01pQfdtQlJQDv4vj4H7zP3366u1kK4UWG4IGo7MPukutGEf0Ri5ZslwYxSdU8UqWFZpqqxSNlF1ngKyZsprc9K06t4vw=w400)
3. h400: 이미지 높이의 픽셀 수를 조정합니다.  
[https://lh3.googleusercontent.com/acHAu1BMPE4ZuqekT-LqxqPF-poKFwGGf7CqUr4lRXIfwS01pQfdtQlJQDv4vj4H7zP3366u1kK4UWG4IGo7MPukutGEf0Ri5ZslwYxSdU8UqWFZpqqxSNlF1ngKyZsprc9K06t4vw=h400](https://lh3.googleusercontent.com/acHAu1BMPE4ZuqekT-LqxqPF-poKFwGGf7CqUr4lRXIfwS01pQfdtQlJQDv4vj4H7zP3366u1kK4UWG4IGo7MPukutGEf0Ri5ZslwYxSdU8UqWFZpqqxSNlF1ngKyZsprc9K06t4vw=h400)
4. w800-h400: 너비는 최대 800,  높이는 최대 400픽셀이 됩니다. 비례로 줄어드므로 치수 중 하나는 지정된 크기가 되고 다른 쪽 치수는 그에 맞게 조정됩니다.  
[https://lh3.googleusercontent.com/acHAu1BMPE4ZuqekT-LqxqPF-poKFwGGf7CqUr4lRXIfwS01pQfdtQlJQDv4vj4H7zP3366u1kK4UWG4IGo7MPukutGEf0Ri5ZslwYxSdU8UqWFZpqqxSNlF1ngKyZsprc9K06t4vw=w800-h400](https://lh3.googleusercontent.com/acHAu1BMPE4ZuqekT-LqxqPF-poKFwGGf7CqUr4lRXIfwS01pQfdtQlJQDv4vj4H7zP3366u1kK4UWG4IGo7MPukutGEf0Ri5ZslwYxSdU8UqWFZpqqxSNlF1ngKyZsprc9K06t4vw=w800-h400)
5. w800-h400-c: 너비는 800 높이는 400으로 사진이 잘립니다. 자를 때 사진의 중심을 유지합니다.  
[https://lh3.googleusercontent.com/acHAu1BMPE4ZuqekT-LqxqPF-poKFwGGf7CqUr4lRXIfwS01pQfdtQlJQDv4vj4H7zP3366u1kK4UWG4IGo7MPukutGEf0Ri5ZslwYxSdU8UqWFZpqqxSNlF1ngKyZsprc9K06t4vw=w800-h400-c](https://lh3.googleusercontent.com/acHAu1BMPE4ZuqekT-LqxqPF-poKFwGGf7CqUr4lRXIfwS01pQfdtQlJQDv4vj4H7zP3366u1kK4UWG4IGo7MPukutGEf0Ri5ZslwYxSdU8UqWFZpqqxSNlF1ngKyZsprc9K06t4vw=w800-h400-c)
6. w800-h400-p: 너비는 800 높이는 400으로 사진이 잘립니다. 자를 때 알고리즘을 이용하여 사진의 가장 적절한 부분을 잘라냅니다. 따라서 사진의 중심을 유지하지 않습니다.  
[https://lh3.googleusercontent.com/acHAu1BMPE4ZuqekT-LqxqPF-poKFwGGf7CqUr4lRXIfwS01pQfdtQlJQDv4vj4H7zP3366u1kK4UWG4IGo7MPukutGEf0Ri5ZslwYxSdU8UqWFZpqqxSNlF1ngKyZsprc9K06t4vw=w800-h400-p](https://lh3.googleusercontent.com/acHAu1BMPE4ZuqekT-LqxqPF-poKFwGGf7CqUr4lRXIfwS01pQfdtQlJQDv4vj4H7zP3366u1kK4UWG4IGo7MPukutGEf0Ri5ZslwYxSdU8UqWFZpqqxSNlF1ngKyZsprc9K06t4vw=w800-h400-p)
7. s0, w0, h0, w0-h0: 원본 크기로 가져옵니다.  
[https://lh3.googleusercontent.com/acHAu1BMPE4ZuqekT-LqxqPF-poKFwGGf7CqUr4lRXIfwS01pQfdtQlJQDv4vj4H7zP3366u1kK4UWG4IGo7MPukutGEf0Ri5ZslwYxSdU8UqWFZpqqxSNlF1ngKyZsprc9K06t4vw=s0](https://lh3.googleusercontent.com/acHAu1BMPE4ZuqekT-LqxqPF-poKFwGGf7CqUr4lRXIfwS01pQfdtQlJQDv4vj4H7zP3366u1kK4UWG4IGo7MPukutGEf0Ri5ZslwYxSdU8UqWFZpqqxSNlF1ngKyZsprc9K06t4vw=s0)

## 링크 축소

혹시나 링크가 길어서 보기가 불편하다는 사람이 있다면 다음 3가지 무료 링크 축소 사이트를 추천합니다.

1. [https://tinyurl.com/](https://tinyurl.com/#terms)
2. [https://muz.so/](https://muz.so/)
3. [https://bit.do/](https://bit.do/)

## 참고

- Google Photo Embed [작동방식](https://www.labnol.org/internet/embed-google-photos-in-website/29194/)
- [다이렉트 링크](https://sites.google.com/site/picasaresources/Home/Picasa-FAQ/google-photos-1/how-to/how-to-get-a-direct-link-to-an-image)(+크기변경)
