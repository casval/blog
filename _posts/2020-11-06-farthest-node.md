---
layout: post
title: 가장 먼 노드
categories:
  - algorithm
tags: 
  - 알고리즘 - 그래프
---
[문제](https://programmers.co.kr/learn/courses/30/lessons/49189)

BFS를 이용하여 그래프 순회하는 문제.

```
function solution(n, edge) {
  if (edge.length === 0) {
    return 1;
  }

  const arr = Array.from({length: n + 1}, () => []);

  edge.forEach(e => {
    arr[e[0]].push(e[1]);
    arr[e[1]].push(e[0]);
  });

  let input = [1];
  let visited = new Set();
  
  const getDepth = input => {
    let set = new Set();
    input.forEach(num => {
      const vertexes = arr[num];
      vertexes.forEach(v => {
        if (!visited.has(v)) {
          set.add(v);
        }
      })
    });
    
    return Array.from(set);
  }
  
  let lastInput = null;
  do {
    visited = new Set([...visited, ...input]);
    lastInput = input;
    input = getDepth(input);
  } while(input.length);

  return lastInput.length;
}
```
이전에 AD 시험때 풀었던 문제와 비슷해서 그대로 풀려다 망한 케이스

그때는 true/false를 이용한 cycle을 찾는 문제였는데 이번건 최종 거리의 노드수 찾는 문제라서 그때 문제가 되던 heap size가 아닌 시간 초과 문제가 발생

초반에는 문제의 output을 잘못 리턴하여... 최대 거리를 리턴해서 몽땅 틀림.

다시 풀었을 때는 시간 초과 및 core dump 에러...(out of index)

전체 노드수 X 노드수 가 아닌 방문에 필요한 노드수만 배열로 가지고 있으면 시간 초과가 발생하지 않는 문제.


### 회고
쉬운 문제 인데... 이상한 방향으로 풀어서 괜히 산으로가서 자신감만 하락하는 문제이네...

