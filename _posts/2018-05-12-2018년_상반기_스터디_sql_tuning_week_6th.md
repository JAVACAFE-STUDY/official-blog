---
title: '2018년 상반기 스터디 - sql tuning 6주'
date: 2018-05-12 21:00:00
tags:
- sql
- sql tuning
author : 정재욱
---


# SQL 튜닝의 시작 책 진도 요약
1. Chapter 05
  - MERGE 구문 이해와 효율적인 SQL 작성하기
    - MERGE 구문 작성 시 발생할 수 있는 에러와 해결방법 알아보기 : MERGE 기본 사용법만 제대로 알아도..
      - TARGET TABLE과 SOURCE TABLE의 조인은 1:1이어야 한다
        - Source Table에서 DISTINCT, GROUP BY 등의 처리
        - ON절의 조인 조건 확인
      - UPDATE 컬럼은 ON절에 사용할 수 없다

2. Chapter 07
  - DECODE & CASE WHEN 이해 및 조건 문 처리하기
    - DECODE
      - 구문
        - DECODE(컬럼 or 계산식, 조건1, 답1[, 조건2, 답2, 조건3, 답3, .., ELSE값])
      - 구문에 대한 상세 설명
        - 부등호 조건 처리가 되지 않기 때문에 예전엔 계산을 통해 처리하는 방법이 있었음(SIN함수를 이용해서)
        - 조건1과 답1, 조건2와 답2는 쌍으로서 역할을 수행
        - 중복된 DECODE로 더 복잡한 조건 처리도 가능하긴 함
        ```javascript
        /* col1의 값이 1일때, 2일때, 1이나 2가 아닐경우의 결과를 추출 */
        SELECT DECODE(col1, 1, 'One', 2, 'Two', 'Else') AS chk_col1
          FROM tab1
        ```
      - DECODE와 성능 이슈
        - row를 column으로 변환 시 DECODE를 활용한 SQL작성법
        - null처리 부분은 chapter 08에서 다루기 때문에 스터디때는 미리 알려드렸지만 정리는 chapter 08에서
        ```javascript
        /* 아래처럼 일별 row 데이터를 컬럼별로 뽑고자 할 경우 활용 */
        /* 비효율적인 부분(, 0)에 대해 NULL 설명 추가로 진행 */
        SELECT
          SUM(DECODE(sale_dt, '20180314', target_cnt,0)) AS target_14
        , SUM(DECODE(sale_dt, '20180314', sale_cnt,0)) AS sale_14
        , SUM(DECODE(sale_dt, '20180315', target_cnt,0)) AS target_15
        , SUM(DECODE(sale_dt, '20180315', sale_cnt,0)) AS sale_15
        ....
        , SUM(DECODE(sale_dt, '20180322', target_cnt,0)) AS target_22
        , SUM(DECODE(sale_dt, '20180322', sale_cnt,0)) AS sale_22
          FROM
          daily_sale
         WHERE  sale_dt BETWEEN '20180314' AND '20180322'
        ```
    - CASE
      - 구문
      ```javascript
      CASE [expression] /* CASE 구문 시작 */
        WHEN condition_1 THEN result_1 /* WHEN : 비교 조건을, THEN : WHEN 조건이 TRUE일 때 */
        WHEN condition_2 THEN result_2 /* 위 조건이 FALSE 일경우 다음 WHEN으로 */
        WHEN condition_3 THEN result_3
        .....
        WHEN condition_n THEN result_n
        ELSE result /* 모든 조건이 해당되지 않을 때 */
       END /* CASE 종료 */
      ```
      - 구문에 대한 상세 설명
        - DECODE와 달리 부등호 비교 외 다양한(>, <, =>, =<, BETWEEN, IN, LIKE 등) 조건식이 가능
      - 단순 CASE와 탐색 CASE 사용법
        - 단순 CASE : CASE 다음 expression(컬럼?)을 지정하고 WHERE에서 비교가 아닌 값으로 조건 처리 가능
        - 탐색 CASE : CASE 사용법을 조회하면 주로 나오는 내용(누구나 아는 내용?)

3. Chapter 08
  - NULL 처리 구문 이해와 효율적인 SQL 작성하기 : 값이 없는 데이터를 의미하는 NULL
    - NULL 처리 함수 이해하기
      - NVL(t1, t2) : t1의 값이 NULL일 경우 t2의 값으로 대체
        - t1과 t2의 데이터 타입은 동일해야 함
      - NLV2(t1, t2, t3) : t1의 값이 NOT NULL일 때 t2, NULL일 때 t3의 값으로 대체
        - t2의 데이터 타입으로 t3가 맞춰짐
        - t2의 데이터 타입이 NUMBER인데 t3에 문자(한글 OR A, B, C등?)가 있을 경우엔 에러 발생
    - NVL 활용하기
      - 실행계획 분리하기 : chapter 12에서 자세히 다루는 것으로 Pass~
      - IS NULL 조회 개선하기
        - NULLABLE 컬럼에 대해 값이 초기엔 없지만 채워지면서 UPDATE가 되어 채워지는 컬럼의 경우 NULL인 부분이 적다고 가정했을 때
        - FBI(Function Based Index)활용에 대해 나왔지만.. FBI에 대해 간단히 설명하고 휙~
    - 그룹함수와 NVL 처리 : 그룹함수에서 NULL은 연산에서 제외된다는 기본 규칙만 알면 효율적처리가 절로 됨
      - DECODE나 CASE WHEN 에서 불필요한 ELSE 처리에서 0을 넣는 부분을 넣지 않기
      - 그룹함수가 쓰이고 나서 최종적으로 NULL인 상태에 대해서 NVL처리 해주기
    - NULLABLE 컬럼 사용에 의한 비효율 COUNT 함수 처리
      - COUNT(*), COUNT('a'), COUNT(0), COUNT(column) 별로 처리하는 결과를 안다면 비효율적인 SQL작성하지 않음
    - IS NULL 조회에 대한 개선방법 찾기
      - NVL 처리와 FUNCTION BASED INDEX 생성
      - 컬럼 속성 변경(DEFAULT 설정)과 NULL 데이터 업데이트
      - 컬럼 추가 및 인덱스 생성 후 WHERE절 변경
    - IS NOT NULL 조회에 대한 개선방법 찾기
      - 다양한 IS NOT NULL 처리와 SQL 성능 문제
      - 조인 처리 시 IS NOT NULL 활용하기
    - ' '(BLNAK)와 NULL 데이터 처리하기 : BLANK[->CHR(0)]은 보이지 않아도 값이다
      - ' '(BLANK) 데이터가 NULL일까?
        - 보이지 않아도 값인 BLANK이기 때문에 NULL로 인식되지 않음
      - TRIM & NVL 처리
        - 공백에 대해서 TRIM으로 공백 제거하면 공백이 없어지니 NULL과 같아짐
      - ' '와 NULL 데이터 처리 관련 성능 문제
        - NULLABLE 컬럼에 공백이 있을 경우에 대한 처리인데.. 여기도 FBI 설명이 나옴
