# Part1. Foundations of Data Systems
각 챕터 개요 정리 
* Chapter 1 : 책에서 설명할 때 주요 사용되는 용어들에 대해 정리한다. (`reliability`, `scalability`, `maintainability` 등) 또한, 이 목표들을 어떻게 이룰 수 있는지 설명한다.
* Chapter 2 : 다른 data model 들과 query language 들을 설명한다. 각각의 model 들이 어떠한 상황에 적합한지 설명한다. 
* Chapter 3 : Storage Engine 에 대해 살펴본다. Database 가 디스크에서 어떻게 동작하는지 살펴본다. 다른 Storage Engine 은 각자 다른 작업에 용이하다. 
* Chapter 4 : 다양한 형태의 데이터 encoding 에 대해 살펴본다. 

(Part 2에서는 분산 시스템에서서 발생할 수 있는 문제들에 대해 살펴본다.)

## Reliability, Scalaility, Maintainability 
* Reliability :  `Tolerating hardware & software faults`, `Human error`
* Scalability : `Measuring load & performance`, `Latency percentiles, throughput`
* Maintainability : `Operability`, `simplicity & evolvability`

## Chapter1. Reliable, Scalable, and Maintainable Applications 
오늘날의 많은 어플리케이션들은 compute-intensive 하지 않고, data-intensive 하다. 많은 어플리케이션들은 다음의 속성들을 만족해야 한다.
* 데이터를 저장하고, 나중에 찾을 수 있어야 한다 (databases)
* 오래 걸리는 작업들을 기억하고, 읽기 속도를 높인다 (caches)
* 유저들에게 키워들을 기반으로 검색할 수 있게 하거나, 필터를 저장할 수 있게 한다. (search indexes)
* 다른 프로세스에 메세지를 전달하고, 비동기적으로 처리한다 (stream processing)
* 주기적으로 많은 양의 데이터를 처리한다 (batch processing)

오늘날에는 위의 기능들이 추상화되어 있다. (이미 만들어진 소프트웨어들이 존재한다.) 하지만 오늘날에는 많은 종류의 데이터베이스 시스템이 있기 때문에, 각자의 용도에 맞는 데이터베이스를 사용해야 한다. 

### Thinking About Data Systems
우리는 database, queue, cache 등을 각각 다른 카테고리의 도구라고 생각한다. 하지만 그들은 공통적으로 데이터를 저장한다는 공통점이 있다. 
* 지난 몇년간 데이터를 다루는 다양한 도구들이 나왔다. 최근에 나온 도구들은 전통적인 니즈에 부가적인 기능을 지니기도 한다. 예를 들어, Redis 는 데이터를 저장하는 도구이지만 Queue 로서도 쓰이고, Apache Kafka 는 Message Queue 이지만 Database 와 같은 Durability 를 지닌다. 각각의 용도에 대한 경계선은 희미해졌다.
* 하나의 작업이 더 이상 모든 니즈를 충족할 수 없게 되었다. 각각의 작업들을 처리할 수 있는 도구들로 나누어지게 되었다. 예ㅔ를 들어, cache 기능을 구현한다고 했을 때, full-text search server(Elasticsearch or Solr) 를 main database 와 분리한다.  

## Reliability 
일반적으로 Reliability 는 다음을 의미한다. 
* 어플리케이션이 사용자가 예상한 대로 동작해야 한다 
* 사용자가 실수하거나, 소프트웨어가 비정상적으로 사용되었을 때에 정상적으로 동작해야 한다.
* 충분히 좋은 성능이 보장되어야 한다 
* 비정상적인 접근을 막을 수 있어야 한다.


### Faults
정상적으로 동작하지 않는 현상을 `faults` 라고 부른다. faults 를 예상하고 이에 대처할 수 있다는 것을 `fault-tolerant` 또는 `resilent` 라고 부른다. fault 는 `failure` 과는 다르다. `fault` 는 하나의 컴포넌트가 정상적으로 동작하지 않는 것을 의미하지만, `failure` 는 시스템 전체가 동작을 멈추는 것을 의미한다. 

### Hardware Faults
system failure 를 떠올려 보면, hardware faults 가 가장 먼저 떠오른다. (Hardk disk crash, RAM fault 등)

### Software Errors 
소프트웨어 버그로 인해 파생되는 에러를 의미한다. 이는 좀 더 예상하기 힘들다.

### Human Errors 
사람의 실수로 인해 발생하는 에러를 의미한다.

## Scalability 
부하가 늘었을 때에 시스템의 성능을 늘릴 수 있는지에 대한 여부를 의미한다. 예를 들어, 트위터의 초기 버전에서는 유저가 트윗을 조회할 때마다 SQL Query 를 날렸지만, 부하가 심해진 이후에는 Cache 레이어를 두었다.

### Describing Performance
퍼포먼스는 다음의 방식으로 측정할 수 있다. 
* 부하를 높였을 경우에, 시스템의 퍼포먼스가 얼마나 저하되는가 ?
* 부하를 높였을 경우에, 동일한 시스템 퍼포먼스를 내려면 

하둡(HADOOP) 과 같은 일괄 처리 시스템은 처리량(throughput, 초당 처리할 수 있는 레코드 수)에 관심을 가지고, 온라인 시스템에서는 응답 시간(response time) 을 주요 성능으로 측정한다.

> 지연 시간(altency) 와 응답 시간(response time)
응답 시간은 클라이언트 관점에서 본 시간이고, 지연 시간은 서버 관점에서 바라본 용어이다. 

응답 시간은 항상 일정하지 않으므로, 산술평균이나 꼬리 지연 시간(tail latency) 값으로 성능을 측정한다. 백분위는 서비스 수준 목표(service level objective) 에 자주 등장한다.

### 부하 대응 접근 방식 
* 사람들은 확장성과 관련해 용량 확장(scaling up, vertical scaling), 규모 확장(scaling out, horizontal scaling)으로 구분해 말하곤 한다. 
* 일부 시스템은 탄력적이다. 즉 부하 증가를 감지하면 컴퓨팅 자원을 자동으로 추가할 수 있다. 
* 다수의 장비에 상태 비저장 서비스를 배포하는 일은 간단하지만, 단일 노드에 상태 유지 데이터 시스템을 분산 설치하는 일은 아주 복잡도가 높다.


### 유지보수성 
소프트웨어 비용의 대부분은 유지보수에서 발생한다. 레거시 소프트웨어를 직접 만들지 않게끔 소프트웨어를 설계할 수 있다. 

* 운용성(operability)
운영팀이 시스템을 원활하게 운영할 수 있게 쉽게 만들기 
* 단순성(simplicity)
시스템에서 복잡도를 최대한 제거해 새로운 엔지니어가 시스템을 이해하기 쉽게 만들기 (UI 의 단순성과는 다르다)
* 발전성(evolvability)
엔지니어가 이후에 시스템을 쉽게 변경할 수 있게 하기. 이 속성은 유연성(extensibility), 수직가능성(modifiability), 적응성(plasticity) 로 알려져 있다.

#### 운용성: 운영의 편리함 만들기 
좋은 운영팀은 일반적으로 다음과 같은 작업 등을 책임진다.
* 시스템 상태를 모니터링하고 상태가 좋지 않다면 빠르게 서비스를 복원 
* 시스템 장애. 성능 저하 등의 문제의 원인을 추적 
* 보안 패치를 포함해 소프트웨어와 플랫폼을 최신상태로 유지 
* 다른 시스템이 서로 어떻게 영향을 주는지 확인해 문제가 생길 수 있는 변경사항을 손산을 입히기 전에 차단 
* 미래에 발생 가능한 문제를 예측해 문제가 발생하기 전에 해결 

좋은 운영성이란 동일하게 반복되는 태스크를 쉽게 수행하게끔 만들어 운영팀이 고부가가치 활동에 노력을 집중한다는 의미다. 데이터 시스템은 동일 반복 태스크를 쉽게 하기 위해 아래 항목 등을  포함해 다양한 일을 할 수 있다. 

* 좋은 모니터링으로 런타임 동작과 시스템 내부에 대한 가시성 제공 
* 표준 도구를 이용해 자동화와 통합을 위한 우수한 지원을 제공 
* 개별 장비 의존성을 회피. 유지보수를 위해 장비를 내리더라도 시스템 전체에 영향을 주지 않고 계속해서 운영 가능해야 함 
* 좋은 문서와 이해하기 쉬운 운영 모델 제공 
* 만족할만한 기본 동작을 제공하고, 필요할 때 기본값을 다시 정의할 수 있는 자유를 관리자에게 부여 
* 적절하게 자기 회복이 가능할 뿐 아니라 필요에 따라 관리자가 시스템 상태를 수동으로 제어할 수 있게 함 
* 예측 가능하게 동작하고 예기치 않은 상황을 최소화함 

#### 단순성: 복잡도 관리 
프로젝트가 커짐에 따라 시스템은 매우 복잡하고 이해하기 어려워진다. 복잡도 수렁에 빠진 소프트웨어 프로젝트를 때론 `커다란 진흙 덩어리(big ball of mud)`로 묘사한다. 단순성이 구축하려는 시스템의 핵심 목표여야 한다. 시스템을 단순하게 만드는 일은 `우발적 복잡도(accidental complexity)` 를 줄인다는 뜻이다. 우발적 복잡도를 제거하기 위한 최상의 도구는 `추상화` 다. 좋은 추상화는 깔끔하고 직관적인 외관 아래로 많은 세부 구현을 숨길 수 있다. 


#### 발전성: 변화를 쉽게 만들기 
에자일(agile) 작업 패턴은 변화에 적응하기 위한 프레임워크를 제공한다.