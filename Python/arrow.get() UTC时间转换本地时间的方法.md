### 【python】arrow.get() UTC时间转换本地时间的方法
1. arrow.now() 本地时区时间， arrow.utcnow() UTC时区时间。

2. arrow.get() 方法默认返回值是 UTC时间，与中国的本地时间不一致，相差8小时。

3. 根据源码文档总结以下几种方法，记录一下。获取某一天的起始时刻 00:00:00，转换成时间戳。

```bash
Python 3.6.9 (default, Nov  7 2019, 10:44:02) 
[GCC 8.3.0] on linux
>>> import datetime, time, arrow
>>> from dateutil import tz

# 标准库
>>> int(time.mktime(datetime.datetime(2020, 3, 2).timetuple()))
1583078400

# naive 自动转换成本地时区, arrow.get() 可传多种格式
>>> int(arrow.get('2020-03-02').naive.timestamp())
1583078400
>>> int(arrow.get(2020, 3, 2).naive.timestamp())
1583078400

# replace 替换成本地时区
>>> arrow.get('2020-03-02').replace(tzinfo=tz.tzlocal()).timestamp
1583078400

# arrow.get() 方法中设置本地时区
>>> arrow.get(datetime.datetime(2020, 3, 2, tzinfo=tz.tzlocal())).timestamp
1583078400

>>> arrow.get(datetime.datetime(2020, 3, 2), 'Asia/Shanghai').timestamp
1583078400

>>> arrow.get('2020-03-02 00:00:00 Asia/Shanghai', 'YYYY-MM-DD HH:mm:ss ZZZ').timestamp
1583078400
```
