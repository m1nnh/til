## :star: DFS/BFS (Reference : 이것이 코딩테스트다.)

### 1. 꼭 필요한 자료구조 기초
- 탐색(Search)란 **많은 양의 데이터 중에서 원하는 데이터를 찾는 과정**을 의미한다. 프로그래밍에서는 그래프, 트리 등의 자료구조 안에서 탐색을 하는 문제를 자주 다룬다.
- 대표적인 탐색 알고리즘으로 **DFS**와 **BFS**를 꼽을 수 있는데 이 두 알고리즘의 원리를 제대로 이해해야 코딩 테스트의 탐색 문제 유형을 풀 수 있다.
- 자료구조(Data Structure)란 **데이터를 표현하고 관리하고 처리하기 위한 구조** 그중 스택과 큐는 자료구조의 기초 개념으로 다음의 두 핵심적인 함수로 구성된다.
    - 삽입(Push) : 데이터를 삽입한다.
    - 삭제(Pop) : 데이터를 삭제한다.
- 물론 실제로 스택과 큐를 사용할 때는 삽입과 삭제 외에도 오버플로와 언더플로를 고민해야 한다. **오버플로**는 특정한 자료구조가 수용할 수 있는 **데이터의 크기를 이미 가득 찬 상태에서 삽입 연산을 수행**할 때 발생한다. **언더플로**는 **데이터가 전혀 없을 때 삭제 연산을 수행**하면 발생한다.

### :computer: Example stack code

```
stack = []

stack.append(5)
stack.append(2)
stack.append(3)
stack.append(7)
stack.pop()
stack.append(1)
stack.append(4)
stack.pop()

print(stack)
print(stack[::-1])
```


### 스택(Stack)
- 스택(Stack)는 **선입후출(First In Last Out)**구조 또는 **후입선출(Last In First Out)** 구조라고 한다.
- 파이썬에서 스택을 이용할 때에는 별도의 라이브러리를 사용할 필요가 없다. 기본 리스트에서 **append()** 와 **pop()** 매소르들 이용하면 스택 자료구조와 동일하게 동작한다.

### 큐(Queue)
- 큐(Queue)는 나중에 온 사람일수록 나중에 들어가기 때문에 흔히 '공정한' 자료구조라고 비유된다. 이러한 구조를 **선입선출(First In First Out)** 구조라고 한다.
- 파이썬으로 큐를 구현할 때는 **collections** 모듈에서 제공하는 **deque** 자료구조를 활용하자. **deque**는 스택과 큐의 장점을 모두 채택한 것인데 데이터를 넣고 빼는 속도가 리스트 자료형에 비해 효율적이며 queue 라이브러리를 이용하는 것보다 더 간단하다.

### :computer: Example queue code

```
from collections import deque

queue = deque()

queue.append(5)
queue.append(2)
queue.append(3)
queue.append(7)
queue.popleft()
queue.append(1)
queue.append(4)
queue.popleft()

print(queue)
queue.reverse()
print(queue)
```

### 재귀 함수(Recursive Function)
- DFS와 BFS를 구현하려면 재귀 함수도 이해하고 있어야 한다. **재귀함수(Recursive Function)**란 **자기 자신을 다시 호출하는 함수**를 의미한다.

### :computer: Example recursive function code

```
def recursive_function():
    print('재귀 함수를 호출합니다.')
    recursive_function()
    
recursive_function()
```

- 위에 코드를 실행하면 '재귀 함수를 호출합니다.'라는 문자열을 무한히 출력한다. 하지만 어느정도 출력 하다가 다음과 같은 오류 메시지를 출력하고 멈출 것이다.

```
RecursionError : maximum recursion depth exceeded while pickling an object
```

- 이 오류 메시지는 재귀의 최대 깊이를 초과했다는 내용이다.
- 따라서 재귀 함수 문제 풀이에서 사용할 때는 재귀 함수가 언제 끝날지, 종료 조건을 꼭 명시해야 한다.

### :computer: Example recursive function exit code

```
def recursive_function(i):
    if i == 100:
        return
    print(i, '번째 재귀 함수에서', i+1, '번째 재귀 함수를 호출합니다.')
    recursive_function(i+1)
    print(i, '번째 재귀 함수를 종료합니다.')

recursive_function(1)
```

- 컴퓨터 내부에서 재귀 함수의 수행은 스택 자료구조를 이용한다. 함수를 계속 호출했을 때 가장 마지막에 호출한 함수가 먼저 수행을 끝내야 그 앞의 함수 호출이 종료되기 때문이다.
- 재귀 함수를 이용하는 대표적 예제로는 팩토리얼 문제가 있다. 팩토리얼을 반복적으로 구현한 방식과 재귀적으로 구현한 두 방식을 비교해보자.

### :computer: Example factorial(recursive function) code

```
def factorial(n):
    if n <= 1:
        return 1
    else:
        return n * factorial(n - 1)


print(factorial(5))
```

- 반목문을 대신해 재귀 함수를 사용했을 때 코드가 더 간결하다. 재귀 함수의 소스코드와 점화식이 매우 닮아있다. 다시 말해 재귀 함수는 반복문을 이용 하는 것과 비교했을 때 더욱 간결한 형태임을 이해할 수 있다.

### 2. 탐색 알고리즘 DFS/BFS
- 스택과 큐, 재귀 함수는 DFS와 BFS에서 가장 중요한 개념이라 배우기에 앞서 간단하게 설명했다.

### DFS(Depth-First-Search)
- DFS는 Depth-First-Search, **깊이 우선 탐색이라고도 부르며, 그래프에서 깊은 부분을 우선적으로 탐색하는 알고리즘**이다.
- 그래프는 **노드(Node)**와 **간선(Edge)**로 표현되며 이때 노드를 **정점(vertex)**이라고도 말한다.
- 그래프 탐색이란 하나의 노드를 시작으로 다수의 노드를 방문하는 것을 말한다. 또한 두 노드가 간선으로 연결되어 있다면 '두 노드는 인접하다'라고 표현한다
- 프로그래밍 그래프는 크게 2가지 방식으로 표현할 수 있는데 코딩 테스트에서는 이 두 방식 모두 필요하니 두 개념에 대해 바르게 알고 있도록 하자.
    - 인접 행렬(Adjacency Matrix) : 2차원 배열로 그래프의 연결 관계를 표현하는 방식
    - 인접 리스트(Adjancency List) : 리스트로 그래프의 연결 관계를 표현하는 방식
- 먼저 **인접 행렬** 방식은 2차원 배열에 각 노드가 연결된 형태를 기록하는 방식이다. 연결이 되어 있지 않은 노드끼리는 무한의 비용이라고 작성한다.

### :computer: Example adjancency matrix code

```
INF = 99999999

graph = [[0, 7, 5], [7, 0, INF], [5, INF, 0]]

print(graph)
```

- **인접 리스트**는 '연결 리스트'라는 자료구조를 이용해 구현한다. 전통적인 프로그래밍 언어에서의 배열과 연결 리스트의 기능을 모두 기본으로 제공한다.

### :computer: Example adjancency list code

```
graph = [[] for _ in range(3)]

graph[0].append((1, 7))
graph[0].append((2, 5))

graph[1].append((0, 7))
graph[2].append((0, 5))

print(graph)
```

- 두 가지 방법의 메모리와 속도 측면에서의 차이점을 알아보자. 메모리 측면에서 보자면 인접 행렬 방식은 모든 관계를 저장하므로 노드 개수가 많을수록 메모리가 불필요하게 낭비된다. 반면에 인접 리스트 방식은 연결된 정보만을 저장하기 때문에 메모리를 효율적으로 사용한다. 하지만 이와 같은 속성 때문에 인접 리스트 방식은 인접 행렬 방식에 비해 특정한 두 노드가 연결되어 있는지에 대한 정보를 얻는 속도가 느리다.
- DFS는 특정한 경로로 탐색하다가 특정한 상황에서 최대한 깊숙이 들어가서 노드를 방문한 후, 다시 돌아가 다른 경로로 탐색하는 알고리즘이다.
- DFS는 **스택** 자료구조를 이용하며 구체적인 동작 과정은 다음과 같다.
    1. 탐색 시작 노드를 스택에 삽입하고 방문 처리를 한다.
    2. 스택의 최상단 노드에 방문하지 않은 인접 노드가 있으면 그 인접 노드를 스택에 넣고 방문 처리를 한다. 방문하지 않은 인접 노드가 없으면 스텍에서 최상단 노드를 꺼낸다.
    3. 2번의 과정을 더 이상 수행할 수 없을 때까지 반복한다
- DFS는 데이터의 개수가 N개인 경우 O(N)의 시간이 소요 된다는 특징이 있따. 또한 DFS는 스택을 이용하는 알고리즘이기 때문에 실제 구현은 재귀 함수를 이용했을 때 매우 간결하게 구할 수 있다.

### :computer: Example depth first search code

```
graph = [[], [2, 3, 8], [1, 7, 8], [1, 4, 5], [3, 5], [3, 4], [7], [6, 8], [1, 7]]

visited = [False] * 9


def dfs(Graph, v, Visited):
    Visited[v] = True
    print(v, end=' ')

    for i in Graph[v]:
        if Visited[i] is False:
            dfs(Graph, i, Visited)


dfs(graph, 1, visited)
```

### BFS(Breadth First Search)
- **BFS(Breadth First Search)** 알고리즘은 **'너비 우선 탐색'**이라는 의미를 가진다. 쉽게 말해 **가까운 노드부터 탐색하는 알고리즘**이다.
- BFS 구현에서는 선입선출 방식인 큐 자료구조를 이용하는 것이 정석이다. 인접한 노드를 반복적으로 큐에 넣도록 알고리즘을 작성하면 자연스럽게 먼저 들어온 것이 먼저 나가게 되어, 가까운 노드부터 탐색을 진행하게 된다.
- 알고리즘의 정확한 동작 방식은 다음과 같다.
    1. 탐색 시작 노드를 큐에 삽입하고 방문 처리를 한다.
    2. 큐에서 노드를 꺼내 해당 노드의 인접 노드 중에서 방문하지 않은 노드를 모두 큐에 삽입하고 방문 처리를 한다.
    3. 2번의 과정을 더 이상 수행할 수 없을 때 까지 반복한다.
- BFS는 deque 라이브러릴 사용하는 것이 좋으며 탐색을 수행함에 있어 O(N)의 시간이 소요된다. 일반적인 경우 실제 수행 시간은 DFS보다 좋은 편이라는 점까지만 추가로 기억하자.

### :computer: Example breadth first search code

```
from collections import deque

graph = [[], [2, 3, 8], [1, 7, 8], [1, 4, 5], [3, 5], [3, 4], [7], [6, 8], [1, 7]]
visited = [False] * 9

def bfs(Graph, s, Visited):
    queue = deque([s])
    Visited[s] = True
    while queue:
        v = queue.popleft()
        print(v, end=' ')

        for i in Graph[v]:
            if Visited[i] is False:
                queue.append(i)
                Visited[i] = True


bfs(graph, 1, visited)
```

### :speech_balloon: 음료수 얼려 먹기

- N x M 크기의 얼음 틀이 있다. 구멍이 뚫려 있는 부분은 0, 칸막이가 존재하는 부분은 1로 표시된다. 구멍이 뚫려 있는 부분끼리 상, 하, 좌, 우로 붙어 있는 경우 서로 연결되어 있는 것으로 간주한다. 이때 얼음 틀의 모양이 주어졌을 때 생성되는 총 아이스크림의 개수를 구하는 프로그램을 작성하시오.

### :computer: Code

```
N, M = map(int, input().split())

graph = []

for i in range(N):
    graph.append(list(map(int, input())))


def dfs(x, y):
    if x < 0 or x >= N or y < 0 or y >= M:
        return False

    if graph[x][y] == 0:
        graph[x][y] = 1

        dfs(x - 1, y)
        dfs(x, y - 1)
        dfs(x + 1, y)
        dfs(x, y + 1)
        return True
    return False


result = 0
for i in range(N):
    for j in range(M):
        if dfs(i, j) is True:
            result += 1
print(result)
```

### :pencil2: 문제 해설

1. 특정한 지점의 주변 상, 하, 좌, 우를 살펴본 뒤에 주변 지점 중에서 값이 '0'이면서 아직 방문하지 않은 지점이 있다면 해당 지점을 방문한다.
2. 방문한 지점에서 다시 상, 하, 좌, 우를 살펴보면서 방문을 다시 진행하면, 연결된 모든 지점을 방문할 수 있다.
3. 1 ~ 2번의 과정을 모든 노드에 반복하며 방문하지 않은 지점의 수를 센다.

### :speech_balloon: 미로 탈출

-  동빈이는 N x M 크기의 직사각형 형태의 미로에 갇혀 있다. 미로에는 여러 마리의 괴물이 있어 이를 피해 탈출해야 한다. 동빈이의 위치는 (1, 1)이고 미로의 출구는 (N, M)의 위치에 존재하며 한 번에 한 칸씩 이동할 수 있다. 이때 괴물이 있는 부분은 0으로, 괴물이 없는 부분은 1로 표시되어 있다. 미로는 반드시 탈출할 수 있는 형태로 제시된다. 이때 동빈이가 탈출하기 위해 움직여야 하는 최소 칸의 개수를 구하시오. 칸을 셀 때는 시작 칸과 마지막 칸을 모두 포함해서 계산한다.

### :computer: Code

```
from collections import deque

N, M = map(int, input().split())
graph = []

for i in range(N):
    graph.append(list(map(int, input())))

dx = [-1, 1, 0, 0]
dy = [0, 0, -1, 1]


def bfs(x, y):
    queue = deque()
    queue.append((x, y))

    while queue:
        x, y = queue.popleft()

        for i in range(4):
            nx = x + dx[i]
            ny = y + dy[i]

            if nx < 0 or ny < 0 or nx >= N or ny >= M:
                continue
            if graph[nx][ny] == 0:
                continue
            if graph[nx][ny] == 1:
                graph[nx][ny] = graph[x][y] + 1
                queue.append((nx, ny))
    return graph[N - 1][M - 1]


print(bfs(0, 0))
```

### :pencil2: 문제 해설

1. 맨 처음에 (1, 1)의 위치에서 시작하며, (1, 1)의 값은 항상 1이라고 문제에서 언급되어 있다.
2. (1, 1) 좌표에서 상, 하, 좌, 우로 탐색을 진행하면 바로 옆 노드인 (1, 2) 위치의 노드를 방문하게 되고 새롭게 방문하게 되는 (1, 2) 노드의 값을 2로 바꾸게 된다.
3. 마찬가지로 BFS를 계속 수행하면 결과적으로 다음과 같이 최단 경로의 값들이 1씩 증가하는 형태로 변경된다.
