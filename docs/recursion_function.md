---
title: "재귀함수"
category: study_algorithm
tags: [Algoritnm]
date: "2022-01-25"
---

## 재귀함수

</br>

### 기본 구현 과정

  1. 하나의 문제를 여러 부분 문제로 나누기
  2. 각 부분 문제를 정의하기 위한 상태 정보 설계
  3. 각 부분 문제의 상태가 원하는 해인지 판별할 조건 설계
  4. 각 부분 문제에서 다음 하위문제를 탐색할 조건 설계

</br>

### 기본 구조

  ~~~
  int recursion(Parameters) {
    if(Termination Condition) { 
      return;
    }
    else {
      while(Search Condition) {
        recursion(Parameters);
      }
    }
  }
  ~~~
