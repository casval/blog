---
layout: post
title: 디스크 컨트롤러
categories:
  - algorithm
tags: 
  - 알고리즘 - 힙(Heap)
---
[문제](https://programmers.co.kr/learn/courses/30/lessons/42627)

우선순위 큐를 이용하여 푸는 문제.

우선순위 큐는 시작시간이 빠르거나, 시작시간이 같을 경우 작업 처리 속도가 빠른 작업이 먼저 나오는 큐.

```
function solution(jobs) {
  var answer = 0;
  const queue = [];
  const jobCount = jobs.length;  
  // 들어온 시간 순으로 정렬 같은 시간일 경우 작업 처리시간이 작은 작업이 우선!
  jobs.sort((a, b) => {
    const result = a[0] - b[0];
    if (result === 0) {
      return a[1] - b[1];
    }
    return result;
  });
  let jobStartTime = 0;
  let sum = 0;
  while(queue.length > 0 || jobs.length > 0) {
    const job = queue.length > 0 && queue.shift() || jobs.shift();

    jobStartTime = jobStartTime > job[0] ? jobStartTime : job[0];

    // 완료시간
    const completeTime = jobStartTime + job[1];
    // 지연시간
    const delayTime = jobStartTime - job[0];
    // 전체 처리 시간 = 기다린 시간 + 필요한시간
    const processingTime = delayTime + job[1];

    sum += processingTime;
    jobStartTime = completeTime;

    // 완료시간보다 시작시간이 작은 작업을 queue에 넣는다.
    const length = jobs.length;
    for (let i = 0; i < length; i++) {
      const j = jobs[0];
      if (completeTime > j[0]) {
        queue.push(jobs.shift());
      } else {
        break;
      }
    }

    // queue를 처리시간이 적은 순으로 정렬
    if (queue.length > 1) {
      queue.sort((a, b) => a[1] - b[1]);
    }
  }

  answer = Math.floor(sum/jobCount);
 
  return answer;
}

// console.log(solution([[0, 3], [1, 9], [2, 6]])) // 9
console.log(solution([[0,4], [0,3], [0,2], [0,1]])) // 6
console.log(solution([[0,1], [0,2], [0,3], [0,4]])) // 6
// console.log(solution([[0, 10], [4, 10], [5, 11], [15, 2]])) // 15
// console.log(solution([[0, 1], [1, 2], [500, 6]])) // 3
// console.log(solution([[24, 10], [28, 39], [43, 20], [37, 5], [47, 22], [20, 47], [15, 2], [15, 34], [35, 43], [26, 1]])) // 72
```
우선 문제를 풀기위한 조건을 잘 이해해야 한다.

1. 먼저 들어온 작업이 먼저 처리 된다. 같은 시간에 들어온 작업은 평균치를 최소로 하는 조건에 맞추려면 처리 속도가 빠른게 먼저 실행 되어야한다.
  - 작업들을 시간 순서로 정렬, 같을 경우 처리속도가 적은 순으로 정렬
2. 첫번째 작업이 실행되는 동안 시작이 되어야하는 작업들을 큐에 넣는다.
  - 우선순위 큐를 만들기 위해 작업 처리속도가 빠른 순으로 정렬을 한다.
3. 첫번째 작업이 완료되면 큐에서 첫번째 작업을 꺼내서 작업하고 그 동안에 시작되어야 하는 작업이 추가되면 다시 큐에 추가한다.
  - 다시 큐 정렬
4. 전체 작업이 완료 되면 평균을 구한다.
  - 큐가 비거나, 전체 작업을 수행
  - 평균의 소수점을 버리므로 parseInt를 하여 정수로 바꾸거나 Math.floor()를 사용.
5. 필요한 계산
  - 이전 작업 완료 시간 = 작업 시작 시간과 이전 작업 완료 시간 중 큰 값.
  - 전체 작업 진행 시간 = 이전 작업 완료시간 - 작업 완료시간 + 작업 진행 시간
  - 작업 완료 시간 = 이전 작업 완료 시간 + 작업 진행 시간  => 다음 작업의 이전 작업 완료시간
  
### 회고
로직을 천천히 정리 후 코딩을 시작해야지 디버깅으로 잡을 생각하면 피로가 밀려오고 산으로 간다.
경우들에 대한 로직을 잘 정리 후 집중해서 짜지 않으면, 어렵지 않은 문제도 풀리지 않는다.
