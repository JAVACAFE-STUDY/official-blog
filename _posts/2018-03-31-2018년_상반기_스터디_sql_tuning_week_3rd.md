---
title: '2018년 상반기 스터디 - sql tuning 3주'
date: 2018-03-31 21:00:00
categories:
- 2018년 상반기 스터디
- sql tuning
tags:
- sql
- sql tuning
author : 정재욱
---


# SQL 튜닝의 시작 책 진도 요약
1. Chapter 02
  - 서브쿼리를 활용한 SQL 성능개선
    - 비효율적인 MINUS 대신 NOT EXISTS를 사용하자
      - X 집합에서 Y 집합에 존재하지 않는 데이터를 추출하기 위해 사용되는 MINUS
      - 중복된 데이터가 있더라도 한 건으로 추출됨
      ```javascript
       **MINUS**
       - UNION, UNION ALL과 같이 비교대상 컬럼의 수가 동일해야 함
       - 각각 대칭되는 컬럼의 데이터 타입이 동일해야 함
       - tab_x 집합에서 추가로 추출해야 하는 컬럼이 있을경우 서브쿼리로 다시 조회해야 할 경우가 있다
       - tab_x를 먼저 수행하고 tab_y를 뒤에 수행(tab_x의 집합이 적더라도 tab_y가 많으면 오래걸림)
        SELECT c1, c2, c3
          FROM tab_x
        MINUS
        SELECT c11, c22, c33
          FROM tab_y
      ```
      - MINUS처럼 차집합을 추출하지만 중복데이터를 한 건으로 만들어 추출하지 않음
      - MINUS 결과처럼 추출하기 위해 DISTINCT를 남발할 경우 중복되지 않을때도 중복제외 하는 비효율 SQL임
      ```javascript
       **NOT EXISTS**
       - tab_x, tab_y 수행 순서를 변경할 수 있음
       - tab_x에서 추출한 데이터와 tab_y에 비교되는 수 만큼 수행
       - tab_y에 적절한 INDEX 전략이 필요
        SELECT c1, c2, c3, c4
          FROM tab_x a
         WHERE NOT EXISTS (SELECT 1
                             FROM tab_y
                            WHERE c11 = a.c1
                              AND c22 = a.c2
                              AND c33 = a.c3
                          )
      ```
    - 조인대신 서브쿼리를 활용하자
      - SELECT 절에서 추출을 하지 않고 WHERE 절에 JOIN으로 FILTER 처리를 위해서만 쓰이는 쿼리의 경우 조인대신에 서브쿼리를 활용
      - 조인은 곱하기(X)로서 1:M 조인의 결과는 M이니 중복결과를 발생시켜야 하는 경우가 아니라면 조인보다는 EXISTS 활용을..
    - WHERE절의 서브쿼리를 조인으로 변경하자
      - 서브쿼리에 SELECT 절이 많을 경우 CBO가 비용산정에 과감히 포기하는 경우가 발생하기에 모수 테이블이 서브쿼리의 결과를 전달 받아서 처리하는게 빠른 SQL이라 생각될 때 서브쿼리를 조인으로 
