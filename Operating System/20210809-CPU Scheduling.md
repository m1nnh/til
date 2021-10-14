## CPU Scheduling

### CPU-burst Time의 분포

<center><img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcyK67R%2Fbtq1JWibCaB%2FWfKcFxSm4BQdPKnKDg4Ka1%2Fimg.png"></center>

(사진 출처 - [블로그](https://sangminlog.tistory.com/entry/cpu-scheduling?category=887652))

여러 종류의 `job (process)`이 섞여 있기 때문에 CPU 스케줄링이 필요하다. CPU와 I/O 장치 등 시스템 자원을 골고루 효율적으로 사용해야 한다. -> 공평성보다 효율성

### 프로세스의 특성 분류

- <span style = "background-color:#FAF4C0">I/O bound process</span>
  - CPU를 잡고 계산하는 시간보다 I/O에 많은 시간이 필요한 job (many short CPU bursts)
- <span style = "background-color:#FAF4C0">CPU bound process</span>
  - 계산 위주의 job (few very long CPU bursts)

### CPU Scheduler & Dispatcher

- CPU Scheduler
  - Ready 상태의 프로세스 중에서 이번에 CPU를 줄 프로세스를 고른다.
- Dispatcher
  - CPU의 제어권을 CPU scheduler에 의해 선택된 프로세스에게 넘긴다.
  - 이 과정을 `context switch` (문맥 교환)라고 한다.
- CPU 스케줄링이 필요한 경우
  - Running -> Blocked (I/O 요청하는 시스템 콜)
  - Running -> Ready (할당시간만료로 timer interrupt)
  - Blocked -> Ready (I/O 완료후 인터럽트)
  - Terminate

`Running` -> `Blocked` / `Terminate` 스케줄링은 `nonpreemptive` (강제로 빼앗지 않고 자진 반납), 나머지는 `preemptive` (강제로 빼앗음)

### Scheduling Criteria (성능 척도)

- CPU utilization (이용률) -> 시스템 입장
  - 전체시간 중 CPU가 놀지 않고 일한 시간의 비율
  - keep the CPU as busy as possible
- Throughput (처리량) -> 시스템 입장
  - 주어진 시간동안 몇 개의 일을 처리했는가
  - of processes that complete their execution per time unit
- Turnaround time (소요시간, 반환시간) -> 사용자 (process) 입장
  - CPU를 쓰러와서 다쓰고 나갈 때까지의 시간
  - amount of time to execute a particular process
- Waiting time (대기 시간) -> 사용자 입장
  - CPU를 얻기까지 ready queue에 대기한 시간
  - amount of time a process has been waiting in the ready queue
- Response time (응답 시간) -> 사용자 입장
  - ready queue에 들어와서 <span style = "background-color:#FAF4C0">처음으로</span> CPU를 얻기까지 걸린 시간
  - amount of time it takes from when a request was submitted until the first response is produced, not output

`Waiting time`과 `Response time`의 차이점은 프로세스 처리가 다 끝날 때까지 CPU를 점유하는 것이 아니기에 사용 중에 CPU를 뺏길 수도 있다. 그래서, 프로세스 하나가 전체 끝날 때까지 걸린 시간이 Waiting time, 그리고, 처음으로 CPU를 얻을 때의 시간이 Response time이다.

## CPU Scheduling Algorithm

### FCFS (First-Come First-Served)

- 먼저 온 순서대로 처리하는 방식 (nonpreemptive) -> 비선점형
- CPU를 오래 쓰는 프로세스가 먼저 와서 CPU를 할당 받으면 나머지 프로세스들은 전부 기다려야 하기 때문에 비효율적이다.

<center><img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fcn7exC%2Fbtq1K4s2sxl%2Fhai9hSGTyTzOH3ULn7kOw0%2Fimg.png"></center>

(사진 출처 - [블로그](https://sangminlog.tistory.com/entry/cpu-scheduling?category=887652))

사진 처럼 어떤 프로세스가 먼저 실행되냐에 따라 전체 대기 시간에 상당한 영향을 끼친다. 그래서 생기는 문제점은 <span style = "background-color:#FAF4C0">긴 프로세스 하나 때문에 짧은 프로세스 여러 개가 기다리게 된다. (Convoy effect)</span>

### SJF (Shortest-Job-First)

- 각 프로세스의 다음번 `CPU burst time`을 가지고 스케줄링에 활용한다.
- CPU burst time이 가장 짧은 프로세스를 제일 먼저 스케줄한다.
- minimum average waiting time 보장 (Preemptive)
- Nonpreemptive
  - 일단 CPU를 잡으면 이번 CPU burst가 완료될 때까지 CPU를 선점 당하지 않는다.
- Preemptive
  - 현재 수행중인 프로세스의 남은 burst time보다 더 짧은 CPU burst time을 가지는 새로운 프로세스가 도착하면 CPU를 빼앗긴다.
  - 이 방법을 Shortest-Remaining-Time-First (SRTF)이라고도 부른다.
- 문제점
  - Starvation (기아 현상) : 짧은 프로세스들로 인해 긴 프로세스가 오랫동안 CPU를 잡지 못하는 현상
  - CPU burst time을 미리 알 수 없음 -> 추정은 가능 (과거 사용 이력)

### Priority Scheduling

- 우선순위가 가장 높은 프로세스에게 CPU를 할당 -> 작은 숫자가 우선순위가 높음
  - Preemptive -> 수행중인 프로세스보다 우선순위가 높은 프로세스가 들어오면 CPU를 빼앗는다.
  - Nonpreemptive
- `SJF`는 일종의 priority scheduling이다. (priority = predicted next CPU burst time)
- 문제점
  - Starvation (기아 현상)
- 해결
  - Aging (노화) : 아무리 우선순위가 낮은 프로세스라 하더라도 시간이 오래 지나면 우선순위를 높여주는 것

### Round Robin (RR)

- 각 프로세스는 동일한 크기의 할당 시간 (time quantum)을 가진다.
- 할당 시간이 지나면 프로세스는 선점 당하고 `ready queue`의 제일 뒤에 가서 다시 줄을 선다.
- n개의 프로세스가 ready queue에 있고 할당 시간이 q time unit인 경우 각 프로세스는 최대 q time unit 단위로 CPU 시간의 1/n을 얻는다. -> 어떤 프로세스도 (n-1)q time unit 이상 기다리지 않는다. (Response time가 빨라진다)
- Peroformance
  - q large -> FCFS
  - q small -> context switch 오버헤드가 커진다.

일반적으로 `SJF`보다 `average turnaround time`이 길지만 `response time`은 더 짧다.

### Multi-Level Queue

- Ready queue를 여러 개로 분할
  - forground (interactive)
  - background (batch - no human interaction)
- 각 큐는 독립적인 스케줄링 알고리즘을 가진다.
  - forground (RR)
  - background (FCFS) -> CPU만 오랫동안 사용 (문맥 교환 오버헤드를 줄이기 위해)
- 큐에 대한 스케줄링이 필요
  - Fixed priority scheduling
    - serve all from forground then from background.
    - Possibility of starvation.
  - Time slice
    - 각 큐에 CPU time을 적절한 비율로 할당
    - ex) 80% to forground in RR, 20% to background in FCFS

### Multi-Level Feedback Queue

- 프로세스가 다른 큐로 이동이 가능하다.
- MLFQ 방식으로 aging 구현 가능 -> 처음엔 time quntum을 짧게 가면 갈수록 길게
- MLFQ를 정의하는 파라미터들
  - queue의 수
  - 각 큐의 스케줄링 알고리즘
  - process를 상위 큐로 보내는 기준
  - process를 하위 큐로 보내는 기준
  - 프로세스가 CPU 서비스를 받으려할 때 들어갈 큐를 결졍하는 기준

<center><img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FzptnL%2Fbtq1G9iWNDR%2FzKNnJSKPhSEOYPw51TN5b0%2Fimg.png"></center>

(사진 출처 - [블로그](https://sangminlog.tistory.com/entry/cpu-scheduling?category=887652))

### Multiple-Processor Scheduling

- CPU가 여러 개인 경우 스케줄링은 더욱 복잡해짐
- Homogeneous processor인 경우
  - Queue에 한줄로 세워서 각 프로세서가 알아서 꺼내가게 할 수 있다.
  - 반드시 특정 프로세서에서 수행되어야 하는 프로세스가 있는 경우에는 문제가 더 복잡해짐
- Load Sharing
  - 일부 프로세서에 job이 몰리지 않도록 부하를 적절히 공유하는 메커니즘이 필요
  - 별개의 큐를 두는 방법 vs 공동 큐를 사용하는 방법
- Symmetric Multiprocessing (SMP)
  - 각 프로세서가 각자 알아서 스케줄링 결정
- Asymmetric multiprocessing -> 하나의 CPU가 다른 CPU를 책임
  - 하나의 프로세서가 시스템 데이터의 접근과 공유를 책임지고 나머지 프로세서는 거기에 따름

### Real-Time Scheduling

- Hard real-time systems
  - Hard real-time task는 정해진 시간 안에 반드시 끝내도록 스케줄링해야 함 (deadline 보장)
- Soft real-time computing
  - Soft real-time task는 일반 프로세스에 비해 높은 우선순위를 갖도록해야 함
  - deadline을 꼭 보장하지 못함

### Thread Scheduling

- Local Scheduling
  - User level thread의 경우 사용자 수준의 thread library에 의해 어떤 thread를 스케줄할지 결정
- Global Scheduling
  - Kernel level thread의 경우 일반 프로세스와 마찬 가지로 커널의 단기 스케줄러가 어떤 thread를 스케줄할지 결정
