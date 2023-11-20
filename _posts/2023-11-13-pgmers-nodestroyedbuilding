---
layout: post
title: "[Algo] 프로그래머스 파괴되지 않은 건물 JAVA "
date: 2023-11-20 22:47:00 +0900
categories: Algorithm
---

```java
import java.util.*;

class Solution {

    public int solution(int[][] board, int[][] skill) {
        int N=board.length;
        int M=board[0].length;
        int len=skill.length;
        int[][]tmp=new int[N+1][M+1];
        for(int i=0; i<len; i++){
            int power=0;
            if(skill[i][0]==1){
                power=skill[i][5]*-1;
            }else{
                power=skill[i][5];
            }
            tmp[skill[i][1]][skill[i][2]]+=power;
            tmp[skill[i][3]+1][skill[i][2]]-=power;
            tmp[skill[i][1]][skill[i][4]+1]-=power;
            tmp[skill[i][3]+1][skill[i][4]+1]+=power;
        }
        for(int i=1; i<N; i++){
            for(int j=0; j<M; j++){
                tmp[i][j]+=tmp[i-1][j];
            }
        }
        for(int i=0; i<N; i++){
            for(int j=1; j<M; j++){
                tmp[i][j]+=tmp[i][j-1];
            }
        }
        int answer=0;
        for(int i=0; i<N; i++){
            for(int j=0; j<M; j++){
                if(board[i][j]+tmp[i][j]>0){
                    answer++;
                }
            }
        }
        return answer;
    }
}
```

### 풀이

이차원 누적합이라는 늑김은 있었지만 풀이는 정말 생각해내기 힘들었다.
도움을 받아 식을 구성할 수 있었다.
스킬이 사용된 모든 칸에 대해 누적합을 진행하면 완탐이 되니, 스킬 범위의 좌표를 찍고 앞뒤로 미리 효과를 준 다음 한 번에 계산을 진행하는 것이다. 질문하기를 보니 정적분을 생각하면 쉽다던데 난 고3 이후로 적분은 싹 까먹었다. 지인에게 물어보니 적분은 시작점과 끝점이 있는 구간합이라고 생각하면 편하다고 했다.
이차원누적합엔 어려운 문제가 많은 것 같다..
