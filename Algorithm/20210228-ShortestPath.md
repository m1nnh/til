## :star: ShortestPath (Reference : 이것이 코딩테스트다.)

### 1. 가장 빠른길 찾기
- **최단 경로** 알고리즘은 말 그대로 가장 짧은 경로를 찾는 알고리즘이다. 그래서 '길 찾기' 문제라고도 불린다. 최단 경로 알고리즘 유형에는 다양한 종류가 있는데, 상황에 맞는 효율적인 알고리즘이 이미 정립되어 있다.
- 예를 들어 '한 지점에서 다른 특정 지점까지의 최단 경로를 구해야 하는 경우', '모든 지점에서 다른 모든 지점까지의 최단 경로를 모두 구해야 하는 경우' 등의 다양한 사례가 존재한다.
- 최단 경로 문제는 보통 그래프를 이용해 표현하는데 각 지점은 그래프에서 '노드'로 표현되고, 지점간 연결된 도로는 그래프에서 '간선'으로 표현된다. 또한 실제 코딩테스트에서는 최단 경로를 모두 출력하는 문제보다는 단순히 최단 거리를 출력하도록 요구하는 문제가 많이 출제된다.
- 이 책에서는 **다익스트라 최단 경로**와 **플로이드 워셜 알고리즘** 유형을 다룬다.

### 다익스트라(Dijkstra) 최단 경로 알고리즘
- 다익스트라 최단 경로 알고리즘은 그래프에서 여러 개의 노드가 있을 때, 특정한 노드에서 출발하여 다른 노드로 가는 각각의 최단 경로를 구해주는 알고리즘이다. 다익스트라 최단 경로 알고리즘은 '음의 간선'이 없을 때 정상적으로 동작한다.
- 다익스트라 최단 경로 알고리즘은 기본적으로 그리디 알고리즘으로 분류된다. 매번 '가장 비용이 적은 노드'를 선택해서 임의의 과정을 반복하기 때문이다. 알고리즘의 원리를 간략히 설명하면 다음과 같다.
    
    1. 출발 노드를 설정한다.
    2. 최단 거리 테이블을 초기화한다.
    3. 방문하지 않은 노드 중에서 최단 거리가 가장 짧은 노드를 선택한다.
    4. 해당 노드를 거쳐 다른 노드로 가는 비용을 계산하여 최단 거리 테이블을 갱신한다.
    5. 위 과정에서 3과 4번을 반복한다.
- 다익스트라 알고리즘은 최단 경로를 구하는 과정에서 '각 노드에 대한 현재까지의 최단 거리' 정보를 항상 1차원 리스트에 저장하며 리스트를 계속 갱신한다는 특징이 있다. 매번 현재 처리하고 있는 노드를 기준으로 주변 간선을 확인한다.
- 다익스트라 알고리즘을 구현하는 방법은 2가지 이다.

    1. 구현하기 쉽지만 느리게 동작하는 코드
    2. 구현하기에 조금 더 까다롭지만 빠르게 동작하는 코드

### :computer: Simple Dijkstra Algorithm Code

```
import sys

input = sys.stdin.readline
INF = int(1e9)

n, m = map(int, input().split())

start = int(input())
graph = [[] for _ in range(n + 1)]
visited = [False] * (n + 1)
distance = [INF] * (n + 1)

for _ in range(m):
    a, b, c = map(int, input().split())
    graph[a].append((b, c))


def get_smallest_node():
    min_value = INF
    index = 0
    for i in range(1, n + 1):
        if distance[i] < min_value and not visited[i]:
            min_value = distance[i]
            index = i
    return index


def dijkstra(start):
    distance[start] = 0
    visited[start] = True
    for j in graph[start]:
        distance[j[0]] = j[1]
    for i in range(n - 1):
        now = get_smallest_node()
        visited[now] = True

        for j in graph[now]:
            cost = distance[now] + j[1]
            if cost < distance[j[0]]:
                distance[j[0]] = cost


dijkstra(start)
for i in range(1, n + 1):
    if distance[i] == INF:
        print('INFINITY')
    else:
        print(distance[i])
```

- 간단한 다익스트라 알고리즘의 시간 복잡도는 O(V<sup>2</sup>)이다. 따라서 코딩 테스트의 최단 경로 문제에서 전체 노드의 개수기 5,000개 이하라면 일반적으로 이 코드로 문제를 풀 수 있을 것이다.

### 힙
- 힙 자료구조는 우선순위 큐를 구현하기 위하여 사용하는 자료구조중 하나이다. **우선순위 큐**는 **우선순위가 가장 높은 데이터를 가장 먼저 삭제**한다는 점이 특징이다.
- 파이썬에서 우선순위 큐가 필요할 때 PriorityQueue 혹은 heapq를 사용할 수 있는데, 이 두 라이브러리는 모두 우선순위 큐 기능을 지원한다. 다만 PriorityQueue 보다는 일반적으로 heapq가 더 빠르게 동작하기 때문에 수행 시간이 제한된 상황에서는 heapq를 사용하는 것을 권장한다.
- 우선순위 큐를 구현할 때는 내부적으로 최소 힙 혹은 최대 힙을 이용한다. 최소 힙을 이용하는 경우 '값이 낮은 데이터가 먼저 삭제'되며, 최대 힙을 이용하는 경우 '값이 큰 데이터가 먼저 삭제'된다.
- 또한 최소 힙을 최대 힙처럼 사용하기 위해서 일부러 우선순위에 해당하는 값에 음수 부호를 붙여서 넣었다가, 나중에 우선순위 큐에서 꺼낸 다음에 다시 음수 부호를 붙여서 원래의 값으로 돌리는 방식을 사용할 수 있다.

### :computer: Improved Dijkstra Algorithm Code

```
import heapq

N, M = map(int, input().split())
start = int(input())
graph = [[] for _ in range(N + 1)]
distance = [int(1e9)] * (N + 1)

for _ in range(M):
    A, B, C = map(int, input().split())
    graph[A].append((B, C))


def dijkstra(start):
    q = []
    heapq.heappush(q, (0, start))
    distance[start] = 0

    while q:
        dist, now = heapq.heappop(q)

        if distance[now] < dist:
            continue
        for i in graph[now]:
            cost = dist + i[1]
            if cost < distance[i[0]]:
                distance[i[0]] = cost
                heapq.heappush(q, (cost, i[0]))


dijkstra(start)

for i in range(1, N + 1):
    if distance[i] == int(1e9):
        print("INFINITY")
    else:
        print(distance[i])
```

- 개선된 다익스트라 알고리즘의 시간복잡도는 O(ElogV)로 훨씬 빠르다. 하지만 직관적으로 봤을 때, 이처럼 우선순위 큐를 이용하는 방식이 훨씬 빠른 이유에 대해서 잘 납득이 가지 않을 수 있다.

### 플로이드 워셜 알고리즘
- **플로이드 워셜 알고리즘**은 모든 지점에서 다른 모든 지점까지의 최단 경로를 모두 구해야 하는 경우에 사용할 수 있는 알고리즘이다.
- 플로이드 워셜 알고리즘은 단계마다 '거쳐 가는 노드'를 기준으로 알고리즘을 수행한다.
- 플로이드 워셜 알고리즘은 다익스트라 알고리즘과는 다르게 2차원 리스트에 '최단 거리' 정보를 저장한다는 특징이 있다. 모든 노드에 대하여 다른 모든 노드로 가는 최단 거리 정보를 담아야 하기 때문이다.
- 플로이드 워셜 알고리즘은 다이나믹 프로그래밍이라는 특징이 있다. 노드의 개수가 N이라고 할 때, N번 만큼의 단계를 반복하며 '점화식에 맞게' 2차원 리스트를 갱신하기 때문에 다이나믹 프로그래밍으로 볼 수 있다.

### :computer: Floyd Warshall Algorithm Code

```
N = int(input())
M = int(input())

graph = [[int(1e9)] * (N + 1) for _ in range(N + 1)]

for i in range(1, N + 1):
    for j in range(1, N + 1):
        if i == j:
            graph[i][j] = 0

for _ in range(M):
    A, B, C = map(int, input().split())
    graph[A][B] = C

for i in range(1, N + 1):
    for j in range(1, N + 1):
        for k in range(1, N + 1):
            graph[i][j] = min(graph[i][j], graph[i][k] + graph[k][j])

for i in range(1, N + 1):
    for j in range(1, N + 1):
        if graph[i][j] == int(1e9):
            print("INFINITY", end=' ')
        else:
            print(graph[i][j], end=' ')
    print()
```

### :speech_balloon: 미래 도시
방문 판매원 A는 많은 회사가 모여 있는 공중 미래 도시에 있다.
공중 미래 도시에는 1번부터 N번까지의 회사가 있는데 특정 회사끼리는 서로 도로를 통해 연결되어 있다.
방문 판매원 A는 현재 1번 회사에 위치해 있으며, X번 회사에 방문해 물건을 판매하고자 한다.
공중 미래 도시에서 특정 회사에 도착하기 위한 방법은 회사끼리 연결되어 있는 도로를 이용하는 방법이 유일하다.
또한 연결된 2개의 회사는 양방향으로 이동할 수 있다.
공중 미래 도시에서의 도로는 마하의 속도로 사람을 이동시켜주기 때문에 특정 회사와 다른 회사가 도로로 연결되어 있다면, 정확히 1만큼의 시간으로 이동할 수 있다.
또한 오늘 방문 판매원 A는 기대하던 소개팅에도 참석하고자 한다.
소개팅의 상대는 K번 회사에 존재한다. 방문 판매원 A는 X번 회사에 가서 물건을 판매하기 전에 먼저 소개팅 상대의 회사에 찾아가서 함께 커피를 마실 예정이다.
따라서 방문 판매원 A는 1번 회사에서 출발하여 K번 회사를 방문 한 뒤에 X번 회사로 가는 것이 목표다. 
이때 방문 판매원 A는 가능한 빠르게 이동하고자 한다. 방문 판매원이 회사 사이를 이동하게 되는 최소 시간을 계산하는 프로그램을 작성하시오.
이때 소개팅의 상대방과 커피를 마시는 시간등은 고려하지 않는다고 가정한다. 예를 들어 N = 5, X = 4, K = 5이고 회사 간 도로가 7개면서
각 도로가 다음과 같이 연결되어 있을 때를 가정할 수 있다.
```
(1번, 2번), (1번, 3번), (1번, 4번), (2번, 4번), (3번, 4번), (3번, 5번), (4번, 5번)
```
이때 방문 판매원 A가 최종적으로 4번 회사에 가는 경로를 (1번 - 3번 - 5번 - 4번)으로 설정하면, 소개팅에도 참석할 수 있으면서 총 3만큼의 시간으로 이동할 수 있다.
따라서 이 경우 최소 이동 시간은 3이다.

### :computer: Code

```
N, M = map(int, input().split())
graph = [[int(1e9)] * (N + 1) for _ in range(N + 1)]

for i in range(1, N + 1):
    for j in range(1, N + 1):
        if i == j:
            graph[i][j] = 0

for _ in range(M):
    A, B = map(int, input().split())
    graph[A][B] = 1
    graph[B][A] = 1

for i in range(1, N + 1):
    for j in range(1, N + 1):
        for k in range(1, N + 1):
            graph[i][j] = min(graph[i][j], graph[k][j] + graph[i][k])

X, K = map(int, input().split())

result = graph[1][K] + graph[K][X]

if result == int(1e9):
    print("Don't meet!")
else:
    print(result)

```

### :pencil2: 문제 해설
이 문제는 전형적인 플로이드 워셜 알고리즘 문제이다. 현재 문제에서 N의 범위가 100 이하로 매우 한정적이다. 따라서 플로이드 워셜 알고리즘을 이용해도 빠르게 풀 수 있기 때문에, 구현이 간단한 플로이드 워셜 알고리즘을 이용하는 것이 유리하다.

### :speech_balloon: 전보
어떤 나라에는 N개의 도시가 있다. 그리고 각 도시는 보내고자 하는 메시지가 있는 경우, 다른 도시로 전보를 보내서 다른 도시로 해당 메시지를 전송할 수 있다.
하지만 X라는 도시에서 Y라는 도시로 전보를 보내고자 한다면, 도시 X에서 Y로 향하는 통로가 설치되어 있어야 한다.
예를 들어 X에서 Y로 향하는 통로는 있지만, Y에서 X로 향하는 통로가 없다면 Y는 X로 메시지를 보낼 수 없다. 또한 통로를 거쳐 메시지를 보낼 때는 일정 시간이 소요된다.
어느 날 C라는 도시에서 위급 상황이 발생했다. 그래서 최대한 많은 도시로 메시지를 보내고자 한다. 메시지는 도시 C에서 출발하여 각 도시 사이에 설치된 통로를 거쳐,
최대한 많이 퍼져나갈 것이다. 각 도시의 버호와 통로가 설치되어 있는 정보가 주어졌을 때, 도시 C에서 보낸 메시지를 받게 되는 도시의 개수는 총 몇 개이며 도시들이
모두 메시지를 받는 데까지 걸리는 시간은 얼마인지 계산하는 프로그램을 작성하시오.

### :computer: Code

```
import heapq

N, M, X = map(int, input().split())
graph = [[] for _ in range(N + 1)]

for _ in range(M):
    A, B, C = map(int, input().split())
    graph[A].append((B, C))

distance = [int(1e9)] * (N + 1)


def dijkstra(start):
    queue = []
    heapq.heappush(queue, (0, start))
    distance[start] = 0
    while queue:
        dist, now = heapq.heappop(queue)
        if distance[now] < dist:
            continue

        for i in graph[now]:
            cost = dist + i[1]
            if distance[i[0]] > cost:
                distance[i[0]] = cost
                heapq.heappush(queue, (cost, i[0]))


dijkstra(X)
count = 0
max_distance = 0

for i in distance:
    if i != int(1e9):
        count += 1
        max_distance = max(max_distance, i)

print(count - 1, max_distance)
```

### :pencil2: 문제 해설
이 문제를 들여다보면 한 도시에서 다른 도시까지의 최단 거리 문제로 치환할 수 있으므로 다익스트라 알고리즘을 이용해서 풀 수 있다.