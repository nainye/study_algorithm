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

  <details>
  <summary>예제) 백준 1759번 </summary>
  <div markdown="1">

  - [백준 - 암호 만들기](https://www.acmicpc.net/problem/1759)

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
  </div>
  </details>

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

  </br>

  <details>
  <summary>예제) 백준 3055번 </summary>
  <div markdown="1">

    - [백준 - 탈출](https://www.acmicpc.net/problem/3055)

  ~~~java
    import java.io.FileInputStream;
    import java.util.LinkedList;
    import java.util.Queue;
    import java.util.Scanner;

    public class Main {
        // 좌, 우, 위, 아래
        static final int[] MX = {-1, 1, 0, 0};
        static final int[] MY = {0, 0, -1, 1};

        static int R, C;
        static char[][] map;
        static int[][] dp;
        static Queue<Point> queue;
        static boolean foundAnswer;

        public static void main(String[] args) throws Exception {

            Scanner sc = new Scanner(System.in);

            R = sc.nextInt();
            C = sc.nextInt();

            map = new char[R][C];
            dp = new int[R][C];
            queue = new LinkedList<>();

            Point st = null;

            for (int r = 0; r < R; r++) {
                String line = sc.next();
                for (int c = 0; c < C; c++) {
                    map[r][c] = line.charAt(c);
                    if (map[r][c] == 'S') {
                        st = new Point(r, c, 'S');
                    } else if (map[r][c] == '*') {
                        queue.add(new Point(r, c, '*'));
                    }
                }
            }
            queue.add(st);

            while(!queue.isEmpty()) {
                //  1. 큐에서 꺼내옴 -> S, ., D, *
                Point p = queue.poll();

                //  2. 목적지인가? -> D
                if (p.type == 'D') {
                    System.out.println(dp[p.y][p.x]);
                    foundAnswer = true;
                    break;
                }

                //  3. 연결된 곳을 순회 -> 좌, 우, 위, 아래
                for (int i = 0; i < 4; i++) {
                    int ty = p.y + MY[i];
                    int tx = p.x + MX[i];

                    //  4. 갈 수 있는가? (공통) -> 맵을 벗어나지 않고
                    if (0 <= ty && ty < R && 0 <= tx && tx < C) {
                        if (p.type == '.' || p.type == 'S') {
                            // 갈 수 있는가? (고슴도치) -> . or D, 방문하지 않은 곳
                            if ((map[ty][tx] == '.' || map[ty][tx] == 'D') && dp[ty][tx] == 0) {
                                //  5. 체크인 -> dp에 현재 + 1 을 기록
                                dp[ty][tx] = dp[p.y][p.x] + 1;

                                //  6. 큐에 넣음
                                queue.add(new Point(ty, tx, map[ty][tx]));
                            }
                        } else if (p.type == '*') {
                            // 갈 수 있는가? (물) -> .
                            if (map[ty][tx] == '.' || map[ty][tx] == 'S') {
                                //  5. 체크인 -> 지도에 * 표기
                                map[ty][tx] = '*';

                                //  6. 큐에 넣음
                                queue.add(new Point(ty, tx, '*'));

                            }
                        }
                    }
                }
            }
            if (foundAnswer == false) {
                System.out.println("KAKTUS");
            }
        }
    }

    class Point {
        int y;
        int x;
        char type;

        public Point(int y, int x, char type) {
            super();
            this.y = y;
            this.x = x;
            this.type = type;
        }

        @Override
        public String toString() { return "[y=" + y + ", x=" + x + ", type=" + type + "]"; }
    }
  ~~~

  </div>
  </details>