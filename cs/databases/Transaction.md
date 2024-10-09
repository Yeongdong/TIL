# 목차

1. [트랜잭션, 커밋, 롤백, 트랜잭션 전파](#1-트랜잭션-커밋-롤백-트랜잭션-전파)

- [트랜잭션](#트랜잭션)
- [커밋](#커밋)
- [롤백](#롤백)
- [트랜잭션 전파](#트랜잭션-전파)

2. [ACID](#2-acid)

- [원자성 (Atomicity)](#원자성atomicity)
- [일관성 (Consistency)](#일관성consistency)
- [격리성 (Isolation)](#격리성isolation)
- [지속성 (Durability)](#지속성durability)

3. [격리성](#3-격리성)

- [격리 수준](#격리-수준)

4. [격리 수준에 따른 현상](#4-격리-수준에-따른-현상)

- [팬텀 리드 (Phantom reads)](#팬텀-리드phantom-reads)
- [반복 가능하지 않은 조회 (Non-repeatable Reads)](#반복-가능하지-않은-조회non-repeatable-reads)
- [더티 리드 (Dirty reads)](#더티-리드dirty-reads)

5. [격리 수준](#격리-수준-1)

- [SERIALIZABLE](#serializable)
- [REPEATABLE_READ](#repeatable_read)
- [READ_COMMITED](#read_commited)
- [READ_UNCOMMITED](#read_uncommited)

# 1. 트랜잭션, 커밋, 롤백, 트랜잭션 전파

## 트랜잭션

* 데이터베이스에서 하난의 논리적 기능을 수행하기 위한 작업의 단위(쿼리를 묶는 단위)

## 커밋

* 여러 쿼리가 성공적으로 처리되었다고 확정하는 명령어
* 트랜잭션 단위로 수행되며 변경된 내용이 모두 영구적으로 저장

## 롤백

* 트랜잭션으로 처리한 하나의 묶음 과정을 일어나기 전으로 돌리는 일(취소)

## 트랜잭션 전파

* 트랜잭션을 수행할 때 커넥션 단위로 수행하기 때문에 커넥션 객체를 넘겨서 수행해야 하는데 이를 넘겨서 수행하지 않고 여러 트랜잭션 관련 메서드의 호출을 하나의 트랜잭션에 묶이도록 하는 것

# 2. ACID

## 원자성(Atomicity)

* 트랜잭션과 관련된 일이 모두 수행되었거나 되지 않았거나를 보장

## 일관성(Consistency)

* '허용된 방식'으로만 데이터를 변경해야 하는 것

## 격리성(Isolation)

* 트랜잭션 수행시 서로 끼어들지 못하는 것

## 지속성(Durability)

* 성공적으로 수행된 트랜잭션은 영원히 반영되어야 하는 것

# 3. 격리성

* 트랜잭션이 순차적으로 실행되면 격리성은 높아지지만, 동시성은 너무 낮아져 성능이 좋지 않음
    * 격리성과 동시성은 반비례 관계
* 이러한 단계들은 DB에서 조정할 수 있음

```mysql
set session transaction isolation level read uncommitted;
```

## 격리 수준

* SERIALIZABLE
* REPEATABLE_READ
* READ_COMMITED
* READ_UNCOMMITED

| Isolation Level  | Dirty reads | Non-repeatable reads | Phantoms    |
|------------------|-------------|----------------------|-------------|
| Read Uncommitted | May occur   | May occur            | May occur   |
| Read Committed   | Don't occur | May occur            | May occur   |
| Repeatable Read  | Don't occur | Don't occur          | May occur   |
| Serializable     | Don't occur | Don't occur          | Don't occur |

# 4. 격리 수준에 따른 현상

## 팬텀 리드(Phantom reads)

* 한 트랜잭션 내에서 동일한 쿼리를 2번 이상 보냈을 때 해당 조회 결과가 다른 것
* 유령(Phantom)같이 생겨버렸다고 해서 팬텀 리드
* Thread A에서 select 문을 수행 후, Thread B에서 INSERT 문이 수행되었을 때, Thread A에서 select 문을 수행하니 하나가 더 발생한 모습

## 반복 가능하지 않은 조회(Non-repeatable Reads)

* 한 트랜잭션 내의 **같은 행에 두 번 이상 조회**를 실행했을 때, 같은 결과를 가져오지 않는 것
* 원래 var의 값이 50이었는데, Thead A에서 UPDATE var = 100 을 수행 후, Thead B에서 select문을 수행하니 50이 나옴. Thread A가 커밋을 하고 다시 Thread B에서
  select문을 수행하니 100으로 나옴

## 더티 리드(Dirty reads)

* 하나의 트랜잭션이 다른 트랜잭션의 아직 커밋되지 않은 데이터를 읽는 것
* T1이 업데이트 이후 롤백을 하여 커밋되지 않은 데이터가 됐음에도 불구하고 T2는 이미 커밋이 되었다라고 판단

# 격리 수준

## SERIALIZABLE

* 커밋 완료된 데이터에 대해서만 조회할 수 있음
* 트랜잭션을 순차적으로 진행시키는 것
* 여러 트랜잭션이 동시에 같은 행에 접근할 수 없음

## REPEATABLE_READ

* 커밋 완료된 데이터에 대해서만 조회할 수 있음
* 반복해서 행을 조회하더라도 똑같은 행을 보장하는 단계
* 하나의 트랜잭션이 수정한 행을 다른 트랜잭션이 수정할 수 없도록 막아줌
* **새로운 행을 추가하는 것은 막지 않음**
* 똑같은 범위 쿼리를 실행했을 때 이후 추가된 행이 발견될 수 있음

## READ_COMMITED

* 커밋 완료된 데이터에 대해서만 조회할 수 있음
* 커밋이 되지 않은 정보는 읽지 못함
* 가장 많이 사용되는 격리 수준
    * 팬텀리드, 반복 가능하지 않은 조회가 일어날 수 있음

## READ_UNCOMMITED

* 가장 낮은 격리 수준
* 가장 빠름
* 다른 트랜잭션이 커밋하지 않은 정보를 읽을 수 있음
    * 팬텀리드, 반복 가능하지 않은 조회, 더티리드가 일어날 수 있음