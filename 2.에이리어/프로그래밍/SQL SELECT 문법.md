---
created: 2025-04-15 22:33
tags:
  - SQL
---

---
# SQL SELECT 예제

`SELECT` 문은 데이터베이스 테이블에서 데이터를 **검색(조회)**하는 데 사용되는 가장 기본적이고 중요한 SQL 명령어임. `[[DML]]`에 속함. 이 문서는 다양한 `SELECT` 문 사용 예제를 보여줌.

_(예제에서는 가상의 `Employees` 테이블 사용 가정. 컬럼: `employee_id`, `first_name`, `last_name`, `department_id`, `salary`, `hire_date` 등)_

### **1. 기본 조회**

- **모든 컬럼 조회:** 테이블의 모든 컬럼 데이터 가져오기.
```sql
SELECT *
FROM Employees;
 ```

- **특정 컬럼 조회:** 원하는 컬럼만 지정하여 데이터 가져오기.
```sql
SELECT employee_id, first_name, salary
FROM Employees;
```

- **중복 제거 조회 (`DISTINCT`):** 특정 컬럼에서 중복된 값을 제외하고 고유한 값만 조회.
```sql
SELECT DISTINCT department_id
FROM Employees;
```

### **2. 조건부 조회 (`WHERE` 절)**

- `[[WHERE]]` 절을 사용하여 특정 조건을 만족하는 행(Row)만 필터링하여 조회.
- 특정 값과 일치하는 데이터 조회.
```sql
SELECT first_name, last_name
FROM Employees
WHERE department_id = 50; -- 부서 ID가 50인 직원
```

- **패턴 매칭 (`LIKE`):** 문자열의 일부 패턴과 일치하는 데이터 조회 (`%`: 여러 문자, `_`: 한 문자).
```sql
SELECT first_name, last_name
FROM Employees
WHERE first_name LIKE 'J%'; -- 이름이 'J'로 시작하는 직원
```

- **목록 포함 (`IN`):** 지정된 목록 안의 값 중 하나와 일치하는 데이터 조회.
```sql
SELECT employee_id, first_name
FROM Employees
WHERE department_id IN (10, 20, 30); -- 부서 ID가 10, 20, 30 중 하나인 직원
```

- **범위 조건 (`BETWEEN`):** 특정 범위 내의 값 조회 (경계값 포함).
```sql
SELECT first_name, hire_date
FROM Employees
WHERE hire_date BETWEEN '2023-01-01' AND '2023-12-31'; -- 2023년에 고용된 직원
```

- **논리 연산자 (`AND`, `OR`, `NOT`):** 여러 조건을 조합하여 조회.

```sql
SELECT *
FROM Employees
WHERE department_id = 50 AND salary > 70000; -- 부서 ID가 50이면서 급여가 70000 초과인 직원
```


### **3. 결과 정렬 (`ORDER BY` 절)**

- `[[ORDER BY]]` 절을 사용하여 조회된 결과를 특정 컬럼 기준으로 정렬.
- 오름차순 정렬 (`ASC`): 기본값, 생략 가능.
- 내림차순 정렬 (`DESC`):
- 다중 컬럼 정렬: 첫 번째 컬럼으로 정렬 후, 값이 같으면 두 번째 컬럼으로 정렬.
```sql
SELECT department_id, first_name, salary
FROM Employees
ORDER BY department_id ASC, salary DESC; -- 부서 오름차순, 부서 내에서는 급여 내림차순
```

### **4. 결과 개수 제한 (DB 제품별 상이)**

- 조회 결과 중 상위 N개의 행만 가져오기. (문법은 DBMS마다 다름)
- **MySQL / PostgreSQL (`LIMIT`):**
```sql
SELECT employee_id, salary
FROM Employees
ORDER BY salary DESC
LIMIT 5; -- 급여 상위 5명
```


### **5. 그룹화 및 집계 (`GROUP BY`, `HAVING`, 집계 함수)**

- `[[GROUP BY]]` 절은 특정 컬럼 값 기준으로 행들을 그룹화하고, `[[집계 함수]]`(`COUNT`, `SUM`, `AVG`, `MAX`, `MIN`)를 사용하여 그룹별 통계 계산.
- **그룹별 개수 세기 (`COUNT`):**
```sql
SELECT department_id, COUNT(*) AS employee_count
FROM Employees
GROUP BY department_id; -- 부서별 직원 수
```

- **그룹 조건 필터링 (`HAVING`):** `GROUP BY`로 그룹화된 결과에 대해 조건 적용. (`WHERE`는 그룹화 전 개별 행에 조건 적용)
```sql
SELECT department_id, AVG(salary) AS average_salary
FROM Employees
GROUP BY department_id
HAVING AVG(salary) > 60000; -- 평균 급여가 60000 초과인 부서만 조회
```




---
# 링크 
## 🔗 아웃링크
```dataview
LIST WITHOUT ID outgoing
FROM ""
WHERE file = this.file
FLATTEN file.outlinks as outgoing
WHERE endswith(lower(outgoing.path), ".md")
SORT outgoing ASC
```
## 🔙 백링크
```dataview
LIST WITHOUT ID file.link
WHERE contains(file.outlinks, this.file.link)
SORT file.link ASC
```
