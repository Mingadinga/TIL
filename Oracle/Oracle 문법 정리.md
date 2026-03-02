실행 및 작성 순서 : **fjw ghsol**

과거 코테 준비 기록(mySQL) :[SQL](https://www.notion.so/SQL-f9ee13be653c4fc5a54133551750323e?pvs=21)

프로그래머스 : https://school.programmers.co.kr/learn/challenges?tab=sql_practice_kit

# 소수점 출력

반올림 : [**ROUND("값", "자리수")**](https://gent.tistory.com/241)

버림 : [**TRUNC(**"값"**,** "옵션"**)**](https://gent.tistory.com/192)

정수로 절상 : CEIL

정수로 절사 : FLOOR

```sql
# 자리수에서 반올림
SELECT ROUND(1235.543)    -- 1236
     , ROUND(1235.443, 0) -- 1235
     , ROUND(1235.345, 2) -- 1235.35
FROM dual
```

# 집계함수

- AVG(컬럼)
-

# ORDER BY

[[Oracle] 오라클 정렬 순서 지정 방법 (ORDER BY 절)](https://gent.tistory.com/441)

```sql
# 기본 정렬
SELECT FLAVOR
FROM FIRST_HALF
ORDER BY
    TOTAL_ORDER DESC,
    SHIPMENT_ID ASC;
```

# 문자열 다루기

## 조건부 출력

CASE WHEN END

```sql
SELECT name,
			 CASE
         WHEN salary >= 5000 THEN 'High'
         WHEN salary >= 3000 THEN 'Mid'
         ELSE 'Low'
       END AS salary_grade
FROM employees;

```

DECODE(컬럼, 조건1, 결과1, 조건2, 결과2, 조건3, 결과3) 

```sql
SELECT gender
		   # gender가 M이면 남자, F이면 여자, 그외 기타
     , DECODE(gender, 'M', '남자', 'F', '여자', '기타') gender2
FROM temp
```

## 문자열 포함 여부

```sql
SELECT *
  FROM emp
 WHERE ename LIKE '%MI%'

# INSTR 함수는 특정 문자열을 찾은 위치를 정수형(숫자)으로 반환한다.
# 문자열을 찾으면 1이상, 못 찾으면 0을 반환한다.
# 조건에 사용하는 컬럼이 인덱스 컬럼인 경우 속도에 영향 있음.
SELECT *
  FROM emp
 WHERE INSTR(ename, 'MI') > 0
```

# NULL 다루기

조건문 : IS NULL, IS NOT NULL

```sql
# 조건문
SELECT *
FROM 테이블명
WHERE 컬럼명 IS NULL;
WHERE 컬럼명 IS NOT NULL;
```

필터링 : NVL, NVL2

- NVL(출력대상, null 대체 문자열) : 첫 번째 인자가 NULL이면 두 번째 인자를 반환

```sql
SELECT name, **NVL(commission, 0)** FROM employees;
```

- NVL2(expr1, expr2, expr3) : expr1이 null이면 expr3 반환, null이 아니면 expr2 반환

```sql
SELECT NVL2('','COREA','KOREA') from dual; # KOREA
```

# 날짜 함수

- TO_CHAR(날짜 데이터 타입, '지정 형식')
  | 지정형식       | 설명                                          | 예                        | 결과            |
  | -------------- | --------------------------------------------- | ------------------------- | --------------- |
  | CC             | 세기                                          | TO_CHAR(SYSDATE, 'CC')    | 24              |
  | **YYYY or YY** | 연도                                          | TO_CHAR(SYSDATE, 'YYYY')  | **2024**        |
  | Y,YYY          | 콤마가 있는 연도                              | TO_CHAR(SYSDATE, 'Y,YYY') | 2,201           |
  | YEAR           | 문자로 표현된 연도                            | TO_CHAR(SYSDATE, 'YEAR')  | TWENTYTWENTYONE |
  | **MM**         | 두 자리 값의 월                               | TO_CHAR(SYSDATE, 'MM')    | **04**          |
  | MONTH          | 아홉 자리를 위해 공백을 추가한 월 이름        | TO_CHAR(SYSDATE, 'MONTH') | 4월             |
  | MON            | 세 자리의 약어로 된 월 이름(영문 설정일 경우) | TO_CHAR(SYSDATE, 'MON')   | 4월             |
  | DDD, **DD**, D | 연, 월, 주의 일                               | TO_CHAR(SYSDATE, 'DD')    | **18**          |
  | DAY            | 아홉자리를 위해 공백을 추가한 요일의 이름     | TO_CHAR(SYSDATE, 'DAY')   | 일요일          |
- YYYY
- MM
- DD

## 날짜 비교

```sql
WHERE TO_CHAR(PUBLISHED_DATE, 'yyyy') = 2021
```

## 날짜 형식 지정하여 출력

```sql
SELECT TO_CHAR(SYSDATE, 'YYYY-MM-DD')
```

# JOIN

inner join

```sql
# 1. EQUI JOIN
SELECT *
FROM A, B
WHERE A.a = B.a;

# 2. NATURAL JOIN : 컬럼명 동일
SELECT *
FROM A INNER JOIN B;

# 3. JOIN ~ USING : 컬럼명 동일
SELECT *
FROM A JOIN B USING(a);
```

outer join

```sql
# 1. left outer join
SELECT *
FROM A LEFT OUTER JOIN B
ON A.a = B.a;

SELECT *
FROM A, B
WHERE A.a = B.a(+);

# 2. right outer join
SELECT *
FROM A RIGHT OUTER JOIN B
ON A.a = B.a;

SELECT *
FROM A, B
WHERE A.a(+) = B.a;

# 3. full outer join
SELECT *
FROM A FULL OUTER JOIN B
ON A.a = B.a;
```

# GROUP BY, HAVING

```sql
SELECT job
     , COUNT(*) cnt
     , SUM(sal) sal
  FROM emp
 WHERE deptno IN ('10', '20', '30')
 GROUP BY job
HAVING COUNT(*) > 2 AND SUM(sal) > 5000

# select하려면 group by에도 컬럼 추가
SELECT job
     , **deptno**
     , COUNT(*) cnt
  FROM emp
 WHERE deptno IN ('10', '20', '30')
 GROUP BY job, **deptno**
HAVING deptno = '20'
```

HAVING 사용 조건 : 집계함수, group by에 사용한 컬럼

```sql
SELECT job
     , COUNT(*) cnt
  FROM emp
 WHERE deptno IN ('10', '20', '30')
 GROUP BY job
HAVING COUNT(*) = COUNT(DISTINCT deptno)
```
