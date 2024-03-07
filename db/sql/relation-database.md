---
description: 관계형 DB에 대해 알아봅니다.
---

# Relation Database

## 관계형 DB 정의

***

관계형 DB는 관계(Relation)와 릴레이션의 조인 연산을 통해 합집합, 교집합, 차집합 등을 만들어 데이터를 저장, 관리하는 데이터베이스입니다.

### DBMS

데이터 관리 시스템(Database Management SysteM)은 데이터베이스를 관리하기 위한 SW를 의미하며, DBMS라고 합니다.

이러한 DBMS는표대적으로 MySQL, MS-SQL, OracleSQL 등이 있습니다.

### Table 구조

관계형 데이터베이스는 릴레이션에 데이터를 저장하고, 릴레이션을 사용한 연산을 통해 다양한 형태로 데이터를 조회합니다.

이러한 릴레이션은 DBMS에서 테이블의 형태로 만들어지고, 그 형태는 아래와 같습니다.

```
```

#### \[1] 기본키(PK)

PK는 하나의 테이블에서 유일성(Unique)와 최소성, NOT NULL을 만족하며 해당 테이블을 대표하는 속성을 의미합니다.

#### \[2] 튜플(Tuple)

튜플은 행(ROW)으로, 하나의 테이블에 저장되는 값을 의미합니다.

#### \[3] 속성(Attribute)

속성은 데이터를 저장하기 위한 필드(Field)로 행(Column)을 의미합니다.

{% hint style="info" %}
테이블은 행과 열로 이루어져 있으며, 이를 DB에서는 튜플과 속성으로 구성된다고 합니다.
{% endhint %}

#### \[4] 외래키(FK)

다른 테이블의 기본키를 참조(조인)하기 위한 속성입니다.



## 관계형 DB 연산

***

관계형 DB의 특징은 릴레이션을 활용한집합 연산과 관계 연산을 통해  다양한 형태로 데이터를 조회할 수 있다는 것입니다.

### 집합 연산

집합 연산은 두 개 이상의 집합을 대상으로 중복을 제거하고 합집합, 교집합 차집합 등의 연산을 수행하는 연산을 의미합니다.

검색된 데이터의 처리를 수행하는 연산을 집합 연산이라 하며, UNION, INTERSECT, EXCEPT 등의 연산이 대표적입니다.

#### 합집합(Union)

두개의 릴레이션을 합치는 연산으로 중복된 튜플은 한 번만 조회됩니다.

#### 차집합(Difference)

기준 릴레이션에는 존재하고, 다른 릴레이션에는 존재하지 않는 튜플을 조회합니다.

#### 교집합(Intersection)

두 개의 릴레이션 간에 공통된 튜플을 조회합니다.

#### 곱집합(Catesian porduct)

각 릴레이션에 존재하는 모든 경우의 데이터 조합을 연산하여 조회합니다.



### 관계 연산

관계 연산이란 관계형 DB에서 사용되는 연산을 의미하며, 데이터를 조작하고 검색하는 데 사용됩니다.

주로 SELECT, INSERT, UPDATE, DELETE 등의 명령문을 통해 데이터를 조작하는 연산을 관계 연산이라 합니다.

#### 선택 연산(Selection)

릴레이션에서 조건에 맞는 튜플을 조회합니다.

#### 투영 연산(Projection)

릴레이션에서 조건에 맞는 속성(Attribute)을 조회합니다.

#### 결합 연산(Join)

여러 릴레이션의 공통된 속성을 사용하여 새로운 릴레이션을 만듭니다.

#### 나누기 연산(Division)

기준 릴레이션에서 나누는 릴레이션과 동일한 속성을 가지는 튜플을 추출한 뒤, 나누는 릴레이션의 속성값을 제거하고 중복된 행을 제거하는 연산입니다.



## SQL(Structured Query Language)

***

SQL은 RDB에 대해서 데이터의 구조를 정의, 조작, 제어할 수 있는 절차형 언어입니다. SQL은 국제 표준을 준수하기에 DBMS가 변경되어도 동일하게 사용할 수 있습니다.

이러한 SQL은 데이터 정의, 조작, 제어 등의 기능을 제공하며 이는 아래와 같습니다.

### DDL(Data Definition Language)

RDB의 구조를 정의하는 언어입니다. 테이블을 생성, 변경, 삭제하여 데이터를 저장할 구조를 정의합니다.

{% content-ref url="ddl.md" %}
[ddl.md](ddl.md)
{% endcontent-ref %}

### DML(Data Manipulation Language)

테이블에서 데이터를 입력, 수정, 삭제, 조회하는 언어입니다. 정의된 테이블 구조에서 데이터를 제어합니다.

{% content-ref url="dml.md" %}
[dml.md](dml.md)
{% endcontent-ref %}

### DCL(Data Control Language)

DB 사용자에 대한 권한을 제어하는 언어입니다.

{% content-ref url="dcl.md" %}
[dcl.md](dcl.md)
{% endcontent-ref %}

### TCL(Transaction Control Language)

트랜잭션을 제어하는 언어입니다.

{% content-ref url="tcl.md" %}
[tcl.md](tcl.md)
{% endcontent-ref %}

{% hint style="info" %}
SQL 언어에 따른 DB 작업 순서는 아래와 같습니다.

1. DCL을 통해 사용자 권한을 부여합니다.
2. 권한이 부여된 사용자가 DDL을 통해 데이터 구조, 즉 테이블을 정의합니다.
3. 테이블이 정의되면 DML을 통해 데이터를 처리하고,  TCL을 통해 트랜잭션을 제어합니다.
{% endhint %}
