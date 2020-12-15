# Tree and Graph

# Tree

(BST heap 전위 중위 등등 용어랑 구조 머릿속 매핑을 위한 간단 정리)

- 이진 트리: 최대 2개의 자식을 갖는 트리
- Binary Search Tree(이진탐색트리) :  모든 왼쪽 자식들 ≤ n ≤ 모든 오른쪽 자식들
- complete binary tree(완전 이진트리) : 모든 높이에서 노드가 꽉 차있고, 마지막 level에서는 꽉차있지 않아도 왼→오른쪽으로 채워진 트리
- full binary tree (전 이진트리) : 모든 노드의 자식이 0 or 2
- perfect binary tree(포화 이진트리): 전 이진트리이면서 완전 이진트리. 노드 개수가 정확히 2^height-1.
    - ⇒ 이진트리가 주어졌을 때, BST나 포화이진트리일거라 생각해버리지 말고 가정을 잘 볼 것

- 균형 트리는 O(logN) 시간에 insert, find를 할 수 있다. → 레드블랙트리 / AVL트리
    - (극단적으로 비균형적 트리는 linkedlist처럼 됨)

### 순회

현재 노드를 중심으로 pre, in, post다.

- 전위(pre-order)순회: 현재노드 → left → right
- 중위(in-order)순회 : left → 현재노드 → right
- 후위 순회 : ~

## **Min Heap**

각 노드의 원소가 자식들의 원소보다 작다. 루트가 제일 작음. 완전이진트리.

## Trie(접두사 트리)

간단한 서제스트 구현할때 썼던거.

[https://github.com/npgall/concurrent-trees](https://github.com/npgall/concurrent-trees) 

# Graph

- DFS(깊이 우선 탐색) : 모든 노드를 방문할 때 선호됨. 방문했었는지 여부를 표시하고, 검사하지 않으면 무한루프에 빠질 수 있음 주의.

```java
void search(Node root) {
	if (root == null) return;
	visit(root);
	root.visited = true;
	for(Node n : root.adjacent) {
		if (n.visited == false) {
			search(n); // recursive call
		}
	}
}
```

- BFS(너비 우선 탐색) : 두 노드 사이의 최단경로 또는 임의의 경로 찾기에 선호됨. Queue를 이용하자!

```java
void search(Node root) {
	Queue queue = new Queue();
	root.marked = true;
	queue.enqueue(root);
	while (!queue.isEmpty()) {
		Node r = queue.dequeue();
		visit(r); // 방문은 queue에 넣어놨던 노드를.
		for (Node n in r.adjacent) { // 찾는게 그 노드가 아니면 그것의 인접 노드부터 돌며 queue에 넣는다.
			if (n.marked == false) {
				n.marked = true;
				queue.enqueue(n);
			}
		}
	}
}
```