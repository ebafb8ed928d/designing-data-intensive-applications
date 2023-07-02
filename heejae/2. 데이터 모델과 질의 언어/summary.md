### 관계형 모델과 문서 모델 
오늘날 잘 알려진 데이터 모델은 1970년 에드가 코드(Edgar Codd) 가 제안한 관계형 모델을 기반으로 한 SQL 이다. 데이터는 관계(relation)로 구성되고 관계는 순서없는 튜플(tuple) 모음이다. 


### NoSQL 의 탄생 
NoSQL 데이터베이스를 채택한 데는 다음과 같은 다양한 원동력이 있다. 
* 대규모 데이터셋이나 매우 높은 쓰기 처리량 달성을 관계형 데이터베이스보다 쉽게 할 수 있는 뛰어난 확장성의 필요 
* 상용 데이터베이스 제품보다 무료 오픈소스 소프트웨어에 대한 선호도 확산
* 관계형 모델에서 지원하지 않는 특수 질의 동작 
* 관계형 스키마의 제한에 대한 불만과 더욱 동적이고 표현력이 풍부한 데이터 모델에 대한 바람 

미래에는 관계형 데이터베이스가 폭넓은 다양함을 가진 비관계형 데이터스토어와 함께 사용될 것이다. 이런 개념을 종종 `다중 저장소 지속성(polygot persistence)` 이라 한다.

### 객체 관계형 불일치 
오늘날 대부분의 어플리케이션은 객체지향 프로그래밍 언어로 개발한다. 데이터를 관계형 테이블에 저장하려면 어플리케이션 코드와 데이터베이스 모델 객체(테이블, 로우, 칼럼) 사이에 거추장스러운 전환 계층이 필요하다. 이런 모델 사이의 분리를 종종 `임피던스 불일치(impedence mismatch)` 라고 부른다. 

예를 들어, 관계형 스키마에서 이력서를 어떻게 표현하는지 보여준다. 
* 전통적인 SQL 에서 가장 일반적인 정규화 표현은 직위, 학력, 연락처 정보를 개별 테이블에 넣고, 외래 키로 user 테이블을 참조한다.
* SQL 데이터베이스 시스템에서 지원하는 JSON 필드에 대한 지원을 사용한다.
* 직업, 학력, 연락처 정보를 JSON 이나 XML 문서로 텍스트 컬럼에 저장한다.

JSON 표현은 다중 테이블(multi-table) 스키마보다 더 나은 지역성(locality)을 갖는다. JSON 표현에서는 모든 관련 정보가 한 곳에 있어 질의 하나로 충분하다.


### 다대일과 다대다 관계 
단순텍스트 필드를 사용하지 않고, ID 를 사용하는 장점은 다음과 같다. ID 자체는 아무런 의믹 ㅏ없기 때뭉네 변경할 필요가 없다. 즉 식별 정보를 변경해도 ID 는 동일하게 유지할 수 있다. 중복된 데이터를 정규화하려면 `다대일(many-to-one)` 관계가 필요한데, 안타깝게도 다대일 관계는 문서 모델에 적합하지 않다. 관계형 데이터베이스에서는 조인이 쉽기 때문에 ID로 다른 테이블의 로우를 참조하는 방식은 일반적이다. 문서 데이터베이스에서는 일대다 트리 구조를 위해 조인이 필요하지 않지만 조ㅇ인에 대한 지원이 보통 약하다. 


### 관계형 모델 
관계형 모델이 하는 일은 알려진 모든 데이터를 배치하는 것이다. 관계(테이블)은 단순히 튜플(로우)의 컬렉션이 전부이다. query optimizer 은 질의의 어느 부분을 어떤 순서로 실행할지를 결정하고 사용할 색인을 자동으로 결정한다. 


### 문서 데이터베이스와의 비교 
문서 데이터베이스는 별도 테이블이 아닌 상위 레코드 내에 중첩된 레코드를 저장한다. 문서 데이터베이스는 일대다 관계를 자동으로 지원하고, 다대일과 다대다 관계를 표현할 때 근본적으로 다르지 않다. 둘 다 관련 항목은 고유한 식별자로 참조한다. 관계형 모델에서는 외래 키라 부르고 문서 모델에서는 문서 참조라 부른다.


### 어떤 데이터 모델이 어플리케이션 코드를 더 간단하게 할까 ?
어플리케이션에서 데이터가 문서와 비슷한 구조라면 무서 모델을 사용하는 것이 좋다. 문서와 비슷한 구조를 여러 테이블에 찢는 관계형 기법은 복잡한 어플리케이션 코드를 발생시킨다. 


### 문서 모델에서의 스키마 유연성 
문서 데이터베이스에서는 대부분 schemaless 하다. 웹페이지 상에 문서를 접근해야 할 때 `저장소 지역성(storage locality)` 을 활용하면 성능 이점이 있다. 지역성의 이점은 한 번에 많은 부분을 필요로 하는 경우에만 적용된다.


### 문서 데이터베이스와 관계형 데이터베이스의 통합 
대부분의 관계형 데이터베이스에서는 JSON 문서 내부에 대해 색인하고 질의하는 기능을 포함한다. 관계형 데이터베이스와 문서 데이터베이스는 시간이 지남에 따라 점점 더 비슷해지고 있다.


### 데이터를 위한 질의 언어 
SQL 은 선언형 질의 언어인 반면, IMS 와 코다실은 명령형 코드를 사용해 데이터에 질의한다. 선언형 질의 언어는 명령형 API 보다 더 간결하고 쉽게 작업할 수 있기 때문에 매력적이다. 또한 데이터베이스 엔진의 상세 구현이 숨겨져 있어 질의를 변경하지 않고도 데이터베이스 시스템의 성능을 향상시킬 수 있다. 


### 맵리듀스 질의 
맵리듀스(MapReduce)는 많은 컴퓨터에서 대량의 데이터를 처리하기 위한 프로래밍 모델로, NoSQL 데이터 저장소는 제한된 형태의 MapReduce 를 제공한다. 맵리듀스의 사용성 문제는 연계된 자바스크립트 함수 두 개를 신중하게 작성해야 한다는 점인데(예시 참고), 이를 보완하기 위해 집계 파이프라인(aggregation pipeline) 이라 부르는 선언형 질의 언어 지원을 추가했다.

```javascript
db.observations.mapReduce(
  function map() {
    var year = this.observationTimestamp.getFullYear();
    var month = this.observationTimestamp.getMonth()+1;
    emit(year + "-" + month, this.numAnimals);
  },
  function reduce(key, values) {
    return Array.sum(values);
  },
  {
    query: {
      family: "Sharks",
      out: "monthlySharkReport".
    }
  }
)
```


### 그래프형 데이터 모델 
어플리케이션이 주로 일대다 관계이거나 레코드 간 관계가 없다면 문서 모델이 적합하다. 

하지만 데이터에서 다대다 관계가 매우 일반적이라면, 그래프로 데이터를 모델링하기 시작하는 편이 더 자연스럽다. 그래프는 두 유형의 객체로 이루어진다. vertex 와 edge 이다. 자동차 내비게이션 시스템이 대표적인 예시이다. 구체적으로 나누어 보면, `속성 그래프 모델` 과 트리플 저장소 모델이 있다.

#### 속성 고래프 
속성 그래프 모델에서 각 정점(Node) 는 다음과 같은 요소로 구성된다.
* 고유한 식별자
* 유출(outgoing) 간선 집합
* 유입(incoming) 간선 집합 
* 속성 컬렉션(key-value pair)

각 간선(Edge) 는 다음과 같은 요소로 구성된다.
* 고유한 식별자
* 간선이 시작되는 정점(꼬리 정점)
* 간선이 끝나는 정점(머리 정점)
* 두 정점 간 관계 유형을 설명하는 레이블 
* 속성 컬렉션(key-value pair)

다음의 postgresql(관계형 데이터베이스) 예시를 샐펴보자 

```sql
CREATE TABLE vertices (
  vertex_id integer PRIMARY KEY,
  properties json
);

CREATE TABLE edges (
  edge_id integer PRIMARY KEY,
  tail_vertex integer REFERENCES vertices (vertex_id),
  head_vertex integer REFERENCES vertices (vertex_id),
  label text,
  properties json
);

CREATE INDEX edges_tails ON edges (tail_vertex);
CREATE INDEX edges_heads ON edges (head_vertex);
```


#### 사이퍼 질의 언어 
사이퍼(Cypher)는 속성 그래프를 위한 선언형 질의 언어로, Neo4j 그래프 데이터베이스용으로 만들어졌다. 다음의 예시를 살펴보자

```
CREATE
  (NAmerica:Location {name: 'North America', type: 'continent'}),
  (USA:Location {name: 'United States', type: 'country'}),
  (Idaho:Location {name: 'Idaho', type: 'state'}),
  (Lucy:Person {name: 'Lucy'}),
  (Idaho) -[:WITHIN]-> (USA) -[:WITHIN]-> (NAmerica),
  (Lucy) -[:BORN_IN]-> (Idaho)
```

다음은 `미국에서 유럽으로 이민 온 모든 사람들의 이름 찾기` 질의이다.

```
MATCH
  (person) -[:BORN_IN]-> () -[:WITHIN*0..]-> (us:Location) {name: 'United States'}
  (person) -[:LIVES_IN]-> () -[:WITHIN*0..]-> (us:Location) {name: 'Europe'}
RETURN person.name
```

다음 두 가지 조건을 만족하는 정점을 찾아라

1. person 은 어떤 정점을 향하는 BORN_IN 유출 간선을 가진다. 이 점점에서 name 속성이 "United States"인 Location 유형의 정점에 도달할 때까지 일련의 WITHIN 유출 간선을 따라간다.
2. 같은 person 정점은 LIVES_IN 유출 간선도 가진다. 이 간선과 WITHIN 유출 간선을 따라가면 결국 name 속성이 "Europe" 인 Location 유형의 정점에 도달하게 된다.

#### SQL 의 그래프 즐의 
SQL 을 사용해서도 위의 내용을 질의할 수 있을까 ? 복잡하지만, `재귀 공통 테이블 식(recursive common table expression)`(WITH RECURSIVE문) 을 통해 표현할 수 있다.

```sql
WITH RECURSIVE 
  -- in_usa 는 미국 내 모든 지역의 정점 ID 집합이다.
  in_usa(vertex_id) AS (
    SELECT vertex_id FROM vertices WHERE properties ->> 'name' = 'United States'
    UNION 
    SELECT edges.tail_vertex FROM edges
      JOIN in_usa ON edges.head_vertex = in_usa.vertex_id 
      WHERE edges.label = 'within'
  )
  -- in_europe은 유럽 내 모든 지역의 정점 ID 집합이다.
  in_europe(vertex_id) AS (
    SELECT vertex_id FROM vertices WHERE properties ->> 'name' = 'Europe'
    UNION 
    SELECT edges.tail_vertex FROM edges
      JOIN in_usa ON edges.head_vertex = in_usa.vertex_id 
      WHERE edges.label = 'within'
  )
  -- born_in_usa 는 미국에서 태어난 모든 사람의 정점 ID 집합이다.
  born_in_usa(vertex_id) AS (
    SELECT edges.tail_vertex FROM edges 
      JOIN in_usa ON edges.head_Vertex = in_usa.vertex_id
      WHERE edges.label = 'lives_in'
  )
  -- lives_in_europ 은 유럽에서 태어난 모든 사람의 정점 ID 집합이다.
  lives_in_europe(vertex_id) AS (
    SELECT edges.tail_vertex FROM edges
      JOIN in_europe ON edges.head_vertex = in_europe.vertex_id
      WHERE edges.label = 'lives_in'
  )

SELECT vertices.properties->>'name'
FROM vertices
-- 미국에서 태어나 유럽에서 자란 사람을 찾아 조인한다.
JOIN born_in_usa ON vertices.vertex_id born_in_usa.vertex_id
JOIN lives_in_europe ON vertices.vertex_id = lives_in_europe.vertex_id
```


#### 트리플 저장소와 스파클 
트리플 저장소 모델은 속성 그래프 모델과 거의 동등하다. 트리플 저장소에서는 모든 정보를 주어(subject), 서술어(predicate), 목적어(object) 처럼 매우 간단한 세 부분 구문 형식으로 처리한다. 트리플의 주어는 그래프의 정점과 동등하다. 목적어는 두 가지 중 하나다.

1. 문자열이나 숫자 같은 원시 데이터 값. 이 경우 트리플의 서술어와 목적어는 주어 정점에서 속성의 키, 값과 동등하다.
2. 그래프의 다른 정점. 이 경우 서술어는 그래프의 간선이고 주어는 꼬리 정점이며 목적어는 머리 정점이다.
다음의 예시를 살펴보자

```
@prefix : <urn:example:>.
_:lucy  a  :Person.
_:lucy  :name  "Lucy".
_:lucy  :bornIn  _:idaho .
_:idaho  a  :Location.
_:idaho  :name  "Idaho".
_:idaho  :type  "state".
_:idaho  :within  _:usa.
_:usa  a  :Location.
_:usa  :name  "United States".
_:usa  :type  "country".
_:usa  :within  _:namerica.
_:namerica  a  :Location.
_:namerica  :name  "North America".
_:namerica  :type  "continent".
```

위 예제에서는 그래프의 Node 를 `_:someName` 으로 표현했다. 서술어(:name :bornIn 등) 은 Edge 역할을 수행한다.


### 시멘틱 웹 
웹사이트는 이미 사람이 읽을 수 있는 텍스트와 그림으로 정보를 게시하고 있으니, 컴퓨터가 읽게끔 기계가 판독 가능한 데이터로도 정보를 게시하는 건 어떨까 하는 개념이다. `자원 기술 프레임워크(Resource Description Framework, RDF)` 는 서로 다른 웹사이트가 일관된 형식으로 데이터를 게시하기 위한 방법을 제안한다. (실제로는 2000년대 초반에 과대평가되었고, 지금까지 현실에서 실현된 흔적은 없다.)


#### RDF 데이터 모델
다음의 예시를 살펴보자
```xml
<rdf:RDF xmlns="urn:example" :xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">

  <Location rdf:nodeID="idaho">
    <name>Idaho</name>
    <type>state</type>
    <within>
      <Location rdf:nodeID="usa">
        <name>United States</name>
        <type>country</type>
        <within>
          <Location rdf:nodeId="namerica">
            <name>North America</name>
            <type>content</type>
          </Location>
        </within>
    </within>
  </Location>
  <Person rdf:nodeID="lucy">
    <name>Lucy</name>
    <bornIn rdf:nodeID="idaho" />
  </Person>
```


#### 스파클 질의 언어 
스파클(SPARQL) 은 RDF 데이터 모델을 사용한 트리플 저장소의 질의 언어다.

```
PREFIX : <urn:example:>

SELECT ?personName WHERE {
  ?person :name ?personName.
  ?person :bornIn / :within* / :name "United States".
  ?person :livesIn / :within* / :name "Europe".
}
```


### 초석: 데이터로그 
데이터로그(Datalog) 는 스파클이나 사이퍼보다 훨씬 오래된 언어이다. 데이터로그의 데이터 모델은 트리플 저장소 모델과 유사하지만 조금 더 일반화됐다. (주어, 서술어, 목적어)로 트리플을 작성하는 대신 서술어(주어, 목적어)로 작성한다


### 정리 
역사적으로 데이터를 하나의 큰 트리(계층 모델)로 표현하려고 노력했지만 다대다 관계를 표현하기에는 트리 구조가 적절하지 않았다. 이 문제를 해결하기 위해 관계형 모델이 고안됐다. 최근 개발자들은 관계형 모델에도 적합하지 않은 어플리케이션이 있다는 사실을 발견했다. NoSQL 은 다음과 같은 두 가지 주요 갈래가 있다.

1. `문서 데이터베이스`는 데이터가 문서 자체에 포함돼 있으면서 하나의 문서와 다른 문서 간 관계가 거의 없는 사용 사례를 대상으로 한다.
2. `그래프 데이터베이스`는 문서 데이터베이스와는 정반대로 모든 것이 잠재적으로 관련 있다는 사용 사례를 대상으로 한다.

세 가지 모델(문서, 관계형, 그래프) 모두 현재 널리 사용되고 있으며 각 모델은 각자의 영역에서 훌륭하다.