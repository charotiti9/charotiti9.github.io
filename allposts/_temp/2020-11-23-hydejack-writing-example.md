---
layout: post
title: Hydejack writing example
tags: [hydejack, example]
image: /assets/img/blog/hydejack-9.jpg
description: >
  Hydejack 작성 시 참고사항 정리
date: 2020-11-23
modified: 2020-11-27
comments: true
sitemap: false
---

다양한 Hydejack 작성 방법을 모아놓은 페이지.

## quotes  
인용은 `>`를 앞에 붙인다.
> Curabitur blandit tempus porttitor. Nullam quis risus eget urna mollis ornare vel eu leo. Nullam id dolor id nibh ultricies vehicula ut id elit.

### Large quotes
커다란 인용은 `>`를 쓴 문장 하단에 `{:.lead}`를 붙인다.
> You can make a quote "pop out".
{:.lead}

## Link
하위 링크 참조하기 `[설명](하위 헤더 이름)`  
[설명](another-page) 

해당 페이지 제목 참조하기 `[설명](#헤더명)` 헤더명은 소문자로, 띄어쓰기는 `-`처리.   
[링크](#link)

다른 페이지 참조하기 `[제목](../../파일명.md){:.heading.flip-title}`
[CHANGELOG](../../CHANGELOG.md){:.heading.flip-title}

이건 무엇인가
`<a href="#">`과 `</a>`를 쓰면 이렇게 된다. 하지만 뭔지 모르겠다.
 <a href="#">본문 열기 같은 것인가</a>, 음 모르겠다. 

## Table contents
테이블 콘텐츠 
`*toc`: 숫자 없는 테이블 콘텐츠
`1. toc`: 숫자 있는 테이블 콘텐츠
`{: toc}`: 기본 테이블 콘텐츠
`{: toc .large-only}`: 1650 이상 너비에서만 작동
따라다니게 하려면 어떻게 해야하지? -> 유료결제가 답
* this unordered seed list will be replaced by toc as unordered list
{:toc}

## Inline HTML elements

모든 HTML 목록은 이쪽을 참고  
[Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/HTML/Element).

- **굵은 글씨**, use `**To bold text**`.
- *이탤릭 체*, use `*To italicize text*` 혹은 `_italic_`
- 마우스 오버레이 설명,  `*[단어]: 설명`.
- 인용, <cite>&mdash; 말한 사람</cite>, should use `<cite>`.
- ~~취소선~~, use `~~deleted~~` 
- <ins>밑줄</ins>, use `<ins>`
- 위로 글쓰기 <sup>text</sup> uses `<sup>` and 아래로 글쓰기 <sub>text</sub> uses `<sub>`.

*[오버레이]: 중간에 넣어도 되지만 하단에 모아서 쓰는게 나중에 찾아보기 편할 듯 하다.

## Code

~~~js
// Example can be run directly in your JavaScript console

// Create a function that takes two arguments and returns the sum of those
// arguments
var adder = new Function("a", "b", "return a + b");

// Call the function
adder(2, 6);
// > 8
~~~


## Lists

숫자 없는 리스트

* Praesent commodo cursus magna, vel scelerisque nisl consectetur et.
* Donec id elit non mi porta gravida at eget metus.
* Nulla vitae elit libero, a pharetra augue.

숫자로 된 리스트

1. Vestibulum id ligula porta felis euismod semper.
2. Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus.
3. Maecenas sed diam eget risus varius blandit sit amet non magna.


구조화된 리스트
- level 1 item
  - level 2 item
  - level 2 item
    - level 3 item
    - level 3 item
- level 1 item
  - level 2 item
  - level 2 item
  - level 2 item
- level 1 item
  - level 2 item
  - level 2 item
- level 1 item


설명 리스트

HyperText Markup Language (HTML)
: The language used to describe and define the content of a Web page

Cascading Style Sheets (CSS)
: Used to describe the appearance of Web content

JavaScript (JS)
: The programming language used to build advanced Web sites and applications


## Images

이미지 두 개 붙여 쓰기  
`<img src="image1.png" width="45%">`  
`<img src="image2.png" width="45%">`

이미지 가운데 정렬
`<p align="center"><img src="image2.png"></p>`

이미지 쓰는 법
: `![이름](링크 "설명"){:.lead data-width="넓이" data-height="높이"}`

캡션 달기
: 이미지 하단에 설명쓰고 그 밑에 `{:.figure}` 달기.

![800x400](https://placehold.it/800x400 "Larg example image")

![400x200](https://placehold.it/400x200 "Medium example image")

![200x200](https://placehold.it/200x200 "Small example image")

캡션 예제
![Full-width image](https://placehold.it/800x100){:.lead data-width="800" data-height="100"}
A caption to an image.
{:.figure}


## Tables


| Name     | Upvotes   | Downvotes |
|:---------|:----------|:----------|
| Alice    |        10 |        11 |
| Bob      |         4 |         3 |
| Charlie  |         7 |         9 |
|==========|===========|===========|
|Totals    |        21 |        23 |


### Large Tables
테이블 아래 `{:.scroll-table}` 사용
왼쪽 정렬 `:-----` 사용
오른쪽 정렬 `-----:` 사용
가운데 정렬 `:----:` 사용

| Default aligned |Left aligned| Center aligned  | Right aligned  | Default aligned |Left aligned| Center aligned  | Right aligned  | Default aligned |Left aligned| Center aligned  | Right aligned  | Default aligned |Left aligned| Center aligned  | Right aligned  |
|-----------------|:-----------|:---------------:|---------------:|-----------------|:-----------|:---------------:|---------------:|-----------------|:-----------|:---------------:|---------------:|-----------------|:-----------|:---------------:|---------------:|
| First body part |Second cell | Third cell      | fourth cell    | First body part |Second cell | Third cell      | fourth cell    | First body part |Second cell | Third cell      | fourth cell    | First body part |Second cell | Third cell      | fourth cell    |
| Second line     |foo         | **strong**      | baz            | Second line     |foo         | **strong**      | baz            | Second line     |foo         | **strong**      | baz            | Second line     |foo         | **strong**      | baz            |
| Third line      |quux        | baz             | bar            | Third line      |quux        | baz             | bar            | Third line      |quux        | baz             | bar            | Third line      |quux        | baz             | bar            |
| Second body     |            |                 |                | Second body     |            |                 |                | Second body     |            |                 |                | Second body     |            |                 |                |
| 2 line          |            |                 |                | 2 line          |            |                 |                | 2 line          |            |                 |                | 2 line          |            |                 |                |
| Footer row      |            |                 |                | Footer row      |            |                 |                | Footer row      |            |                 |                | Footer row      |            |                 |                |
{:.scroll-table}


## 평행선 긋기

* * *

## Message boxes
**NOTE**: 메세지 박스는 `{:.messgae}`를 하단에 붙인다. 
 
한칸을 띄어쓰면 메세지 박스는 사라진다.  
붙여 쓰면 같이 쓸 수 있다.  
{:.message}

## Large text
다른 글씨보다 좀 더 크게 쓰려면 `{:.lead}`를 하단에 붙인다.

마찬가지로 띄어쓰면 사라진다.  
붙여쓰면 같이 사용가능하다.
{:.lead}

## Faded text
`{:.faded}`를 쓰면 흐릿하게 나온다.

하지만 별로 티가 안나는듯 함. 그래도 언젠가 쓸 일이 있겠지.
{:.faded}



*[HTML]: HyperText Markup Language
*[CSS]: Cascading Style Sheets
*[JS]: JavaScript
