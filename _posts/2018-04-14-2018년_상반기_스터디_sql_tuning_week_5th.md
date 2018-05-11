---
title: '2018년 상반기 스터디 - sql tuning 5주'
date: 2018-04-14 21:00:00
categories:
- 2018년 상반기 스터디
- sql tuning
tags:
- sql
- sql tuning
author : 정재욱
---

# SQL 튜닝의 시작 책 진도 요약
1. Chapter 04
  - WITH절 이해와 효율적인 SQL 작성하기
  : 동일한 데이터를 반복적으로 사용해야 하는 SQL의 성능을 개선하기 위한 방법으로 사용
    - WITH절 동작방식 이해하기
      - MATERIALIZE 동작방식
        - Main SQL에서 WITH절을 2번 이상 수행해야 함(Oracle에서 Hint를 사용하지 않을경우 CBO의 처리)
        - Global Temporary Table(이후 GTT)을 생성한 후, WITH절에서 추출한 결과 셋을 저장
        - Main SQL에서 WITH절을 호출하면, 저장되어 있는 GTT에서 데이터를 읽어옴
      - INLINE VIEW 동작방식
        - Main SQL에서 WITH절을 1번 수행할 때 수행되는 방식(FROM절에서 (SELECT ...)로 짠 것과 동일한 형태라고 해야할까요?)
    - SQL 성능 개선을 위한 WITH절 활용하기
      - 데이터 중복 액세스 제거하기
        - 데이터 건수는 적으나, 데이터 추출 시 I/O 처리량이 많을 때 효과적인 WITH절
        - WITH절에서 가져온 데이터가 너무 많을 경우 GTT에 저장하는 비용과 읽을 때의 비용도 만만치 않음을 주의
      - VIEW PREDICATING 성능 문제 제거하기
        - VIEW PREDICATING란 뷰 외부의 조건을 뷰 내부에서 적용된 상태
        - (1) Main SQL의 조건을 서브쿼리에 조인으로 중복 처리하는 방법
          - 동일한 데이터를 2 번 수행하면서 일어나는 비효율을 감안
        - (2) WITH절을 선언하여 필요한 데이터를 미리 추출한 후 재사용 하는 방법
      - 계층 쿼리의 데이터 처리 최소화 하기
        - START WITH -> CONNECT BY -> WHERE 순으로 처리하는 방식에 대한 Oracle의 계층쿼리 설명으로 대체
    - WITH절을 사용할 때 주의해야 할 점은?
      - 동시성이 높은 경우 MATERIALIZE 동작방식은 피하자
      - 추출 건수가 많은 경우 WITH절은 피하자
      - WITH절 선언 부붐은 SQL의 가장 앞에 위치시키자
      - WITH절에 동작방식 힌트를 추가하자
2. Chapter 05
  - MERGE 구문 이해와 효율적인 SQL 작성하기 : Oracle에서 UPDATE, DELETE, INSERT를 동시에 처리하기 위한 DML
    - MERGE 구문의 구성요소 알기
    ```javascript
        MERGE INTO tab1 a
        USING (SELECT 
                 b.col1
               , b.col2
               , c.col3
                 FROM 
                 tab2 b
               , tab3 c
                WHERE  b.col6 = c.col6
              ) d
           ON (a.col1 = d.col1 AND a.col4 > 10)
        WHEN MATCHED THEN 
        UPDATE SET a.col2 = d.col2
        DELETE WHERE a.col3 < 3
        WHEN NOT MATCHED THEN
        INSERT (a.col1, a.col2, a.col3) VALUES (d.col1, d.col2, d.col3)
        WHERE d.col2 = 100
    ```
      - INTO 절
        - (1) Target table 정의 (2) Hint 구문 적용 가능
        - 1개의 대상 테이블만 지정할 수 있음
      - USING 절
        - Target 테이블에 적용할 데이터를 추출하는 부분으로 ON절에서 조인할 컬럼은 Unique해야 함
      - ON 절
        - UPDATE, DELETE, INSERT 할 부분에 대해 MATCH 하는지 vs. 안하는지 조인 조건 적용
    - MERGE 구문으로 처리되는 데이터 이해하기 : SAMPLE 데이터로 건수 결과가 정리되어 있음
      - 몇 건 UPDATE 될까?
      - 몇 건 DELETE 될까?
      - 몇 건 INSERT 될까?
