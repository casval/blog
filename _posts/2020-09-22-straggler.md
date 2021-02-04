---
layout: post
title: 완주하지못한선수
categories:
  - 해시
tags: 
  - 알고리즘
---

오랜만에 푼 알고리즘.
완전 기초인데...

```
const solution = (p, c) => {
  p.sort();
  c.sort();
  
  let answer = '';
  let index = 0;
  while(index < p.length) {
    if (p[index] !== c[index]) {
      answer = p[index];
      break;
    }
    index++;
  }

  return answer;
};
```

## 쩌는 문제 풀이
```
var solution=(_,$)=>_.find(_=>!$[_]--,$.map(_=>$[_]=($[_]|0)+1))
```
### 위에껄 풀이
```
var solution=(participant,completion)=>participant.find(name=>!completion[name]--,completion.map(name=>completion[name]=(completion[name]|0)+1))
```

위의 문제를 이해하기 위한 [find](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/find) [map](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/map)

completion을 map을 이용하여 항목에 있는 것들의 갯수를 세서 completion에 key-value로 추가한다.
이때 추가된 completion을 find에서 사용하여 participant의 name을 key로 개수를 세서 개수가 0인것을 찾는다.


[출처: programmers](https://programmers.co.kr/learn/courses/30/lessons/42576)




#### 회고
- sort를 생각하지 못하고 단순히 해시라는 문구에 현혹되어 끌려간 내자신
- 이건 잘못된 풀이의 과정이었지만 forEach내에서 break를 쓰기위해 some을 사용한다.
- while 사용시 조건문 확인 (debugging 없이 코드만으로 기본 로직은 파악이 되어야 하는데 너무 코딩을 놓았나..)


