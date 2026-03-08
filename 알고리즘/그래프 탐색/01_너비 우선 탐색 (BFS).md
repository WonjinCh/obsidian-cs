---
tags: [알고리즘, 하위개념, 그래프탐색]
aliases: [BFS, Breadth-First Search]
상위개념: "[[00_그래프 탐색]]"
---

# 너비 우선 탐색 (BFS)

상위개념: [[00_그래프 탐색]] | 인덱스: [[알고리즘 개요]]

---

## 개요

시작 정점에서 가까운 정점부터 **레벨(거리) 순서**로 탐색하는 그래프 탐색 알고리즘. **큐(Queue)** 를 사용한다.

---

## 핵심 아이디어

- 시작 노드를 큐에 삽입
- 큐에서 노드를 꺼내 인접 노드를 모두 큐에 추가
- 방문 처리로 중복 방지
- **가중치 없는 그래프에서 최단 거리 보장**

---

## 자료구조

- [[그래프]] — 탐색 대상
- 큐 (Queue) — FIFO 방식으로 레벨 순서 보장
- 방문 배열 (visited[])

---

## 시간복잡도 / 공간복잡도

| | 복잡도 |
|--|--------|
| 시간 | O(V + E) — 모든 정점·간선 1회 방문 |
| 공간 | O(V) — 큐 + 방문 배열 |

> 자세한 내용: [[시간복잡도와 공간복잡도]]

---

## 동작 원리 (단계별)

```
그래프: 1-2, 1-3, 2-4, 3-5  시작: 1

초기:  Queue=[1], visited={1}
Step1: pop(1), 인접=[2,3] → Queue=[2,3], visited={1,2,3}
Step2: pop(2), 인접=[4]   → Queue=[3,4], visited={1,2,3,4}
Step3: pop(3), 인접=[5]   → Queue=[4,5], visited={1,2,3,4,5}
Step4: pop(4), 인접=[]    → Queue=[5]
Step5: pop(5), 인접=[]    → Queue=[]  → 종료

탐색 순서: 1 → 2 → 3 → 4 → 5
```

---

## 예시 코드

### Java
```java
void bfs(List<List<Integer>> graph, int start) {
    Queue<Integer> queue = new LinkedList<>();
    boolean[] visited = new boolean[graph.size()];

    queue.offer(start);
    visited[start] = true;

    while (!queue.isEmpty()) {
        int node = queue.poll();
        System.out.print(node + " ");

        for (int neighbor : graph.get(node)) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                queue.offer(neighbor);
            }
        }
    }
}
```

### JavaScript
```javascript
function bfs(graph, start) {
    const visited = new Set([start]);
    const queue = [start];
    const order = [];

    while (queue.length > 0) {
        const node = queue.shift();
        order.push(node);

        for (const neighbor of graph[node]) {
            if (!visited.has(neighbor)) {
                visited.add(neighbor);
                queue.push(neighbor);
            }
        }
    }
    return order;
}
```

---

## 언제 사용하나

- **최단 거리** 문제 (가중치 없는 그래프)
- 레벨 단위 탐색이 필요할 때
- 연결 요소 탐색
- 2차원 격자(미로) 탐색

---

## 주의사항

- 방문 처리는 **큐에 넣을 때** 해야 중복 삽입 방지
- 가중치 있는 그래프는 [[03_다익스트라]] 사용
- 큐 구현: Java `LinkedList`, JS `shift()` (대규모엔 deque 고려)

---

## 연관 알고리즘

- [[02_깊이 우선 탐색 (DFS)]] — 동일 그래프, 다른 탐색 순서
- [[03_다익스트라]] — 가중치 있는 최단경로
- [[00_위상 정렬]] — BFS 기반 구현 가능
