## :star: Binary Search (Reference : 이것이 코딩테스트다.)

### 1. 범위를 반씩 좁혀가는 탐색
- 순차 탐색(Sequential Search)이란 **리스트 안에 있는 특정한 데이터를 찾기 위해 앞에서부터 데이터를 하나씩 차례대로 확인하는 방법이다.

### :computer: Sequential Search Code

```
input_data = input().split()
array = list(map(str, input().split()))
count = 0
for i in array:
    count += 1
    if input_data[1] == i:
        print(count)
        break
```

- 순차 탐색은 데이터 정렬 여부와 상관없이 가장 앞에 있는 원소부터 하나씩 확인해야 한다는 점이 특징이다. 따라서 데이터의 개수가 N개일 때 쵀대 N번의 비교 연산이 필요하므로 순차 탐색의 최악의 경우 시간 복잡도는 O(N)이다.

### 이진 탐색 : 반으로 쪼개면서 탐색하기
- 이진 탐색(Binary Search)은 배열 내부의 데이터가 정렬되어 있어야만 사용할 수 있는 알고리즘이다. 데이터가 무작위일 때는 사용할 수 없지만, 이미 정렬되어 있다면 매우 빠르게 데이터를 찾을 수 있다는 특징이 있다.
- 이진 탐색은 위치를 나타내는 변수 3개를 사용하는데 탐색하고자 하는 범위의 시작점, 끝점, 그리고 중간점이다. **찾으려는 데이터와 중간점 위치에 있는 데이터를 반복적으로 비교**해서 원하는 데이터를 찾는 게 이진 탐색 과정이다.

### :computer: Binary Search Code(Recursion)

```
def binary_search(arr, start, end, s):
    if start > end:
        return None
    mid = (start + end) // 2
    if array[mid] == s:
        return mid
    elif array[mid] > s:
        return binary_search(arr, start, mid - 1, s)
    else:
        return binary_search(arr, mid + 1, end, s)


N, S = map(int, input().split())
array = list(map(int, input().split()))
index = binary_search(array, 0, len(array) - 1, S)
print(index + 1)
```

### :computer: Binary Search Code(Repeat)

```
N, S = map(int, input().split())
array = list(map(int, input().split()))

start = 0
end = len(array) - 1
result = True
mid = 0
while start <= end:
    mid = (start + end) // 2
    if array[mid] == S:
        result = False
        break
    elif array[mid] > S:
        end = mid - 1
    elif array[mid] < S:
        start = mid + 1

if result is True:
    print('존재하지 않습니다.')
else:
    print(mid + 1)
```

- 코딩 테스트에서의 이진 탐색은 실제 구현이 까다롭다. 심지어 이진 탐색의 원리는 다른 알고리즘에서도 폭 넓게 적용되는 원리와 유사하기 때문에 매우 중요하다. 
- 탐색 범위가 2,000만을 넘어가면 이진 탐색으로 문제에 접근해보길 권한다. 처리해야 할 데이터의 개수나 값이 1,000만 단위 이상으로 넘어가면 이진 탐색과 같이 O(logN)의 속도를 내야 하는 알고리즘을 떠올려야 문제를 풀 수 있는 경우가 많다는 점을 기억하자.

### 트리 자료구조
- 이진 탐색은 전제 조건이 데이터 정렬이다. 데이터베이스는 내부적으로 대용량 데이터 처리에 적합한 트리 자료구조를 이용하여 항상 데이터가 정렬되어 있다. 따라서 데이터베이스에서의 탐색은 이진 탐색과는 조금 다르지만, 이진 탐색과 유사한 방법을 이용해 탐색을 항상 빠르게 수행하도록 설계되어 있어서 데이터가 많아도 탐색하는 속도가 빠르다.
- 트리는 부모 노드와 자식 노드의 관계로 표현된다.
- 트리의 최상단 노드를 루트 노드라고 한다.
- 트리의 최하단 노드를 단말 노드라고 한다.
- 트리에서 일부를 떼어내도 트리 구조이며 이를 서브 트리라 한다.
- 트리는 파일 시스템과 같이 계층적이고 정렬된 데이터를 다루기에 적합하다.

### 이진 탐색 트리
- 트리 자료구조 중에서 가장 간단한 형태가 이진 탐색 트리이다. 이진 탐색 트리란 이진 탐색이 동작 할 수 있도록 고안된, 효율적인 탐색이 가능한 자료구조이다.
- 보통 이진 탐색 트리는 다음과 같은 특징을 가진다.
    - 부모 노드보다 왼쪽 자식 노드가 작다.
    - 부모 노드보다 오른쪽 자식 노드가 크다.
    - 좀 더 간단하게 표현하면 **왼쪽 자식 노드 < 부모 노드 < 오른쪽 자식 노드**

### :speech_balloon: 부품 찾기
- 동빈이네 전자 매장에는 부품이 N개 있다. 각 부품은 정수 형태의 고유한 번호가 있다.
어느 날 손님이 M개 종류의 부품을 대량으로 구매하겠다며 당일 날 견적서를 요청했다.
동빈이는 때를 놓치지 않고 손님이 문의한 부품 M개 종류를 모두 확인해서 견적서를 작성해야 한다.
이때 가게 안에 부품이 모두 있는지 확인하는 프로그램을 작성해보자.
이때 손님이 요청한 부품 번호의 순서대로 부품을 확인해 부품이 있으면 yes를, 없으면 no를 출력한다. 구분은 공백으로 한다.

### :computer: Binary Search Code

```
def binary_search(arr, start, end, f):
    mid = (start + end) // 2
    while start <= end:
        if arr[mid] == f:
            return True
        elif arr[mid] > f:
            return binary_search(arr, start, mid - 1, f)
        else:
            return binary_search(arr, mid + 1, end, f)
    return False


N = int(input())
array = list(map(int, input().split()))
M = int(input())
find = list(map(int, input().split()))
result = False
for i in find:
    result = binary_search(array, 0, len(array) - 1, i)
    if result is True:
        print('yes', end=' ')
    else:
        print('no', end=' ')
```

### :computer: Count Sort Code

```
N = int(input())
array = [0] * 100001

for i in input().split():
    array[int(i)] = 1

M = int(input())
find = list(map(int, input().split()))

for i in find:
    if array[i] == 1:
        print('yes', end=' ')
    else:
        print('no', end=' ')

```

### :computer: Set Data Type Code

```
N = int(input())
array = list(map(int, input().split()))
M = int(input())
find = list(map(int, input().split()))

for i in find:
    if i in array:
        print('yes', end=' ')
    else:
        print('no', end=' ')
```

### :pencil2: 문제 해설
- 이 문제는 3가지 방법을 이용해 모두 효과적으로 풀 수 있다는 점을 기억하자.

### :speech_balloon: 떡볶이 떡 만들기
- 오늘 동빈이는 여행 가신 부모님을 대신해서 떡집 일을 하기로 했다. 오늘은 떡볶이 떡을 만드는 날이다.
동빈이네 떡볶이 떡은 재밌게도 떡볶이 떡의 길이가 일정하지 않다. 대신에 한 봉지 안에 들어가는 떡의 총 길이는 절단기로 잘라서 맞춰준다.
절단기에 높이(H)를 지정하면 줄 지어진 떡을 한 번에 절단한다. 높이가 H보다 긴 떡은 H 위의 부분이 잘릴 것이고, 낮은 떡은 잘리지 않는다.
예를 들어 높이가 19, 14, 10, 17cm인 떡이 나란히 있고 절단기 높이를 15cm로 지정하면 자른뒤 떡의 높이는 15, 14, 10, 15cm가 될 것이다.
잘린 떡의 길이는 차례대로 4, 0, 0, 2cm이다. 손님은 6cm만큼의 길이를 가져간다.
손님이 왔을 때 요청한 총 길이가 M일 때 적어도 M만큼의 떡을 얻기 위해 절단기에 설정할 수 있는 높이의 최댓값을 구하는 프로그램을 작성하시오.

### :computer: Code

```
N, M = map(int, input().split())
array = list(map(int, input().split()))

start = 0
end = max(array)
result = 0

while start <= end:
    mid = (start + end) // 2
    total = 0
    for i in array:
        if i > mid:
            total += i % mid
    if total == M:
        result = mid
        break
    elif total > M:
        start = mid + 1
    else:
        end = mid - 1
print(result)

```

### :pencil2: 문제 해설
- 이 문제에서는 현재 얻을 수 있는 떡볶이의 양에 따라서 자를 위치를 결정해야 하기 때문에 이를 재귀적으로 구현하는 것은 귀찮은 작업이 될 수 있다. 따라서 일반적으로 이 문제와 같은 파라메트릭 서치 문제 유형은 이진 탐색을 재귀적으로 구현하지 않고 반복문을 이용해 구현하면 더 간결하게 문제를 풀 수 있다.
