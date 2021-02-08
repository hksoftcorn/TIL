# Wrap Up

> HTML이란, 웹 페이지가 어떻게 구조화되어 있는지 알 수 있도록 하는 마크업 언어
>
> CSS 란, 사용자에게 문서(HTMl)를 표시하는 방법을 지정하는 언어

W3C vs WHATWG

### HTML

##### 정의

- `H`yper `T`ext : 다중으로 연결된 페이지

  `M`arkup : 단순 Text 정보에 구조와 의미를 부여하다

  `L`anguage : 언어

✔ 하이퍼링크를 통해 텍스트를 이동하는 것 + 태그 등을 이용하여 문서나 데이터의 구조를 명시하는 언어



##### 구조

- head 요소 - 정보

문서 제목, 문자코드(인코딩)와 같이 해당 문서 `정보`를 담고 있으며, 브라우저에 나타나지 않습니다.

`Open Graph Protocol`: 메타정보를 표현하는 새로운 규약 

- body 요소 - 내용

`DOM` : Document Object Model 트리, 부모 관계 - 형제 관계

`시맨틱 태그` : 의미론적 요소를 담은 태그의 등장 - header, nav, aside, section, article, footer

`시맨틱 웹` : 웹 상에 존재하는 수많은 웹 페이지들을 메타데이터를 부여하여, 관련성을 가지는 거대한 데이터 베이스로 구착하고자 하는 발상



##### 문서 구조화

- 💥그룹 컨텐츠
  - p
  - hr
  - ol, ul
  - pre, blockquote
  - div

- ✔ 텍스트 관련 요소
  - `a`
  - `b` vs `strong`
  - `i` vs `em`
  - `span`, `br`, `img`

`웹 접근성 (Web Accessibility)`

>  `b` vs `strong` 그리고 `i` vs`em` 기능적인 측면에서는 동일하다. 하지만 후자의 태그들이 단순히 보여지는 강조/기울임을 넘어 페이지 내의 중요한 부분으로 브라우저에게 알려주는 역할을 함. 이는 웹 접근성에 큰 기여를 합니다. 또한 스크린 리더에도 영향을 미칩니다.

- 💥form 기본 속성 : 서버에서 처리될 데이터를 제공하는 역할

  - `action`
  - `method`
  - `input`

- 💥input 기본 속성

  - `name`, `placeholder`
  - `required`
  - `autofocus`

  

### CSS

스타일, 레이아웃 등을 통해 문서(HTML)를 표시하는 방법을 지정하는 언어

##### 구조/정의 방법

- 구조 : 선택자, 선언, 속성,  값
- 정의 방법 : 인라인, 내부 참조, 외부 참조

##### 선택자

- 전체 선택자, 요소 선택자
- ✔ 클래스(.), 아이디 선택자(#), 속성(요소) 선택자(div)
- ✔ 자손 결합자 vs 자식 결합자

1. 하위 선택자(자손 결합자)

   - 하위 선택자는 선택자 사이를 `공백`을 사용하여 나타냅니다.

   - 앞 요소의 `자손`인 뒷 요소를 선택합니다.

     ```css
     section ul {
         text-shadow: none;
     }
     ```

1. 자식 선택자(자식 결합자)

   - 하위 선택자는 선택자 사이를 `>` 를 사용하여 나타냅니다.

   - 앞 요소의 `자식` 인 뒷 요소를 선택합니다.

     ```css
     section > ul {
         text-shadow: none;
     }
     ```

> **자손과 자식의 차이는 무엇일까요?**
>
> 자손은 자식을 포괄하는 의미입니다. 자손은 모든 하위 요소를 의미하고 자식은 바로 아래의 자식 요소에만 적용합니다.
>



##### 우선순위

- ✔ !important / 인라인 / id / class / 요소 / 소스 순서

##### 상속

- 상속이 되는 것은 Text 관련 요소 (font, color, text-align, opacity, visibility)

##### 크기 단위

- px
- %
- em
- rem
- viewport 기준 단위

> `em` : 배수 단위, 요소에 지정된 사이즈에 상대적인 사이즈를 가짐
>
> → 상속의 영향으로 사이즈가 의도치 않게 변경될 수 있음
>
> `rem` : 최상위 요소(html)의 사이즈를 기준으로 배수 단위를 가짐, 1rem = 16px

##### 색상 단위

- 색상 키워드
- RGB 색상 : # + 16진수 표기법, rgb() 함수형 표기법
- HSL 색상 : hsl(120, 100%, 0);

##### Box model

- Margin, Border, Padding, Content
- content-box에서 border-box로 !
- margin 상쇄

##### Display

HTML 요소들을 시각적으로 어떻게 보여줄지 결정하는 속성

- display : block - 블록 안에 인라인 요소가 들어갈 수 있음
  - div / ul, ol, li / p / hr/ form
- display : inline - content 너비만큼 가로 폭을 차지한다. 여백은 line-height만 가능
  - span / a / img / input, label / b, em, i, strong
- display : inline-block
- display : none (vs visibility: hidden)

##### ✔ Position

문서 상에서 요소를 배치하는 방법을 지정한다.

- `static` : 기본값

- relative : 원래 위치에서 (상대적인) 위치 계산
- absolute : 상위 요소인 static 위치를 기준으로 위치 결정
- fixed : 브라우저 화면의 상대 위치

##### float

- float 하면 공백이 생김, 이를 해결하려면 float한 객체와 똑같은 block을 만들어줘야함

  ```css
  .clearfix::after {
      content: "";
      display: block;
      clear: both;
  }
  ```

##### Layout - Flexbox

요소 간 공간 배분과 정렬 기능을 위한 1차원(단반향) 레이아웃

- 축과 요소 
- Justify-content / align-items

### Bootstrap

다양한 화면 크기를 가진 디바이스들이 등장함에 따라 Responsive web design 개념이 등장했음

- CDN : 컨탠츠를 효율적으로 전달하기 위해 여러 노드에 가진 네트워크에 데이터를 제공하는 시스템

  ​			개별 end-user의 가까운 서버를 통해 빠르게 전달이 가능(지리점 이점), 외부 서버이용

##### Layout - Grid

- 12개의 column

- 6개의 grid breakpoints : ss / sm(576) / md(768) / lg(992) / xl / xxl

  ```css
  <div class="container">
      <div class="row">
        <div class="col"></div>
        <div class="col"></div>
        <div class="col"></div>
      </div>
  </div>
  ```

  