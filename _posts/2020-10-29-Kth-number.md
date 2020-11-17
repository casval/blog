---
layout: post
title: K번째수
categories:
  - algorithm
tags: 
  - 알고리즘 - 정렬
---

# K번째수

js의 Array에서 제공하는 splice와 sort를 이용하면 풀수 있는 간단한 문제
```
function solution(array, commands) {
  var answer = [];
  commands.forEach(cmd => {
      const clone = array.concat([]);
      const result = clone.splice(cmd[0] - 1, cmd[1] - cmd[0] + 1);
      result.sort((a, b) => a-b);
      answer.push(result[cmd[2] - 1])
  });
  return answer;
}
```
지만 sort를 제대로 이해하지 못해 틀림;;

아래의 내용을 숙지해야 함
## Array.sort 
### 문자
```
var fruit = ['orange', 'apple', 'banana'];

/* 일반적인 방법 */
fruit.sort(); // apple, banana, orange
```

### 숫자
```
var score = [4, 11, 2, 10, 3, 1]; 

/* 오류 */
score.sort(); // 1, 10, 11, 2, 3, 4 
              // ASCII 문자 순서로 정렬되어 숫자의 크기대로 나오지 않음

/* 정상 동작 */
score.sort(function(a, b) { // 오름차순
    return a - b;
    // 1, 2, 3, 4, 10, 11
});

score.sort(function(a, b) { // 내림차순
    return b - a;
    // 11, 10, 4, 3, 2, 1
});
```

### 객체
```
ar student = [
    { name : "재석", age : 21},
    { name : "광희", age : 25},
    { name : "형돈", age : 13},
    { name : "명수", age : 44}
]

/* 이름순으로 정렬 */
student.sort(function(a, b) { // 오름차순
    return a.name < b.name ? -1 : a.name > b.name ? 1 : 0;
    // 광희, 명수, 재석, 형돈
});

student.sort(function(a, b) { // 내림차순
    return a.name > b.name ? -1 : a.name < b.name ? 1 : 0;
    // 형돈, 재석, 명수, 광희
});

/* 나이순으로 정렬 */
var sortingField = "age";

student.sort(function(a, b) { // 오름차순
    return a[sortingField] - b[sortingField];
    // 13, 21, 25, 44
});

student.sort(function(a, b) { // 내림차순
    return b[sortingField] - a[sortingField];
    // 44, 25, 21, 13
});
```

[출처: programmers](https://programmers.co.kr/learn/courses/30/lessons/42748)

[참고: 개인 블로그](http://dudmy.net/javascript/2015/11/16/javascript-sort/)

[문서: MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)
