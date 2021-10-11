## :star: Graph Algorithm (Reference : 이것이 코딩테스트다.)

### 1. 다양한 그래프 알고리즘
- 이 장에서 배울 내용은 크루스칼 알고리즘은 그리디 알고리즘으로 분류되며, 위상 정렬 알고름은 앞서 배운 큐 자료 구조 혹은 스택 자료구조를 활용해야 구현할 수 있다.
- 그래프 구현 방법은 2가지 방식이 존재한다.
    - 인접 행렬 : 2차원 배열을 사용하는 방식
    - 인접 리스트 : 리스트를 사용하는 방식
- 두가지 모두 그래프 알고리즘에서 매우 많이 사용된다. 두 방식은 메모리와 속도 측면에서 구별되는 특징을 가진다는 점을 기억하자.

### 서로소 집합
- 수학에서 서로소 집합이란 **공통 원소가 없는 두 집합**을 의미한다.
- 서로소 집합 자료구조란 **서로소 부분 집합들로 나누어진 원소들의 데이터를 처리하기 위한 자료구조**라고 할 수 있다.
- 서로소 집합 자료구조는 union과 find 이 2개의 연산으로 조작할 수 있다.
- union(합집합) 연산은 2개의 원소가 포함된 집합을 하나의 집합으로 합치는 연산이다.
- find(찾기) 연산은 특정한 원소가 속한 집합이 어떤 집합인지 알려주는 연산이다. 따라서 서로소 집합 자료구조는 union-find 자료구조라고 불리기도 한다.
- 서로소 집합 정보가 주어졌을 때 트리 자료구조를 이용해서 집합을 표현하는 서로소 집합 계산 알고리즘은 다음과 같다
    1. union 연산을 확인 하여, 서로 연결된 두 노드 A, B를 확인한다.
        1. A와 B의 루트 노드 A, B를 각각 찾는다.
        2. 'A'를 'B'의 부모 노드로 설정한다(B가 A를 가리키도록 한다).
    2. 모든 union 연산을 처리할 때까지 1번과정을 반복한다.

### :computer: Basic Union - Find Algorithm Code

```
def find_parent(parent, x):
    if parent[x] != x:
        return find_parent(parent, parent[x])
    return x


def union_parent(parent, a, b):
    a = find_parent(parent, a)
    b = find_parent(parent, b)
    if a < b:
        parent[b] = a
    else:
        parent[a] = b


N, M = map(int, input().split())
graph = [[] for _ in range(N + 1)]
parent = [0] * (N + 1)

for i in range(N + 1):
    parent[i] = i

for i in range(M):
    A, B = map(int, input().split())
    union_parent(parent, A, B)

print('각 원소가 속한 집합 : ', end=' ')
for i in range(1, N + 1):
    print(find_parent(parent, i), end=' ')
print()

print('부모 테이블: ', end=' ')
for i in range(1, N + 1):
    print(parent[i], end=' ')
```

- 이렇게 구하면 답을 구할 수는 있지만 find 함수가 비효율적으로 동작한다. 최악의 경우 find 함수가 모든 노드를 다 확인하는 터라 시간 복잡도가 O(N)이라는 점이다.
- 하지만 이러한 find함수는 아주 간단한 과정으로 최적화가 가능하다. 바로 **경로 압축** 기법을 적용하면 시간 복잡도를 개선시킬 수 있다.

### :computer: Path Compression Code

```
def find_parent(parent, x):
    if parent[x] != x:
        parent[x] = find_parent(parent, parent[x])
    return parent[x]
```

### :computer: Improved Union - Find Algorithm Code

```
def find_parent(parent, x):
    if parent[x] != x:
        parent[x] = find_parent(parent, parent[x])
    return parent[x]


def union_parent(parent, a, b):
    a = find_parent(parent, a)
    b = find_parent(parent, b)
    if a < b:
        parent[b] = a
    else:
        parent[a] = b


N, M = map(int, input().split())
graph = [[] for _ in range(N + 1)]
parent = [0] * (N + 1)

for i in range(N + 1):
    parent[i] = i

for i in range(M):
    A, B = map(int, input().split())
    union_parent(parent, A, B)

print('각 원소가 속한 집합 : ', end=' ')
for i in range(1, N + 1):
    print(find_parent(parent, i), end=' ')
print()

print('부모 테이블: ', end=' ')
for i in range(1, N + 1):
    print(parent[i], end=' ')
```

- 서로소 집합 알고리즘의 시간 복잡도는 노드의 개수가 V개이고, 최대 V-1개의 union 연산과 M개의 find 연산이 가능할 때 경로 압축 방법을 적용한 시간 복잡도는 O(V + M(1 + log<sub>2-M/V</sub>))라는 것이 알려져 있다.

### 서로소 집합을 활용한 사이클 판별
- 서로소 집합은 다양한 알고리즘에 사용될 수 있다. 특히 서로소 집합은 무방향 그래프 내에서의 사이클을 판별할 때 사용할 수 있다는 특징이 있다.
- 앞서 union 연산은 그래프에서의 간선으로 표현될 수 있다고 했다. 따라서 간선을 하나씩 확인하면서 두 노드가 포함되어 있는 집합을 합치는 과정을 반복하는 것만으로도 사이클을 판별할 수 있다.
    1. 각 간선을 확인하며 두 노드의 루트 노드를 확인한다.
        1. 루트 노드가 서로 다르다면 두 노드에 대하여 union 연산을 수행한다.
        2. 루트 노드가 서로 같다면 사이클이 발생한 것이다.
    2. 그래프에 포함되어 있는 모든 간선에 대하여 1번 과정을 반복한다.

### :computer: Use Of Union - Find To Determine Cycles Code   

```
def find_parent(parent, x):
    if parent[x] != x:
        parent[x] = find_parent(parent, parent[x])
    return parent[x]


def union_parent(parent, a, b):
    a = find_parent(parent, a)
    b = find_parent(parent, b)

    if a < b:
        parent[b] = a
    else:
        parent[a] = b


V, E = map(int, input().split())
parent = [0] * (V + 1)
cycle = False

for i in range(1, V + 1):
    parent[i] = i

for i in range(E):
    A, B = map(int, input().split())
    if find_parent(parent, A) == find_parent(parent, B):
        cycle = True
        break
    else:
        union_parent(parent, A, B)

if cycle:
    print('cycle이 일어났습니다.')
else:
    print('cucle이 일어나지 않았습니다.')
```

### 신장 트리
- 신장 트리는 그래프 알고리즘 문제로 자주 출제되는 문제 유형이다. 기본적으로 신장 트리란 **하나의 그래프가 있을 때 모든 노드가 포함하면서 사이클이 존재하지 않는 부분 그래프**를 의미한다.
- 신장 트리 중에서 최소 비용으로 만들 수 있는 신장 트리를 찾는 알고리즘을 '최소 신장 트리 알고리즘'이라고 한다. 대표적인 최소 신장 트리 알고리즘으로는 **크루스칼 알고리즘**이 있다.
- 크루스칼 알고리즘을 사용하면 가장 적은 비용으로 모든 노드를 연결할 수 있는데 크루스칼 알고리즘은 그리디 알고리즘으로 분류된다. 먼저 모든 간선에 대하여 정렬을 수행한 뒤에 가장 거리가 짧은 간선부터 집합에 포함시키면 된다.
    1. 간선 데이터를 비용에 따라 오름차순으로 정렬한다.
    2. 간선을 하나씩 확인하며 현재의 간선이 사이클을 발생시키는지 확인한다.
        1. 사이클이 발생하지 않는 경우 최소 신장 트리에 포함시킨다.
        2. 사이클이 발생하는 경우 최소 신장 트리에 포함시키지 않는다.
    3. 모든 간선에 대하여 2번의 과정을 반복한다.

### :computer: Kruskal Algorithm Code

```
def find_parent(parent, x):
    if parent[x] != x:
        parent[x] = find_parent(parent, parent[x])
    return parent[x]


def union_parent(parent, a, b):
    a = find_parent(parent, a)
    b = find_parent(parent, b)
    if a < b:
        parent[b] = a
    else:
        parent[a] = b


V, E = map(int, input().split())
parent = [0] * (V + 1)
edges = []
result = 0

for i in range(1, V + 1):
    parent[i] = i

for i in range(E):
    a, b, cost = map(int, input().split())
    edges.append((cost, a, b))

edges.sort()

for edge in edges:
    Cost, A, B = edge
    if find_parent(parent, A) != find_parent(parent, B):
        union_parent(parent, A, B)
        result += Cost

print(result)
```

- 크루스칼 알고리즘은 간선의 개수가 E개일 때, O(ElogE)의 시간 복잡도를 가진다. 왜냐하면 크루스칼 알고리즘에서 시간이 가장 오래 걸리는 부분이 간선을 정렬하는 작업이며, E개의 테이블을 정렬했을 때의 시간 복잡도는 O(ElogE)이기 때문이다.

### 위상 정렬
- **위상 정렬**은 정렬 알고리즘의 일종이다. 위상 정렬은 순서가 정해져 있는 일련의 작업을 차례대로 수행해야 할 때 사용할 수 있는 알고리즘이다.
- 좀 더 이론적으로 설명하자면, 위상 정렬이란 **방향 그래프의 모든 노드를 방향성에 거스르지 않도록 순서대로 나열하는 것**이다.
- 위상 정렬 알고리즘을 자세히 살펴보기 전에, 먼저 진입차수를 알아햐한다. 진입차수란 **특정한 노드로 들어오는 간선의 개수**를 의미한다
    1. 진입차수가 0인 노드를 큐에 넣는다.
    2. 큐가 빌 때까지 다음의 과정을 반복한다.
        1. 큐에서 원소를 꺼내 해당 노드에서 출발하는 간선을 그래프에서 제거한다.
        2. 새롭게 진입차수가 0이 된 노드를 큐에 넣는다.
- 모든 원소를 방문하기 전에 큐가 빈다면 사이클이 존재한다고 판단할 수 있다.

### :computer: Topology Sort Code

```
from collections import deque

V, E = map(int, input().split())

indegree = [0] * (V + 1)
graph = [[] for i in range(V + 1)]

for i in range(E):
    A, B = map(int, input().split())
    graph[A].append(B)
    indegree[B] += 1


def topology_sort():
    result = []
    queue = deque()

    for i in range(1, V + 1):
        if indegree[i] == 0:
            queue.append(i)
    while queue:
        now = queue.popleft()
        result.append(now)

        for i in graph[now]:
            indegree[i] -= 1
            if indegree[i] == 0:
                queue.append(i)

    for i in result:
        print(i, end=' ')
```

- 위상 정렬의 시간 복잡도는 O(V+E)이다. 위상 정렬을 수행할 때는 차례대로 모든 노드를 확인하면서, 해당 노드에서 출발하는 간선을 차례대로 제거해야 한다.

### :speech_balloon: 팀 결성
학교에서 학생들에게 0번부터 N번까지의 번호를 부여했다. 처음에는 모든 학생이 서로 다른 팀으로 구분 되, 총 N+1개의 팀이 존재한다.
이때 선생님은 '팀 합치기' 연산과 '같은 팀 여부 확인' 연산을 사용할 수 있다.
    1. '팀 합치기' 연산은 두 팀을 합치는 연산이다.
    2. '같은 팀 여부 확인' 연산은 특정한 두 학생이 같은 팀에 속하는지를 확인하는 연산이다.
선생님이 M개의 연산을 수행할 수 있을 때, '같은 팀 여부 확인' 연산에 대한 연산 결과를 출력하는 프로그램을 작성하시오.

### :computer: Code

```
def find_team(parent, x):
    if parent[x] != x:
        parent[x] = find_team(parent, parent[x])
    return parent[x]


def union_team(parent, a, b):
    a = find_team(parent, a)
    b = find_team(parent, b)

    if a < b:
        parent[b] = a
    else:
        parent[a] = b


N, M = map(int, input().split())
parent = [0] * (N + 1)

for i in range(N + 1):
    parent[i] = i

for _ in range(M):
    A, B, C = map(int, input().split())
    if A == 0:
        union_team(parent, B, C)
    else:
        if find_team(parent, B) != find_team(parent, C):
            print('NO')
        else:
            print('YES')
```

### :pencil2: 문제 해설
전형적인 서로소 집합 알고리즘 문제로 N과 M의 범위가 모두 최대 100,000이다. 따라서 경로 압축 방식의 서로소 집합 자료구조를 이용하여 시간 복잡도를 개선해야 한다.

### :speech_balloon: 도시 분할 계획
동물원에서 막 탈출한 원숭이 한 마리가 세상 구경을 하고 있다. 어느 날 원숭이는 '평화로운 마을'에 잠시 머물렀는데 마침 마을 사람들은
도로 공사 문제로 머리를 맞대고 회의 중이었다. 마을은 N개의 집과 그 집들을 연결하는 M개의 길로 이루어져 있다. 길은 어느 방향으로든지
다닐 수 있는 편리한 길이다. 그리고 길마다 길을 유지하는데 드는 유지비가 있다.
마을의 이장은 마을을 2개의 분리된 마을로 분할할 계획을 세우고 있다. 마을이 너무 커서 혼자서는 관리할 수 없기 때문이다.
마을을 분할할 때는 각 분리된 마을 안에 집들이 서로 연결되도록 분할해야 한다. 각 분리된 마을 안에 있는 임의의 두 집 사이에 경로가 항상 존재해야 한다는 뜻이다.
마을에는 집이 하나 이상 있어야 한다.
그렇게 마을의 이장은 계획을 세우다가 마을 안에 길이 너무 많다는 생각을 하게 되었다. 일단 분리 된 두 마을 사이에 있는 길들은 필요가 없으므로 없앨 수 있다.
그리고 각 분리된 마을 안에서도 임의의 두 집 사이에 경로가 항상 존재하게 하면서 길을 더 없앨 수 있다.
마을의 이장은 위 조건을 만족하도록 길들을 모두 없애고 나머지 길의 유지비의 합을 최소로 하고 싶다. 이것을 구하는 프로그램을 작성하시오.

### :computer: Code

```
def find_parent(parent, x):
    if parent[x] != x:
        parent[x] = find_parent(parent, parent[x])
    return parent[x]


def union_parent(parent, a, b):
    a = find_parent(parent, a)
    b = find_parent(parent, b)

    if a < b:
        parent[b] = a
    else:
        parent[a] = b


N, M = map(int, input().split())
parent = [0] * (N + 1)

for i in range(1, N + 1):
    parent[i] = i

edges = []
result = 0

for _ in range(M):
    A, B, C = map(int, input().split())
    edges.append((C, A, B))

edges.sort()
last = 0

for edge in edges:
    cost, a, b = edge
    if find_parent(parent, a) != find_parent(parent, b):
        union_parent(parent, a, b)
        result += cost
        last = cost

print(result - last)
```

### :pencil2: 문제 해설
이 문제의 핵심 아이디어는 전체 그래프에서 2개의 최소 신장 트리를 만들어야 한다는 것이다. 가장 간단한 방법은 크루스칼 알고리즘으로 최소 신장 트리를 찾은 뒤에 최소 신장 트리를 구성하는 간선 중에서 가장 비용이 큰 간선을 제거하는 것이다.

### :speech_balloon: 커리큘럼
동빈이는 온라인으로 컴퓨터공학 강의를 듣고 있다. 이때 각 온라인 강의는 선수 강의가 있을 수 있는데, 선수 강의가 있는 강의는 선수 강의를 먼저 들어야만 해당 강의를 들을 수 있다.
예를 들어 '알고리즘' 강의의 선수 강의로 '자료구조'와 '컴퓨터 기초'가 존재한다면, '자료구조'와 컴퓨터 기초'를 모두 들은 이후에 '알고리즘' 강의를 들을 수 있다.
동빈이는 총 N개의 강의를 듣고자 한다. 모든 강의는 1번부터 N번까지의 번호를 가진다. 또한 동시에 여러 개의 강의를 들을 수 있다고 가정한다.
예를 들어 N = 3일 때, 3번 강의의 선수 강의로 1번과 2번 강의가 있고, 1번과 2번 강의는 선수 강의가 없다고 가정하자.
그리고 각 강의에 대하여 강의 시간이 다음과 같다고 가정하자.
    1번 강의 : 30시간
    2번 강의 : 20시간
    3번 강의 : 40시간
이 경우 1번 강의를 수강하기까지의 최소 시간은 30시간, 2번 강의를 수강하기까지의 최소 시간은 20시간, 3번 강의를 수강하기까지의 최소 시간은 70시간이다.
동빈이가 듣고자 하는 N개의 강의 정보가 주어졌을 때, N개의 강의에 대하여 수강하기까지 걸리는 최소 시간을 각각 출력하는 프로그램을 작성하시오.

### :computer: Code

```
from collections import deque
import copy

N = int(input())
indegree = [0] * (N + 1)
graph = [[] for i in range(N + 1)]
time = [0] * (N + 1)

for i in range(1, N + 1):
    data = list(map(int, input().split()))
    time[i] = data[0]
    for x in data[1:-1]:
        indegree[i] += 1
        graph[x].append(i)
print(graph)


def topology_sort():
    result = copy.deepcopy(time)
    queue = deque()

    for i in range(1, N + 1):
        if indegree[i] == 0:
            queue.append(i)

    while queue:
        now = queue.popleft()
        for i in graph[now]:
            result[i] = max(result[i], result[now] + time[i])
            indegree[i] -= 1

            if indegree[i] == 0:
                queue.append(i)

    for i in range(1, N + 1):
        print(result[i])


topology_sort()
```

### :pencil2: 문제 해설
이 문제는 위상 정렬 알고리즘의 응용문제이다. 각 강의에 대하여 인접한 노드를 확인할 때, 인접한 노드에 대하여 현재보다 강의 시간이 더 긴 경우를 찾는다면, 더 오랜 시간이 걸리는 경우의 시간 값을 저장하는 방식으로 결과 테이블을 갱신하여 답을 구할 수 있다.