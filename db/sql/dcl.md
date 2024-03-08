---
description: Data Control Language 에 대해 알아봅니다.
---

# DCL

## GRANT

***

GRANT문은 DB 사용자에게 권한을 부여할 때 사용합니다.

```sql
GRANT privileges ON table1 TO user1;
```

여기서 privileges는 권한을 의미하며, table1에 대한privliges를 user1에게 부여한다는 의미입니다.

### 권한(Privileges)

GRANT 문에서 사용할 수 있는 권한은 아래와 같습니다.

<table><thead><tr><th width="168">권한</th><th>설명</th></tr></thead><tbody><tr><td>SELECT</td><td>지정된 테이블에 대해 SELECT 권한을 부여</td></tr><tr><td>INSERT</td><td>지정된 테이블에 대해 INSERT 권한을 부여</td></tr><tr><td>UPDATE</td><td>지정된 테이블에 대해 UPDATE 권한을 부여</td></tr><tr><td>DELETE</td><td>지정된 테이블에 대해 DELETE 권한을 부여</td></tr><tr><td>REFERENCES</td><td>지정된 테이블을 참조하는 제약조건을 생성할 수 있는 권한 부여</td></tr><tr><td>ALTER</td><td>지정된 테이블에 대해 수정할 수 있는 권한 부여</td></tr><tr><td>INDEX</td><td>지정된 테이블에 대해 인덱스를 생성할 수 있는 권한 부여</td></tr><tr><td>ALL</td><td>테이블에 대한 모든 권한 부여</td></tr></tbody></table>

아래는 GRANT 문 사용 예시입니다.

```sql
GRANT SELECT ON table1 TO user1;
GRANT SELECT, UPDATE, ALTER ON table1 TO user2;
```

### WITH GRANT OPTION

앞선 권한 부여를 할 수 있는 권한을 부여할 수도 있습니다.

이를 위해 WITH GRANT OPTION 구문을 사용합니다.

```sql
GRANT ... ON ... TO ... WITH GRANT OPTION;
GRANT ... ON ... TO ... WITH ADMIN OPTION;
```

이를 정리하면 아래와 같습니다.

<table><thead><tr><th width="230">GRANT 옵션</th><th>설명</th></tr></thead><tbody><tr><td>WITH GRANT OPTION</td><td>특정 사용자에 대한 권한을 부여할 수 있는 권한을 부여합니다.</td></tr><tr><td>WITH ADMIN OPTION</td><td>테이블에 대한 모든 권한을 부여합니다.</td></tr></tbody></table>

{% hint style="info" %}
만약 A 사용자가 B 사용자에게 권한을 부여하고 B가 다시 C에게 권한을 부여했을 때, A 사용자가 [#revoke](dcl.md#revoke "mention")를 통해 권한을 회수하면 B, C 사용자 모두 권한이 회수됩니다.
{% endhint %}



## REVOKE

***

사용자에게 부여된 권한을 회수하고 싶다면 REVOKE 구문을 사용합니다.

```sql
REVOKE privileges ON table1 FROM user1;
```
