---
layout: post
title: 위장
categories:
  - algorithm
tags: 
  - 알고리즘 - 해시
---
대장, 소장 같은 위장(胃, stomach)이 아니라... [위장(僞裝)](https://namu.wiki/w/%EC%9C%84%EC%9E%A5)

```
function solution(clothes) {
  const m = {};
  clothes.forEach(c => {
      const type = c[1];
      m[type] ? m[type]++ : m[type] = 1;
  });
  const values = Object.values(m);
    const acc = (a,b)=> {
      const ret = a*(b+1);
      return ret;
    }
    const result = values.reduce(acc, 1)-1;    
    return result;
}
```
## 풀이
- 얼굴: n, 상의: m, 하의: k, 겉옷: h 일경우
- (n + 1) * (m + 1) * (k + 1) * (h + 1) -1 이 정답이다.
- 각의 경우의 수에 하나도 안입을 경우 1을 더해 모두 곱한 후 하나도 안입을 경우의 수 1을 빼주면 정답.

### 회고
- 아 level2 인데 너무 어렵게 생각했다. 문제를 제대로 이해 못한것 같기도...
- [reduce](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce) 사용법에 대한 이해 필요.

[출처: programmers](https://programmers.co.kr/learn/courses/30/lessons/42578)

