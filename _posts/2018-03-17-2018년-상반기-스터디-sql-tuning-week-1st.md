---
title: '2018년 상반기 스터디 - sql tuning 1주'
date: 2018-03-17 21:00:00
categories:
- 2018년 상반기 스터디
- sql tuning
tags:
- sql
- sql tuning
author : 정재욱
---


# 스터디 전 기본으로 알아둬야 할 용어들 정리

## 서브쿼리별 설명(스칼라서브쿼리, 인라인뷰서브쿼리, 서브쿼리)
 * 스칼라 서브쿼리 : SELECT 절에 쓰이는 서브쿼리
  ```javascript
    SELECT
      col1
    , col2
    , (SELECT scalar_sub FROM sub_table WHERE col1 = a.col1) AS scalar_sub1 /* 스칼라 */
      FROM 
      tab1 a
     WHERE  a.var = 10
  ```
 * 인라인뷰 서브쿼리 : FROM 절에 쓰이는 서브쿼리
  ```javascript
    SELECT
      col1
    , col2
    ,  AS scalar_sub1
      FROM 
      tab1 a
    , (SELECT 
         inline_sub 
       , col1
         FROM 
         sub_table 
        WHERE  var >= 10
      ) b /* 인라인뷰 */
     WHERE  a.var = 10
       AND  b.col1 = a.col1
  ```
 * 서브쿼리 : WHERE 절에 쓰이는 서브쿼리
  ```javascript
    SELECT
      col1
    , col2
      FROM 
      tab1 a
     WHERE  a.var = 10
       AND  a.col1 IN (SELECT col1 FROM sub_table WHERE var >= 100) /* 서브쿼리 */
  ```

## 문제점이 있는 SQL 수정할 곳 찾아내기
 1. 수정이 필요한 부분이 어디인지 집어내고 어떻게 수정해야 할지에 대한 토론
 ```javascript
   SELECT
     b.부서명
   , COUNT(*) AS 사원수
   , SUBSTR(TO_CHAR(a.입사일, 'yyyymmdd'), 1, 4) AS 입사년도
     FROM 
     직원정보 a
   , 부서정보 b
    WHERE  b.부서id = a.부서id /* 조인전에 줄일수 있는 집합이 있는지 확인(부서 레벨로 집합을 묶어서 조인하면 더 효과적) */
      AND  TO_CHAR(a.입사일, 'yyyy') >= '2000' /* 컬럼에 함수를 사용하여 INDEX를 사용하지 못하게 되는 조건절에 대해 수정 필요 */
   GROUP BY
     b.부서명
   , SUBSTR(TO_CHAR(a.입사일, 'yyyymmdd'), 1, 4)
 ```
 2. 수정 후 SQL
 ```javascript
   SELECT
     b.부서명
   , a.사원수
   , a.입사년도
     FROM 
     (SELECT
        부서id
      , TO_CHAR(a.입사일, 'yyyy') AS 입사년도
      , COUNT(*) AS 사원수
       FROM
       직원정보
      WHERE  입사일 >= TO_DATE('20000101', 'yyyymmdd')
     GROUP BY
       부서id
     , TO_CHAR(a.입사일, 'yyyy')
     ) a
   , 부서정보 b
    WHERE  b.부서id = a.부서id
 ```

## 실행계획 읽는 법
 1. 위에서 아래로
 2. 안에서 밖으로 (안에서 밖으로를 우선으로 두어 가장 안쪽에 있는 구문을 처음 읽는것이라고 착각해서는 안됨)

# SQL 튜닝의 시작 책 진도 요약
 1. Chapter 01
  - SQL 튜닝의 시작은?
    - SQL 튜닝의 시작은 SQL을 작성한 의도를 제대로 파악하는 것
    - 원본 SQL에서 추출하고자 하는 결과 집합을 망치지 않고 개선하는 것
    - SQL의 의미를 해석하는 것으로부터, 이미 SQL 튜닝이 시작된다
 2. Chapter 02
  - 서브쿼리에 대한 기본 내용 이해하기
    - 서브쿼리란
      - WHERE 절에 비교조건으로 사용되는 SELECT 쿼리
    - 서브쿼리 사용 패턴에 대해 알아보자
      - 추출 결과가 반드시 1건이어야 하는 패턴
      - EXISTS 나 IN 연산자를 사용한 경우로 서브쿼리 결과가 여러 건 추출되는 패턴
    - 서브쿼리의 기본 특성
      - 조인과 달리 모수테이블에 중복 결과가 추출되지 않는다
      - 서브쿼리의 컬럼이 ()밖에서 사용될 수 없다

# SQL 공부하는데 도움될 사이트
1. https://www.techonthenet.com/index.php
