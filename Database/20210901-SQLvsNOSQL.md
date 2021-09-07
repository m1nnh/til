## SQL vs NoSQL

웹 앱을 개발할 때, `MySQL`과 같은 SQL과 `MongoDB`와 같은 NoSQL 중에서 데이터베이스를 고민하게 된다. 단순히 서버 랭기지의 프레임워크에 따라 결정하는 것은 아니다. 둘의 차이점을 명백히 알고, 알맞는 데이터베이스를 선택해야 한다.

### SQL (관계형 데이터베이스)

SQL을 이용하면 RDBMS에서 데이터를 저장, 수정, 삭제 및 검색을 할 수 있다. 관계형 데이터베이스에는 핵심적인 두 가지 특징이 있다.

- 데이터는 정해진 데이터 스키마에 따라 테이블에 저장된다.
- 데이터는 관계를 통해 여러 테이블에 분산된다.

데이터는 테이블에 레코드로 저장되는데, 각 테이블마다 명확하게 정의된 구조가 있다. 해당 구조는 필드의 이름과 데이터 유형으로 정의된다. (Attribute) 따라서 스키마를 준수하지 않은 레코드는 테이블에 추가할 수 없다. 즉, 스키마를 수정하지 않는 이상은 정해진 구조에 맞는 레코드만 추가가 가능한 것이 관계형 데이터베이스의 특징 중 하나다.

또한, 데이터의 중복을 피하기 위해 관계를 이용한다.

<center><img src = "https://user-images.githubusercontent.com/78870076/131658270-580c7488-378b-4261-a816-4906048ff9b6.png"></center>

(사진 출처 - [깃허브](https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Database/SQL과%20NOSQL의%20차이.md))

하나의 테이블에서 중복 없이 하나의 데이터만을 관리하기 때문에 다른 테이블에서 부정확한 데이터를 다룰 위험이 없어지는 장점이 있다.

### NoSQL (비관계형 데이터베이스)

말 그대로 SQL을 사용하지 않는 것이다. 따라서, 스키마도 없고 관계도 없다. NoSQL에서는 레코드를 문서(documents)라고 부른다. 여기서 SQL과 핵심적인 차이가 있는데, SQL은 정해진 스키마를 따르지 않으면 데이터 추가가 불가능했다. 하지만 NoSQL에서는 다른 구조의 데이터를 같은 컬렉션에 추가가 가능하다.

문서는 Json과 비슷한 형태를 가지고 있다. 관계형 데이터베이스처럼 여러 테이블에 나누어담지 않고, 관련 데이터를 동일한 컬렉션에 넣는다. 따라서 위 사진에 SQL에서 진행한 `Orders`, `Users`, `Products` 테이블로 나눈 것을 NoSQL에서는 `Orders`에 한꺼번에 포함해서 저장하게 된다. 따라서 여러 테이블을 조인할 필요없이 이미 필요한 모든 것을 갖춘 문서를 작성하는 것이 `NoSQL`이다. (Join이라는 개념이 필요없음)

하지만, 조인하고 싶을 때는 컬렉션을 통해 데이터를 복제하여 각 컬렉션 일부분에 속하는 데이터를 정확하게 산출하도록 한다. 그렇게되면 데이터가 중복되어 서로 영향을 줄 위험이 있다. 따라서 조인을 잘 사용하지 않고 자주 변경되지 않는 데이터일 때 NoSQL을 쓰면 상당히 효율적이다.

### SQL vs NoSQL

두 데이터베이스를 비교할 때 중요한 `Scaling` 개념도 존재한다. 데이터베이스 서버의 확장성은 수직적 확장과 수평적 확장으로 나누어진다.

- 수직적 확장 : 단순히 데이터베이스 서버의 성능을 향상시키는 것 (ex. CPU Upgrade)
- 수평적 확장 : 더 많은 서버가 추가되고 데이터베이스가 전체적으로 분산됨을 의미 (하나의 데이터베이스에서 작동하지만 여러 호스트에서 작동)

데이터 저장 방식으로 인해 SQL 데이터베이스는 일반적으로 수직적 확장만 지원한다. 수평적 확장은 NoSQL 데이터베이스에서만 가능하다.

#### SQL의 장점과 단점 

- 장점
    - 명확하게 정의된 스키마, 데이터 무결성 보장 
    - 관계는 각 데이터를 중복없이 한번만 저장 
- 단점
    - 덜 유연하다. 데이터 스키마를 사전에 계획하고 알려야 함. (ERD) -> 수정비용 증가
    - 관계를 맺고 있어서 조인문이 많은 복잡한 쿼리가 만들어질 수 있음 
    - 대체로 수직적 확장만 가능함 

관계를 맺고 있는 데이터가 자주 변경되는 애플리케이션의 경우나, 변경될 여지가 없고, 명확한 스키마가 사용자와 데이터에게 중요한 경우 SQL을 이용한다.

- 복잡한 join문을 만들지 않고 설계하여 단점 제거

#### NoSQL의 장점과 단점 

- 장점
    - 스키마가 없어서 유연함. 언제든지 저장된 데이터를 조정하고 새로운 필드 추가 가능
    - 데이터는 애플리케이션이 필요로 하는 형식으로 저장됨. 데이터를 읽어오는 속도가 빨라짐
    - 수직 및 수평 확장이 가능해서 애플리케이션이 발생시키는 모든 읽기/쓰기 요청 처리 가능
- 단점
    - 유연성으로 인해 데이터 구조 결정을 미루게 될 수 있음
    - 데이터 중복을 꼐속 업데이트 해야 함
    - 데이터가 여러 컬렉션에 중복되어 있기 때문에 수정 시 모든 컬렉션에서 수행해야 함

정확한 데이터 구조를 알 수 없거나 변경/확장 될 수 있는 경우나, 읽기를 자주 하지만, 데이터 변경은 자주 없는 경우나, 데이터베이스를 수평적으로 확장해야 하는 경우 사용한다.

- 중복 데이터를 줄이는 방법으로 설계
