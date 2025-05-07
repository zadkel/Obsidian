---
created: 2025-04-15 11:48
tags:
  - SQL
---

---

# SQL 기초

## 1. SQL이란 무엇인가?

SQL(Structured Query Language, 구조적 질의 언어)은 관계형 데이터베이스 관리 시스템(RDBMS)에서 데이터를 관리하고 처리하기 위해 설계된 표준 언어입니다. SQL을 사용하여 다음 작업들을 수행할 수 있습니다.

* 데이터베이스 및 테이블 생성, 수정, 삭제
* 데이터 삽입, 조회, 수정, 삭제
* 데이터에 대한 접근 권한 제어
* 데이터 무결성 설정

SQL은 배우기 쉽고 강력하며, 대부분의 현대 데이터베이스 시스템(MySQL, PostgreSQL, Oracle, SQL Server, SQLite 등)에서 표준으로 사용됩니다.

## 2. 데이터베이스와 테이블

* **데이터베이스(Database):** 구조화된 데이터의 집합입니다. 관련된 정보들을 효율적으로 저장, 관리, 검색할 수 있도록 구성됩니다. 예를 들어, 회사 데이터베이스에는 직원 정보, 부서 정보, 급여 정보 등이 저장될 수 있습니다.
* **테이블(Table):** 데이터베이스 내에서 실제 데이터가 저장되는 기본 단위입니다. 행(Row)과 열(Column)으로 구성된 2차원 구조를 가집니다.
    * **열(Column / Field / Attribute):** 테이블의 각 열은 특정 종류의 데이터를 나타냅니다. 예를 들어, '직원' 테이블에는 '직원 ID', '이름', '입사일', '부서 ID' 등의 열이 있을 수 있습니다. 각 열은 고유한 이름과 데이터 타입(예: 숫자, 문자열, 날짜)을 가집니다.
    * **행(Row / Record / Tuple):** 테이블의 각 행은 개별적인 데이터 항목을 나타냅니다. 예를 들어, '직원' 테이블의 한 행은 특정 직원 한 명의 정보를 담고 있습니다.

**예시: `employees` 테이블**

| employee_id (INT) | first_name (VARCHAR) | last_name (VARCHAR) | hire_date (DATE) | department_id (INT) |
| :---------------- | :------------------- | :------------------ | :--------------- | :------------------ |
| 101               | John                 | Doe                 | 2023-01-15       | 10                  |
| 102               | Jane                 | Smith               | 2023-03-10       | 20                  |
| 103               | Peter                | Jones               | 2022-11-01       | 10                  |

## 3. SQL 명령어의 종류

SQL 명령어는 크게 몇 가지 범주로 나눌 수 있습니다.

* **DDL (Data Definition Language - 데이터 정의어):** 데이터베이스 객체(테이블, 인덱스, 뷰 등)의 구조를 정의하는 명령어입니다.
    * `CREATE`: 데이터베이스 객체를 생성합니다. (`CREATE TABLE`, `CREATE DATABASE`)
    * `ALTER`: 기존 객체의 구조를 변경합니다. (`ALTER TABLE`)
    * `DROP`: 데이터베이스 객체를 삭제합니다. (`DROP TABLE`, `DROP DATABASE`)
* **DML (Data Manipulation Language - 데이터 조작어):** 테이블의 데이터를 조작(삽입, 조회, 수정, 삭제)하는 명령어입니다. 가장 빈번하게 사용됩니다.
    * `SELECT`: 테이블에서 데이터를 조회합니다.
    * `INSERT`: 테이블에 새로운 데이터를 삽입합니다.
    * `UPDATE`: 테이블의 기존 데이터를 수정합니다.
    * `DELETE`: 테이블에서 데이터를 삭제합니다.
* **DCL (Data Control Language - 데이터 제어어):** 데이터에 대한 접근 권한을 부여하거나 회수하는 명령어입니다.
    * `GRANT`: 사용자에게 특정 권한을 부여합니다.
    * `REVOKE`: 사용자로부터 권한을 회수합니다.
* **TCL (Transaction Control Language - 트랜잭션 제어어):** DML 작업들을 논리적인 단위(트랜잭션)로 묶어 관리하는 명령어입니다.
    * `COMMIT`: 트랜잭션을 확정하여 변경 사항을 영구 저장합니다.
    * `ROLLBACK`: 트랜잭션 작업을 취소하고 이전 상태로 되돌립니다.
    * `SAVEPOINT`: 트랜잭션 내에 임시 저장점을 설정합니다.








---
# 링크 
## 🔗 아웃링크
```dataview
LIST WITHOUT ID outgoing
FROM ""
WHERE file = this.file
FLATTEN file.outlinks as outgoing
SORT outgoing ASC
```
## 🔙 백링크
```dataview
LIST WITHOUT ID file.link
WHERE contains(file.outlinks, this.file.link)
SORT file.link ASC
```
