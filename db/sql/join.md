---
description: 조인에 대해 알아봅니다.
---

# Join

## EQUI / Non-EQUI 조인

***

조인은 여러 개의 릴리이션을 사용해서 새로운 릴레이션을 만드는 것을 말합니다.

### EQUI JOIN

EQUI 조인은 비교 연산자 = 를 이용하여 교집합에 해당하는 릴레이션을 만드는 조인을 의미합니다.

```sql
SELECT * FROM table1, table2
    WHERE table1.column1 = table2.column1;
    AND ... // some other condition
```

EQUI 조인이 수행될 경우 교집합에 해당하는 영역이 릴레이션으로 생성됩니다.



### Non-EQUI JOIN

Non-EQUI 조인은 비교 연산자 = 외에 다른 연산자들을 사용하는 조인을 의미합니다.

```sql
SELECT * FROM table1, table2
    WHERE table1.colum1 > table2.colum2;
```

EQUI 조인과는 다르게 교집합이 아닌 아래의 영역이 릴레이션으로 생성됩니다.



## INNER JOIN

***

INNER JOIN은 [#equi-join](join.md#equi-join "mention")과 마찬가지로 두 테이블 사이에 교집합 영역을 조인합니다.

ON 문을 활용하여 테이블을 연결하며 아래와 같이 사용합니다.

```sql
SELECT * FROM table1 INNER JOIN table2
    ON table1.column1 = table2.colum2;
```



## OUTER JOIN

***

OUTER JOIN은 두 개의 테이블 간에 교집합(EQUI JOIN)을 조회하고, 한쪽 테이블에만 있는 데이터도 포함시켜서 조회하는 것을 의미합니다.

이때,  해당 값이 없는 경우 NULL 이 포함되어 반환됩니다.

### LEFT OUTER JOIN

마지막에 포함하는 한쪽 테이블을 기준으로 왼쪽 테이블을 포함시킨다면 LEFT OUTER JOIN이라 합니다.

```sql
SELECT * FROM table1
    LEFT OUTER JOIN table2
    ON table1.column1 = table2.column2;
```

위 코드는 table1과 table2의 교집합과 이후 table1의 남은 튜플을 포함한 릴레이션이 생성되게 됩니다.

이를 다이어그램으로 나타내면 아래와 같습니다.





### RIGHT OUTER JOIN

LEFT와는 다르게 오른쪽테이블을 포함시킨다면 RIGHT OUTER JOIN 이라 합니다.&#x20;

```sql
SELECT * FROM table1
    RIGHT OUTER JOIN table2
    ON table1.column1 = table2.column1;
```

위 코드는 두 테이블의 교집합과 table2의 남은 튜플을 더한 릴레이션이 생성되게 됩니다.

이를 다이어그램으로 나타내면 아래와 같습니다.



### FULL OUTER JOIN

앞선LEFT와 RIGHT를 모두 수행한다면 FULL OUTER JOIN 이라고 합니다.

```sql
SELECT * FROM table1
    FULL OUTER JOIN table2
    ON table1.column1 = table2.column1;
```

이를 다이어그램으로 나타내면 아래와 같습니다.





## CROSS JOIN

***

CROSS JOIN 은 조인 조건구 없이 2개의 테이블을 하나로 조인하는 것을 의미합니다.

조인 조건이 없기 때문에 카사디안 곱이 발생하며 아래와 같이 사용합니다.

```sql
SELECT * FROM table1 CROSS JOIN table2;
```

또한 기존 SELECT에 2개의 테이블을 사용하게 되면 CROSS JOIN과 동일한 효과가 발생하게 됩니다.

```sql
SELECT * FROM table1, table2;
```



## 집합 연산자

***

앞선 조인 외에도 집합 연산자를 통해 조인과 동일한 효과를 낼 수 있습니다.

### UNION

UNION 연산은 두 개의 테이블을 하나로 만드는, 즉 합집합 연산자입니다.

```sql
SELECT * FROM table1
UNION
SELECT * FROM table2;
```

단, UNION 연산 시 반드시 두 테이블은 데이터 형식과 칼럼 수가 일치해야 합니다.

```sql
SELECT column1 FROM table1
UNION
SELECT column1, column2 FROM table2; // 불가능
```

또한, UNION 연산을 수행하게 되면 두 개의 테이블이 하나로 합쳐지며 중복된 데이터가 자동으로 제거됩니다.

{% hint style="info" %}
UNION 연산은 중복 제거를 위해 sort를 발생시킵니다.
{% endhint %}

### UNION ALL

앞선 UNION 과 다르게 UNION ALL은 중복을 제거하지 않고, 이에 따라 정렬이 수행되지 않습니다.

```sql
SELECT * FROM table1
UNION ALL
SELECT * FROM table2;
```

그 외에 수행 조건은 UNION 과 동일합니다.

### INTERSECT

INTERSECT 연산은 두 개의 테이블에서 교집합을 조회합니다.

즉, 두 테이블에서 공통된 값을 조회하며 아래와 같이 사용합니다.

```sql
SELECT * FROM table1
INTERSECT
SELECT * FROM table2;
```

### MINUS

MINUS 연산은 두 테이블 간에 차집합을 구하는 연산입니다.

즉, 앞선 릴레이션에는 있고 이후 릴레이션에는 없는 집합을 조회합니다.

```sql
SELECT * FROM table1
MINUS
SELECT * FROM table2;
```

