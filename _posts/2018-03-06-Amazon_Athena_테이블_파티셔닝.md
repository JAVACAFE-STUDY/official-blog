---
title: Amazon Athena 테이블 파티셔닝
date : 2018-03-06 12:20:00
author : 최용호
---


아테나에서 테이블 파티셔닝 하는 방법은 두가지가 있다. 첫번째는 S3 버킷명을 아테나에서 파티셔닝 가능하도록 지정한 경우 자동으로 매핑되도록 하는 방법이고, 두번째는 그렇지 않은 경우 수동으로 일자에 해당하는 버킷은 연결하도록 지정하는 방법이다.



## 버킷명으로 자동 매핑

먼저 첫번째 방법으로 자동 매핑이 가능하도록 하려면 아래와 같은 형식의 버킷명 하위에 파일들이 위치해야한다.

```
s3://yourBucket/pathToTable/<PARTITION_COLUMN_NAME>=<VALUE>/<PARTITION_COLUMN_NAME>=<VALUE>/
```

예를 들면 아래와 같은 형식이다.

```
s3://hive-live-log/DEBUG/year=2018/month=03/day=03/
혹은
s3://hive-live-log/DEBUG/dt=2018-03-03/
```

여기서 사용한 year, month, day, dt는 아테나에서 쿼리 시에 버킷 범위를 지정하기 위해 사용할 파티션 컬럼이다. 쿼리가 가능하도록 원하는 컬럼명과 형식으로 지정하면 되기 때문에 year, month, day로 컬럼명을 맞출 필요는 없다. 이렇게 버킷을 생성하고 아테나에서 테이블 생성 시 파티션 컬럼을 설정하는 부분에서 아래와 같이 파티션 명에 맞춰서 컬럼을 생성한다.

![](http://tech.javacafe.io/img/blog/20180306/athena_partitioning.png)

테이블이 생성되고 나면 자동으로 파티션을 지정할 수 있도록 아래 명령을 수행한다. 여기서는 테이블명을 debug_partitioned로 생성했다.

```
MSCK REPAIR TABLE debug_partitioned;
```

그러면 약간의 시간이 흐른 뒤에 아래와 같은 결과가 반환 된다.

```
debug_partitioned:year=2018/month=03/day=03
Repair: Added partition to metastore debug_partitioned:year=2018/month=03/day=03
```



## 수동 매핑

만약 버킷명이 위와같이 생성되어 있지 않은 경우에는 어쩔 수 없이 ALTER TABLE 명령으로 수동 매핑을 해주어야 한다. 아래와 같은 구조로 버킷이 지정되어 있다고 가정해보자.

```
s3://hive-live-log/DEBUG/2018/03/03/
```

이런 경우 아테나 테이블의 파티션 컬럼과 매핑이 되지 않기 때문에 아래와 같은 쿼리를 통해 일자별로 파티션 등록을 해주어야 한다.

```
Alter Table <tablename> add Partition (PARTITION_COLUMN_NAME = <VALUE>, PARTITION_COLUMN2_NAME = <VALUE>) LOCATION ‘s3://yourBucket/pathToTable/YYYY/MM/DD/
```

예를 들면 아래와 같다.

```
Alter Table debug_partitioned add Partition (year=2018, month=3, day=3) LOCATION ‘s3://hive-live-log/DEBUG/2018/03/03/
```



## 쿼리

쿼리 시에는 파티션 컬럼으로 지정한 값으로 범위를 지정해 주어야 파티셔닝이 적용된 쿼리가 수행된다.  먼저 파티션 설정이 되어 있지 않은 테이블에서의 쿼리를 살펴보자.

```sql
SELECT * FROM "hive_live"."trace" where timestamp between timestamp '2017-12-10 00:00:00.000' and timestamp '2017-12-11 00:00:00.000' order by timestamp desc limit 10;
```

hive_live라는 데이터베이스의 trace 테이블에서 쿼리를 수행하는데, 이 테이블은 trace 로그에 대한 모든 날짜에 대해 쿼리를 수행한다.  전체 파일을 대상으로 쿼리를 수행했기 때문에 스캔 한 파일의 크기가 굉장히 크고, 쿼리를 수행하는데 걸린 시간도 오래걸렸다.

```
(Run time: 32.07 seconds, Data scanned: 481.38GB)
```

이제 파티션이 적용된 테이블에서 쿼리를 수행해보도록 하자. 동일한 쿼리를 아래와 같이 파티션 컬럼과 함께 수행한다.

```sql
SELECT * FROM "hive_live"."trace_partitioned" where timestamp between timestamp '2017-12-10 00:00:00.000' and timestamp '2017-12-11 00:00:00.000' and year = 2017 and month = 12 and day = 10 order by timestamp desc limit 10;
```

그러면 파티션 컬럼인 year, month, day에 의해 매핑되는 S3 버킷만을 참조하기 때문에 스캔 범위가 굉장히 축소된다. 이로 인해 비용도 줄일 수 있고 쿼리 시간도 단축시킬 수 있게 된다.

```
(Run time: 4.72 seconds, Data scanned: 3.84GB)
```



## 정리

이렇게 파티셔닝 기능을 사용하여 테이블을 구성하면 쿼리당 스캔되는 데이터 양을 줄일 수 있기 때문에 성능을 향상 시킬 수 있다.  이 기능을 몰랐을 때는 S3 버킷의 범위별로 테이블을 하나씩 생성해서 테이블이 굉장히 많아졌었는데 단일 테이블로 사용할 수 있어서 편리했다. 나의 경우에는 초기에 파티셔닝 기능을 염두하고 버킷을 생성하지 않았기 때문에 수동 매핑 방식을 사용하였는데 일자가 변경될 때마다 ALTER TABLE 명령을 수행해야한다는 불편함이 존재하긴 했지만 자동 스크립트를 통해 자동화 할 수 있었다.



## 참고

* [Amazon Athena – 10가지 성능 향상 팁](https://aws.amazon.com/ko/blogs/korea/top-10-performance-tuning-tips-for-amazon-athena/)
* [Amazon Athena Partitioning](https://docs.aws.amazon.com/ko_kr/athena/latest/ug/partitions.html)
