---
description: Data Definition Language에 대해 알아보고, 데이터 구조를 제어하는 명령어를 살펴봅니다.
---

# DDL

## CREATE

***

CREATE는DB를 사용하기 위해 테이블을 생성하는 명령어입니다.

새로운 테이블을 생성하며 생성  시 기본키, 외래키, 제약사항 등을 설정할 수 있습니다.

### CREATE 기본 구조

기본적인 CREATE 문은 아래와 같습니다.

```sql
[1]CREATE TABLE {테이블명} (
    [2]{칼럼 정보} {데이터 타입} [[3]제약사항],
    ...
);
```

#### \[1] CREATE TABLE

CREATE TABLE 테이블명 ( ... ); 문은 해당 테이블을 생성하라는 명령어입니다.&#x20;

#### \[2] 칼럼 정보

테이블에 생성되는 속성 이름과 데이터 타입을 입력합니다.

#### \[3] 제약 사항

해당 속성의 제약 사항이 필요하다면 제약 사항을 기재합니다. 기본키, 외래키 등의 옵션이 제약 사항에 기재될 수 있습니다.

{% tabs %}
{% tab title="PRIMARY KEY" %}
테이블의 각 행을 고유하게 식별하는 열을 지정합니다.

```sql
CREATE TABLE Users (
    ID INT PRIMARY KEY,
    Name VARCHAR(50)
);
```
{% endtab %}

{% tab title="FOREIGN KEY" %}
다른 테이블의 기본 키 또는 유일한 키와 관계를 맺는 열을 지정합니다.

```sql
CREATE TABLE Orders (
    OrderID INT,
    CustomerID INT,
    FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)
);
```
{% endtab %}

{% tab title="UNIQUE" %}
열(또는 열의 조합)에 중복된 값이 없어야 함을 지정합니다.

```sql
CREATE TABLE Users (
    Email VARCHAR(100) UNIQUE,
    Name VARCHAR(50)
);
```
{% endtab %}

{% tab title="NOT NULL" %}
열이 NULL 값을 허용하지 않도록 지정합니다.

```sql
CREATE TABLE Employees (
    EmployeeID INT NOT NULL,
    Name VARCHAR(50)
);
```
{% endtab %}

{% tab title="CHECK" %}
특정 조건을 충족시키는지 확인합니다.

```sql
CREATE TABLE Employees (
    Age INT CHECK (Age >= 18),
    Name VARCHAR(50)
);
```
{% endtab %}

{% tab title="DEFAULT" %}
새로운 행이 삽입될 때 열에 기본값을 지정합니다.

```sql
CREATE TABLE Students (
    ID INT,
    Name VARCHAR(50),
    Grade CHAR DEFAULT 'A'
);
```
{% endtab %}
{% endtabs %}

### CONSTRAINT

앞선 [#create-1](ddl.md#create-1 "mention")처럼 속성의 맨 뒤에 제약 사항을 직접 기재할 수 있지만, CONSTRAINT를 통해 별도로 지정할 수 있습니다.

아래는 PK 설정 시 CONSTRAINT 를 활용한 예시입니다.

```sql
CREATE TABLE table_name (
    {column_name} {data_type},
    ...,
    CONSTRAINT {pk_name} primary key(column_name) // 2개의 기본 키 지정 시, primary key(name1, name2)
);
```

또한 FK 역시 CONSTRAINT를 사용할 수 있습니다.

```sql
CONSTRAINT fk_name FOREIGN KEY(column_name)
    REFERENCES other_table(column_name)
```

### ON DELETE CASCADE

테이블 생성 시 ON DELET CASCADE 옵션을 적용하여 참조 테이블의 데이터가 삭제될 경우 연쇄적으로 삭제되게 할 수 있습니다.

```sql
CREATE TABLE Orders (
    OrderID INT,
    CustomerID INT,
    FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID) ON DELETE CASCADE
);

// or

CREATE TABLE Orders (
    OrderID INT,
    CustomerID INT,
    CONSTRAINT customerIDfk FOREIGN KEY (CustomerID)
        REFERENCES Customners(CustomnerID)
        ON DELETE CASCADE
);
```

## ALTER

***

생성된 테이블은 ALTER TABLE 문을 통해 변경할 수 있습니다.

### 테이블 명 변경

RENAME TO를 통해 테이블 이름을 변경할 수 있습니다.

```sql
ALTER TABLE table_name RENAME TO new_tabel_name;
```

### 칼럼 추가

ADD 를 통해 칼럼을 추가할 수 있습니다.

```sql
ALTER TABLE table_name ADD (colunm_name data_type [options]);
```

### 칼럼 변경

MODIFY 를 통해 기존 칼럼을 변경할 수 있습니다.

```sql
ALTER TABLE table_name MODIFY (column_name data_type [options]);
```

단, 기존 데이터가 있는 경우 변경이 불가능하며 에러가 발생하게 됩니다.

### 칼럼 삭제

DROP COLUMN 을 통해 특정 칼럼을 삭제할 수 있습니다.

```sql
ALTER TABLE table_name DROP COLUMN column_name;
```

### 칼럼명 변경

RENAME COLUMN \~ TO 를 통해 특정 칼럼명을 변경할 수 있습니다.

```sql
ALTER TABLE table_name RENAME COLUMN column_name TO new_column_name;
```

## DROP

***

테이블의 삭제는 DROP TABLE문을 사용하여 삭제할 수 있습니다.

```sql
DROP TABLE table_name;
```

DROP TABLE을 사용할 경우 기존 데이터와 테이블의 구조를 모두 삭제합니다.

{% hint style="info" %}
데이터 뿐만 아니라 테이블 자체가 삭제됨에 유의합니다.
{% endhint %}

또한, CASECADE CONSTRAINT 옵션을 통해 해당 테이블의 데이터를 참조한 다른 테이블과 관련된 제약사항도 삭제할 수 있습니다.

```sql
DROP TABLE table_name CASCADE CONSTRAINT;
```
