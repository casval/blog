---
layout: post
title: Tagged Template Literals
categories:
  - study
tags: 
  - javascript
---
## Tagged Template Literals

### Template Literals

ES6 부터 나온 기능으로 백틱( `` )을 사용하여 문자열을 표현하는 것

```javascript
const hello = "Hello"

// before ES6
console.log(hello + 'World!!')

// after ES6
console.log(`${hello} World!!`)
```



### Tagged Template Literals

Template Literals를 이용하여 함수의 인자를 파싱하여 넘겨주는 것.

```javascript
const meal = 'dinner'
const taste = 'good'

const getSnackTaste = (arr, eat, flavor) => {
  const snack = eat === 'breakfast' && 'cookie' || 'milk'
  const feel = flavor === 'bad' && 'good' || 'bad'
  
  return arr[0] + snack + arr[1] + feel + `~`
}

getSnackTaste`Today, ${meal} is ${taste}`
  // Today, cookie is bad~
```

위의 코드에서 함수 getSnackTaste의 파라미터들을 console.log로 출력해 보면 아래와 같이 나타납니다.

`[Today, '', ' is ', ''] 'dinner' 'good'`

Template Literals에서 사용한 문자열은 첫번째 파라미터의 배열로 들어가고 나머지 변수 값들은 각각 파라미터에 값이 들어가는 것을 확인 할 수 있습니다.

Rest Parameters를 이용하여 함수를 다음과 같이 수정하면 파라미터가 가변인자가 되어 확장성 있게 사용할 수 있습니다.

```javascript
const getSnackTaste = (arr, ...values) {
  ...
}
```



Tagged Template Literals를 잘 활용하는 라이브러리는 [styled-components](https://styled-components.com/)입니다.

이글을 쓰게 된 이유도 styled-components를 사용한 코드를 보고 작성하게 되었습니다.

자주 사용되는 문법은 아니지만 알아두면 유익한 문법으로 보입니다.

