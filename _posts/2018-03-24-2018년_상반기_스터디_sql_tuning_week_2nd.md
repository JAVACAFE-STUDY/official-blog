---
title: '2018년 상반기 스터디 - sql tuning 2주'
date: 2018-03-24 21:00:00
categories:
- 2018년 상반기 스터디
- sql tuning
tags:
- sql
- sql tuning
author : 정재욱
---


# 3가지 조인 동작방식 이해하기
  - Nested LOOP
  ```javascript
  /* 기본적인 동작방식으로 구구단 프로그래밍 코드를 예로 들자면
     x가 드라이빙(모수) 테이블이고 y가 연결되는 테이블 혹은 서브쿼리에서 처리할 때의 방식
     밖에서 호출하면 안에서 호출한 결과에 따라 여러번 수행되는 것처럼 처리
     # 기본적(?) 처리 방식으로 순차적으로 처리
     # 연결하는 컬럼에 인덱스가 없을경우 full scan을 드라이빙 테이블 건수만큼 처리하는 불상사가..
   */
    for(x = 2; x <= 9; x++)
    {
      for(y = 2; y <= 9; y++)
      {
         xy = x * y;
      }
    }
  ```
  - Sort Merge
  ```javascript
  /* 조인처리할 테이블을 정렬하여 x에서 y로, y에서 x로 서로 비교하며 조인하는 방식
     x와 y를 각각 정렬하여 왔다갔다 비교하며 추출
     # 조인할 테이블의 정렬이 모두 되어야만 비교하기에 자원 소모가 크다
   */
   x     y
   1     2
   2     3
   3     5
   4     6
   5     8
   6     9
   7     10
   8     11
  ```
  - HASH
  ```javascript
  /* 드라이빙 테이블에 hash 함수(설명을 위해 f(x)라 하겠음) 처리를 한 결과와
     조인할 테이블에 hash 함수를 적용하여 같은 값과 조인하는 방식
     x에 f(x) 처리한 결과와 y에 동일한 f(x)를 적용하여 같은 값이 나오는 것을 추출
     # x 쪽의 데이터가 y 보다 많을 경우 f(x) 처리할 값이 많으므로 비효율적
   */
   x     f(x)         y
   1     4a1251        2
   8     62ddf        3
   2     1y3478f      51
   76    1g94g2z      8
   7     2y3dri       9
   11    12y90gy      1
   13    1kgh2kgf     6
   15    1gho39gh     78
  ```

# Oracle 조인과 ANSI SQL 조인 구문 설명
 - WHERE에서 조인 처리를 하는 Oracle
   - INNER JOIN : a = b
   - OUTER JOIN : a = b(+) -- a에 대해 b가 부족하니 b는 있는 것만 조인
 - WHERE에서도 하고, 조인하는 키워드로 처리하는 ANSI SQL
   - INNER JOIN
   - LEFT OUTER JOIN
   - RIGHT OUTER JOIN
   - FULL OUTER JOIN

# SQL 튜닝의 시작 책 진도 요약
 1. Chapter 02
 - 서브쿼리 동작방식 이해하기
   - FILTER 동작방식
     - 서브쿼리가 쓰인 SQL 에서 실행계획 확인 시 FILTER 문구가 나타남
     - 항상 Main SQL 이 먼저 수행되고, 서브쿼리는 Main SQL 의 데이터를 받아서 확인하는 형태
     - 서브쿼리의 결과값을 Cache 하여 동일한 값 조건으로 확인할 경우 Cache 된 결과값을 이용
   - JOIN 동작방식
     - 서브쿼리와 Main SQL 의 결과를 조인동작 방식처럼 처리
     - FILTER 방식과 달리 서브쿼리가 먼저 수행될 수 있음
   - 서브쿼리 동작방식을 제어하는 힌트들
     - no_nnest, unnest, nl_sj, hash_sj, ns_aj 등이 소개 되어있지만 Oracle 위주이고
     - Oracle hint에 관해 궁금한 경우 Googling 하여 보는게 더 빠를거에요
