---
layout: post
title: 타겟 넘버
categories:
  - DFS/BFS
tags: 
  - 알고리즘
---
# 타겟 넘버
[문제](https://programmers.co.kr/learn/courses/30/lessons/43165)
```
function solution(numbers, target) {
  var answer = 0;
  let sum = 0;
  let index = 0;
  
  answer = search(numbers, index, sum, target, answer, false);
  answer = search(numbers, index, sum, target, answer);
  
  return answer;
}

const search = (numbers, index, sum, target, answer, plus = true) => {
  if (plus) {
      sum += numbers[index];
  } else {
      sum -= numbers[index];
  }
  
  if (numbers.length - 1 === index) {
    if (sum === target) {
      answer++;
    }
  } else {
      answer = search(numbers, index + 1, sum, target, answer, false);
      answer = search(numbers, index + 1, sum, target, answer);
  }
  return answer;
}
```
DFS/BFS 카테고리 제목을 안보고 풀었으면 뭔가 삽질을 했을꺼 같은 문제.

모든 케이스에 대해 다 체크해보는 완전 탐색을 썼을꺼 같은데... 문제 유형을 외워야 하나 싶다.

recursive를 이용한 DFS 로 풀긴 했으나 자바 스크립트의 매개변수가 call-by ref. 인줄알고 ref를 넘겨도 answer가 업데이트 되지 않았다.

아마도 객체나 배열만 해당하는 것 같다. 값을 넣는 변수는 값만 넘겨지고 객체의 경우 해당 객체의 주소로 참조하나 보다.

### 회고
문제 유형을 익히지 않으면 어버버 하다 끝날듯. 

이런 문제는 DFS라는 것을 외워야 할것 같다.
