---
categories: [SWACADEMY]
---

# 반응형 웹

- **반응형**이란 하나의 웹사이트에서 PC, Table, Mobile 등 접속하는 기기의 화면 크기에 맞게 사이트가 자동으로 반응하는 기법
- **적응형**이란 접속하는 기기에 따라서 PC용 웹사이트, Mobile용 웹사이트를 보여주는 기법

## 1. HTML
***

### 1.1. 반응형 페이지를 만들기 위한 메타 태그

```html
<meta name="viewport" content="width=device-witdh, initial-scale=1.0"
```
viewport meta 태그를 추가하여 페이지가 호출될 때 어떻게 화면을 보여줄지 계산해서 보여줄 수 있다.
- `name` : 메타태그 중 viewport를 사용하겠다고 선언한다.
- `content` : viewport가 가진 content 값을 지정하여 사용한다.
  - `width` : 브라우저에게 "페이지의 가로 너비"를 계산하는 방법을 알려주며 "device-width"를 지정하면 접속하는 기기의 브라우저 너비에 맞게 페이지를 맞춰준다.
  - `initial-scale` : 최초 화면 확대 값으로, 1.0을 두는 것으로 페이지를 확대하지 않고, 1배율로 보겠다고 정의한다.

### 1.2. 반응형 이미지 처리
- picture 태그를 사용하여 의도한 viewport 너비에 원하는 이미지를 넣을 수 있다.
```html
<picture>
  <source media="(max-width: 768px)" srcset="https://via.placeholder.com/1000x1000?text=Mobile">
  <source media="(max-width: 1024px)" srcset="https://via.placeholder.com/1000x1000?text=Tablet">
  <img src="https://via.placeholder.com/1000x1000?text=PC" alt="대체 텍스트">
</picture>
```
## 2. CSS
***
### 2.1. 미디어 쿼리(Media Query)
- 기기의 유형과 어떤 특성이나 수치에 따라 스타일을 수정할 수 있다.
- `@media`로 시작한다.
- 기기 유형
  - `all` : 모든 장치
  - `print` : 인쇄 결과물 및 출력 미리보기
  - `screen` : 화면
- 규칙
  - 뷰포트 너비(가장 많이 사용)
  - 뷰포트의 가로세로비

```css
div {
  font-size: 20px;
}

@media screen and (max-width: 1024px) {
  div {
    font-size: 16px;
  }
}
```
- 데스크톱에선 글씨 크기가 20px
- 뷰포트 너비가 1024px 이하에선 16px로 줄어든다.
- and 연산자를 통해 조건을 계속 추가할 수도 있다.

### 2.2. CSS의 호출 방법
- `<link>` 태그를 사용하여 원하는 유형과 조건에 따라 스타일 파일을 불러올 수 있다.
```html
<link rel="stylesheet" href="prin.css" media="print">
<link rel="stylesheet" href="mobile.css" media="screen and (max-width: 600px)"
```
### 2.3. 주의사항
1. 레이아웃의 "고정 크기"의 사용 지양
   - width값을 고정하면 반응형 매 구간마다 고정값을 변경해야 하는 불편함이 발생할 수 있다.
     - max-width를 사용한 "최대 너비" 적극 활용
   - height값을 고정하면 유동적으로 변하는 높이에 대응할 수 없다.
     - min-height를 사용한 "최소 높이" 적극 활용
2. 인라인 스타일은 반응형을 처리할 수 없다.
   - 태그에 style 속성으로 직접 들어간 인라인 CSS는 미디어 쿼리를 적용할 수 없다.