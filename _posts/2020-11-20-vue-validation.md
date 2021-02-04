---
layout: post
title: Vueform  validateion
categories:
  - vue
tags:
  - front-end
---

# Vue의 Form Validation

HTML5는 required 속성을 제공하여 빈 인풋에 대한 validation을 제공한다.

```html
<input required>
```

이것은 빈 인풋을 submit 하려 하면 자동 에러메시지를 제공한다.

<img src="https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1578365475785_0.png?alt=media&token=949c4be5-ff5e-455e-a4c1-71d7fad53d11" width="300">

## .prevent modifier

Event modifier로 submit 이벤트가 페이지를 다시 로드하는 것을 방지하는 데 사용되는 이벤트 수정자.

```html
<form class="review-form" @submit.prevent="onSubmit">
      <p>
        <label for="name">Name:</label>
        <input id="name" v-model="name" placeholder="name">
      </p>
    ...
```



## .number modifier

v-model에서 제공하는 .number modifier는 좋은 기능이지만 버그가 있다. 값이 비면 String을 반환하는 버그인데, 이것은 Number() 를 이용하여 wrapping 함으로써 해결할 수 있다.

```html
<input
      v-model.number="coffee"
      type="number"
      name="coffee"
    > Coffee <br/>
```

```javascript
Number(this.coffee)
```



출처: [Vue.js: Forms & v-model - Intro to Vue2](https://www.vuemastery.com/courses/intro-to-vue-js/forms)
