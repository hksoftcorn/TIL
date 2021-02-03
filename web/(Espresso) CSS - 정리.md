# CSS

### CSS (Cascading Style Sheets)

기본 개념 | 선택자 | 단위 | Box Model | Layout

```css
h1 {
    color: blue;
    font-size: 100px;
}
```

- 정의 방법 - 1. 인라인
- 정의 방법 - 2. 내부 참조
- 정의 방법 - 3. 외부 참조



##### 선택자

> - 기본 선택자
>   - 전체 선택자, 요소 선택자
>   - 클래스 선택자, 아이디 선택자, 속성 선택자
>     - 클래스 선택자 : 마침표(.) 문자로 시작하며 해당 클래스가 적용된 문서의 모든 항목을 선택합니다.
>     - 아이디 선택자 : 샵(#) 문자로 시작하며 기본적으로 클래스 선택자와 같은 방식으로 사용.
>     - id는 문서당 한 번만 사용해야 함, 단일 id 값만 적용할 수 있다.
> - 결합자
> - 의사 클래스/요소
> - 우선순위
>   - important / 인라인 / id 선택자 / class 선택자 / 요소 선택자
>   - 소스 코드 순서
> - CSS 상속
>   - 상속 되는 것 예시 : Text 관련 요소
>   - 상속 되지 않는 것 : Text 외의 요소들 예시!



##### 단위

> - (상대) 크기 단위
>   - px (픽셀)
>   - %
>   - em : 배수 단위, 요소에 지정된 사이즈에 상대적인 사이즈를 가짐
>   - `rem` : 최상위 요소(html)의 사이즈를 기준으로 배수 단위를 가짐.
>   - Viewprot
> - 색상 단위
>   - 색상 키워드
>   - `RGB 색상`
>   - HSL 색상
> - 문서 표현
>   - 텍스트
>   - 컬러, 배경
>   - 목록 꾸미기



##### CSS Box Model

> CSS에서는 모든 객체를 네모 Box로 바라봐야 합니다.
>
> 1. Margin : 박스 테두리 밖 여백입니다.
> 2. Border : 박스 테두리 영역입니다.
> 3. Padding : 테두리 안쪽 여백입니다.
> 4. Content : 글이나 이미지 등 요소의 실제 내용입니다.
>
> 
>
> - Margin
>   - 시계방향으로 margin 값 설정
> - Border
>   - border-width
>   - border-style
>   - border-color
> - Padding
> - Content
>
> - Box-sizing
>   - content-box vs border-box



##### CSS Display

> - 블록 레벨 요소
> - 인라인 레벨 요소
> - Inline-block
> - None



##### CSS Position

> - Static : 브라우저 좌측 상단 고정
> - Coordinate Property
>   - relative : 상대위치 
>   - absolute : 절대위치 (집나간 형)
>   - fixed : 스크롤시에도 항상 같은 곳에 위치



##### CSS Emmet

>  HTML & CSS를 작성할 때 보다 빠른 마크업을 위해서 사용되는 오픈소스입니다.

```css
#container>div.log+ul#navbar>li*5>a{item $}*3
```



### ✅ CSS layout

Display | Position | Float | Flexbox | Grid with Bootstrap

##### Float

> 본래는 이미지 좌, 우측 주변으로 텍스트를 둘러싸는 레이아웃을 위해 도입했습니다. 더 나아가 현재는 이미지 뿐만 아닌 다른 요소들에도 적용해 웹 사이트의 전체 레이아웃을 만드는데까지 발전하게 됐습니다. 하지만 absolute와 같이 기존에 있던 박스 위치를 잃어버리기 때문에, 다른 박스 요소에게 영향을 줍니다. `clearfix::after` float 부모 이후 자식들에게 `clear: both;` float 속성을 해제시킨 빈 상자를 만들어, 다른 컴포넌트가 움직이는 것을 방지합니다. 최근에는 다이내믹 그리드와 같이 동적영역을 담당하는 기술들이 생겨 float가 초기의 목적으로 회귀하게 되었고 mdn에서는 legacy field, 오래된 기술로 치부하고 있습니다.
>
> - 속성
>   - none, left, right
>   - clearfix::after

⬜ 0203-00_float.html 

```css
// head - style
  <style>
    .box {
      width: 150px;
      height: 150px;
      border: 1px solid black;
      background-color: crimson;
      margin: 20px;
    }

    .left {
      float: left;
    }

    .right {
      float: right;
    }

  </style>

// body
<div class="box">float left</div>
<div class="box right">float right</div>
<p>lorem300</p>

```

⬜ 0203-01_float.html 

```css
  <style>
    .box1 {
      width: 150px;
      height: 150px;
      border: 1px solid black;
      background-color: crimson;
      color: white;
      text-align: center;
      line-height: 150px;
    }

    .box2 {
      width: 300px;
      height: 150px;
      border: 1px solid black;
      background-color: blue;
      color: white;
      text-align: center;
      line-height: 150px;
    }

    .clearfix::after {
      content: "";
      display: block;
      clear: both;
    }
  </style>

// body

<header class="clearfix">
  <div class="box1">div</div>
</header>
<div class="box2">div</div>
```

>  ✅ TIL in CSS-float
>
>  - lorem300 : 아무 말 300자를 넣을 수 있다.
>
>  - float 기능을 넣고 싶으면 부모에  clearfix 속성을 추가하자.
>
>    .clearfix::after {
>          content: "";
>          display: block;
>          clear: both;
>        }



##### Flexible Box Layout

> 요소 간 공간 배분과 정렬 기능을 위한 1차원(단반향) 레이아웃 기능을 제공합니다. `요소`와 `축` 두 가지의 속성이 중요합니다. 축은 main-axis 방향만을 고려하면 됩니다. main-axis는 row가 기본 축입니다. reverse는 메인 축의 item들이 쌓이는 순서가 반대로 바뀌는 것입니다. 교차축은 메인축의 수직을 이룹니다. 
>
> 축을 정했으니 정렬하는 방법으로 우선 justify와 align 방법 두 가지가 있습니다. `justify`는 메인축 정렬, `align`은 교차축 정렬을 의미합니다. 이후 `content`는 여러 줄, `items` 한 줄, `self`는 개별 요소를 의미합니다. 정렬 방향으로는 start, end, center 등을 설정할 수 있습니다.
>
> flex의 시작은 부모 태그에 display flex를 선언해야 합니다.
>
> - 요소
>   - Flex Container (부모 요소)
>   - Flex Item (자식 요소)
> - 축
>   - Main Axis (메인축)
>   - Cross Axis (교차축)
> - 속성
>   - flex-direction : 배치 방향 설정
>   - justify-content : 메인축 방향 정렬
>   - align-items, align-self, align-content : 교차축 방향 정렬
>   - flex-wrap, flex-flow, flex-grow

⬜ 0203-02_flexbox.html 

```css
<style>
    .flex-container {
      display: flex;
      display: inline-flex;
    }
</style>

<div class="box flex-container">
    <div class="item1">1</div>
    <div class="item2">2</div>
    <div class="item3">3</div>

// 정렬하려고 하는 부모 요소에 선언합니다.
// display: flex; 상자들이 왼쪽에서 오른쪽으로 쌓이게 됩니다.


<style>
    .flex-container {
      display: flex;
      flex-wrap: wrap;
    }
</style>

// 요소들을 강제로 한 줄에 표현할 것인지 설정합니다.
// flex-wrap: wrap; container 초과분은 다음 행으로 넘어갑니다.


<style>
  .flex-container {
    flex-direction: row;
    flex-direction: row-reverse;      
    flex-direction: column;
    flex-direction: column-reverse;
  }
</style>

// 메인축 방향 설정, 쌓이는 방향을 설정합니다.
// flex-direction: row-reverse; 쌓이는 순서가 오른쪽에서 왼쪽으로 쌓이게 됩니다.
// flex-direction: column; 쌓이는 순서가 위에서 아래으로 쌓이게 됩니다.
// flex-direction: column-reverse; 쌓이는 순서가 아래에서 위로 쌓이게 됩니다.

<style>
  .flex-container {
    justify-content: flex-start;
    justify-content: flex-end;
    justify-content: center;
    justify-content: space-between;
    justify-content: space-around;
    justify-content: space-evenly;
  }
</style>

// 
// justify-content: flex-start;
// justify-content: flex-end;
// justify-content: center;
// justify-content: space-between;
// justify-content: space-around;
// justify-content: space-evenly;


<style>
  .flex-container {
      align-items: stretch;
      align-items: flex-start;
      align-items: flex-end;
      align-items: center;
  }
</style>

// align-items: stretch; 기본값
// align-items: flex-start; 왼쪽 상단 작은 박스
// align-items: flex-end; 왼쪽 하단 작은 박스
// align-items: center; 왼쪽 가운데 작은 박스 
// TIP 정중앙 : justify-content: center; + align-items: center;


<style>
  .flex-container {
	  /* flex-direction과 flex-wrap의 shorthand */
      flex-flow: column wrap;
  }
</style>



<style>
    .item1 {
      align-self: flex-start;
    }

    .item2 {
      align-self: center;
    }

    .item3 {
      align-self: flex-end;
    }
</style>
<style>
  .item1 {
      order: 0;
  }
  .item2 {
      order: -1;
  }
  .item3 {
      order: 1;
  }
</style>
<style>
  .item1 {
      flex-grow: 1;
  }
  .item2 {
      flex-grow: 2;
  }
  .item3 {
      flex-grow: 3;
  }
</style>



// self만 특별하게 개별요소에 적용합니다!
// align-self : 개별 요소의 위치를 정해줍니다.
// order : 기본값 0 입니다. 1로 값을 주면 기본값보다 크게 되어 순서가 뒤로 가게됩니다.
// flex-grow : 주축에서 남는 영역을 분배/할당하는 것을 말합니다. 🔺크기비율이 아닙니다!

```



⬜ 0203-03_flexbox_card.html 

> 1. 전체 구조를 확인합니다.
>    - card
>      - card-nav
>      - card-header
>      - card-body
>      - card-footer

```
.card {
  display: flex;
  /* 주축 column으로 변경 */
  flex-direction: column; 
  
  width: 700px;
  border: 2px dashed black;

  /* header와 footer는 시작과 끝에 붙어있음 */
  justify-content: space-between;
  margin: 0 auto;
}

.card-nav {
  display: flex;
  justify-content: center;
}
```

>  ✅ TIL in CSS-flex
>
>  - main-axis : justify
>
>  - cross-axis : align
>
>  - https://flexboxfroggy.com/
>
>    ![img]()





### Responsive Web Design

> 반응형 웹의 정신(spirit) : one source, multi usage 
>
> `Bootstrap` - quickly `responsive` and the world's `most popular` front-end 

### Bootstrap

> [Bootstrap](https://getbootstrap.com/) 공식 사이트로 들어갑니다. Download에서 Compiled CSS and JS 압축파일을 다운로드합니다.
>
> bootstrap.css 와 bootstrap.min.css 내용에 차이가 없지만, 후자가 모든 코드가 한 줄에 입력되어 있습니다. 하지만 실습에서는 보기 편한 bootstrap.css 파일을 선택합니다. js에서는 bootstrap.bundle.js 파일을 선택합니다.
>
> html 문서의 변경사항이 곧바로 적용됩니다. 이는 자동으로 초기화했기 때문입니다. 초기화하는 방법은 Reset과 Normalize 두 가지가 있습니다. 요즘의 초기화 방법은 Normalize 방법을 채택합니다. Reset은 aggresive한 방법이며 Normalize는 gentle한 방법입니다.

##### CDN

Content Delivery(Distribution) Network

컨텐츠를 효율적으로 전달하기 위해 여러 노드에 가진 네트워크에 데이터를 제공하는 시스템입니다. 개별 end-user와 가까운 서버를 통해 빠르게 전달 가능(지리적 이점)과 외부 서버를 활용함으로써 본인 서버의 부하가 적어진다는 장점이 있습니다.

> - ```
>   <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta1/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-giJF6kkoqNQ00vy+HMDP7azOuL0xtbfIcaT9wjKHr8RbDVddVHyTfAAsrekwKmP1" crossorigin="anonymous">
>   ```
>
> - 
>   
>   ```css
>   <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta1/dist/js/bootstrap.bundle.min.js" integrity="sha384-ygbV9kiqUc6oa4msXn9868pTtWMgiQaeYH7/t7LECLbyPA2x65Kgf80OJFdroafW" crossorigin="anonymous"></script>
>   ```

##### spacing

>  ```css
> .mt-1 {
>     margin-top: 0.25rem !important;
> }
> .mx-0 {
>     margin-right: 0 !important;
>     margin-left: 0 !important;
> }
> .py-0 {
>     padding-top: 0 !important;
>     padding-bottom: 0 !important;
> }
>  ```
>
> - mt-1 = 0.25 rem = 16 px * 0.25 = 4px
> - mx-0  / mx-auto
> - py-0 / py-auto

##### Grid

> *"Use our powerful mobile-first flexbox grid to build layouts of all shapes and sizes thanks to a twelve column system, six default responsive tiers, Sass variables and mixins, and dozens of predefined classes."*
>
> Bootstrap Grid system은 flexbox로 제작되었습니다. container, rows, column으로 컨탠츠를 배치하고 정렬합니다.
>
> - 속성
>   - 12개의 column
>   - 6개의 grid breakpoints
>
> ```css
> <div class="container">
>   <div class="row">
>     <div class="col">
>       1 of 2
>     </div>
>     <div class="col">
>       2 of 2
>     </div>
>   </div>
>   <div class="row">
>     <div class="col">
>       1 of 3
>     </div>
>     <div class="col">
>       2 of 3
>     </div>
>     <div class="col">
>       3 of 3
>     </div>
>   </div>
> </div>
> ```
>
> - size options
>   - (xs,) sm, md, lg, xl, xxl

##### Nesting

> 중첩 Grid라고도 합니다.
>
> ```css
> <h2>nesting</h2>
>   <div class="row">
>     <div class="box col-6">
>       <div class="row">
>         <div class="box col-3">1</div>
>         <div class="box col-3">2</div>
>         <div class="box col-3">3</div>
>         <div class="box col-3">4</div>
>       </div>
>     </div>
>     <div class="box col-6">2</div>
>     <div class="box col-6">3</div>
>     <div class="box col-6">4</div>
>   </div>
> ```
>

##### Grid breakpoints

> ```css
> <div class="row">
>     <div class="box col-2 col-sm-8 col-md-4">1</div>
>     <div class="box col-8 col-sm-2 col-md-4">2</div>
>     <div class="box col-2 col-sm-2 col-md-4">3</div>
> </div>
> ```

##### offset

> ```css
> <h2>offset</h2>
> <div class="row">
>     <div class="box col-4 offset-4">1</div>
>     <div class="box col-4">2</div>
> </div>
> ```

##### alignment

> ```css
> <h2>alignment</h2>
> <div class="row parent justify-content-center align-items-center">
>     <div class="box col-4">1</div>
> </div>
> ```

##### Position

> ```css
> // fixed 
> <div class="box fixed-top">fixed-top</div>
> <div class="box fixed-bottom">fixed-bottom</div>
> 
> // 다만 sticky를 쓸 때는 fixed를 주석처리 해야한다.
> <div class="box sticky-top bg-primary">sticky-top</div>
> <div class="box sticky-top bg-success">sticky-top</div>
> ```

