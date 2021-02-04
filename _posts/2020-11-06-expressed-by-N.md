---
layout: post
title: N으로 표현
categories:
  - 동적계획법
tags: 
  - 알고리즘
---

[문제](https://programmers.co.kr/learn/courses/30/lessons/42895)
```
function solution(N, number) {
  let answer = -1;

  if (N === number) {
    return 1;
  }

  const map = {}
  map[1] = new Set();
  map[1].add(N);
  let depth = 2;


  const op = (set1, set2, set) => {
      set1.forEach(v1 => {
        set2.forEach(v2 => {
          set.add(v1 + v2);
          set.add(v1 - v2);
          set.add(v1 * v2);
          set.add(v1 / v2);
        });
      });

      if (set.has(number)) {
        return number;
      }
    return -1;
  }

  while(depth <= 8 && answer < 0) {
    const depthN = Number(N.toString().repeat(depth));
    const set = new Set().add(depthN);
    map[depth] = set;

    for(let i = 1; i <= depth -1; i++) {
      const set1 = map[i];
      const set2 = map[depth - i];
      const result = op(set1, set2, set);
      if (result > 0) {
        answer = depth;
        break;
      }
    }

    depth++;
  }

  return answer;
}
```

##처음 도전
동적 계획법을 잘못생각해서 (크게 잘못생각하지는 않았지만..) 특정 부분에 대한 답만 나옴 - 1차 맨붕

## 재도전
모든 케이스에 대해 체크를 하는 것에 대한 오버헤드를 생각해 memoization할 궁리를 하다 시간을 다 보냄...

결국 memoization은 필요가 없었다?!? 가 아니고 그냥 특정 레벨의 값을 저장해 놓는 정도만 필요

## 마지막 고비
모든 경우에 대해 체크를 하며 찾는데 도대체 시간이 오래 걸림!!

문제 해결을 위해 컨닝을 했더니 Set() !!! 이놈이 이렇게 성능이 좋을 줄이야!!

array로 체크 하던걸 set으로 변경하니 high pass 로 통과!! ㅠㅠ

이제부터 array를 쓰던 내용을 set을 변경하는 걸 적극 고려해 봐야겠다.

### 회고
set 채고!!
동적 계획법에 쫄지말자.


### 배우자!
**Set**이 array를 순회는 것보다 search에 대해 성능이 월등히 좋다.
1. array에 특정 값이 없는 경우에만 추가하는 경우 
    - **Set** 을 사용 : `set.add()`
2. array에 특정 값이 있는가?
    - **Set**을 사용하는것이 `array.find()` 보다 성능이 월등히 좋다. : `set.has()`
3. 특정 숫자를 연속으로 붙여서 표현하는 경우 ex) 3을 입력 받아 3333333 만들어야 할때
    ```javascript
      const N = 3;
      N.toString().repeat(5); // 3을 5개 붙여서 반환해 준다.
    ```
4. 문제 유형을 익히자!
    - 특정 숫자를 사용하여 특정 넘버를 만드는데 필요한 최소의 갯수 -> 동적 계획법!!

