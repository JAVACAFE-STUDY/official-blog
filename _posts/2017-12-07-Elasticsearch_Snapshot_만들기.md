---
title: 'Elasticsearch Snapshot 만들기'
date : 2017-12-07 16:40:00
author : 김동우
---

클러스터와 인덱스가 커질수록 누적된 데이터를 유지해야할 필요성이 커집니다. 실제로 복원 할 수 없는 데이터가 존재 한다면 당신은 어떻게 하시겠습니까? 
ELasticsearch에서는 버전별로 데이터를 저장하여 이전상태를 유지 할 수 있는 Snapshot이라는 기능을 제공합니다. 이 모듈은 인덱스의 스탭샷 또는 클러스터 전체의 스냅샷을 만들 수 있습니다. 또한 리포트로 데이터의 저장을 지원 합니다. 

어떻게 스냅샷을 만들고 실행하는지 알아 보도록 하겠습니다. 
먼저 스냅샷을 저장할 경로에 폴더를 만듭니다.

~~~
#mkdir /home/javacafe/elastic/backup
~~~

elasticsearch의 config의 elasticsearch.yml 파일을 열어 아래와 같은 내용을 마지막줄에 추가 합니다. 해당 내용은 /home/javacafe/elastic/backup의 디렉토리에 스냅샷을 저장하겠다는 의미 입니다. 

~~~
path.repo: ["/home/javacafe/elastic/backup"]
~~~
서버의 설정은 실시간으로 반영 되지 않기 때문에 반영을 위해 elasticsearch를 재기동 합니다. 

백업 저장소는 API의 Request Body에 데이터를 담아 스냅샷의 저장소를 정의 합니다.

~~~
#curl -XPUT 'http://localhost:9200/_snapshot/my_backup' -d {
  "type": "fs",
  "settings": {
     "location": "/home/javacafe/elastic/backup",
     "compress": true
  }
}'
~~~

해당 내용을 응답으로 받았다면 정상적으로 저장 된 것 입니다. 

~~~
{
    "acknowledged": true
}
~~~

스냅샷은 전체 클러스터 또는 특정 Index의 백업을 만들 수 있습니다. 해당 API를 통해 전체 클러스터 백업을 질의 할 수 있습니다. 여기서 wait_for_completion = true의 옵션을 주지 않으면 프로세스가 백그라운드로 실행 됩니다. 

~~~
#curl -XPUT "localhost:9200/_snapshot/my_backup/snapshot_1?wait_for_completion=true"
~~~

질의에 대한 응답은 아래와 같습니다.
~~~
{
    "snapshot": {
        "snapshot": "snapshot_1",
        "uuid": "FG0iLYA9Sc-JYsDnsOpLwg",
        "version_id": 5060099,
        "version": "5.6.0",
        "indices": [
            "tweet",
            ".kibana"
        ],
        "state": "SUCCESS",
        "start_time": "2017-11-14T11:38:14.926Z",
        "start_time_in_millis": 1510659494926,
        "end_time": "2017-11-14T11:38:16.203Z",
        "end_time_in_millis": 1510659496203,
        "duration_in_millis": 1277,
        "failures": [],
        "shards": {
            "total": 6,
            "failed": 0,
            "successful": 6
        }
    }
}
~~~

snapshot_1에 대해서 백업이 시작된 시간 부터 끝나는 시간까지 어떠한 인덱스를 백업하였는지에 대한 정보를 확인 할 수 있습니다. 
만약 현재 백업되어 있는 데이터의 스냅샷을 보려면 해당 백업을 GET 메소드를 통해 확인 할 수 있습니다. 이렇게 조회된 내용은 위에 결과로 받은 내용과 같습니다. 

~~~
#curl -XGET 'localhost:9200/_snapshot/my_backup/_all?pretty'
~~~

마지막으로 백업된 스냅샷을 제거하고 싶다면 DELETE 메소드를 사용하여 해당 백업을 삭제하면 됩니다. 
~~~
#curl -XDELETE 'localhost:9200/_snapshot/my_backup/snapshot_1'
~~~

아래와 같은 메시지를 받았다면 성공적으로 제거가 되었습니다. 
~~~
{
    "acknowledged": true
}
~~~


