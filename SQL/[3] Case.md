# SQL Case 문

```sql
SELECT 
    CASE
        WHEN(조건 A) THEN A
        WHEN(조건 B) THEN B
    ELSE C END AS 컬럼명
    FROM TABLE명
```
- 조건 A를 만족하면 A, B를 만족하면 B 둘다 아니면 C를 지정한 컬럼명에 출력
- 해당 테이블 데이터 전체를 순회하면서 조건을 체크한다.
- SQL의 조건문이라 생각하면 쉬울듯
