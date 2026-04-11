# BFS

BFS(Breadth-First Search)는 너비 우선 탐색이다.

- 루트 노드의 자식 노드들을 먼저 모두 방문한 뒤, 그다음 레벨의 노드들을 차례대로 방문한다.
- 낮은 레벨부터 높은 레벨 순서로 탐색한다.
- 큐를 사용해 구현한다.

## 기본 진행 방식

1. 큐에 루트 노드를 넣는다.
2. 큐에서 노드를 하나 꺼내 처리한 뒤, 그 자식 노드들을 큐에 넣는다.
3. 큐가 빌 때까지 반복한다.

## 기본 BFS

```java
public void bfs() {
		if (isEmpty())
			return;

		Queue<Integer> queue = new ArrayDeque<>();
		queue.offer(1);

		while (!queue.isEmpty()) {
			int current = queue.poll();
			System.out.println(current + ": " + nodes[current]);
			if (current * 2 <= lastIndex) queue.offer(current * 2);
			if (current * 2 + 1 <= lastIndex) queue.offer(current * 2 + 1);

		}
	}
```

## 너비를 관리하는 BFS

```java
public void bfs3() {
		if (isEmpty())
			return;

		int breadth = 0;
		Queue<Integer> queue = new ArrayDeque<>();
		queue.offer(1);

		while (!queue.isEmpty()) {
			int size = queue.size();

      //너비가 같은 데이터만 탐색
			while(--size >= 0) {
				int current = queue.poll();
				System.out.println(nodes[current]+"/"+ breadth);
				if (current * 2 <= lastIndex) queue.offer(current * 2);
				if (current * 2 + 1 <= lastIndex) queue.offer(current * 2 + 1);
			}

			breadth++;
		}
	}
```
