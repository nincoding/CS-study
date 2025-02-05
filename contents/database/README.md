# Database (데이터베이스)

> 작성자 : [박재용](https://github.com/ggjae), [서그림](https://github.com/Seogeurim), [윤가영](https://github.com/yoongoing), [이세명](https://github.com/3people), [장주섭](https://github.com/wntjq68), [정희재](https://github.com/Hee-Jae)

<details>
<summary>Table of Contents</summary>

- [데이터베이스](#데이터베이스)
- [정규화](#정규화)
- [반정규화](#반정규화)
- [인덱스 (Index)](#인덱스-(index))
- [트랜잭션(Transaction)과 교착상태](#트랜잭션(transaction)과-교착상태)
- [NoSQL](#nosql)

</details>

---

## 데이터베이스

### 데이터베이스 용어 정리

#### 긍정적 단어 😃

- **독립성 (Data Independence)**
    - 논리적 독립성 : 응용 프로그램과 데이터베이스를 독립시킴으로써, 데이터의 논리적 구조를 변경시키더라도 응용 프로그램은 변경되지 않는다.
    - 물리적 독립성 : 응용 프로그램과 보조기억장치 같은 물리적 장치를 독립시킴으로써, 데이터베이스 시스템의 성능 향상을 위해 새로운 디스크를 도입하더라도 응용 프로그램에는 영향을 주지 않고 데이터의 물리적 구조만을 변경한다.
- **무결성 (Data Integrity)** : 삽입, 삭제, 갱신 등의 연산 후에도 데이터베이스에 저장된 데이터가 정해진 제약 조건을 항상 만족해야 한다.
- **일관성 (Data Consistency)** : 데이터베이스에 저장된 데이터와 특정 질의에 대한 응답이 변함 없이 일정해야 한다.

#### 부정적 단어 🙁

- **종속성 (Data Dependency)** : (⇔ 독립성) 응용 프로그램의 구조가 데이터의 구조에 영향을 받는다.
- **중복성 (Data Redundancy)** : 같은 데이터가 중복되어 저장되는 것으로, 데이터 수정/삭제 시 연결된 모든 데이터를 수정/삭제해줘야 하는 문제점이 있다.
- **비일관성 (Data Inconsistency)** : 동일한 데이터의 여러 사본이 서로 다른 값을 보유하고 있는 상태로, 데이터 중복성에 이어서 발생할 수 있다.

### 데이터베이스 시스템의 목적

데이터베이스 시스템이 등장하기 전까지는 운영체제의 의해 지원되는 파일 시스템을 통해 데이터를 관리하였다. 이 때 여러가지 문제점이 존재하는데, 데이터베이스 시스템의 목적은 이 문제점을 해결하기 위하는 데 있다. **파일 시스템을 사용했을 때의 주요 문제점**은 다음과 같다.

- 데이터의 중복과 비일관성
- 데이터 접근 시 필요한 데이터를 편리하고 효율적으로 검색하기 힘들다.
- 데이터가 여러 파일에 흩어져 있고 파일 형식이 서로 다를 수 있다.
- 데이터 동시 접근 시 데이터가 잘못 업데이트될 수 있다.
- 무결성 문제
- 보안 문제

데이터베이스는 여러 사람들이 공유하여 사용할 목적으로 체계화해 통합, 관리하는 데이터의 집합을 말한다. DBMS를 통해 요구를 처리하고, SQL을 사용해 데이터에 접근한다. **데이터베이스**는 다음의 **장점**을 가진다.

- 데이터의 논리적, 물리적 독립성, 일관성, 무결성, 보안성 보장
- 데이터 중복 최소화
- 저장된 자료를 공동으로 이용할 수 있다.
- 데이터를 통합하여 관리할 수 있다.
- 데이터의 실시간 처리가 가능하다.

한편 **단점**도 존재한다.

- 전산화 비용이 증가한다.
- 대용량 디스크로의 집중적인 Access로 과부하(Overhead)가 발생할 수 있다.

### 관계형 데이터베이스 시스템 (RDBMS)

RDBMS는 테이블 기반(Table based) DBMS로, 테이블들의 집합으로 데이터들의 관계를 표현한다.

- 데이터를 테이블 단위로 관리
- 중복 데이터 최소화 (정규화)
- 여러 테이블에 분산되어 있는 데이터를 검색 시 테이블 간의 관계(join)를 이용해 필요한 데이터를 검색

### 데이터베이스 언어

#### DDL (Data Definition Language) : 데이터 정의어

- CREATE, ALTER, DROP, RENAME
- 데이터베이스 객체(table, view, index...)의 구조를 정의 (생성, 변경, 제거)

#### DML (Data Manipulation Language) : 데이터 조작어

- INSERT, SELECT, UPDATE, DELETE
- 데이터베이스 테이블의 레코드를 CRUD (Create, Read, Update, Delete)

#### DCL (Data Control Language) : 데이터 제어어

- GRANT, REVOKE
- 데이터베이스와 그 구조에 대한 접근 권한을 제공하거나 제거

#### TCL (Transaction Control Language) : 트랜잭션 제어어

- COMMIT, ROLLBACK, Savepoint
- DML 명령문으로 수행한 변경을 관리 (트랜잭션 관리)

---

## 정규화

정규화란 **함수의 종속성 이론**을 통해 데이터의 중복성을 최소화하고 일관성 등을 보장하여 데이터베이스의 품질과 성능을 향상키시는 과정이다. 정규화 수준이 높을수록 유연한 데이터 구축이 가능하고 데이터의 정확성이 높아지는 반면, 물리적 접근이 복잡하고 너무 많은 조인으로 인해 조회 성능이 저하된다는 특징이 있다

### 정규화의 목적

- 데이터 구조의 안정성과 무결성을 유지한다
- 데이터 모형의 단순화가 가능하다
- 효과적인 검색 알고리즘을 생성할 수 있다
- 데이터 중복을 배제하여 `이상(Anomaly)` 발생을 방지하고 저장 공간을 최소화한다

### 이상(Anomaly)의 개념 및 종류

정규화를 거치지 않으면 **데이터베이스 내에 데이터들이 불필요하게 중복**되어 릴레이션 조작 시 문제가 생기는데, 이를 이상이라고 하며
삽입 이상, 삭제 이상, 갱신 이상이 있다

- **삽입 이상** (Insertion Anomaly) : 데이터를 삽입할 때 원하지 않은 값들도 함께 삽입되는 현상
- **갱신 이상** (Update Anomaly) : 데이터를 수정할 때 일부 튜플의 정보만 갱신되어 정보에 모순이 생기는 현상
- **삭제 이상** (Deletion Anomaly) : 데이터를 삭제할 때 의도와는 상관 없는 값들도 함께 삭제되는 현상

### 정규화 과정

#### 1NF(제 1 정규형)

릴레이션에 속한 모든 도메인이 원자값만으로 되어있는 정규형이다

#### 2NF(제 2 정규형)

1NF를 만족하고, 부분 함수적 종속을 제거하여 기본키가 아닌 모든 속성이 기본키에 대하여 완전 함수적 종속을 만족하는 정규형이다

```bash
완전 함수적 종속 : 만약 (속성 A, 속성 B) -> 속성 C 일때, A->C, B->C 모두 성립될때 완전 함수적 종속이라 한다
부분 함수적 종속 : 만약 (속성 A, 속성 B) -> 속성 C 일때, A->C, B->C 중 하나만 성립될때(모두 성립 x) 부분 함수적 종속이라 한다
```

#### 3NF(제 3 정규형)

2NF를 만족하고, 이행적 함수 종속을 제거하는 정규형이다

```bash
이행적 종속 : A -> B, B -> C 의 종속관계에서 A -> C를 만족하는 관계를 의미한다
```

#### BCNF(Boyce-Codd 정규형)

결정자가 모두 후보키인 정규형이다.(후보키가 아닌 결정자를 제거하는 정규형이다)
</br>

[BCNF의 제약 조건]
- 키가 아닌 모든 속성은 각 키에 대하여 완전 종속해야 한다
- 키가 아닌 모든 속성은 그 자신이 부분적으로 들어가 있지 않은 모든 키에 대해 완전 종속해야 한다
- 어떤 속성도 키가 아닌 속성에 대해서는 완전 종속 할 수 없다

```bash
결정자 : 다른 속성을 고유하게 결정하는 하나 이상의 속성 (속성 간의 종속성을 규명할 때 기준이 되는 값)
종속자 : 결정자의 값에 의해 정해지는 값
후보키 : 테이블에서 각 행을 유일하게 식별할 수 있는 최소한의 속성들의 집합  
```

#### 4NF(제 4 정규형)

다치 종속을 제거하는 정규형이다.

```bash
다치 종속 : 속성 A -> (속성 B, 속성 C) 일때, A->B를 만족하고, **B와 C가 무관**할때 B는 A에 다치종속 관계라고 하며, A->>B 라고 한다. 

다치종속을 제거하지 않으면 A->>B 상황에서 C값이 중복될수 있다.
예를들어,

(회원번호)-> (이름, 주문번호) 인 테이블에서
(회원번호 ->> 주문번호) 일때,

흐쟈미란 고객이 책을 두번 주문하면 흐쟈미 이름이 불필요하게 두번 중복된다.
이를 해결하기 위해서는 (회원번호->이름), (회원번호->주문번호)로 쪼개주는것이 제 4정규형이다.
```

#### 5NF (제 5 정규형)

릴레이션 R의 모든 조인종속이 R의 후보키를 통해서만 성립되는 정규형이다.

```bash
한 테이블을 분해했다가 분해된 결과들을 다시 조인하면 당연히 원래의 테이블로 복원된다고 기대하지만 그렇지 못한 경우가 있다.
다시 조인하면 예상하지 못했던 튜플들이 생성되는 경우가 발생한다.

조인 종속 :  테이블을 분해한 결과를 다시 조인했을 때 원래의 테이블과 동일하게 복원되는 제약조건이다.
```

<details>
<summary>5NF를 실시하는 이유 예시로 보기</summary>  
<p>

> "다시 조인하면 예상하지 못했던 튜플들이 생성되는 경우"

릴레이션 R이 다음과 같을때,

|A|B|C|
|---|---|---|
|s1|p1|c2|
|s1|p2|c1|
|s2|p1|c1|
|s1|p1|c1|

[A,B], [B,C], [A,C]로 쪼개봅시다.

|A|B|
|---|---|
|s1|p1|
|s1|p2|
|s2|p1|

|B|C|
|---|---|
|p1|c2|
|p2|c1|
|p1|c1|

|A|C|
|---|---|
|s1|c2|
|s1|c1|
|s2|c1|

다시 합치면

|A|B|C|
|---|---|---|
|s1|p1|c2|
|s1|p2|c1|
|s2|p1|c1|
|s1|p1|c1|
|**s2**|**p1**|**c2**| 

===> 마지막 튜플에서 이상값 발견!!!!

이런 상황을 방지하기 위해 제 5정규형을 시행합니다.

</p>
</details>

---

## 반정규화

### 반정규화 개념

반정규화란 시스템의 성능 향상, 개발 및 운영의 편의성 등을 위해 정규화된 데이터 모델을 통합, 중복, 분리하는 과정으로 의도적으로 정규화 원칙을 위배하는 행위이다.

* 반정규화를 수행하면 시스템의 성능이 향상되고 관리 효율성은 증가하지만, 데이터의 일관성과 정합성은 저하될 수 있다.
* 과도한 반정규화는 성능을 저하시킨다.
* 반정규화를 위해서는 사전에 **데이터의 일관성과 무결성을 우선으로 할지, 데이터베이스의 성능과 단순화를 우선으로 할지** 생각한다.
* 반정규화 방법에는 테이블 통합, 테이블 분할, 중복 테이블 추가, 중복 속성 추가 등이 있다.

### 테이블 통합

두 개의 테이블이 조인되는 경우가 많아 하나의 테이블로 합쳐 사용하는 것이 성능 향상에 도움이 될 경우 수행한다.
`두 개의 테이블에서 발생하는 프로세스가 동일하게 자주 처리되는 경우`, `두 개의 테이블을 이용하여 항상 조회를 수행하는 경우` 테이블 통합을 고려한다.

#### 테이블 통합 시 고려사항

* 검색은 간편하지만, 레코드 증가로 인해 처리량이 증가한다.
* 테이블 통합으로 인해 입력, 수정, 삭제 규칙이 복잡해질 수 있다.

### 테이블 분할

테이블을 수직 또는 수평으로 분할하는 것이다.

1. **수평 분할** : 레코드를 기준으로 테이블을 분할한다.
2. **수직 분할** : 하나의 테이블에 속성이 너무 많을 경우 속성을 기준으로 테이블을 분할한다.
    * `갱신위주의 속성 분할` : 데이터 갱신 시 레코드 잠금으로 인해 다른 작업을 수행 할 수 없으므로 갱신이 자주 일어나는 속성들을 수직 분할하여 사용한다.
    * `자주 조회되는 속성 분할` : 테이블에서 자주 조회되는 속성이 극히 일부일 경우 자주 사용되는 속성들을 수직분할하여 사용한다.
    * `크기가 큰 속성 분할` : 이미지, 2GB 이상 저장될 수 있는 텍스트 형식으로 된 속성들을 수직분할하여 사용한다.
    * `보안을 적용해야 하는 속성 분할` : 테이블 내의 특정 속성에 대해 보안을 적용할 수 없으므로 보안을 적용해야 하는 속성들을 수직분할 하여 사용한다.

#### 테이블 분할 시 고려사항

* 기본키의 유일성 관리가 어려워진다.
* 데이터 양이 적거나 사용 빈도가 낮은 경우 테이블 분할이 필요한 지를 고려해야한다.
* 분할된 테이블로 인해 수행 속도가 느려질 수 있다.
* 검색에 중점을 두어 테이블 분할 여부를 결정해야 한다.

### 중복 테이블 추가

여러 테이블에서 데이터를 추출해서 사용해야 하거나 다른 서버에 저장된 테이블을 이용해야 하는 경우 중복 테이블을 추가하여 작업의 효율성을 향상시킬 수 있다.

1. 중복 테이블을 추가하는 경우
    * 정규화로 인해 수행 속도가 느려지는 경우
    * 많은 범위의 데이터를 자주 처리해야하는 경우
    * 특정 범위의 데이터만 자주 처리해야 하는 경우
    * 처리 범위를 줄이지 않고는 수행 속도를 개선할 수 없는 경우

2. 중복 테이블을 추가하는 방법
    * `집계 테이블의 추가` : 집계 데이터를 위한 테이블을 생성하고, 각 원본 테이블에 트리거를 설정하여 사용하는 것으로, 트리거의 오버헤드에 유의한다.
    * `진행 테이블의 추가` : 이력 관리 등의 목적으로 추가하는 테이블로, 적절한 데이터 양의 유지와 활용도를 높이기 위해 기본키를 적절히 설정한다.
    * `특정 부분만을 포함하는 테이블의 추가` : 데이터가 많은 테이블의 특정 부분만을 사용하는 경우 해당 부분만으로 새로운 테이블을 생성한다.

### 중복 속성 추가

조인해서 데이터를 처리할 때 데이터를 조회하는 경로를 단축하기 위해 자주 사용하는 속성을 하나 더 추가하는 것이다.

> 중복 속성을 추가하면 데이터의 무결성 확보가 어렵고, 디스크 공간이 추가로 필요하다.

1. 중복 속성을 추가하는 경우
    * 조인이 자주 발생하는 속성인 경우
    * 접근 경로가 복잡한 속성인 경우
    * 액세스 조건으로 자주 사용되는 속성인 경우
    * 기본키의 형태가 적절하지 않거나 여러 개의 속성으로 구성된 경우

2. 중복 속성 추가 시 고려 사항
    * 테이블 중복과 속성의 중복을 고려한다.
    * 데이터 일관성 및 무결성에 유의해야 한다.
    * 저장공간의 지나친 낭비를 고려한다.

---

## 인덱스 (Index)

아래의 자료에서 자세한 설명을 볼 수 있다.

- 작성자 윤가영 | [데이터베이스와 Index](./materials/윤가영_database_&Index.pdf)

---

## 트랜잭션(Transaction)과 교착상태

트랜잭션이란? 데이터베이스의 상태를 변화시키기 위해 수행되는 작업의 논리적 단위이다.

### ACID

- Atomicity(원자성) : 트랜잭션에 해당하는 작업 내용이 (모두 성공했을 시) 모두 반영되거나, (하나라도 실패했을 시) 모두 반영되지 않아야 한다.
- Consistency(일관성) : 트랜잭션 처리 결과는 항상 데이터의 일관성을 보장해야 한다.
- Isolation(고립성) : 둘 이상의 트랜잭션이 동시에 실행되고 있을 때, 각 트랜잭션은 서로 간섭 없이 독립적으로 수행되어야 한다.
- Durability(지속성) : 트랜잭션이 성공적으로 완료된다면, 그 결과가 데이터베이스에 영구적으로 반영되어야 한다.

#### 주의사항

Isolation(고립성)을 보장하기 위해 무차별적으로 Lock을 걸다보면 대기시간이 매우 길어지므로 트랜잭션은 최소한으로 사용해야한다.

### 트랜잭션 상태

![image](https://user-images.githubusercontent.com/22047374/125165837-a951ee00-e1d3-11eb-9b0b-486cc5eff6b2.png)

- Active : 트랜잭션이 실행중인 상태(SQL 실행)
- Parital Commit : 트랜잭션의 마지막 연산까지 실행했지만, commit 연산이 실행되기 직전의 상태
- Commited : 트랜잭션이 성공적으로 종료되고 commit 연산까지 실행 완료된 상태
- Failed : 트랜잭션 실행에 오류가 발생한 상태
- Aborted: 트랜잭션이 비정상적으로 종료되어 Rollback 연산을 수행한 상태

1. Commit  :  데이터베이스 내의 연산이 성공적으로 종료되어 연산에 의한 수정 내용을 지속적으로 유지하기 위한 명령어이다.
2. Rollback  : 데이터베이스 내의 연산이 비정상적으로 종료되거나 정상적으로 수행이 되었다 하더라도 수행되기 이전의 상태로 되돌리기 위해 연산 내용을 취소할 때 사용하는 명령어이다.

### 트랜잭션에서 발생할 수 있는 문제들

#### Dirty Read Problem

한 트랜잭션 진행 중에 변경한 값을 다른 트랜잭션에서 읽을 때 발생한다.
커밋되지 않은 상태의 트랜잭션을 다른 트랜잭션에서 읽을 수 있을 때 발생하는 문제이다.

#### Non-repeatable Read Problem

한 트랜잭션에서 같은 값을 두 번 이상 읽었을 때 그 값이 다른 경우를 말한다.
한 트랜잭션 도중 다른 트랜잭션이 커밋되면 발생할 수 있는 문제이다.

#### Phantom Read Problem

한 트랜잭션에서 같은 쿼리문을 두 번 이상 실행했을 때 새로운 데이터가 조회되는 경우를 말한다.
A 트랜잭션 도중 B 트랜잭션에서 update 쿼리를 수행하고 커밋하더라도 A 트랜잭션에서 그 결과는 볼 수 없지만, 
A 트랜잭션 도중 B 트랜잭션에서 insert 쿼리를 수행할 경우 A 트랜잭션에서 처음에 안 보였던 새로운 데이터가 조회될 수 있다.

---

## NoSQL

아래의 자료에서 자세한 설명을 볼 수 있다.

- 작성자 이세명 | [NoSQL](./materials/이세명_database_NoSQL.pdf)

---

## 질의응답

_질문에 대한 답을 말해보며 공부한 내용을 점검할 수 있으며, 클릭하면 답변 내용을 확인할 수 있습니다._

<!-- 데이터베이스 개요 -->

<details>
<summary> 데이터베이스는 왜 사용하나요? </summary>  
<p>

기존에는 파일 시스템을 사용해 데이터를 관리했습니다. 그러나 파일 시스템에는 '데이터 중복', '데이터 불일치', '데이터 보안문제' 등 다양한 문제점을 가지고 있습니다. 데이터베이스를 사용하면 이러한 문제들을 어느 정도 해소할 수 있습니다.

</p>
</details>

<details>
<summary> 데이터베이스의 성능을 좋아지게 하려면 어떤 방법들이 있을까요? </summary>  
<p>

DB설계 튜닝
- 테이블 분해, 통합
- 테이블 정규화
- 적절한 데이터 타입 설정

DBMS 튜닝
- I/O 최소화
- Commit 주기 조정
- Middleware 기능과 연동

SQL 튜닝
- Join 방식 조정
- Index 활용

</p>
</details>

---

<!-- 정규화 -->

<details>
<summary>관계형 데이터베이스 설계의 목표는 무엇일까요?</summary>  
<p>

모든 각각의 릴레이션이 3NF와 BCNF를 이행하도록 하는 것

</p>
</details>

<details>
<summary> 정규화는 왜 할까요? </summary>  
<p>

데이터 생성, 수정, 삭제시 데이터 중복, 불일치 등 갱신 이상을 없애기 위해서 합니다. 데이터베이스의 성능을 높이기 위한 이유도 있습니다. (테이블을 분해해서 중복되는 열을 제거하기 때문에 메모리를 절약할수 있음)

</p>
</details>

<details>
<summary> 제1 정규형은 데이터베이스 정규화에서 사용하는 최소 정규형입니다. 제1 정규형에서 어떤 정규화 과정을 진행하는지 예시를 들어 설명할 수 있나요?</summary>  
<p>

ex1) 핸드폰 번호, 전화 번호를 동일한 칼럼에 중복되어 받는 경우(: 555-403-1659, 02-2651-4656)를 제거합니다.  
ex2) ex1에 대한 중복을 제거하기 위한 여러개의 전화번호 행을 두는 것(동일한 도메인과 의미를 두는 것은 최소 정규형에 위배)을 제거합니다.  
위와 같은 정규화 과정을 거쳐 제1 정규형이 됩니다.

</p>
</details>
   
<details>
<summary>제3 정규형을 '추이 종속' or '함수적 이행 종속' 단어를 사용하여 설명할 수 있나요?</summary>  
<p>

테이블이 제2 정규형을 만족하며 모든 속성이 기본 키에만 의존하여 다른 키에 의존하지 않는 정규형을 제3 정규형이라고 합니다.  
만약 의존한다면 함수적 이행 종속이 일어나서 A->B->C 순환적으로 모든 클래스가 의존하게 되는 문제가 일어날 수 있습니다.

</p>
</details>

<details>
<summary><strong>개인정보</strong>에 관한 데이터베이스에서 수정 이상이 일어나 select로 정확한 정보 파악이 되지 않아 정규화를 진행했습니다.  
하지만 이전보다 훨씬 느려졌다고 가정해봅시다. 지원자는 반정규화를 진행할것입니까?
</summary>
<p>

반정규화는 진행하면 안됩니다. 개인정보에 관한 데이터베이스로서 데이터의 무결성과 보안이 제일 중요합니다.   
만약 반정규화로 인해 '삽입, 삭제, 수정 이상'이 발생하는 경우에는 데이터의 무결성이 깨질 수 있고 더군다나 개인정보라서 데이터의 무결성이 깨지면 복원에 큰 어려움이 있습니다. 따라서 반정규화가 아닌 데이터베이스 튜닝, 재구성이 필요해 보입니다.
   
</p>
</details>

<details>
<summary> 정규화 작업 이후, 알맞게 정규화를 했는지 검증하고 싶을땐 어떻게 할까요? </summary>  
<p>

테이블 분해 이후 무손실 조인을 검사해봅니다.  
테이블 분해 이후 함수의 종속성이 보존되는지 확인합니다.

</p>
</details>

---

<!-- Index -->

<details>
<summary>Index가 무엇인지 설명해주세요.</summary>  
<p>

데이터 레코드를 빠르게 접근하기 위해 <키 값, 포인터> 쌍으로 구성되는 데이터 구조.

</p>
</details>

<details>
<summary>Index의 특징에 대해 아는대로 말해보세요.</summary>  
<p>

1. 인덱스는 데이터가 저장된 물리적 구조와 밀접한 관계에 있다.
2. 인덱스는 레코드가 저장된 물리적 구조에 접근방법을 제공한다.
3. 파일의 레코드에 대한 액세스를 빠르게 수행한다.
4. 레코드의 삽입과 삭제가 수시로 일어나는 경우, 인덱스의 개수를 최소로 하는것이 효율적이다.
5. 인덱스가 없으면 특정한 값을 찾기 위해 모든 데이터 페이지를 확인하는 TABLE SCAN이 발생한다.
6. 기본키를 위한 인덱스를 기본 인덱스라 하고, 기본 인덱스가 아닌 인덱스를 보조 인덱스라 한다.
7. 레코드의 물리적 순서가 인덱스의 엔트리 순서와 일치하게 유지되도록 구성되는 인덱스를 클러스터드 인덱스라고 한다.

</p>
</details>

<details>
<summary>Index 테이블 선정 기준에 대해 말해주세요</summary>  
<p>

랜덤 액세스가 빈번하거나, 다른 테이블과 순차적 조인이 발생되는 테이블 혹은 트겆ㅇ 범위나 순서로 데이터 조회가 필요한 테이블에 인덱스를 적용한다.

</p>
</details>

<details>
<summary>Index를 구현하는 자료구조에 대해서 말씀해주세요</summary>  
<p>

Index는 대표적으로 `해시 테이블`, `B+ 트리` 등으로 구현할 수 있다.  

1. 해시 테이블

해시테이블로 구현할 경우 검색에 대하여 시간복잡도 `O(1)`을 갖는다. 즉, 빠른 검색이 가능하다.  
하지만 해시는 등호(=) 연산자에 적합하고 실제 데이터베이스에서 자주 일어나는 부등호(<, >) 연산에는 적합하지 않다.
> 해시가 눈사태 효과(avalanche effect) 를 갖고 있기 때문이다. 


2. B+ 트리

![스크린샷 2021-05-16 오후 5 32 45](https://user-images.githubusercontent.com/22493971/118390940-bc7d8d00-b66c-11eb-9210-fc42db7de0da.png)  
_출처: https://blog.jcole.us/2013/01/10/btree-index-structures-in-innodb/_

리프 노드에만 데이터를 담아둠으로 더 많은 key 값을 저장할 수 있는 구조이다.  
나머지 노드들은 원하는 리프로 갈 수 있도록 안내하는 인덱스만 제공한다.

같은 레벨의 노드들은 `이중 연결리스트`로 연결되어 있기 때문에 부등호를 사용한 `순차검색`에 최적화 되어있다. 
원하는 데이터를 얻기 위해 리프노드까지 가야만한다는 단점이 있지만, 해시 인덱스에 비하여 인덱싱에 더 알맞은 자료구조로 여겨진다.

검색에 있어 시간복잡도 `O(log2 n)` 을 갖는다.

</p>
</details>

<details>
<summary>MULTI BLOCK READ가 무엇이며, 인덱스 테이블 선정기준과 연관지어 설명해보세요.</summary>  
<p>

테이블 액세스시 메모리에 한번에 읽어 들일 수 있는 블록의 수를 MULTI BLOCK READ라고 하며, MULTI BLOCK READ수에 따라 판단하여 인덱스를 사용할지 정한다. 예를들어 MULTI BLOCK READ가 16이면, 테이블의 크기가 16블록 이상일 경우 인덱스가 필요하다.

</p>
</details>

<details>
<summary>인덱스 컬럼 선정 기준에 대해 아는대로 말해보세요.</summary>  
<p>

가능한 한 수정이 빈번하지 않고, ORDER BY/ GROUP BY/ UNION이 빈번한 컬럼을 선정한다. 혹은 분포도가 10% ~ 15% 이내인 컬럼을 선정한다.

</p>
</details>

<details>
<summary>인덱스 설계시, 인덱스와 테이블 데이터의 저장공간이 분리되도록 설계하는 이유에 대해 설명하시오.</summary>  
<p>

데이터 저장시 인덱스의 영향을 받지 않아 빠르기 때문에 저장공간을 분리하여 설계한다.

</p>
</details>

<details>
<summary>비트맵 인덱스의 장점은 무엇인가요?</summary>  
<p>

데이터가 Bit로 구성되어 있어 효율적인 논리연산이 가능하고 저장공간이 작다.

</p>
</details>


---

<!-- NoSQL -->

<details>
<summary>NoSQL의 장점은 무엇인가?</summary>  
<p>

JOIN 처리가 없기 때문에 스케일 아웃을 통한 노드확장이 용이하다. 또한 가변적인 데이터 구조로 데이터를 저장할 수 있어 유연성이 높다.

</p>
</details>

<details>
<summary>어떤 상황에서 NoSQL을 사용하는것이 적합한가?</summary>  
<p>

비정형 데이터 혹은 많은 양의 로그를 수집해야할 때 적합하다.

</p>
</details>

<details>
<summary>NoSQL에서 정규화가 필요할까?</summary>  
<p>

NoSQL에서는 정규화 과정이 필요하지 않다.  
RDB와 달리 NoSQL에서는 `쿼리의 효율성`을 극대화하기 위해 오히려 비정규화한다.  

정규화는 중복을 줄이지만 그만큼 다수의 `JOIN` 을 요구하는 등 성능에 악영향을 미칠수도 있다.  
NoSQL에서는 의도적으로 중복을 허용하여 성능적인 이점을 얻는다.

</p>
</details>

---

<!-- Transaction -->

<details>
<summary>트랜잭션이란 무엇이며 왜 사용해야 하는지 말씀해주세요.</summary>  
<p>

- 키워드 1: 완전성을 보장해 주는 것
- 키워드 2: 작업의 논리적 단위

트랜잭션이란 데이터베이스의 상태 변화에 대한 작업의 논리적 단위를 말합니다. 사용 이유는 데이터의 완전성을 보장하기 위해서입니다. 트랜잭션을 통해 여러 쿼리 중 하나라도 실패했을 때 데이터베이스의 상태를 일관성을 유지한 상태로 원상복귀시킬 수 있고, 이를 통해 오류가 발생하더라도 언제나 데이터의 상태를 신뢰할 수 있습니다.

</p>
</details>

<details>
<summary>트랜잭션은 ACID라는 4가지 특성을 만족해야 합니다. 이 4가지 특성에 대해 자세히 말씀해주세요.</summary>  
<p>

- A : 원자성 (Atomicity) | 트랜잭션에 해당하는 작업 내용이 (모두 성공했을 시) 모두 반영되거나, (하나라도 실패했을 시) 모두 반영되지 않아야 한다.
- C : 일관성 (Consistency) | 트랜잭션 처리 결과는 항상 데이터의 일관성을 보장해야 한다.
- I : 고립성 (Isolation) | 둘 이상의 트랜잭션이 동시에 실행되고 있을 때, 각 트랜잭션은 서로 간섭 없이 독립적으로 수행되어야 한다.
- D : 지속성 (Durability) | 트랜잭션이 성공적으로 완료된다면, 그 결과가 데이터베이스에 영구적으로 반영되어야 한다.

</p>
</details>

<details>
<summary>A 계좌에서 B계좌로 일정 금액을 이체하는 작업에 대해 생각해봅시다. 이 때 트랜잭션은 어떻게 정의할 수 있을까요?</summary>  
<p>

(예시) A 계좌의 잔액 확인 SELECT문 + A 계좌의 금액 차감 UPDATE문 + B 계좌의 금액 추가 UPDATE문으로 정의할 수 있습니다.

</p>
</details>

<details>
<summary>(위 질문과 연결) 방금 말씀해주신 쿼리문에 대해서 Commit 연산과 Rollback 연산이 일어나는 경우는 각각 어떤 상황일까요?</summary>  
<p>

Commit 연산은 한 트랜잭션 단위에 포함된 모든 쿼리문이 성공적으로 완료되었을 경우 수행됩니다. 하나의 트랜잭션이 성공적으로 끝났고, 데이터베이스가 일관성이 있는 상태라는 것을 알려주기 위해 사용되는 연산입니다.
Rollback 연산은 트랜잭션에 포함된 쿼리 중 하나라도 실패하면 수행되며 모든 쿼리문을 취소하고 이전 상태로 돌려놓습니다. Rollback 연산을 통해 트랜잭션 처리가 실패하여 데이터베이스의 일관성을 깨뜨렸을 때, 트랜잭션의 원자성을 구현할 수 있습니다.

</p>
</details>

<details>
<summary>트랜잭션의 Commit 연산에 대해서 트랜잭션의 상태를 통해 설명해주세요.</summary>  
<p>

트랜잭션이 시작되면 활동(Active) 상태가 됩니다. 그리고 트랜잭션에 포함된 모든 연산이 실행되면 부분 완료(Partially Commited) 상태가 됩니다. 그리고 Commit 연산을 실행한 후, 완료(Commited) 상태가 되어 트랜잭션이 성공적으로 종료되게 됩니다.

</p>
</details>

<details>
<summary>트랜잭션의 Rollback 연산에 대해서 트랜잭션의 상태를 통해 설명해주세요.</summary>  
<p>

트랜잭션이 시작되면 활동(Active) 상태가 됩니다. 그리고 트랜잭션 중 오류가 발생하여 중단되면 실패(Failed) 상태가 됩니다. 그리고 Rollback 연산을 수행한 후, 철회(Aborted) 상태가 되어 트랜잭션이 종료되고 트랜잭션 실행 이전 데이터 상태로 돌아가게 됩니다.

</p>
</details>

<details>
<summary>Partial Commited 상태에서 Commited 상태가 되는 과정에 대해 자세히 설명해주세요.</summary>  
<p>

Commit 요청이 들어오면 Partial Commited 상태가 되는데, 이 때 Commit 연산을 문제 없이 수행하면 Commited 상태가 됩니다. 하지만 Commit 연산 수행 중 오류가 발생하면 Failed 상태로 돌아가게 됩니다.

</p>
</details>

<details>
<summary>트랜잭션 격리 수준(transaction isolation level)이란 무엇이며, 왜 필요한가요?</summary>  
<p>

트랜잭션에서 일관성이 없는 데이터를 허용하도록 하는 수준을 말합니다. 이것이 필요한 이유는 효율적인 Locking 방법이 필요하기 때문입니다. Locking 범위를 높인다면 동시에 수행되는 많은 트랜잭션들을 순차적으로 처리할 것이고, 이는 DB의 성능을 떨어뜨리게 됩니다. 반면 범위를 줄인다면 응답성은 높아지겠지만 잘못된 값이 처리될 수 있습니다. 따라서 트랜잭션 격리 수준을 두어 Locking 수준을 관리합니다.

</p>
</details>

<details>
<summary>Isolation level의 종류 중 한 가지를 설명해주시고, 이 때 발생할 수 있는 현상도 함께 설명해주세요.</summary>  
<p>

- Read Uncommited : Dirty Read, Non-Repeatable Read, Phantom Read
- Read Committed : Non-Repeatable Read, Phantom Read
- Repeatable Read : Phantom Read
- Serializable : 완벽한 일관성 보장

</p>
</details>

---

<details>
<summary>
트랜잭션을 병행 제어없이 병행으로 DB에 동시에 접근 할 경우 발생하는 문제점이 무엇인가요?</summary>  
<p>

1. 갱신 분실 : 같은 데이터를 공유하여 갱신 할 때 갱신 결과의 일부가 사라짐

2. 모순성 : 동시에 같은 데이터를 갱신할 때, 데이터의 상호 불일치 발생

3. 연쇄 복귀 : 트랜잭션 하나에 문제가 발생하여 롤백시 나머지 트랜잭션도 다같이 롤백됨

</p>
</details>

<details>
<summary>
교착상태가 일어나기위한 네가지 조건을 설명해주세요</summary>  
<p>

1. Mutual Exclusion : 자원을 공유하지 않음, 자원을 공유한다면 서로 기다릴 필요가 없음.

2. Hold & Wait : 자원을 소유하고 있으면서 다른 자원도 기다리는 상태

3. No Preemption : 자원을 한번 얻으면 완전 종료까지 자원을 놓지 못함, (상대방이 가로채갈 수 없음)

4. Circular Wait : 원형의 형태로 자원을 기다림
![circularwait](https://user-images.githubusercontent.com/22339356/119250905-3ae2ad80-bbde-11eb-9031-64cfeb36bc43.jpeg)

</p>
</details>

<details>
<summary>
Deadlock Prevention 과 Avoidance 의 차이는 무엇인가요?</summary>  
<p>

1. Prevention : 네가지 조건 중 한가지를 막아 Deadlock이 일어나지 않게 하는 것(사전 차단)

2. Avoidance : Deadlock의 가능성이 없는 경우에만 (safe state) 에만 자원을 할당

+그렇다면 Unsafe state == Deadlock 인가?
NO, Unsafe state는 Deadlock이 일어 날 수 도 있는 상태를 의미함

</p>
</details>

<details>
<summary>
Deadlock Prevention , Avoidance 그리고 Ignorance 세가지 방법중에 실제 일반적인 OS에서 가장 많이 사용하는 방식은 무엇이라 생각하시나요?</summary>  
<p>

1. Ignorance

    이유: Deadlock Prevention, Avoidance를 위해서는 많은 비용이 필요함. 따라서 일반적으로 사용하는 OS에서는 Deadlock을 사전에 차단하지 않고 Deadlock발생 시 수동으로 해소시키는 방법을 사용.

2. 로켓 발사나 비행기 운전 시스템과 같이 Deadlock 절대 걸려서는 안되는 정밀하고 중요한 시스템 OS에서만 Prevention이나 Avoidance를 사용함


</p>
</details>

---

## Reference

> - Database System Concepts - 6th edition
> - 시나공 정보처리기사 필기

