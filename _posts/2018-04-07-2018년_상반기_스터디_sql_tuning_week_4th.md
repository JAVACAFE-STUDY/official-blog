---
title: '2018년 상반기 스터디 - sql tuning 4주'
date: 2018-04-07 21:00:00
categories:
- 2018년 상반기 스터디
- sql tuning
tags:
- sql
- sql tuning
author : 정재욱
---

# SQL 튜닝의 시작 책 진도 요약
1. Chapter 03
  - 스칼라 서브쿼리의 이해와 효율적인 SQL 작성하기
  : SELECT Column에서 사용된 서브쿼리
    - 최대 결과 건수만큼 반복적으로 수행된다
      - "최대" 반복 수행 횟수는 Main SQL 결과에 따라
      - 동일한 입력값에 대한 결과의 경우 Multi Buffer에 저장해서 리턴
    - 추출되는 데이터는 항상 1건만 유효하다
      - 2건 이상의 결과를 리턴할 경우 에러 발생
    - 데이터가 추출되지 않아도 된다
      - 데이터가 추출되지 않아도 SQL 수행에 영향을 미치지 않음(ERROR가 아닌 FAIL 상태)
    - !! (항상 1건 이하와 데이터가 추출되지 않아도..) 특성에 따라 Outer Join 으로 처리하여도 동일 경과 추출 가능

  - 스칼라 서브쿼리와 조인의 이해 및 활용하기
  : 두 가지 성능문제(1. 수행 위치에 다른, 조인 관계에서)
    - 스칼라 서브쿼리는 최종 결과 만큼 수행하자
      - 아래 두 SQL의 수행방식을 참고하면 메인결과의 건수를 줄일 수 있는 만큼 줄이고 처리해야 효율적이라는걸 알 수 있음
    ```javascript
        /* 스칼라 서브쿼리를 메인 결과 건수만큼 다 처리하고 10건을 추출 */
        SELECT
          rownum AS rnum
        , a.*
          FROM
          ( SELECT
              t1.c1
            , t1.c2
            , t1.c3
            , (SELECT t2.c3 FROM tab2 t2 WHERE t2.c1 = t1.c1) AS t2_c3
              FROM
              tab1 t1
            ORDER BY
              c1
            , c2
          ) a
         WHERE  rownum <= 10
    ```
    ```javascript
        /* 메인 결과를 10건으로 제한 후 스칼라 서브쿼리를 메인 결과에 따라 추출 */
        SELECT
          rownum AS rnum
        , a.*
        , (SELECT t2.c3 FROM tab2 t2 WHERE t2.c1 = a.c1) AS t2_c3
          FROM
          ( SELECT
              t1.c1
            , t1.c2
            , t1.c3
              FROM
              tab1 t1
            ORDER BY
              c1
            , c2
          ) a
         WHERE  rownum <= 10
    ```
    - 스칼라 서브쿼리와 조인의 관계로 보는 SQL 성능 문제 :: 앞 내용 복습하는 느낌
      _ **일반적으로 SQL을 작성할 때 어느 작성 방법이 성능상 유리한지 알 수 있는 공식은 존재하지 않는다
      - **그러나 처리 데이터량, 인덱스 유무, 컬럼 제약조건 등을 고려하지 않은 SQL은 성능 문제를 야기한다
      - 비효율 스칼라 서브쿼리는 조인으로 변경하자
        - 일반적으로 (데이터가 많이 처리되는)배치 프로그램성의 쿼리에는 스칼라 서브쿼리를 작성하지 않는 것이 유리
      - 조인을 스칼라 서브쿼리로 변경하자
        - Multi Buffer를 이용할 수 있는 경우에 유리(메인 결과의 건수가 적거나 입력 데이터가 같은게 많으면)
