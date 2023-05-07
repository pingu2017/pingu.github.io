---
layout: post
title: "[Hadoop] Hive에서 auto increment 설정하기"
date: 2023-03-15 21:02:17 +0900
categories: TIL
---

3시간 동안 삽질한 이야기.

hive를 이용해 데이터를 만들고있는데.. sql의 기본인 자동숫자증가기가 없을리 없잖아요?

그래서 열심히 구글링을 해봤지만 한글로 된 블로그는 당연히 하나도 없고 스택오버플로우봐도 신통치 않았습니다..

어쨋든 열심히 뒤져서 만들었습니다.

누군가는 삽질을 하지 않게 인터넷 세상에 공개합니다!!

```
하둡 버전: 3.3.4
하이브 버전: 3.1.2
```

하이브 라이브러리에 자동증가 시스템이 있대요. 그걸 찾아주는 과정

{% highlight ruby %}
cd /home/hadoop/apache-hive-3.1.2-bin/lib/
ll hive-contrib\*
{% endhighlight %}

디렉토리로 옮겨주기
{% highlight ruby %}
hdfs dfs -put /home/hadoop/apache-hive-3.1.2-bin/lib/hive-contrib-3.1.2.jar /user/hive/lib
{% endhighlight %}

하이브 켜고 jar 추가

{% highlight ruby %}
hive
add jar hdfs:///user/hive/lib/hive-contrib-3.1.2.jar;
{% endhighlight %}

함수 생성. 일회성 함수만들고싶으면 TEMPORARY 사용
(create temporary function ~~)

{% highlight ruby %}
create function row_sequence as 'org.apache.hadoop.hive.contrib.udf.UDFRowSequence'
using jar 'hdfs:///user/hive/lib/hive-contrib-3.1.2.jar';
{% endhighlight %}

함수확인!

{% highlight ruby %}
SHOW FUNCTIONS;
describe function row_sequence;

#row_sequence() - Returns a generated row sequence number starting from 1
{% endhighlight %}
