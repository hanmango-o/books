---
description: Data Manipulation Language에 대해 알아보고, 데이터를 처리하는 방법을 살펴봅니다.
---

# DML

## SELECT

***

SELECT를 통해 테이블에 입력된 데이터를 조회할 수 있습니다.

```sql
SELECT * FROM table_name;
SELECT column_name1, column_name2 ... FROM table_name;
```

특정 칼럼 이름을 지정하여 해당 칼럼만을 조회할 수도 있고, \* 를 통해 모든 칼럼을 조회할 수도 있습니다.

또한, || 를 통해 특정 문자를 결합하여 조회가 가능합니다.

```sql
SELECT name || '님' FROM table_name;
```

위 예시에서는 name 칼럼을 조회하되 '님'을 붙여서 출력하게 됩니다.

### Order by

테이블 조회 시 ORDER BY 를 통해 정렬된 데이터를 조회할 수 있습니다.

```sql
SELECT * FROM table_name ORDER BY colum_name;
```

특정 컬럼을 기준으로 오름차순정렬하기 위해 ORDER BY {특정 칼럼명} 의 형태로 사용하게 됩니다. 이러한 ORDER BY는 SELECT 문에서 가장 늦게 실행되는 구문으로 맨 마지막에 위치하게 됩니다.

만약 정렬 기준 컬럼이 여러 개라면 아래와 같이 나열하여 작성합니다.

```sql
SELECT * FROM table_name ORDER BY column_name1, column_name2;
```

#### 내림차순

기본적으로 ORDER BY는 오름차순 정렬이 되게 됩니다.

만약 내림차순 정렬이 필요하다면, DESC 옵션을 추가합니다.

```sql
SELECT * FROM table_name ORDER BY column_name DESC;
```

{% hint style="info" %}
만약 2개 이상의 칼럼 기준으로 정렬할 때, 두 칼럼의 정렬 기준이 각각 오름차순과 내림차순으로 다르다면 아래와 같이 별도로 기재할 수 있습니다.

```sql
SELECT * FROM table_name ORDER BY column_name1 DESC, column_name2;
```

위 예시에서는 column\_name1을 기준으로 내림차순 정렬한 뒤, column\_name2를 기준으로 오름차순 정렬하게 됩니다.
{% endhint %}

### DISTINCT

데이터 조회 시 DISTINCT 옵션을 통해 중복된 데이터를 한 번만 조회할 수 있습니다.

```sql
SELECT DISTINCT * FROM table_name;
```

### Alias

SELECT 문 사용 시 컬럼과 테이블에 별칭(Alias)을 지정하여 보다 간략하게 사용할 수 있습니다.

특정 컬럼에 대한 별칭을 지정하고 싶다면 아래와 같이 AS를 사용하여 지정  가능합니다.

```sql
SELECT column_name1 AS "이름" FROM table_name;
```

또한 특정 테이블에 대한 별칭을 지정하고 싶다면 해당 테이블 바로 다음에 사용하고자 하는 별칭을 작성하여 사용 가능합니다.

```sql
SELECT t1.column_name FROM table_name t1;
```

설정한 별칭은 `.` 을 통해 해당 테이블의 칼럼에 접근 가능합니다.

### GROUP BY

GROUP BY는 테이블에서 소규모 행을 그룹화하여 합계, 평균, 최댓값, 최솟값 등을 계산할 때 사용합니다.

```sql
SELECT group_function FROM table_name
    GROUP BY column_name;
```

테이블에서 특정 컬럼을 기준으로 그룹화하고, 해당 그룹을 대상으로 집계함수를 동작하거나 조회를 수행합니다.

#### 집계함수

GROUP BY 에서 사용할 수 있는 집계 함수는 아래와 같습니다.

| 집계 함수         | 설명           |
| ------------- | ------------ |
| COUNT()       | 행 수 계산       |
| SUM()         | 합계 계산        |
| AVG()         | 평균 계산        |
| MAX() / MIN() | 최댓값 / 최솟값 계산 |
| STDDEV()      | 표준편차 계산      |
| VARIAN()      | 분산 계산        |

집계 함수를 사용한 GROUP BY 예시는 아래와 같습니다.

```sql
SELECT age.AVG(score) FROM student GROUP BY age;
```

위 예시는 학생 테이블의 학생들 중 동일한 나이의 학생들끼리 그룹화하고, 해당 그룹의 성적 평균을 출력하는 예시입니다.

{% hint style="info" %}
COUNT 함수의 경우 NULL 값을 포함하지 않은 행의 개수를 출력하게 됩니다.

만약 NULL을 포함한 행의 개수를 알고 싶다면 COUNT(\*)의 형태로 사용할 수 있습니다.
{% endhint %}

#### HAVING

GROUP BY는 HAVING 구를 통해 조건문을 설정하여 조건에 맞는 그룹화가 가능합니다.

```sql
SELECT ... FROM table_name
    GROUP BY column_name
    HAVING 조건문
```

이때, 조건문에 사용 가능한 연산자는 [#operator](dml.md#operator "mention")만 가능합니다.

### WHERE

SELECT 문은 WHERE 문을 사용하여 조건부 조회가 가능합니다.

WHERE 문이 사용 가능한 조건문은 아래와 같습니다.

#### &#x20;Operator

{% tabs %}
{% tab title="비교 연산자" %}
<table><thead><tr><th width="179">비교 연산자</th><th>설명</th></tr></thead><tbody><tr><td>=</td><td>같음(== 가 아님에 주의)</td></tr><tr><td>&#x3C;</td><td>작음</td></tr><tr><td>&#x3C;=</td><td>작거나 같음</td></tr><tr><td>></td><td>큼</td></tr><tr><td>>=</td><td>크거나 같음</td></tr></tbody></table>
{% endtab %}

{% tab title="부정 비교 연산자" %}
<table><thead><tr><th width="179">부정 비교 연산자</th><th>설명</th></tr></thead><tbody><tr><td>!=</td><td>같지 않음</td></tr><tr><td>^=</td><td>같지 않음</td></tr><tr><td>&#x3C;></td><td>같지 않음</td></tr><tr><td>NOT 칼럼명 = </td><td>같지 않음</td></tr><tr><td>NOT 칼럼명 ></td><td>크지 않음</td></tr></tbody></table>
{% endtab %}

{% tab title="논리 연산자" %}
<table><thead><tr><th width="180">논리 연산자</th><th>설명</th></tr></thead><tbody><tr><td>AND</td><td>조건 모두 참</td></tr><tr><td>OR</td><td>조건 모두 거짓</td></tr><tr><td>NOT</td><td>부정</td></tr></tbody></table>
{% endtab %}

{% tab title="SQL 연산자" %}
<table><thead><tr><th width="209">SQL 연산자</th><th>설명</th></tr></thead><tbody><tr><td>LIKE '비교 문자열'</td><td>문자열 비교</td></tr><tr><td>BETWEEN A AND B</td><td>A와 B 사이의 값 조회(A &#x3C;= target &#x3C;= B)</td></tr><tr><td>IN (list)</td><td>OR 연산과 동일, list 값 중 하나만 일치해도  조회</td></tr><tr><td>IS NULL</td><td>NULL 값을 조회</td></tr></tbody></table>
{% endtab %}

{% tab title="부정 SQL 연산자" %}
| 부정 SQL 연산자          | 설명             |
| ------------------- | -------------- |
| NOT BETWEEN A AND B | A미만 B초과 값 조회   |
| NOT IN (list)       | list와 불일치하면 조회 |
| IS NOT NULL         | NULL이 아니면 조회   |
{% endtab %}
{% endtabs %}

#### LIKE

LIKE 문을 활용한 와일드 카드를 통해 데이터를 조회할 수 있습니다.

<table><thead><tr><th width="172">와일드 카드</th><th>설명</th></tr></thead><tbody><tr><td>%</td><td>어떤 문자를 포함한 모든 것을 조회</td></tr><tr><td>_</td><td>단일 문자를 의미</td></tr></tbody></table>

만약 망고로 시작하는 모든 문자를 조회하고 싶다면, `'망고%'` 의 형태로 사용이 가능합니다.

또한 망과 고 사이에 한글자만 포함된 형태이며 망과 고로 끝나지 않는 문자를 찾는 경우 `'%망_고%'` 의 형태로 사용 가능합니다.

#### IN

IN의 경우 아래와 같이 사용 가능합니다.

```sql
SELECT * FROM table_name IN (column1, column2);
```

또한, 여러 개의 리스트를 사용하는 것도 가능합니다.

```sql
SELECT * FROM table_name IN ((column1, column2), (column3, column4));
```

#### NULL

NULL은 모르는 값, 값의 부재, 잘못된 연산, 알 수 없음 을 의미합니다.

이러한 NULL을 활용한 함수는 아래와 같습니다.

| NULL 함수                                | 설명                                               |
| -------------------------------------- | ------------------------------------------------ |
| NVL(column, new\_value)                | column이 NULL 이면 new\_value로 변환                   |
| NVL2(column, new\_value1, new\_value2) | column이 NULL 이면 new\_value2, 아니면 new\_value1로 변환 |
| NULLIF(column1, column2)               | 두 값이 같으면 NULL, 아니면 첫번째 값을 반환                     |
| COALESCE(exp1, exp2, .....)            | 주어진 exp 중 NULL 이 아닌 첫번째 값을 반환                    |

### SELECT 문 실행 순서

SELECT 문의 실행 순서는 아래와 같습니다.

1. FROM
2. WHERE
3. GROUP BY
4. HAVING
5. SELECT
6. ORDER BY

이에 작성 순서 역시 해당 순서대로 작성합니다.

{% hint style="info" %}
단, SELECT는 예외로 생각합니다. SELECT를 제외한 나머지 구문들은 실행 순서와 작성 순서가 일치합니다.
{% endhint %}

## INSERT

***

INSERT는 테이블에 데이터를 입력하는 DML문입니다.

```sql
INSERT INTO table1(column1, column2 ...) VALUES (exp1, exp2 ...);
```

INTO 구문 뒤에 작성한 칼럼의 순서대로 VALUES 뒤에 넣고자 하는 값을 작성합니다.

만약 모든 칼럼에 대해 데이터를 삽입하는 경우 칼럼명을 생략합니다.

```sql
INSERT INTO table1 VALUES (exp1, exp2, ...);
```

### SELECT 문 활용

INSERT 수행 시 기존에 있는 테이블의 데이터를 새로운 테이블로 삽입한다면, SELECT 문을 활용하여 구문을 작성할 수 있습니다.

```sql
INSERT INTO table1 SELECT * FROM table2;
```

이때, VALUES 가 생략되며 두 테이블의 칼럼 구조가 동일해야 합니다.



## UPDATE

***

입력된 데이터를 수정하기 위해서 UPDATE 문을 사용할 수 있습니다.

```sql
UPDATE table1 SET column1 = exp1 [WHERE conditions];
```

특정 테이블에 특정 칼럼값을 변경하고 싶다면 SET 이후에 변경하고 싶은 칼럼 = 변경값 의 형태로 기재합니다. 아래는 실제 사용 예시입니다.

```sql
UPDATE student SET name = '홍길동'; // 모든 데이터의 이름이 홍길동으로 변환 됨
```

또한, UPDATE는 WHERE 조건문의 사용이 가능하여 조건에 맞는 데이터만 수정 가능 합니다.&#x20;

조건을 기재하지 않으면 해당 테이블의 모든 데이터의 해당 칼럼값이 변경하고자 하는 값으로 변경됩니다.

```sql
UPDATE student SET name = '홍길동' WHERE id = 10;
// id가 10번인 학생의 이름을 홍길동으로 변경
```



## DELETE

***

원하는 조건에 맞는 행을 삭제하고 싶다면 DELETE를 사용합니다.

```sql
DELETE FROM table1 WHERE conditions;
```

만약 DELETE 문에 조건을 기재하지 않으면 모든 데이터가 삭제됩니다.

{% hint style="info" %}
[#drop](ddl.md#drop "mention")의 경우 테이블을 삭제하고, DELETE는 테이블이 아닌 데이터를 삭제합니다.

즉, DELETE를 수행하더라도 해당 테이블이 삭제된 것은 아닙니다.
{% endhint %}

### TRUNCATE

DELETE의 경우 데이터를 삭제하더라도 테이블의 용량이 감소하지 않습니다.

만약 데이터 삭제 후 테이블의 용량을 초기화하고 싶다면, TRUNCATE 를 사용합니다.

```sql
TRUNCATE TABLE table1;
```

위와 같이 작성할 경우 해당 테이블의 모든 데이터를 삭제하고 테이블의 용량을 초기화합니다.
