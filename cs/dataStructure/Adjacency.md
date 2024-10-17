# 인접행렬(Adjacency matrix)

* 컴퓨터에게 내가 이러한 그래프를 그렸다고 알려주는 표현방법(인접 행렬, 인접 리스트)
* 인접해있다 = 연결되어있다
* 그래프에서 정점과 간선의 관계를 나타내는 bool 타입의 정사각형 행렬
    * 정사각형 행렬의 각 요소가 0 또는 1이라는 값을 가짐
    * 0은 두 정점 사이의 경로가 없음을 의미
    * 1은 두 정점 사이의 경로가 있음을 의미

![img_1.png](img_1.png)

|   | 0 | 1 | 2 | 3 |
|---|---|---|---|---|
| 0 | 0 | 1 | 1 | 1 |
| 1 | 1 | 0 | 1 | 0 |
| 2 | 1 | 1 | 0 | 0 |
| 3 | 1 | 0 | 0 | 0 |

* 위 표를 코드로 표현하면

```java
boolean[][] a = new boolean[][]{
        {false, true, true, true},
        {true, false, true, false},
        {true, true, false, false},
        {true, false, false, false}
};
```

* 이를 기반으로 코드를 구축하는 방법은
* 보통 이중 for문을 통해 i에서 j로 가는 경로가 있다면 어떠한 로직 또는 해당 정점으로부터 탐색하는 로직을 구축함

```java
public static void main(String[] args) {
    boolean[][] a = new boolean[V][V];
    for (int i = 0; i < V; i++) {
        for (int j = 0; j < V; j++) {
            if (a[i][j]) {
                // 출력하는 로직
                System.out.println(i + "부터 " + j + "까지 경로가 있습니다.");
            }
        }
    }
}
```

## Q. 3번 노드에서 5번 노드로 가는 단방향 경로가 있고 이를 인접 행렬로 표현한다면 어떻게 될까?

* a[3][5] = true;

## Q. 3번 노드에서 5번 노드로 가는 양방향 경로가 있고 이를 인접 행렬로 표현한다면 어떻게 될까?

* a[3][5] = true;
* a[5][3] = true;

## Q. 정점의 갯수가 20개인 그래프가 있다. 이를 인접행렬로 표현한다고 해씅ㄹ 때 메모리를 최소로 쓴다고 하면 배열을 어떻게 만들어야 할까?

* boolean[][] a = new boolean[20][20];

## Q. 인접 행렬을 기반으로 탐색하기

### 1번

* 정점은 0번부터 9번까지 10개의 노드가 있다. 1 - 2 / 1 - 3 / 3 - 4 라는 경로가 있다. 이를 인접행렬로 표현한다면?

```java
public static void main(String[] args) {
    boolean[][] a = new boolean[10][10];
    a[1][2] = true;
    a[1][3] = true;
    a[3][4] = true;
    a[2][1] = true;
    a[3][1] = true;
    a[4][3] = true;
}
```

### 2번

* 0번부터 방문 안한 노드를 찾고 해당 노드부터 방문, 연결된 노드를 이어서 방문해서 출력하는 재귀함수를 만들고 싶다면 어떻게 해야할까? 또한, 정점을 방문하고 다시 방문하지 않게 만들려면 어떻게 해야할까?

```java
static final int V = 10;
static boolean[][] a = new boolean[V][V];
static boolean[][] visited = new boolean[V];

static void go(int from) {
    visited[from] = true;
    System.out.println(from);

    for (int i = 0; i < V; i++) {
        if (visited[i]) continue;
        if (a[from][i]) {
            go(i);
        }
    }
}

public static void main(String[] args) {
    a[1][2] = true;
    a[1][3] = true;
    a[3][4] = true;
    a[2][1] = true;
    a[3][1] = true;
    a[4][3] = true;

    for (int i = 0; i < V; i++) {
        for (int j = 0; j < V; j++) {
            if (a[i][j] && visited[i]) {
                go(i);
            }
        }
    }
}
```

# 인접리스트(Adjacency list)

* 그래프에서 정점과 간선의 관계를 나타내는 연결리스트
  ![img_1.png](img_1.png)
* 이 그래프를 코드로 나타내면 아래와 같음

```java
import java.util.*;

public class AdjacencyList {
    static final int V = 4; // 정점의 개수
    static List<Integer>[] adj = new ArrayList[V]; // 인접 리스트 배열

    public static void main(String[] args) {
        // 인접 리스트 초기화
        for (int i = 0; i < V; i++) {
            adj[i] = new ArrayList<>();
        }

        // 간선 추가
        adj[0].add(1);
        adj[0].add(2);
        adj[0].add(3);
        adj[1].add(0);
        adj[1].add(2);
        adj[2].add(0);
        adj[2].add(1);
        adj[3].add(0);

        // 첫 번째 출력 방법 (for-each)
        for (int i = 0; i < V; i++) {
            System.out.print(i + " :: ");
            for (int there : adj[i]) {
                System.out.print(there + " ");
            }
            System.out.println();
        }

        // 두 번째 출력 방법 (인덱스 기반 for문)
        for (int i = 0; i < V; i++) {
            System.out.print(i + " :: ");
            for (int j = 0; j < adj[i].size(); j++) {
                System.out.print(adj[i].get(j) + " ");
            }
            System.out.println();
        }
    }
}
```

## Q. 왜 인접리스트가 아닌 list도 될까?

* 앞의 코드를 LinkedList 대신 List로 구현했는데, 시간복잡도의 차이를 보면 이유를 알 수 있다

### 연결리스트의 시간복잡도

* n번째 인덱스에 삽입, 삭제: O(1)
* 마지막 요소에 삽입, 삭제: O(1)
* 특정 요소 탐색: O(n)
* n번째 요소 참조: O(n)

### list의 시간복잡도

* n번째 인덱스에 삽입, 삭제: O(n)
* 마지막 요소에 삽입, 삭제: O(1)
* 특정 요소 탐색: O(n)
* n번째 요소 참조: O(1)

### 결론

* 인접리스트를 구현할 때 많이 사용하는 연산은 마지막 요소에 삽입과 해당 배열을 탐색하는 연산
* 그렇기 때문에 list로 구혀냏도 무방함. 편한 list 추천

## 인접 리스트를 기반으로 탐색하기

### 1번

* 정점은 0번 ~ 9번까지 10개의 노드가 있다. 1 - 2 / 1 - 3 / 3 - 4 라는 경로가 있다. 이를 인접리스트로 표현한다면?

```java
import java.util.*;

public class AdjacencyList {
  static final int V = 10;
  static List<Integer>[] adj = new ArrayList[V];

  public static void main(String[] args) {
    for (int i = 0; i < V; i++) {
      adj[i] = new ArrayList<>();
    }

    adj[1].add(2);
    adj[2].add(1);
    adj[1].add(3);
    adj[3].add(1);
    adj[3].add(4);
    adj[4].add(3);
  }
}
```

### 2번

* 0번부터 방문 안한 노드를 찾고 해당 노드부터 방문, 연결된 노드를 이어서 방문해서 출력하는 재귀함수를 만들고 싶다면 어떻게 해야할까? 또한, 정점을 방문하고 다시 방문하지 않게 만들려면 어떻게 해야할까?
```java
import java.util.*;

public class AdjacencyList {
    static final int V = 10;
    static List<Integer>[] adj = new ArrayList[V];
    static int[] visited = new int[V];

    public static void go(int idx) {
        System.out.println(idx);    // 현재 노드 출력
        visited[idx] = 1;           // 현재 노드를 방문 처리

        // 연결된 노드들 탐색
        for (int there : adj[idx]) {
            if (visited[there] == 1) continue; // 이미 방문한 노드는 건너뜀
            go(there);    // 재귀적으로 방문
        }
    }

    public static void main(String[] args) {
        for (int i = 0; i < V; i++) {
            adj[i] = new ArrayList<>();
        }

        adj[1].add(2);
        adj[2].add(1);
        adj[1].add(3);
        adj[3].add(1);
        adj[3].add(4);
        adj[4].add(3);

        for (int i = 0; i < V; i++) {
            if (visited[i] == 0) {
                go(i);
            }
        }
    }
}
```

# 인접행렬과 인접리스트
## 공간복잡도
* 인접행렬: O(V^2)
* 인접리스트: O(V + E)

```java
// 인접행렬
boolean[][] adj = new boolean[V][V];
// 인접리스트
List<Integer>[] adj = new ArrayList<V>; 
```

## 시간복잡도: 간선 한 개 찾기
* 인접행렬: O(1)
* 인접리스트: O(V)
```java
// 인접행렬
for(int i = 0; i < V; i++) {
    for(int j = 0; j < V; j++) {
        if(a[i][j]) {
        }
    }
}
// 인접리스트
for(int j = 0; j < adj[i].size(); j++) { }
```
## 시간복잡도: 모든 간선찾기
* 인접행렬: O(V^2)
* 인접리스트: O(V + E)
### 그래프가 희소할 때는 인접리스트, 조밀할 때는 인접행렬이 좋다
* 그래프가 희소할 때는 인접행렬이 인접리스트보다 메모리를 더 많이 써야 함
  * 간선이 없어서 인접행렬의 대부분의 요소가 0인데도 불구하고 해당 부분을 포함해 2차원 배열을 만들어야하기 때문
* 그래프가 조밀할 때는 인접행렬이 인접리스트보다 좋음
  * 어차피 다 연결되어있기 때문에 메모리적 효율성은 동일해지고 정점 i에서 정점 j까지의 간선이 있는지 확인하는 속도가 더 빠르기 때문에 인접행렬이 더 빠름
* 보통은 인접리스트를 사용하는게 좋음. 문제에서 희소한 그래프가 많이 나옴
* 다만, 문제 또는 코테에서 인접행렬로 주어진다면 그대로 인접행렬로 푸는 것이 좋다