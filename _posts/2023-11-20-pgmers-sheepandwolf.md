---
layout: post
title: "[Algo] 프로그래머스 양과 늑대 JAVA "
date: 2023-11-20 22:47:00 +0900
categories: Algorithm
---

```java
import java.util.*;
class Solution {
    static List<Integer> parent;
    static boolean []check;
    static List<List<Integer>> graph;
    static int len;
    static int sheep, wolf, answer;
    public int solution(int[] info, int[][] edges) {
        answer = 1;
        len=info.length;
        graph =new ArrayList<>();

        for(int i=0; i<len; i++){
            graph.add(new ArrayList<>());
        }
        for(int i=0; i<len-1; i++){
            graph.get(edges[i][0]).add(edges[i][1]);
        }
        check=new boolean[len];
        parent= new ArrayList<>();
        for(int i=0; i<graph.get(0).size(); i++){
            parent.add(graph.get(0).get(i));
        }
        sheep=1;
        wolf=0;
        check[0]=true;
        dfs(0, parent, info);


        return answer;
    }
    public void dfs(int node, List<Integer> parent, int[] info) {
        List<Integer> tmp = new ArrayList<>(parent);
        for (int i = 0; i < parent.size(); i++) {
            int pick = parent.get(i);
            if (!check[pick] && sheep > wolf + info[pick]) {
                if (info[pick] == 0) {
                    sheep++;
                } else {
                    wolf++;
                }
                // 자식노드 저장
                for (int k = 0; k < graph.get(pick).size(); k++) {
                    tmp.add(graph.get(pick).get(k));
                }
                check[pick] = true;
                answer = Math.max(sheep, answer);
                dfs(pick, tmp, info);
                tmp = new ArrayList<>(parent);
                check[pick] = false;
                if (info[pick] == 0) {
                    sheep--;
                } else {
                    wolf--;
                }
            }
        }
    }
}
```

### 풀이

트리 탐색이긴한데.. 자식노드만을 탐색하는 것이 아니라 다른 자식으로 점프할 수 있다!
따라서 이 자식은 만인의 자식이 될 수 있음을 염두에 두고 풀이를 생각했다.
parent 배열을 만들어서 방문할 수 있는 배열을 모두 저장한다. 하지만 재귀의 단계에 따라 그 내용이 달라지므로 tmp배열에 깊은복사를 진행해 오염을 방지한다. node 가 17개이기 때문에 마음 편하게 완탐을 돌고 돌았다.
양이 가장 많은 순간을 answer에 저장한다.
