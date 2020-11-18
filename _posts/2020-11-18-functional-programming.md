---
layout: post
title: 함수형 프로그래밍
categories:
  - study
tags:
  - front-end
---

# 함수형 프로그래밍의 컨셉
1. 변경 가능한 상태를 불변상태(immutable)로 만들어 side-effect를 없애자.
1. 모든것은 객체이다. (1급 객체)
1. 코드를 간결하게 하고 가독성을 높여 구현할 로직에 집중시키자.
1. 동시성 작업을 보다 쉽게 안전하게 구현 하자.

## Immutable
## SideEffect
## First-class citizen (1급 객체)
아래의 3가지 조건을 만족
1. 변수나 데이타에 할당 가능
1. 객체의 인자로 넘길 수 있어야 함
1. 객체의 리턴값으로 사용 가능

JS의 함수는 위의 세가지 조건을 모두 만족하기 때문에 1급 객체.
## Higher-order functions (고차함수)
아래의 2가지 중 하나 이상을 만족하는 함수.
- 함수를 파라미터로 전달 받는 함수
- 함수를 리턴하는 함수
