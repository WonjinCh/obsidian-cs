---
tags: [알고리즘, 하위개념, 그래프탐색]
aliases: [DFS, Depth-First Search]
상위개념: "[[00_그래프 탐색]]"
---

# 깊이 우선 탐색 (DFS)

상위개념: [[00_그래프 탐색]] | 인덱스: [[알고리즘 개요]]

---

## 개요

한 방향으로 **갈 수 있는 끝까지** 탐색하다가 막히면 돌아와 다른 방향을 탐색하는 알고리즘. **스택(재귀)** 을 사용한다.

---

## 핵심 아이디어

- 현재 노드를 방문 처리
- 인접 노드 중 미방문 노드를 재귀 탐색
- 더 이상 방문할 노드가 없으면 복귀
- 완전탐색, 경로 탐색, 사이클 감지에 유용

---

## 자료구조

- [[그래프]] — 탐색 대상
- [[트리]] — 트리 순회에도 DFS 사용
- 스택 / 재귀 호출 스택

---

## 시간복잡도 / 공간복잡도

| | 복잡도 |
|--|--------|
| 시간 | O(V + E) — 모든 정점·간선 방문 |
| 공간 | O(V) — 재귀 스택 + 방문 배열 |

> 자세한 내용: [[시간복잡도와 공간복잡도]]

---

## 동작 원리 (단계별)

```
그래프: 1-2, 1-3, 2-4, 3-5  시작: 1

dfs(1): 방문1, → dfs(2)
  dfs(2): 방문2, → dfs(4)
    dfs(4): 방문4, 더 없음 → 복귀
  복귀2 → dfs(3)
dfs(3): 방문3, → dfs(5)
  dfs(5): 방문5, 더 없음 → 복귀

탐색 순서: 1 → 2 → 4 → 3 → 5
```

---

## 예시 코드

### Java — 재귀
```java
void dfs(List<List<Integer>> graph, boolean[] visited, int node) {
    visited[node] = true;
    System.out.print(node + " ");

    for (int neighbor : graph.get(node)) {
        if (!visited[neighbor]) dfs(graph, visited, neighbor);
    }
}
```

### Java — 반복 (스택)
```java
void dfsIterative(List<List<Integer>> graph, int start) {
    Deque<Integer> stack = new ArrayDeque<>();
    boolean[] visited = new boolean[graph.size()];
    stack.push(start);

    while (!stack.isEmpty()) {
        int node = stack.pop();
        if (visited[node]) continue;
        visited[node] = true;
        System.out.print(node + " ");
        for (int neighbor : graph.get(node)) {
            if (!visited[neighbor]) stack.push(neighbor);
        }
    }
}
```

### JavaScript
```javascript
function dfs(graph, start) {
    const visited = new Set();
    const order = [];

    function explore(node) {
        visited.add(node);
        order.push(node);
        for (const neighbor of graph[node]) {
            if (!visited.has(neighbor)) explore(neighbor);
        }
    }

    explore(start);
    return order;
}
```

---

## 언제 사용하나

- 모든 경로 탐색
- 연결 요소(Connected Component) 개수 세기
- 사이클 감지
- 트리 순회 (전위/중위/후위)

---

## 주의사항

- 깊은 재귀는 스택 오버플로 위험 → 반복(스택) 방식으로 대체
- BFS와 달리 최단 경로를 보장하지 않음
- 방문 배열 초기화 필수

---

## 연관 알고리즘

- [[01_너비 우선 탐색 (BFS)]] — 레벨 탐색, 최단거리
- [[03_백트래킹]] — DFS + 가지치기
- [[00_위상 정렬]] — DFS 기반 구현 가능
