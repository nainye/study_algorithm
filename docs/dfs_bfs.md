---
title: "DFS, BFS"
category: study_algorithm
tags: [Algorithm, DFS, BFS]
date: "2022-01-25"
---

***

## Table of contents

1. [DFS](#dfs)  
2. [BFS](#bfs)  

</br>

***

## DFS

(Depth First Search, 깊이 우선 탐색)

- 한 경로로 탐색하다 특정 상황에서 `최대한 깊숙하게 들어가` 확인 후, 다시 돌아와 다른 경로로 탐색하는 방식
- 재귀함수 또는 Stack을 이용하여 구현
- 활용 예) 백트랙킹, 단절선 찾기, 단절점 찾기, 위상정렬, 사이클 찾기 등

</br>

- **기본 구현 과정**
  : 각 단계별 영역을 명확히 나누고 구현하는 것이 좋음

  - 1.`체크인` : 탐색 과정에서 방문 여부, 방문 위치를 확인하는 것. 재방문을 하지 않기 위함 (방문 전 / 방문 중 / 방문 완료)
  - 2.`목적지인가?` : 원하는 **최종** 지점인지 확인. 목적지가 아니면 계속 진행
  - 3.`연결된 곳 순회`
    - 4.`갈 수 있는가?` (only 방문 전)
      - 5.**`간다`** (방문 중)
  - 6.`체크아웃` : 주어진 일을 수행한 이후 다른 작업을 수행할 수 있도록 반드시 해줘야 함 (방문 완료)

</br>

- 예제)

  - [백준 1759번](https://www.acmicpc.net/problem/1759)

  ~~~java
  import java.io.FileInputStream;
  import java.io.FileNotFoundException;
  import java.util.Arrays;
  import java.util.LinkedList;
  import java.util.List;
  import java.util.Scanner;

  public class Main {

      // 최종 results는 static(공용)으로 선언해 놓는 것이 편함
      static int L, C;
      static char[] data;
      static List<String> result;

      public static void main(String[] args) throws Exception {

          Scanner sc = new Scanner(System.in);

          L = sc.nextInt();
          C = sc.nextInt();

          // System.out.println(L + ", " + C);

          data = new char[C];
          result = new LinkedList<>();

          for (int i = 0; i < C; i++) {
              data[i] = sc.next().charAt(0);
          }

          // System.out.println(Arrays.toString(data));

          Arrays.sort(data);

          dfs(0, 0, 0, -1, "");

          // System.out.println(Arrays.toString(data));

          for (int i = 0; i < result.size(); i++) {
              System.out.println(result.get(i));
          }
      }

      static void dfs(int length, int ja, int mo, int current, String pwd) {
  //      1. 체크인 - 생략 가능
  //      2. 목적지인가? - 길이 + 자음,모음 개수
          if (length == L) {
              if(ja >= 2 && mo >= 1) {
                  result.add(pwd);
              }
          } else{
              //      3. 연결된 곳 순회 - 나보다 높은 알파벳
              for (int nextIndex = current + 1; nextIndex <ㅠ data.length; nextIndex++) {
                  char nextData = data[nextIndex];
                  //      4.  갈 수 있는가? - 생략 가능
                  if(nextData == 'a' || nextData == 'e' || nextData == 'i' || nextData == 'o' || nextData == 'u') { // 모음
                      //      5.      간다 - dfs(next) -> 모음
                      dfs(length+1, ja, mo + 1, nextIndex, pwd + nextData);
                  } else {
                      //      5.      간다 - dfs(next) -> 자음
                      dfs(length+1, ja + 1, mo, nextIndex, pwd + nextData);
                  }
              }
          }
  //      6. 체크 아웃 - 생략 가능
      }
  }
  ~~~

</br>

***

## BFS

(Breadth First Search, 너비 우선 탐색)

- 시작 노드에서 시작하여 `인접한 노드를 먼저` 탐색하는 방식
- Queue를 이용하여 구현
- 활용 예) 최단경로 찾기, 위상정렬 등

</br>

- **기본 구현 과정**
  : 각 단계별 영역을 명확히 나누고 구현하는 것이 좋음

  **큐** : 갈 예정인 곳. 보통 `큐가 비는 것`(갈 곳이 없음)을 종료 조건으로 함

  - 1.`큐에서 꺼내옴`
  - 2.`목적지인가?` : 원하는 **최종** 지점인지 확인. 목적지가 아니면 계속 진행
  - 3.`연결된 곳 순회`
    - 4.`갈 수 있는가?` (only 방문 전)
      - 5.`체크인` (방문 중)
      - 6.`큐에 넣음`
  - 7.`체크아웃` (**보통 생략**) (방문 완료)
