# WEB

### 01 Intro

WHATWG 주도권 승리

웹표준 : 브라우저 Test, Can I use ?

mdn : 웹 표준문서 입니다. 



### 02 HTML

###### Intro

info.cern.ch

`H`yper `T`ext : 다중으로 연결된 페이지

`M`arkup : 단순 Text 정보에 구조와 의미를 부여하다

`L`anguage : 언어

➡ 태그 등을 이용하여 문서나 데이터의 구조를 명시하는 언어입니다.

​	웹 페이지를 작성하기 위한(의미와 구조를 정의하는) 언어



##### 기본 구조

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
```

- Head 요소 : 브라우저에 나타나지 않는다.
  - Open Graph Protocol : HTML 문서의 메타 데이터를 통해 문서의 정보를 전달합니다.

- Body 요소 : 브라우저에 보여지는 본문이 들어가있습니다.
- DOM (Document Object Model) 트리
- 요소 (element) : 태그와 내용으로 구성되어 있습니다.
- 속성 (attribute) : 태그별로 사용할 수 있는 속성이 다릅니다.



##### 시맨틱 태그

- Block tag
  - img src=" " alt=" "
    - src = "sns_img.png"
    - alt = ""
- Inline tag
