---
description: Transaction Control Language 에 대해 알아봅니다.
---

# TCL

## COMMIT

***

COMMIT은 INSERT, UPDATE, DELETE 문으로 변경한 데이터를 DB에 반영할 때 사용합니다.

```sql
COMMIT;
```

COMMIT을 사용하게 되면 변경 전 이전 데이터는 잃어버리게 됩니다.

즉, A 값을 B로 변경하고 COMMIT을 수행했다면 A값은 잃어버리고 B값이 반영됩니다.



## ROLLBACK

***

ROOLBACK은 데이터에 대한 변경 사용을 모두 취소하고 트랜젝션을 종료할 때 사용합니다.

```sql
ROLLBACK;
```

ROOLBACK을 사용할 경우 지금까지 작업한 INSERT, UPDATE, DELET 작업이 취소되고 이전 COMMIT 시점까지 복구됩니다.



## SAVEPOINT

***

SAVEPOINT는 트랜젝션을 작게 분할하여 관리하고자 할 때 사용합니다.

```sql
SAVEPOINT savepoint_name;
```

SAVEPOINT 를 사용하면 해당 시점까지 수행한 트랜젝션을 지정한 이름으로 저장하게 됩니다.

이후 해당 SAVEPOINT로 ROLLBACK을 수행하고 싶다면 아래와 같이 작성합니다.

```sql
ROLLBACK TO savepoint_name;
```
