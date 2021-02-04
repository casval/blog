---
layout: post
title: 가장 큰 수
categories:
  - 정렬
tags: 
  - 알고리즘
---
소트 함수를 구현하는 간단한 문제 였지만, 난 어렵게 생각하고 객체까지 만들며 풀었다...

풀고나서 패스가 안되서 사람들이 준 테스트 케이스를 이용하여 해결하고 나니!!

이것은 정렬 내부에서 미리 비교만 해보면 되는 문제였구나!!

문제에서 가장 큰수를 만들라고 하면 이건 소팅을 하면되고, 그 소팅 조건이 가장 큰 수를 만드는 조건으로 하면 되는 문제.

## 어렵게 푼 답안 (삽질에 삽질)
```
function solution(numbers) {
  var answer = '';
  
  let values = Array.from({length: 1000}, () => []);
  numbers.forEach(n => {
      const strN = String(n);
      const key = strN[0];
      let v = values[Number(key)];
      if (!v.maxLength) {
          v.maxLength = 0;
      }
      
      v.maxLength = strN.length > v.maxLength ? strN.length : v.maxLength;
      const nv = {
          key,
          realValue: strN,
      }
      v.push(nv);
  });
  
  values = values.filter(vs => vs.length > 0);

  values.reverse().forEach(vs => {
      if (vs.length > 1) {
        vs.sort((a, b) => {
            const valueA = a.realValue + b.realValue;
            const valueB = b.realValue + a.realValue;
            return Number(valueB) - Number(valueA);
        });
      }
      vs.forEach(v => {
          answer += v.realValue;
      })
  });

  if (Number(answer) === 0) {
    answer = '0';
  }

  return answer;
}

console.log(solution([3, 30, 34, 5, 9]))
```
맨 앞자리를 키로 각각 분류해 넣고 자리수가 다르면 가장 긴 숫자의 자리수만큰 맨 앞자리 수를 넣어서 풀려고 시도 했으나 결국 sort 함수 구현시 알게됨...

다 부질없구나!!


## 다시 정리하여 푼 코드
```
function solution(numbers) {
  var answer = '';
  numbers.sort((a, b) => {
    const valueA = String(a) + String(b);
    const valueB = String(b) + String(a);
    return Number(valueB) - Number(valueA);
  })

  numbers.forEach(n => {
    answer += String(n);
  })

  return Number(answer) === 0 ? '0' : answer;
}

console.log(solution([3, 30, 34, 5, 9]))
```
정렬 부분에서 미리 합쳐보고 비교한 후에 더 큰 경우가 앞으로 가도록 정렬하면 되는 문제 였음...

코너 케이스로 모든 숫자가 0 일경우 '0000'이 아닌 '0'을 리턴.

###
문제 유형을 숙지하고 푸는 요령을 익혀야 한다!
