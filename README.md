# 1주차 
---

### 1. SELECT (데이터 조회)
```
SELECT column1, column2
FROM table_name;
```
#### 모든 컬럼 조회
```
SELECT * FROM employees;
```
#### 특정 컬럼만 조회
```
SELECT name, salary FROM employees;
```

### 2. WHERE (데이터 필터링)
```
SELECT * 
FROM employees
WHERE salary > 50000;
```
#### 여러 조건 결합
```
SELECT * 
FROM employees
WHERE salary > 50000 AND department = 'IT';
```

### 3. 집계함수, GROUP BY (월별 총 매출 계산 등)
```
SELECT department, COUNT(*) as emp_count, AVG(salary) as avg_salary
FROM employees
GROUP BY department;
```

#### 월별 매출 합계
```
SELECT DATE_FORMAT(order_date, '%Y-%m') as month, SUM(amount) as total_sales
FROM orders
GROUP BY DATE_FORMAT(order_date, '%Y-%m');
```

### 4. HAVING (그룹화된 결과 필터링)
```
SELECT department, AVG(salary) as avg_salary
FROM employees
GROUP BY department
HAVING AVG(salary) > 60000;
```

### 5. ORDER BY (정렬)
```
SELECT name, salary
FROM employees
ORDER BY salary DESC;
```
#### 여러 컬럼으로 정렬
```
SELECT name, department, salary
FROM employees
ORDER BY department ASC, salary DESC;
```

### 6. JOIN (테이블 결합)
#### INNER JOIN
```
SELEC e.name d.department_name
FROM employees e
INNER JOIN departments d O e.department_id  d.id;
```
#### LEFT JOIN
```
SELEC e.name d.department_name
FROM employees e
LEFT JOIN departments d O e.department_id  d.id;
```

### 7. SUBQUERY (평균 매출보다 높은 매출을 가진 고객의 평균 연령 조회 등)
#### WHERE 절의 서브쿼리
```
SELECT name, salary
FROM employees
WHERE salary >
 (SELECT AVG(salary) FROM employees);
```
#### FROM 절의 서브쿼리

```
SELECT dept, avg_sal
FROM (
    
    SELECT department as dept, AVG(salary) as avg_sal
    FROM employees
    GROUP BY department
) as dept_avg
WHERE avg_sal > 50000;
```

### 8. CASE WHEN (조건부 로직)
```
SELECT name, salary,
    CASE 
        WHEN salary < 30000 THEN '하'
        WHEN salary BETWEEN 30000 AND 60000 THEN '중'
        ELSE '상'
    END as salary_level
FROM employees;
```

### 9. NULL 처리
#### NULL 값 확인
```
SELECT * FROM employees WHERE manager_id IS NULL;
```
#### NULL 값 대체
```
SELECT name, COALESCE(phone, '전화번호 없음') as phone
FROM employees;
```
#### IFNULL (MySQL)
```
SELECT name, IFNULL(bonus, 0) as bonus
FROM employees;
```

### 10. 윈도우 함수 (매출액 누적합 계산 등)
#### 순위 매기기
```
SELECT name, salary,
    RANK() OVER (ORDER BY salary DESC) as salary_rank
FROM employees;
```
#### 부서별 순위
```
SELECT name, department, salary,
    RANK() OVER (PARTITION BY department ORDER BY salary DESC) as dept_rank
FROM employees;
```
#### 누적 합계
```
SELECT order_date, amount,
    SUM(amount) OVER (ORDER BY order_date) as cumulative_sales
FROM orders;
```

### 11. 날짜 변환 (날짜 포맷에서 연도만 추출 등)
#### 연도 추출
```
SELECT YEAR(order_date) as year, SUM(amount) as total_sales
FROM orders
GROUP BY YEAR(order_date);
```
#### 월 추출
```
SELECT MONTH(order_date) as month, COUNT(*) as order_count
FROM orders
GROUP BY MONTH(order_date);
```
#### 날짜 포맷 변경
```
SELECT DATE_FORMAT(order_date, '%Y년 %m월 %d일') as formatted_date
FROM orders;
```

### 12. 문자열 처리 (몇 번째 문자만 추출 등)
#### 부분 문자열 추출
```
SELECT SUBSTRING(name, 1, 3) as short_name FROM employees;
```
#### 문자열 연결
```
SELECT CONCAT(first_name, ' ', last_name) as full_name FROM employees;
```
#### 문자열 길이
```
SELECT name, LENGTH(name) as name_length FROM employees;
```
#### 대소문자 변환
```
SELECT UPPER(name) as upper_name, LOWER(name) as lower_name FROM employees;
```