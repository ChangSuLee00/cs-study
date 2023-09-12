# Isolation과 연관된 이상 현상의 종류

데이터베이스는 고립성과 관련되어 다양한 오류가 발생한다.

(예시 SALES 테이블)

| NAME | QNT | PRICE |
| --- | --- | --- |
| PR 1 | 10 | 1000 |
| PR 2 | 20 | 9000 |

## Dirty reads

하나의 트랜잭션이 다른 트랜잭션에 의해 커밋 되지 않은 데이터를 읽는 것.

```sql
TR1 - SELECT QNT*PRICE FROM SALES // 10000, 18000

TR2 - UPDATE SALES SET QNT = QNT + 5 WHERE NAME = PR1

TR1 - SELECT QNT*PRICE FROM SALES // 15000, 18000

TR2 Rollback
```

Commit조차 되지 않은 데이터를 읽어 오기 때문에 Inconsistance한 정보를 얻는 문제가 발생한다.

## Non-repeatable reads

트랜잭션이 읽은 값을 다른 트랜잭션이 첫 번째 트랜잭션이 완료되기 전에 수정하면 첫 번째 트랜잭션이 다시 값을 다시 읽으려고 하면 다른 결과를 얻게 되는 이상현상.

```sql
TR1 - SELECT QNT*PRICE FROM SALES // 10000, 18000

TR2 - UPDATE SALES SET QNT = QNT + 5 WHERE NAME = PR1

TR2 Commit

TR1 - SELECT QNT*PRICE FROM SALES // 15000, 18000
```

같은 트랜잭션 내에서 같은 쿼리를 실행했지만 다른 값을 읽어온다.

## Phantom reads

트랜잭션 실행 도중 레코드가 삭제 되거나 추가 되면 다른 값을 읽어오는 이상현상이다.

```sql
TR1 - SELECT QNT*PRICE FROM SALES // 10000, 18000

TR2 - INSERT INTO SALES VALUES ('PR3', 10, 3000)

TR2 Commit

TR1 - SELECT QNT*PRICE FROM SALES // 10000, 18000, 30000
```

## Lost Updates

두 트랜잭션이 값을 읽고 수정할 때 서로의 변경 내용을 덮어쓰게 되면서 발생하는 이상현상이다.

```sql
TR1 - UPDATE SALES SET QNT = QNT + 10 FROM SALES WHERE NAME = 1

TR2 - UPDATE SALES SET QNT = QNT + 5 FROM SALES WHERE NAME = 1

TR2 Commit // PR1 QNT = 15

TR1 - SELECT QNT*PRICE FROM SALES // 15000, 18000
```

# Transaction Isolation Leve (트랜잭션 격리 수준)

ACID원칙 중 Isolation (고립성) 속성을 구현하는 방법으로 데이터베이스에서 여러 트랜잭션이 동시에 수행될 때 얼마나 격리되어야 하는지를 정의한다.

## Read Uncommitted

한 트랜잭션에서 아직 커밋 되지 않은 변경을 다른 트랜잭션이 읽을 수 있다. 가장 낮은 격리 수준으로, 성능은 빠르지만 일관성 문제가 발생할 수 있다.

Read Uncommitted에서는 다음의 오류가 발생한다

- Dirty reads (커밋 되지 않은 트랜잭션을 읽을 수 있다)
- Non-repeatable reads
- Phantom reads
- Lost Updates

## Read Committed

한 트랜잭션에서 변경한 내용이 커밋 된 후에만 다른 트랜잭션이 해당 내용을 읽을 수 있다. 일반적으로 사용되는 격리 수준이며 적절한 성능을 갖추고 있지만 트랜잭션 중간에 실제로 Commit 되는 Non-repeatable Read는 막을 수 없다.

Read committed에서는 다음의 오류가 발생한다

- Non-repeatable reads (커밋 되었으면 읽힌다)
- Phantom reads
- Lost Updates

## Repeatable Read

트랜잭션이 실행되는 동안 하나의 행을 몇번을 반복해서 읽어 오더라도 값이 바뀌지 않는다. 그러나 다른 트랜잭션이 새로운 레코드를 추가하는 것은 허용된다. 따라서 Phantom Read는 방지 되지 않는다.

- Phantom reads (방지 되지 않는다)

## Serializable

각각의 트랜잭션이 동시에 실행되지 않고 순서대로 실행된다. 가장 높은 Isolation level이지만 가장 성능이 떨어진다.

- Serializable에서는 위의 4가지 오류가 방지 된다.

## 정리 표

| 격리 수준 | Dirty reads | Lost updates | Non-repeatable reads | Phantom reads |
| --- | --- | --- | --- | --- |
| Read Uncommitted | O | O | O | O |
| Read Committed | X | O | O | O |
| Repeatable Read | X | X | X | O |
| Serializable | X | X | X | X |