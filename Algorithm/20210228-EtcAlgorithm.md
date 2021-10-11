## :star: Etc . Algorithm (Reference : 이것이 코딩테스트다.)

### 1. 더 알아두면 좋은 알고리즘
- 소수란 **2보다 큰 자연수 중에서 1과 자기 자신을 제외한 자연수로는 나누어떨어지지 않는 자연수**이다.

### 에라토스테네스의 체
- 에라토스테네스의 체 알고리즘은 여러 개의 수가 소수인지 아닌지를 판별할 때 사용하는 대표적인 알고리즘이다. 에라토스테네스의 체는 N보다 작거나 같은 모든 소수를 찾을 때 사용할 수 있다. 에라토스테네스의 체 알고리즘은 다음과 같다.
    i. 2부터 N까지의 모든 자연수를 나열한다.
    ii. 남은 수 중에서 아직 처리하지 않은 가장 작은 수 i를 찾는다.
    iii. 남은 수 중에서 i의 배수를 모두 제거한다.(i는 제거하지 않는다)
    iv. 더 이상 반복할 수 없을 때까지 2번과 3번의 과정을 반복한다.

### :computer: Sieve of Eratosthenes Code

```
import math

N = int(input())
J = [True for _ in range(N + 1)]
J[0], J[1] = False, False

for i in range(2, int(math.sqrt(N)) + 1):
    if J[i] is True:
        for j in range(i * 2, N + 1, i):
            J[j] = False
```
### 투 포인터
- 투 포인터 알고리즘은 **리스트에 순차적으로 접근해야 할 때 2개의 점의 위치를 기록하면서 처리**하는 알고리즘을 의미한다.
- 투 포인터 알고리즘의 특징은 2개의 변수를 이용해 리스트 상의 위치를 기록한다는 점이다. 구체적인 알고리즘은 다음과 같다.
    1. 시작점과 끝점이 첫 번째 원소의 인덱스(0)를 가리키도록 한다.
    2. 현재 부분합이 M과 같다면 카운트한다.
    3. 현재 부분합이 M보다 작으면 end를 1 증가시킨다.
    4. 현재 부분합이 M보다 크거나 같으면 start를 1 증가시킨다.
    5. 모든 경우를 확인할 때까지 2번부터 4번까지의 과정을 반복한다.

### :computer: Two Pointer Code

```
N = 5
M = 5
data = [1, 2, 3, 2, 5]

count = 0
interval_sum = 0
end = 0

for start in range(N):
    while interval_sum < M and end < N:
        interval_sum += data[end]
        end += 1

        if interval_sum == M:
            count += 1
        interval_sum -= data[start]

print(count)
```

- 이 밖에도 투 포인터 알고리즘은 '정렬되어 있는 두 리스트의 합집합' 같은 문제에 효과적으로 사용 할 수 있다.
    1. 정렬된 리스트 A와 B를 입력받는다.
    2. 리스트 A에서 처리되지 않은 원소 중 가장 작은 원소를 i가 가리키도록 한다.
    3. 리스트 B중에서 처리되지 않은 원소 중 가장 작은 원소를 j가 가리키도록 한다.
    4. A[i]와 B[j] 중에서 더 작은 원소를 결과 리스트에 담는다.
    5. 리스트 A와 B에서 더 이상 처리할 원소가 없을 때까지 2~4번의 과정을 반복한다.

### :computer: Two Pointer Code

```
N, M = map(int, input().split())

A = list(map(int, input().split()))
B = list(map(int, input().split()))

result = []

i = 0
j = 0

while i < N or j < M:

    if j >= M or (i < N and A[i] <= B[j]):
        result.append(A[i])
        i += 1
    else:
        result.append(B[j])
        j += 1

print(result)
```

### 구간 합
- N개의 수에 대하여 접두사 합(Prefix Sum)을 계산하여 배열 P에 저장한다.
- 매 M개의 쿼리 정보 [L, R]을 확인할 때, 구간 합은 P[R]-P[L-1]이다.

### :computer: Prefix Sum Code

```
N = int(input())
data = list(map(int, input().split()))
prefix_sum = [0]
sum_value = 0

for i in data:
    sum_value += i
    prefix_sum.append(sum_value)

L, R = map(int, input().split())
print(prefix_sum[R] - prefix_sum[L - 1])
```

### 순열과 조합
- **순열**이란 **서로 다른 n개에서 r개를 선택하여 일렬로 나열하는 것**을 의미한다.
- itertools에 permutations()함수를 사용하면 된다.
- **조합**이란 **서로 다른 n개에서 순서에 상관없이 서로 다른 r개를 선택하는 것**을 의미한다.
- itertools에 combinations()함수를 사용한다.

### :speech_balloon: 소수 구하기
M 이상 N 이하의 소수를 모두 출력하는 프로그램을 작성하시오.

### :computer: Code

```
import math

M, N = map(int, input().split())
check = [True] * (N + 1)
check[0], check[1] = False, False

for i in range(2, int(math.sqrt(N)) + 1):
    if check[i] is True:
        for j in range(i * 2, N + 1, i):
            check[j] = False

for i in range(M, N):
    if check[i] is True:
        print(i, end=' ')

```

### :pencil2: 문제 해설
앞서 배운 에라토스테네스의 체를 이용해서 풀면 된다.

### :speech_balloon: 암호 만들기
기바로 어제 최백준 조교가 방 열쇠를 주머니에 넣은채 깜빡하고 서울로 가버리는 황당한 상황에 직면한 조교들은
702호에 새로운 보안 시스템을 설치하기로 하였다. 이 보안 시스템은 열쇠가 아닌 암호로 동작하는 시스템이다.
암호는 서로 다른 L개의 알파벳 소문자들로 구성되며 최소 한 개의 모음과 최소 두 개의 저음으로 구성되어 있다고 알려져 있다.
또한 정렬된 문자열을 선호하는 조교들의 성향으로 미루어보아 암호를 이루는 알파벳이 암호에서 증가하는 순서로 배열되었을 것이라고 추측한다.
즉 abc는 가능성이 있는 암호이지만 bac는 그러지 않다.
새 보안 시스템에서 조교들이 암호로 사용했을 법한 문자의 종류는 C가지가 있다고 한다. 이 알파벳을 입수한 민식, 영식 형제는
조교들의 방에 침투하기 위해 암호를 추측해보려고 한다. C개의 문자들이 모두 주어졌을 때, 가능성이 있는 암호들을 모두 구하는 프로그램을 작성하시오.
### :computer: Code

```
from itertools import combinations

L, C = map(int, input().split())
String = list(map(str, input().split()))
vowels = ('a', 'e', 'i', 'o', 'u')
String.sort()

for password in combinations(String, L):
    count = 0
    for i in password:
        if i in vowels:
            count += 1
    if count >= 1 and count <= L - 2:
        print(''.join(password))
```

### :pencil2: 문제 해설
조합 알고리즘 문제 유형에 속한다. 파이썬의 combinations() 라이브러리를 이용하면 된다.