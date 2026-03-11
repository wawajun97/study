최소신장트리 : 모든 정점을 연결하는 간선들의 가중치의 합이 최소가 되는 트리

크루스칼(kruskal) 알고리즘 : 최소신장트리를 활용한 그리디 알고리즘 종류  
- n개의 노드를 가진 트리를 만들기 위해서 가중치가 낮은 간선부터 n-1개의 간선을 선택한다.(사이클이 발생하면 안됨)

1. 최초, 모든 간선을 가중치에 따라서 오름차순으로 정렬
2. 가중치가 가장 낮은 간선부터 선택하면서 트리를 증가시킴(사이클이 존재하면 다음 간선으로 넘어감 => findSet(a) == findSet(b) 이면 사이클)
3. n-1개의 간선이 선택될 때까지 2번 반복

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;
import java.util.StringTokenizer;

public class MST_Kruskal {

	public static class Edge implements Comparable<Edge> {
		int start,end,weight;

		public Edge(int start, int end, int weight) {
			this.start = start;
			this.end = end;
			this.weight = weight;
		}

		@Override
		public int compareTo(Edge o) {
			return Integer.compare(this.weight, o.weight);
		}		
	}
	
	public static Edge[] edgeList;
	public static int[] parents;
	public static int V, E;
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer st = new StringTokenizer(br.readLine(), " ");
		V = Integer.parseInt(st.nextToken());
		E = Integer.parseInt(st.nextToken());
		
		edgeList = new Edge[E];
		
		for(int i =0;i<E;i++) {
			st = new StringTokenizer(br.readLine(), " ");
			int s = Integer.parseInt(st.nextToken());
			int e = Integer.parseInt(st.nextToken());
			int w = Integer.parseInt(st.nextToken());
			edgeList[i] = new Edge(s,e,w);
		}
		
		Arrays.sort(edgeList); //간선 가중치 기준으로 오름차순 정렬
		
		makeSets();
		
		int count = 0, result = 0;
		for (Edge edge : edgeList) {
			//합칠 수 있다면 간선 사용
			if(union(edge.start, edge.end)) {
				count++;
				result += edge.weight;
				if(count == V-1) break;
			}
		}
		
		System.out.println(result);
	}

	public static void makeSets() {
		parents = new int[V];
		for(int i =0;i<V;i++) {
			parents[i] = -1; //부모가 없으면 -1로
		}
	}
	
	public static int findSet(int a) {
		//음수라는 것은 내가 대표자라는 뜻
		if(parents[a] < 0) return a;
		
		return parents[a] = findSet(parents[a]);
	}
	
	public static boolean union(int a, int b) {
		int aRoot = findSet(a);
		int bRoot = findSet(b);
		
		if(aRoot == bRoot) return false;
		
		//사이즈가 음수이니까 a가 더 큰 경우이다
		if(parents[aRoot] <= parents[bRoot]) {
			//음수여도 일단 더함 -> 마지막에 사이즈 계산 시 절댓값 취하면 됨
			//음수이면 해당 대표자의 트리 사이즈가 될 것이고 양수이면 부모를 가르킬 것
			parents[aRoot] += parents[bRoot];
			parents[bRoot] = aRoot;	
		} else {
			parents[bRoot] += parents[aRoot];
			parents[aRoot] = bRoot;
		}
		
		return true;
	}
}

```

PRIM 알고리즘 : 하나의 정점에서 연결된 간선들 중에 하나씩 선택하면서 MST를 만들어 가는 방식  
- 인접 행렬 또는 인접 리스트를 사용하여 구현
- 크루스칼은 v개의 트리가 1개의 트리로 만드는 거라면 PRIM은 트리가 확장되는 방식(정점을 하나씩 추가)  
1. 임의 정점을 하나 선택해서 시작
2. 선택한 정점과 인접하는 정점들 중의 최소 비용의 간선이 존재하는 정점을 선택
3. 모든 정점이 선택될 때까지 2번 반복
