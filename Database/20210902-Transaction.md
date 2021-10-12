## 트랜잭션 (Transaction)

트랜잭션이란 상태를 변화시키기 위해 수행하는 작업 단위이다.

- 상태변화 -> SQL 질의어를 통해 데이터베이스에 접근
    - select, insert, delete, update 
- 작업 단위 -> 많은 SQL 명령문들을 사람이 정하는 기준에 따라 정하는 것

트랜잭션의 가장 대표적인 예시는 은행 송금이다. 사용자 A가 B에게 만원을 송금하는 API를 설계한다고 가정을 하면, 데이터베이스에서 이루어지는 작업은

- A의 계좌에서 만원 삭감 -> update
- B의 계좌에서 만원 추가 -> update 

이렇게 동시에 두개의 질의어가 합쳐져 현재 작업 단위는 출금 update문 + 입금 update문이다. 이를 통틀어 하나의 트랜잭션이라고 한다. 

- 위 두 쿼리문 모두 성공적으로 완료되어야만 하나의 작업 단위가 완료되는 것이다. -> `commit`
- 작업 단위에 속하는 쿼리 중 하나라도 실패하면 모든 쿼리문을 취소하고 이전 상태로 돌려놓아야 한다. -> `Rollback`

즉, 하나의 트랜잭션 설계를 잘 하는 것이 데이터를 다룰 때 많은 이점을 가져다준다.

### 트랜잭션 특징

트랜잭션 특징에는 대표적으로 4가지가 있다.

- 원자성 (Atomicity)
    - 트랜잭션이 데이터베이스에 모두 반영되거나, 혹은 전혀 반영되지 않아야 된다.
- 일관성 (Consistency)
    - 트랜잭션의 작업 처리 결과는 항상 일관성 있어야 한다.
- 독립성 (Isolation)
    - 둘 이상의 트랜잭션이 동시에 병행 실행되고 있을 때, 어떤 트랜잭션도 다른 트랜잭션 연산에 끼어들 수 없다.
- 지속성 (Durability)
    - 트랜잭션이 성공적으로 완료되었으면, 결과는 영구적으로 반영되어야 한다.

### Commit과 Rollback

- Commit
    - 하나의 트랜잭션이 성공적으로 끝났고, 데이터베이스가 일관성 있는 상태일 때 알려주기 위해 사용하는 연산 -> 데이터베이스의 결과를 저장한다는 의미로 생각하면 된다.
- Rollback
    - 하나의 트랜잭션 처리가 비정상적으로 종료되어 트랜잭션 원자성이 깨진 경우 트랜잭션의 시작 상태로 돌아가는 연산 -> 데이터베이스 질의문이 실행이 취소 됐다고 생각하면 된다.

### 트랜잭션 관리를 위한 DBMS 전략

#### DBMS의 구조

DBMS는 크게 `Query Processor` (질의 처리기), `Storage System` (저장 시스템) 두 가지로 나뉜다. 입출력 단위는 고정 길이의 `page` 단위로 `disk`에 읽거나 쓴다. 저장 공간은 비휘발성 저장 장치인 disk에 저장, 일부분은 `Main Memory`에 저장

<center><img src = "https://user-images.githubusercontent.com/78870076/131861403-79e97dca-e9ea-4a2a-8a63-ee5927724de5.png"></center>

(사진 출처 - [깃허브](https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Database/Transaction.md))

#### Page Buffer Manager / Buffer manager 

DBMS의 Storage System에 속하는 모듈 중 하나로, Main Memory에 상주하는 페이지를 관리하는 모듈이다. Buffer 관리 정책에 따라, `UNDO` 복구와 `REDO` 복구가 요구되거나 그렇지 않게 되므로, 트랜잭션 관리에 매우 중요한 결정을 가져온다.

#### UNDO 

수정된 page들이 Buffer 교체 알고리즘에 따라서 디스크에 출력될 수 있다. Buffer 교체는 트랜잭션과 무관하게 Buffer의 상태에 따라서 결정된다. 이로 인해, 정상적으로 종료되지 않은 트랜잭션이 변경한 page들은 원상 복구 되어야 하는데, 이 복구를 `UNDO`라고 한다. 

undo에는 두 가지 정책이 있다. (수정된 페이지를 디스크에 쓰는 시점으로 분류)

- steal : 수정된 페이지를 언제든지 디스크에 쓸 수 있는 정책 
    - 대부분의 DBMS가 채택하는 Buffer 관리 정책 
    - UNDO logging과 복구를 필요로 함.
- ¬steal : 수정된 페이지들을 EOT (End Of Transaction)까지는 버퍼에 유지하는 정책
    - undo 작업이 필요하지 않지만, 매우 큰 메모리 버퍼가 필요하다. 

#### REDO

이미 commit한 트랜잭션의 수정을 재반영하는 복구 작업이다. Buffer 관리 정책에 영향을 받는다.

- 트랜잭션이 종료되는 시점에 해당 트랜잭션이 수정한 page를 디스크에 쓸 것인가 아닌가로 기준 
    - Force : 수정했던 모든 페이지를 트랜잭션 commit 시점에 disk에 반영 
    - 트랜잭션이 commit 되었을 때 수정된 페이지들이 disk 상에 반영되므로 redo 필요 없음
    - ¬Force : commit 시점에 반영하지 않는 정책
    - 트랜잭션이 disk 상의 데이터베이스에 반영되지 않을 수 있기에 redo 복구가 필요. (대부분의 DBMS 정책)