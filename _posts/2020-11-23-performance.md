---
title: 성능 최적화
tags:
  - javascript
  - frot-end
---
# 웹 페이지 성능 최적화

## 리소스 요청 수 줄이기

### 이미지 스프라이트

- 여러개의 이미지를 받는 경우 여러번 받아야 하므로 요청 수가 늘어나 로딩 시간이 길어짐
- 하나의 파일에 이미지를 모두 담아 보내면 요청이 하나로 줄어 로딩이 빨라짐

### CSS, JS 번들링

* 번들링 되지 않은 경우 모든 js 및 css에 대해 요청을 하게 되므로 로딩 시간이 길어짐
* 번들러를 사용하여 CSS, JS를 하나의 파일로 번들링 하면 요청을 한번만 하면 되므로 로딩이 짧아짐

### 내부 스타일시트

- link 를 이용하여 외부 스타일시트를 가져오는 대신 문서 내부에 `<style>` 태그를 사용할 수 있는데 이것을 내부 스타일시트라 하며, 외부에 요청하는 요청이 줄어든다.
- 이 경우 리소스 캐시는 사용할 수 없어 html에 css가 매번 포함되므로 필요시에만 사용한다.

### 작은 이미지를 HTML, CSS로 대체

- 아이콘 이미지 개수가 적거나, 크기가 작은 경우 DATA URI로 처리하여 사용할 수 있다.
- 이 경우에도 캐시 문제가 있으므로 필요시에만 사용.

## 리소스의 용량을 줄이기

불필요한 데이터를 제거하고 압축하여 사용한다.

### 중복 코드 제거

- 자주 사용되는 코드는 `util.js`와 같이 하나의 파일로 정리하여 사용.
- 반복된 코드로 용량이 늘어나는 것을 막을 수 있다.

### 만능 유틸 사용 주의하기

- `lodash`와 같은 만능 유틸 사용시 주의.
- `import _ from 'lodash'`로 가져와 사용할 경우 전체가 번들에 포함되므로 파일 용량이 커진다. 
- 필요한 부분만 가져와 사용한다.
- 필요하지 않은 기능이 많은 라이브러리를 사용하는걸 지양. 필요한 것만 들어있는 것으로 선택.

```javascript
// bad
import _ from 'lodash';
_.array(...);
_.object(...);

// good
import array from 'lodash/array';
import object from 'lodash/fp/object';

array(...);
object(...);
```



### HTML 마크업 최적화

- 태그의 중첩을 최소화하여 단순하게 구성
- 공백, 주석 등을 제거하여 사용.
- 권장하는 DOM tree의 node 수는 1500개 미만, depth는 32, child를 갖는 부모 노드는 60개 미만

## 간결한 CSS Selector 사용

스타일 적용시 css selector를 간결하게 사용한다. ID 대신 클래스 선택자를 사용하면 중복 스타일을 묶어 처리할 수 있다.

selector를 최소화하여 사용하도록 한다.

```html
// bad
<html>
  <head>
    <style type="text/css">
      #wrapper {
        border: 1px solid blue; 
      }
      
      #wrapper #foo {
        color: red;
        font-size: 15px;
      }
      
      #wrapper #bar {
        color: red;
        font-size: 15px;
        font-weight: bold;
      }
      
      #wrapper #bar > span {
         color: blue;
         font-weight: normal;
      }
    </style>
  </head>
  <body>
    <div id="wrapper">
      <span id="foo">hello</span>
      <span id="bar">
        javascript <span>world</span>
      </span>
    </div>
  </body>
</html>
```



```html
// good
<html>
  <head>
    <style type="text/css">
      .wrapper {
        border: 1px solid blue; 
      }
      
      .text {
        color: red;
        font-size: 15px;
      }
      
      .strong {
        font-weight: bold;
      }
      
      .wrapper .text {
        color: blue;
        font-weight: normal;
      }
    </style>
  </head>
  <body>
    <div class="wrapper">
      <span class="text">hello</span>
      <span class="text strong">
        javascript <span class="text">world</span>
      </span>
    </div>
  </body>
</html>
```

## 압축하여 사용하기

HTML, JS, CSS 모두 압축 사용 가능. 불필요한 주석이나 공백을 제거 후 난독화 하여 사용한다.

