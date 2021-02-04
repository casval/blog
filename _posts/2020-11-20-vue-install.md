---
layout: post
title: Vue install
categories:
  - vue
tags:
  - front-end
---

# Vue.js 사용법

## 호환성

Vue는 ES5 기능을 사용하기 때문에 IE8 이하 버전을 지원하지 않음.

모든 ES5 호환 브라우저를 지원



## 사용방법

### 직접 <script>에 추가

[다운로드](https://kr.vuejs.org/js/vue.js)를 받아 vue.js를 script에 추가.

```html
<script src="./vue.js"
```

### CDN

프로토타이핑 또는 학습 목적

```html
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```

프로덕션 - 특정 버전의 빌드를 추가해야 새 버전에서 추가된 기능으로의 오류를 막을 수 있다.

```html
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.0"></script>
```

기본 ES 모듈을 사용하는 경우 ES 모듈 호환 빌드 파일을 제공

```javascript
<script type="module">
  import Vue from 'https://cdn.jsdelivr.net/npm/vue@2.6.11/dist/vue.esm.browser.js'
</script>
```

https://cdn.jsdelivr.net/npm/vue/ 에서 npm 패키지의 소스를 볼 수 있다.

[Vue의 각 빌드간 차이점](https://kr.vuejs.org/v2/guide/installation.html#%EA%B0%81-%EB%8B%A4%EB%A5%B8-%EB%B9%8C%EB%93%9C%EA%B0%84-%EC%B0%A8%EC%9D%B4%EC%A0%90)

## NPM 

Vue를 이용한 대규모 어플리케이션 구축시 npm을 이용한 설치를 권장

[SFC](https://kr.vuejs.org/v2/guide/single-file-components.html)를 위한 도구도 제공

```bash
$ npm install vue
```

## CLI

Vue.js는 단일 페이지 어플리케이션 구축을 위한 [CLI](https://github.com/vuejs/vue-cli)를 제공.

