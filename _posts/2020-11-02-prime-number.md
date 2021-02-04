---
layout: post
title: 소수찾기
categories:
  - 완전탐색
tags: 
  - 알고리즘
---

# 소수찾기
문제[프로그래머스](https://programmers.co.kr/learn/courses/30/lessons/42839)

```
function solution(numbers) {
  var answer = 0;
  const prime = [];
  
  combination('', numbers, prime);
  
  return prime.length;
}

const combination = (str, remains, prime) => {
  for(let i = 0; i < remains.length; i++) {
      const newStr = str + remains[i];
      const num = Number(newStr);
      if (isPrime(num)) {
          if (!prime.includes(num)) {
            prime.push(num);
          }
      }
      if (remains.length !== 1) {
          const newRemains = remains.concat([]);
          const ra = newRemains.split('');
          ra.splice(i, 1);
          combination(newStr, ra.join(''), prime);
      }
  }
}

const isPrime = (num) => {
  if (num === 0 || num === 1) {
    return false;
  }

  // 소수 체크: 해당 숫자를 i의 2부터 제곱근까지 나누어 본다
  // -> i를 제곱한 수로 나눈다와 같은 의미
  for (let i = 2; i*i <= num; i++) {
      if(num % i === 0) {
          return false;
      }
  }
  return true;
}

console.log(solution('17'));

```

### 회고
전체 조합(combination). 소수를 판단 하는 법을 필요로 하는 문제
전체 조합에 대해 모두 소수 체크를 하는 완전 탐색.
소수 체크: (https://myjamong.tistory.com/139)
